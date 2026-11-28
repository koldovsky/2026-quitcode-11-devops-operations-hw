# Здача WS11

Скорочення: WS (workshop — заняття курсу), PM (project manager — менеджер проєкту), SHA (Secure Hash Algorithm — тут: 40-символьний ідентифікатор коміту), URL (Uniform Resource Locator — адреса), SLO (Service Level Objective — ціль рівня сервісу), RPO/RTO (Recovery Point / Recovery Time Objective — скільки даних можна втратити і скільки продукт може не працювати), UAT (User Acceptance Testing — приймальне тестування), CI (continuous integration — автоматичні перевірки кожної зміни), PR (pull request — запит на злиття змін), MCP (Model Context Protocol — спосіб дати агенту інструменти зовнішнього сервісу).

## Дані

- Роль: dev
- Команда: school-purchasing
- GitHub-логін: kdeineko-tech
- Ваше репо: https://github.com/kdeineko-tech/school-purchasing
- SHA: 57621aa52cb1db9b5aae4d0d2c241035806c3269
- Репо вимог PM: https://github.com/ValiaBihun/2026-quitcode-dream-team
- Прод-URL: https://school-purchasing-lemon.vercel.app
- Staging-URL: https://school-purchasing-git-staging-kate-bdb7.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://school-purchasing-lemon.vercel.app/api/health — `"db":"ok"`, `"sha"` = `57621aa52cb1db9b5aae4d0d2c241035806c3269` (SHA main)
- Зелений прогін `smoke` на `main`: https://github.com/kdeineko-tech/school-purchasing/actions/runs/46118999894 (job `prod`, після деплою `57621aa` — останнього злиття)
- `accounts.zip` на SHA: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/accounts.zip
- Ролі тестових акаунтів: адміністратор — admin@example.test, pm-admin@example.test; працівник (requester) — member1@example.test, member2@example.test, pm-requester@example.test; схвалювач — pm-approver@example.test (роль схвалювача — через відділ, який він схвалює)
- Версія `next`: next@16.4.6

### Середовища й реліз (трек A)

- Гілка `staging`: https://github.com/kdeineko-tech/school-purchasing/tree/staging
- Зміни з `main` у `staging`: fast-forward `origin/main` → `staging` (`3d8e1d3..57621aa`, без merge-коміту) — https://github.com/kdeineko-tech/school-purchasing/commit/57621aa52cb1db9b5aae4d0d2c241035806c3269
- Змінна лише для гілки `staging`: `APP_ENV` — поле `"appEnv":"staging"` у `/api/health` на staging (на проді `null`)
- Проєкт Supabase для staging: `school-purchasing-staging`; supabaseRef staging: `aiqatxeupwveczluemen`; supabaseRef проду: `tyxyvgilaoykzftynvtm`
- Міграції в базу staging через CI: https://github.com/kdeineko-tech/school-purchasing/actions/runs/46077450619 — job `db-deploy-staging` друкує «Ціль міграцій: проєкт aiqatxeupwveczluemen (staging)», потім `db-deploy` (прод); файл https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.github/workflows/ci.yml#L128; міграція PR #99 (видалення застарілих таблиць з іншого навчального завдання) — https://github.com/kdeineko-tech/school-purchasing/actions/runs/46093297398: `db-deploy-staging`, потім `db-deploy`, обидва success
- Workflow smoke деплою: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.github/workflows/smoke.yml — адреса з `client_payload.url`, заголовок `x-vercel-protection-bypass` із секрету лише для хостів проєкту
- Зелений smoke на деплой staging: https://github.com/kdeineko-tech/school-purchasing/actions/runs/46119554945 (коміт `57621aa`; job `prod` — skipped, хибного прод-алерту немає) — `"appEnv":"staging"`, `"supabaseRef":"aiqatxeupwveczluemen"`
- Smoke на деплой одного PR-preview: https://github.com/kdeineko-tech/school-purchasing/actions/runs/46081657793 (preview гілки `task/incident-profile`) — `"supabaseRef":"aiqatxeupwveczluemen"` (база staging, не прод)
- Доступ PM до staging: захист Vercel для preview вимкнено (відкриває всі preview), вхід у застосунок — акаунтами з `accounts.zip`; https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/runbook.md (таблиця «Де що», рядок «Доступ PM до staging»; розділ 9 — залишкові ризики)
- Реліз `v1.0.0`: https://github.com/kdeineko-tech/school-purchasing/releases/tag/v1.0.0
- Нотатки за категоріями: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.github/release.yml (PR #97)
- Прод на тегу: тег `v1.0.0` → коміт `57621aa52cb1db9b5aae4d0d2c241035806c3269` = `"sha"` у https://school-purchasing-lemon.vercel.app/api/health (`"env":"production"`, `"db":"ok"`)
- Приймання PM перед тегом: приймання PM не отримано: запит 2026-11-27, повідомлення в чаті команди з адресою staging (посилання немає: приватний Telegram-чат команди). Тег поставлено без підпису PM; `staging` лишається на `57621aa` (= `v1.0.0`), доки PM не прийме реліз

### Сигнали (трек B)

- Крок алерту: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.github/workflows/smoke.yml#L93-L94 (job `prod`) і #L168-L169 (job `deploy`) — `if: failure()`, токен і chat id із Secrets, без них — попередження, smoke однаково червоний
- Навмисно червоний прогін: https://github.com/kdeineko-tech/school-purchasing/actions/runs/46030065141 (ручний запуск з `url: https://example.invalid`); справжній — дріл, https://github.com/kdeineko-tech/school-purchasing/actions/runs/46088550677
- Знімок алерту: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/ops/smoke-alert-telegram.png (знімок з групи «Alerts✨»; chat id і токена на ньому немає)
- Хто отримує алерт: Telegram-група команди «Alerts✨»: розробниця і PM (та бот)
- Трекер помилок: власний `onRequestError` → та сама Telegram-група; https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/src/instrumentation.ts і https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/src/lib/ops/error-alert.ts (PR #96)
- Дані моделі в трекері: власний `onRequestError`: без заголовків запиту (cookie), тіла, `?query` і `error.message`; маршрут — лише шаблон `routePath`; стек — до 5 рядків «at …» зі скороченими шляхами; https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/src/lib/ops/error-alert.ts#L4-L5
- Подія навмисної помилки: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/ops/error-tracker-telegram-staging.png — повідомлення «SERVER ERROR» з маршрутом `/api/ops/test-error` і стеком, staging
- Тестовий маршрут прибрано чи закрито: закрито — `/api/ops/test-error` лише для активного адміністратора (401 гостю, 403 іншим ролям) і 404 на Production; PR https://github.com/kdeineko-tech/school-purchasing/pull/96
- Журнал `llm.call`: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/src/lib/ai/llm-log.ts#L43 — поля: `requestId`, `route`, `model`, `fallbackUsed`, `state`, `latencyMs`, `inputTokens`, `outputTokens`, `costUsd`, `promptVersion`, `errorKind` (перелік кодів); PR https://github.com/kdeineko-tech/school-purchasing/pull/95
- Тест «без тексту користувача»: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/tests/llm-log.test.ts і https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/tests/purchases/llm-call.test.ts (маркер з опису не потрапляє в журнал); зелений CI https://github.com/kdeineko-tech/school-purchasing/actions/runs/46077450619; рядок `llm.call` у Vercel Logs (staging): https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/ops/llm-call-vercel-logs-staging.png

### Інцидент-дріл (трек C)

- Сценарій: хибне значення `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` (один символ) лише для гілки `staging` у Vercel → Redeploy; без міграцій і без видалення даних, прод і база проду не зачеплені. Прогноз сигналу до поломки: https://github.com/kdeineko-tech/school-purchasing/issues/101
- Сигнал і час: червоний smoke `deploy` https://github.com/kdeineko-tech/school-purchasing/actions/runs/46088550677 (`/api/health` 503 `invalid-api-key`) → алерт у Telegram; поломка (деплой Ready) о 02:33:45, алерт о 02:33:58 (Київ) — від поломки до алерту 13 с; відновлення о 02:55:53 — 22 хв
- Профіль «лише пропонує»: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.claude/settings.json і https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.claude/settings.incident.json (PR #100) — інструменти запису MCP Vercel і Supabase заборонені поіменно, читання — дозволено поіменно; після рев'ю заборонено також git-записи, `vercel` без підкоманди і `filter_project_envs`
- MCP-конфіг: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.mcp.incident.json — Vercel (OAuth, без токенів) і Supabase з `read_only=true` і `project_ref` проєкту staging; у розборі Supabase MCP не підключали (гіпотеза не про базу)
- Стенограма розбору: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/incidents/2026-11-28-staging-publishable-key-rca.md — дослівно (`/export`), із заголовком
- Виправлення людиною, не агентом: перевизначення змінної для гілки `staging` видалила розробниця у Vercel → Redeploy → зелений smoke https://github.com/kdeineko-tech/school-purchasing/actions/runs/46090488819; агент лише запропонував (спроба запису — відмова профілю, у стенограмі). PR чи коміту виправлення немає й не могло бути: поломка — лише значення змінної у Vercel, виправлення — видалення цього перевизначення в налаштуваннях Vercel, без змін коду; хронологія й усунення — https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/incidents/2026-11-28-staging-publishable-key.md#хронологія і https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/incidents/2026-11-28-staging-publishable-key.md#усунення
- Postmortem: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/incidents/2026-11-28-staging-publishable-key.md
- Runbook: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/runbook.md (PR #98)
- Дія з postmortem → перевірка: PR https://github.com/kdeineko-tech/school-purchasing/pull/102 — дріл виявив хибний прод-алерт від події `status`; подію прибрано з `smoke.yml`, тест `tests/smoke-workflow.test.ts` (на старому `smoke.yml` червоний) — зелений прогін CI на main: https://github.com/kdeineko-tech/school-purchasing/actions/runs/46100704187
- Розбір PM: не надходило

### Ланцюг постачання (трек D)

- `dependabot.yml`: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.github/dependabot.yml
- Dependabot запускався: https://github.com/kdeineko-tech/school-purchasing/pull/91
- `.npmrc`: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.npmrc
- Дії за SHA: https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/.github/workflows/smoke.yml#L67 — `uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1`
- Push protection і CodeQL: прогін CodeQL https://github.com/kdeineko-tech/school-purchasing/actions/runs/46091246858; налаштування Code security (push protection): https://github.com/kdeineko-tech/school-purchasing/blob/57621aa52cb1db9b5aae4d0d2c241035806c3269/docs/ops/code-security-codeql-push-protection.png
- `next` ≥ 16.4.5: https://github.com/kdeineko-tech/school-purchasing/pull/90 — next 16.4.6, злито, CI зелений

## Бонус

—
