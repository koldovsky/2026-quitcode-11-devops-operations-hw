# Здача WS11

Скорочення: WS (workshop — заняття курсу), PM (project manager — менеджер проєкту), SHA (Secure Hash Algorithm — тут: 40-символьний ідентифікатор коміту), URL (Uniform Resource Locator — адреса), SLO (Service Level Objective — ціль рівня сервісу), RPO/RTO (Recovery Point / Recovery Time Objective — скільки даних можна втратити і скільки продукт може не працювати), UAT (User Acceptance Testing — приймальне тестування), CI (continuous integration — автоматичні перевірки кожної зміни), PR (pull request — запит на злиття змін), MCP (Model Context Protocol — спосіб дати агенту інструменти зовнішнього сервісу).

## Дані

- Роль: dev
- Команда: school-purchasing
- GitHub-логін: mvorobets-blip
- Ваше репо: https://github.com/mvorobets-blip/school-purchasing
- SHA: 8c1a11ddb07ac1b14491e15f94bf2075a72bddcd
- Репо вимог PM: https://github.com/ValiaBihun/2026-quitcode-dream-team
- Прод-URL: https://therapy-scheduling-ten.vercel.app
- Staging-URL: https://school-purchasing-git-staging-mvorobets-blip.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://therapy-scheduling-ten.vercel.app/api/health — `"db":"ok"`, `"sha":"8c1a11ddb07ac1b14491e15f94bf2075a72bddcd"`
- Зелений прогін `smoke` на `main`: https://github.com/mvorobets-blip/school-purchasing/actions/runs/45578770694 (деплой проду, job `prod`: `"sha":"8c1a11ddb07ac1b14491e15f94bf2075a72bddcd"`, `"supabaseRef":"tqbvafkcniaspvluverp"`, `"db":"ok"`)
- `accounts.zip` на SHA: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/accounts.zip
- Ролі тестових акаунтів: адміністратор — admin@example.test, pm-admin@example.test; схвалювач — approver-primary@example.test, approver-office@example.test; працівник (заявник) — member1@example.test, member2@example.test, pm-requester@example.test, pm-approver@example.test (схвалює через призначення у відділі)
- Версія `next`: next@16.4.5

### Середовища й реліз (трек A)

- Гілка `staging`: https://github.com/mvorobets-blip/school-purchasing/tree/staging
- Зміни з `main` у `staging`: fast-forward з `main` (`git push origin origin/main:refs/heads/staging`, runbook розділ 6) — https://github.com/mvorobets-blip/school-purchasing/commit/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd
- Змінна лише для гілки `staging`: `APP_ENV` (Vercel → Preview → гілка `staging`) — поле `appEnv` у `/api/health`: на staging `"staging"`, на preview з PR `null` (з 23.11, коли змінну обмежено гілкою), на проді `null`
- Проєкт Supabase для staging: `school-purchasing-staging`; supabaseRef staging: `rnawhejsgqhgbtjajjmb`; supabaseRef проду: `tqbvafkcniaspvluverp`
- Міграції в базу staging через CI: https://github.com/mvorobets-blip/school-purchasing/actions/runs/45578708307 — job `db-deploy-staging` друкує «Ціль міграцій: проєкт rnawhejsgqhgbtjajjmb (staging)», «Apply migrations» — success, далі `db-deploy` (production); файл https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.github/workflows/ci.yml#L172
- Workflow smoke деплою: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.github/workflows/smoke.yml
- Зелений smoke на деплой staging: https://github.com/mvorobets-blip/school-purchasing/actions/runs/45580169407 (`"sha":"8c1a11ddb07ac1b14491e15f94bf2075a72bddcd"`, `"appEnv":"staging"`, `"supabaseRef":"rnawhejsgqhgbtjajjmb"`, `"db":"ok"`)
- Smoke на деплой одного PR-preview: https://github.com/mvorobets-blip/school-purchasing/actions/runs/45217536189 (PR #107: `"appEnv":null`, `"supabaseRef":"rnawhejsgqhgbtjajjmb"`)
- Доступ PM до staging: спільне посилання Vercel (Share), токен посилання — лише PM особисто — https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/runbook.md#де-що
- Реліз `v1.0.0`: https://github.com/mvorobets-blip/school-purchasing/releases/tag/v1.0.0
- Нотатки за категоріями: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.github/release.yml
- Прод на тегу: SHA тегу `v1.0.0`: `7b294469d074a21d8b6a522110f5e59015ec0aea`; SHA у `/api/health` проду: `8c1a11ddb07ac1b14491e15f94bf2075a72bddcd` — різні. Після тегу злито лише #109 (документація: 1 файл, 3 рядки — прибрано локальний шлях зі стенограми RCA): https://github.com/mvorobets-blip/school-purchasing/compare/7b29446...8c1a11d; код застосунку, CI й міграції ті самі. Прод, `main` і `staging` — на `8c1a11d`, smoke зелені (прод https://github.com/mvorobets-blip/school-purchasing/actions/runs/45578770694, staging https://github.com/mvorobets-blip/school-purchasing/actions/runs/45580169407). Тег не переносимо: PM приймала саме `7b29446`
- Приймання PM перед тегом: https://github.com/mvorobets-blip/school-purchasing/issues/103#issuecomment-7137527673 (24.11.2026; staging на `7b29446` з 13:06 UTC — smoke https://github.com/mvorobets-blip/school-purchasing/actions/runs/45400028999; тег — після приймання: реліз опубліковано 24.11 16:37 UTC)

### Сигнали (трек B)

- Крок алерту: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.github/workflows/smoke.yml#L183 (job `deploy`; для проду — той самий крок у job `prod`, рядок 109)
- Навмисно червоний прогін: https://github.com/mvorobets-blip/school-purchasing/actions/runs/44943997387
- Знімок алерту: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/ops/ws11-telegram-alert.png
- Хто отримує алерт: Telegram-група команди: розробник, PM
- Трекер помилок: власний `onRequestError` — https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/src/instrumentation.ts, повідомлення — https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/src/lib/error-alert.ts
- Дані моделі в трекері: власний `onRequestError` — без заголовків, cookie, тіла й рядка запиту та `error.message`; надсилаємо лише маршрут, тип, метод, назву помилки, digest, до 5 рядків стеку, середовище, деплой і коміт — https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/src/lib/error-alert.ts#L3-L8
- Подія навмисної помилки: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/ops/ws11-error-tracker.png — staging
- Тестовий маршрут прибрано чи закрито: https://github.com/mvorobets-blip/school-purchasing/pull/102
- Журнал `llm.call`: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/src/lib/ai/llm-log.ts — поля: модель, затримка, токени, стан, fallback, версія промпту, id запиту (знімок — https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/ops/ws11-llm-call-log.png)
- Тест «без тексту користувача»: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/tests/llm-log.test.ts і https://github.com/mvorobets-blip/school-purchasing/actions/runs/45578708307

### Інцидент-дріл (трек C)

- Сценарій: хибне значення `NEXT_PUBLIC_SUPABASE_URL` (один символ ref) лише для Preview → гілка `staging` + Redeploy; без міграцій і без видалення даних, прод і база проду не зачеплені — issue https://github.com/mvorobets-blip/school-purchasing/issues/105
- Сигнал і час: червоний smoke https://github.com/mvorobets-blip/school-purchasing/actions/runs/45211781650; поломка (деплой готовий) ~16:01:30 UTC, алерт 16:01:50 UTC — від поломки до алерту ~17–20 с; виправлено 16:22:30 UTC (https://github.com/mvorobets-blip/school-purchasing/actions/runs/45215733895); знімок — https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/ops/ws11-drill-alert.png
- Профіль «лише пропонує»: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.claude/settings.json і окремий профіль дрілу https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.claude/settings.incident.json — інструменти запису MCP Vercel і Supabase заборонені поіменно
- MCP-конфіг: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.mcp.json — Supabase з `read_only=true`, `project_ref` зі змінної `SUPABASE_PROJECT_REF`, без токенів; для дрілу — окремий https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.mcp.incident.json: Vercel (OAuth) і Supabase з `read_only=true` і `project_ref` staging, без токенів
- Стенограма розбору: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/incidents/2026-11-23-staging-supabase-url-rca.md
- Виправлення людиною, не агентом: https://github.com/mvorobets-blip/school-purchasing/pull/107 — PR, що документує виправлення людиною (postmortem: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/incidents/2026-11-23-staging-supabase-url.md#L50 і https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/incidents/2026-11-23-staging-supabase-url.md#L93). Саме виправлення — лише в налаштуваннях Vercel, тому коду воно не змінювало: людина видалила перевизначення `NEXT_PUBLIC_SUPABASE_URL` для гілки `staging` й зробила Redeploy — коментар https://github.com/mvorobets-blip/school-purchasing/issues/105#issuecomment-7107047135, зелений smoke https://github.com/mvorobets-blip/school-purchasing/actions/runs/45215733895
- Postmortem: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/incidents/2026-11-23-staging-supabase-url.md
- Runbook: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/runbook.md
- Дія з postmortem → перевірка: https://github.com/mvorobets-blip/school-purchasing/pull/106 і прогони: червоний https://github.com/mvorobets-blip/school-purchasing/actions/runs/45349576560 (зламаний деплой дрілу; нова перевірка в журналі: `supabaseRef` `rnawhejsgqhgbtjajjmc`, а очікувався `rnawhejsgqhgbtjajjmb`), зелений https://github.com/mvorobets-blip/school-purchasing/actions/runs/45349590267 (staging)
- Розбір PM: https://github.com/mvorobets-blip/school-purchasing/pull/107#issuecomment-7107635893 (розбір); зміни за розбором — коміт https://github.com/mvorobets-blip/school-purchasing/commit/686a040 у #107; прийнято — https://github.com/mvorobets-blip/school-purchasing/pull/107#issuecomment-7128100726

### Ланцюг постачання (трек D)

- `dependabot.yml`: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.github/dependabot.yml
- Dependabot запускався: https://github.com/mvorobets-blip/school-purchasing/pull/96
- `.npmrc`: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.npmrc
- Дії за SHA: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/.github/workflows/smoke.yml
- Push protection і CodeQL: https://github.com/mvorobets-blip/school-purchasing/blob/8c1a11ddb07ac1b14491e15f94bf2075a72bddcd/docs/ops/ws11-code-security.png і прогін CodeQL https://github.com/mvorobets-blip/school-purchasing/actions/runs/45140612094
- `next` ≥ 16.4.5: https://github.com/mvorobets-blip/school-purchasing/pull/96 — next@16.4.5

## Бонус

—
