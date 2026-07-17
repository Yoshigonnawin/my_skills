# Шпаргалка: когда что запускать

Короткая практическая версия. Обоснование и детали permissions — в
[opencode_orchestration_guide_ru.md](opencode_orchestration_guide_ru.md).

## TL;DR

| Задача | Что использовать |
|---|---|
| Опечатка, локальное переименование, тривиальный однострочный фикс | `build` напрямую |
| Нужно сперва разобраться в коде, не факт что будете что-то менять | `plan` |
| Endpoint, интеграция, бизнес-логика в нескольких файлах (`standard`) | `/prepare` → `/review-plan` → `/implement` → `/verify` |
| Миграция, auth, платежи, публичный контракт, необратимая операция (`risky`) | та же цепочка команд + human gate после `/review-plan` |
| «Просто сделай X» в свободном чате с orchestrator, без команд | не делайте так для standard/risky — см. «Частые ошибки» |

## Встроенные primary-агенты: `plan` и `build`

- `build` — рабочий режим по умолчанию, сразу редактирует файлы. Годится
  только для тривиальных, изолированных правок.
- `plan` — read-only режим для исследования кода перед решением, что делать.
  Ничего не меняет.
- Ни `plan`, ни `build` не создают task-spec, не вызывают reviewer и не
  гейтуют реализацию. Для standard/risky задач их не хватает — переходите на
  командную цепочку ниже.

## orchestrator и команды инженерного workflow

`orchestrator` — кастомный primary-агент (`agents/orchestrator.md`), но
**переключаться на него вручную не нужно**: у каждой команды ниже во
frontmatter уже стоит `agent: orchestrator`, и OpenCode сам подставляет его
на время выполнения команды — независимо от того, в каком режиме вы сейчас
находитесь (`plan`, `build` или сам `orchestrator`).

Порядок вызова команд — это и есть весь workflow:

```
/prepare <task-id> <описание>   draft -> researched        (внутри: researcher)
/review-plan <task-id>          researched/reviewed -> approved  (reviewer, режим plan)
/implement <task-id>            approved -> verification    (внутри: coder-easy/medium/hard)
/review-diff <task-id>          опционально: сверяет diff с планом (reviewer, режим diff)
/verify <task-id>               verification -> done/blocked (внутри: verifier)
/task-status <task-id>          в любой момент: показать состояние, ничего не меняет
```

Важные технические свойства, не только соглашения:

- `/implement` откажется запускать реализацию для `standard`/`risky` задачи,
  если в task-spec нет записи `Plan Review: APPROVED` — сначала обязателен
  `/review-plan`.
- orchestrator никогда не пишет код сам. Реализацию делают `coder-easy`,
  `coder-medium` или `coder-hard` — они скрытые (`hidden: true`), вызываются
  только изнутри `/implement`, напрямую вы их не вызываете.

## Когда НЕ заводить task-spec

Для риска `simple` (локальный тривиальный фикс, переименование, простой
тест) — просто `build`, без `/prepare`. Task-spec — накладные расходы,
оправданные только для `standard`/`risky`.

## Частые ошибки

- Переключаться на `orchestrator` вручную «про запас» перед командами — не
  нужно, команды делают это сами.
- Просить orchestrator в свободном чате «сделай X» вместо явной цепочки
  команд — тогда решение, нужен ли `/review-plan`, отдаётся на усмотрение
  модели, а не гарантируется структурой.
- Пропускать `/review-plan` для standard/risky задачи, посчитав её «на вид
  простой» — риск и сложность реализации не одно и то же.
- Вызывать `coder-*` или `researcher`/`reviewer`/`verifier` напрямую — это
  internal-роли для orchestrator'а, а не для пользователя.
