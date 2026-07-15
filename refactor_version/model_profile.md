# Профили моделей для агентов OpenCode

## Проблема

Модель для каждого агента сейчас жёстко прописана во frontmatter
`.opencode/agents/<name>.md` (`model: opencode/gpt-5.6-terra` и т.д.). Чтобы
попробовать другой набор моделей на задаче -- дешёвые модели для
research/verify, более сильную для review, -- приходится вручную
редактировать четыре файла. Нет способа выбрать набор моделей перед началом
конкретной задачи и быстро вернуться к прежнему.

## Идея

OpenCode загружает конфигурацию в следующем порядке (позже -- приоритетнее):

```text
remote config -> global config -> custom config (OPENCODE_CONFIG) -> project config
```

`OPENCODE_CONFIG` -- это путь к произвольному json-файлу, который можно
задать переменной окружения перед запуском. Это даёт готовый механизм
профилей: один json-файл на набор моделей, выбор -- через переменную
окружения перед стартом `opencode`.

Модель агента при этом переопределяется централизованно, через ключ `agent`
в конфиге, а не через frontmatter:

```json
{
  "agent": {
    "orchestrator": { "model": "provider/model-id" }
  }
}
```

Если агент не задаёт `model` во frontmatter, используется значение из
конфига. Значит: уберите `model:` из `.md`-файлов агентов, и модель полностью
определяется активным профилем.

## Структура файлов

```text
.opencode/
├── profiles/
│   ├── quality.json
│   ├── balanced.json
│   └── cheap.json
├── agents/
│   ├── orchestrator.md   # без строки model:
│   ├── researcher.md     # без строки model:
│   ├── reviewer.md       # без строки model:
│   └── verifier.md       # без строки model:
```

`profiles/` хранит наборы моделей как явные, версионируемые файлы. Их можно
коммитить в Git -- в отличие от `tasks/`, это не временное состояние, а
конфигурация процесса.

## Пример профиля: `cheap.json`

```json
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "orchestrator": { "model": "deepseek/deepseek-v4-flash" },
    "researcher":   { "model": "deepseek/deepseek-v4-flash" },
    "reviewer":     { "model": "minimax/minimax-3" },
    "verifier":     { "model": "deepseek/deepseek-v4-flash" }
  }
}
```

## Пример профиля: `quality.json`

```json
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "orchestrator": { "model": "openai/gpt-5.6-terra" },
    "researcher":   { "model": "anthropic/claude-fable-5" },
    "reviewer":     { "model": "anthropic/claude-sonnet-5" },
    "verifier":     { "model": "anthropic/claude-fable-5" }
  }
}
```

## Пример профиля: `balanced.json`

```json
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "orchestrator": { "model": "openai/gpt-5.6-terra" },
    "researcher":   { "model": "deepseek/deepseek-v4-flash" },
    "reviewer":     { "model": "anthropic/claude-sonnet-5" },
    "verifier":     { "model": "deepseek/deepseek-v4-flash" }
  }
}
```

Названия провайдеров и точные model-id перед фиксацией нужно сверить в
конкретной установке OpenCode (`opencode models`), а не переносить вслепую.

## Запуск

Разовый запуск с профилем:

```bash
OPENCODE_CONFIG=.opencode/profiles/quality.json opencode
```

Профиль на всю сессию терминала:

```bash
export OPENCODE_CONFIG=.opencode/profiles/cheap.json
opencode
```

## Shell-хелпер как энам

```bash
oc() {
  local profile="$1"; shift
  case "$profile" in
    quality|balanced|cheap) ;;
    *)
      echo "usage: oc {quality|balanced|cheap} [opencode args]"
      return 1
      ;;
  esac
  OPENCODE_CONFIG=".opencode/profiles/${profile}.json" opencode "$@"
}
```

Использование: `oc quality`, `oc cheap`, `oc balanced --continue`.

Добавление нового профиля -- это новый json-файл в `profiles/` и новая ветка
`case`; агенты и команды при этом не меняются.

## Важная ловушка: приоритет project-config

Project-level `opencode.json` в корне репозитория грузится **после**
`OPENCODE_CONFIG` и имеет более высокий приоритет. Если в нём уже есть секция
`agent.<name>.model`, она победит любой профиль -- профиль в этом случае
молча проигнорируется, без ошибки.

Правило: модели агентов задаются **только** в файлах `profiles/*.json`.
Project-level `opencode.json` используется для того, что не должно меняться
между профилями -- `permission`, `instructions`, `$schema`, и т.п., -- и не
должен содержать ключ `agent.*.model`.

## Ограничение флага `--model`/`-m`

`opencode --model provider/model-id` переопределяет модель только у primary
агента текущей сессии. Если у subagent'а (researcher, reviewer, verifier)
модель задана явно -- во frontmatter или в конфиге, -- флаг её не затронет:
подстановка по флагу не каскадируется на субагентов. Это ещё один довод в
пользу переноса `model:` из frontmatter в профили: иначе `--model` создаёт
иллюзию смены модели, которая на деле работает только для orchestrator.

## Что нужно сделать при внедрении

1. Убрать строку `model:` из frontmatter четырёх файлов в `.opencode/agents/`.
2. Создать `.opencode/profiles/{quality,balanced,cheap}.json` с реальными,
   проверенными в установке model-id.
3. Проверить, что project-level `opencode.json` не задаёт `agent.*.model`.
4. Добавить shell-функцию `oc` (или аналог) в `.bashrc`/`.zshrc` либо
   задокументировать прямой вызов через `OPENCODE_CONFIG=...`.
5. Прогнать `/prepare` на тестовой задаче с каждым профилем и убедиться, что
   вызванные субагенты действительно получили ожидаемую модель (видно в логе
   сессии или через `opencode debug config`).
