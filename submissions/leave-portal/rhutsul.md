# Здача WS11

Скорочення: WS (workshop — заняття курсу), PM (project manager — менеджер проєкту), SHA (Secure Hash Algorithm — тут: 40-символьний ідентифікатор коміту), URL (Uniform Resource Locator — адреса), SLO (Service Level Objective — ціль рівня сервісу), RPO/RTO (Recovery Point / Recovery Time Objective — скільки даних можна втратити і скільки продукт може не працювати), UAT (User Acceptance Testing — приймальне тестування), CI (continuous integration — автоматичні перевірки кожної зміни), PR (pull request — запит на злиття змін), MCP (Model Context Protocol — спосіб дати агенту інструменти зовнішнього сервісу).


## Дані

- Роль: dev
- Команда: leave-portal
- GitHub-логін: rhutsul
- Ваше репо: https://github.com/rhutsul/leave-portal
- SHA: 62de93ce7206b5ca877b69c473f30609c2cb8e8f
- Репо вимог PM: https://github.com/mkozlovskyi-ops/course-staff-management-spec
- Прод-URL: https://leave-portal-olive.vercel.app
- Staging-URL: https://leave-portal-git-staging-rh-3ddc.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://leave-portal-olive.vercel.app/api/health — `"db":"ok"`, `"sha"` = SHA `main`
- Зелений прогін `smoke` на `main`: https://github.com/rhutsul/leave-portal/actions/runs/45964737916 (job `prod`, 62de93c, ручний запуск 27.11)
- `accounts.zip` на SHA: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/accounts.zip
- Ролі тестових акаунтів: адміністратор — admin@example.test; менеджери — manager1@example.test, manager2@example.test, manager3@example.test; працівники — teacher1@example.test, teacher2@example.test, teacher3@example.test, assistant1@example.test
- Версія `next`: next@16.4.6

### Середовища й реліз (трек A)

- Гілка `staging`: https://github.com/rhutsul/leave-portal/tree/staging
- Зміни з `main` у `staging`: лише fast-forward (`git push origin origin/main:refs/heads/staging`), без окремої розробки — staging = https://github.com/rhutsul/leave-portal/commit/62de93ce7206b5ca877b69c473f30609c2cb8e8f; правило — https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/runbook.md#6-staging
- Змінна лише для гілки `staging`: `APP_ENV` (Vercel: Preview · staging) — поле `"appEnv":"staging"` у `/api/health` на staging (на PR-preview — `null`)
- Проєкт Supabase для staging: `leave-portal-staging`; supabaseRef staging: tgvhqyboeeusptbjskio; supabaseRef проду: jfuyhspluullwcrimszx
- Міграції в базу staging через CI: прогін CI на main https://github.com/rhutsul/leave-portal/actions/runs/45924946039 — job `db-deploy-staging` друкує «Ціль міграцій: проєкт tgvhqyboeeusptbjskio (staging)», success, потім `db-deploy` (прод); файл https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.github/workflows/ci.yml#L128 (ADR https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/decisions/0010-staging-and-signals.md)
- Workflow smoke деплою: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.github/workflows/smoke.yml — job `deploy`: адреса з `client_payload.url`, заголовок `x-vercel-protection-bypass` із секрету; очікує ref staging
- Зелений smoke на деплой staging: https://github.com/rhutsul/leave-portal/actions/runs/45964952260 — 62de93c, `"appEnv":"staging"`, `"supabaseRef":"tgvhqyboeeusptbjskio"`
- Smoke на деплой одного PR-preview: https://github.com/rhutsul/leave-portal/actions/runs/44755494478 — гілка `chore/deps-nov`, `"supabaseRef":"tgvhqyboeeusptbjskio"` (staging), `"appEnv":null`
- Доступ PM до staging: спільне посилання Vercel (Shareable Link) на адресу гілки staging, надіслано в чат команди — https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/runbook.md#де-що
- Реліз `v1.0.0`: https://github.com/rhutsul/leave-portal/releases/tag/v1.0.0 (нотатки від `ws9-base`, категорії з `.github/release.yml`; перед ним — кандидат https://github.com/rhutsul/leave-portal/releases/tag/v1.0.0-rc.1 на тому самому коміті)
- Нотатки за категоріями: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.github/release.yml
- Прод на тегу: SHA тегу v1.0.0 = `"sha"` у /api/health = 62de93ce7206b5ca877b69c473f30609c2cb8e8f (на момент приймання PM). Після нього — реліз `v1.1.0` https://github.com/rhutsul/leave-portal/releases/tag/v1.1.0 (зріз 08, кроки 3–4: правка запису до рішення, AC-BEHALF-06…10; документи за розбором PM; дрібниці вигляду; окремо PM не приймав, CI й e2e зелені) — прод зараз = SHA тегу v1.1.0 = e8a15ef38ea0ddf1463adaf5f5995cc781e1e32b
- Приймання PM перед тегом: https://github.com/rhutsul/leave-portal/issues/142#issuecomment-7220944479 (2026-11-27, staging, сценарій demo day — прийнято); тег поставлено після нього

### Сигнали (трек B)

- Крок алерту: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.github/workflows/smoke.yml#L102 — `if: failure()`, токен із Secrets, без Telegram — попередження, а не падіння (`scripts/telegram-alert.mjs`)
- Навмисно червоний прогін: https://github.com/rhutsul/leave-portal/actions/runs/44741145512 (ручний запуск на `example.invalid`, «Алерт надіслано в Telegram» о 13:04:55 UTC — саме він на знімку нижче); перший такий прогін — https://github.com/rhutsul/leave-portal/actions/runs/44735640287
- Знімок алерту: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/ops/alert-red-smoke.png
- Хто отримує алерт: Telegram-група команди — розробник і PM
- Трекер помилок: власний `onRequestError` → Telegram — https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/src/instrumentation.ts
- Дані моделі в трекері: без заголовків, cookie і тексту запиту — лише метод, шаблон маршруту, тип і повідомлення помилки: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/src/instrumentation.ts#L42-L52
- Подія навмисної помилки: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/ops/error-tracker-staging.png — staging
- Тестовий маршрут прибрано чи закрито: закрито — лише для ролі адміністратора (`profiles.role`), інакше 404: https://github.com/rhutsul/leave-portal/pull/102
- Журнал `llm.call`: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/src/lib/ai/llm-log.ts — модель, затримка, токени, стан, fallback, версія промпту, id запиту; знімок зі staging — https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/ops/llm-call-staging.png
- Тест «без тексту користувача»: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/tests/llm-log.test.ts і зелений прогін CI https://github.com/rhutsul/leave-portal/actions/runs/45924946039

### Інцидент-дріл (трек C)

- Сценарій: хибне значення `NEXT_PUBLIC_SUPABASE_URL` лише для гілки staging (останній символ ref `o` → `x`); без міграцій і видалення даних, прод і база проду не зачеплені (smoke проду в той самий час зелений — https://github.com/rhutsul/leave-portal/actions/runs/44765995433); прогноз — https://github.com/rhutsul/leave-portal/issues/114
- Сигнал і час: червоний smoke https://github.com/rhutsul/leave-portal/actions/runs/44766000674; поломка (деплой Ready) о 15:42:38 UTC, алерт надіслано о 15:42:55 UTC (журнал прогону: «Алерт надіслано») — від поломки до алерту ≈ 17–20 с
- Профіль «лише пропонує»: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.claude/settings.incident.json (правила зі зразка materials/ops-kit; plan, заборона Edit/Write, git push, команд vercel/supabase, що змінюють деплой, змінні чи базу; інструменти запису MCP Vercel і Supabase — поіменно в deny)
- MCP-конфіг: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.mcp.json — Supabase MCP з `read_only=true` (і Vercel MCP, OAuth); для розбору — https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.mcp.incident.json — Vercel (OAuth, без токенів) і Supabase з `read_only=true` і `project_ref` staging
- Стенограма розбору: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/incidents/2026-11-20-staging-bad-supabase-url-rca.md
- Виправлення людиною, не агентом: запис виправлення — https://github.com/rhutsul/leave-portal/pull/115 (postmortem, розділ «Усунення»; коміт https://github.com/rhutsul/leave-portal/commit/43f9c8806dc7cfef7a19a0e4de79956156729c73); у Vercel — перевизначення змінної видалено людиною о 16:03:28 UTC, деплой Ready о 16:05:12, зелений smoke https://github.com/rhutsul/leave-portal/actions/runs/44769793265 (зміна змінної, не коду — коміту з кодом немає); розділ «Усунення» postmortem — https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/incidents/2026-11-20-staging-bad-supabase-url.md#усунення
- Postmortem: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/incidents/2026-11-20-staging-bad-supabase-url.md
- Runbook: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/runbook.md
- Дія з postmortem → перевірка: https://github.com/rhutsul/leave-portal/pull/116 — `/api/health` називає змінну, що веде не туди (лише назву), smoke і алерт її друкують; CI https://github.com/rhutsul/leave-portal/actions/runs/45924946039
- Розбір PM: https://github.com/rhutsul/leave-portal/pull/115#issuecomment-7220229635 (2026-11-27); 5 зауважень враховано — https://github.com/rhutsul/leave-portal/pull/146, відповідь — https://github.com/rhutsul/leave-portal/pull/115#issuecomment-7220555762

### Ланцюг постачання (трек D)

- `dependabot.yml`: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.github/dependabot.yml — npm і github-actions, щотижня, cooldown, групи
- Dependabot запускався: https://github.com/rhutsul/leave-portal/pull/117 (груповий, злито)
- `.npmrc`: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.npmrc — `min-release-age=7`
- Дії за SHA: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.github/workflows/smoke.yml#L72 — `uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1` (так само в усіх workflow)
- Push protection і CodeQL: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/ops/security-push-protection.png, https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/ops/security-codeql.png, https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/ops/security-dependabot.png
- `next` ≥ 16.4.5: https://github.com/rhutsul/leave-portal/pull/98 (Dependabot, 16.4.3 → 16.4.5, злито), далі 16.4.6 у https://github.com/rhutsul/leave-portal/pull/112

## Бонус

### Розробник

- Варіант: резервна копія і відновлення
- Workflow: https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/.github/workflows/backup.yml — дії за SHA
- Прогін: https://github.com/rhutsul/leave-portal/actions/runs/45806227645 (ручний 26.11, з тренуванням відновлення; дамп даних застосунку — схеми public і private); за розкладом — https://github.com/rhutsul/leave-portal/actions/runs/45595071129 (25.11)
- Резервна копія: шифрування й строк зберігання: gpg AES256 з `BACKUP_PASSPHRASE` (секрет environment production), в артефакт іде лише `*.gpg`, відкриті файли видаляються; артефакт живе 7 днів; дамп у відкритому вигляді — ніде
- Резервна копія: тренування відновлення: job `restore-drill` у тому самому прогоні — відновлення в одноразову базу раннера, «Кількість рядків збігається в усіх таблицях»; розділ «Відновлення» — https://github.com/rhutsul/leave-portal/blob/62de93ce7206b5ca877b69c473f30609c2cb8e8f/docs/runbook.md#9-відновлення-бонус-зашифрована-копія-бази
- Асистент чергового: чернетка розбору: —
- Асистент чергового: межі, ліміти й вартість: —

### PM

—
