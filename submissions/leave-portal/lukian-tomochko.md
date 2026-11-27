# Здача WS11

Скорочення: WS (workshop — заняття курсу), PM (project manager — менеджер проєкту), SHA (Secure Hash Algorithm — тут: 40-символьний ідентифікатор коміту), URL (Uniform Resource Locator — адреса), SLO (Service Level Objective — ціль рівня сервісу), RPO/RTO (Recovery Point / Recovery Time Objective — скільки даних можна втратити і скільки продукт може не працювати), UAT (User Acceptance Testing — приймальне тестування), CI (continuous integration — автоматичні перевірки кожної зміни), PR (pull request — запит на злиття змін), MCP (Model Context Protocol — спосіб дати агенту інструменти зовнішнього сервісу).

## Дані

- Роль: dev
- Команда: leave-portal
- GitHub-логін: lukian-tomochko
- Ваше репо: https://github.com/lukian-tomochko/leave-portal
- SHA: b0e493bc9f4ff1816d23d4cbf000749925da83dc
- Репо вимог PM: https://github.com/mkozlovskyi-ops/course-staff-management-spec
- Прод-URL: https://leave-portal-blush.vercel.app
- Staging-URL: https://leave-portal-git-staging-ai-automation-team1.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://leave-portal-blush.vercel.app/api/health — на момент здачі `"db":"ok"`, `sha` `b0e493bc9f4ff1816d23d4cbf000749925da83dc` (= `main`)
- Зелений прогін `smoke` на `main`: https://github.com/lukian-tomochko/leave-portal/actions/runs/46016828565
- `accounts.zip` на SHA: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/accounts.zip
- Ролі тестових акаунтів: працівник — teacher1@example.test, teacher2@example.test, pm-employee@example.test; менеджер — manager1@example.test, manager2@example.test, pm-manager@example.test; адміністратор — admin@example.test, pm-admin@example.test. Паролі — лише в accounts.zip
- Версія `next`: next@16.4.6

### Середовища й реліз (трек A)

- Гілка `staging`: https://github.com/lukian-tomochko/leave-portal/tree/staging
- Зміни з `main` у `staging`: fast-forward без окремої розробки, `staging` і `main` на одному коміті: https://github.com/lukian-tomochko/leave-portal/commit/b0e493bc9f4ff1816d23d4cbf000749925da83dc
- Змінна лише для гілки `staging`: `APP_ENV` — поле `appEnv` у `/api/health` (на staging `staging`, на проді й preview з PR інше)
- Проєкт Supabase для staging: leave-portal-staging; supabaseRef staging: nvahbpcqsouhzqmwqctm; supabaseRef проду: quxrineulrgegydlmbow — різні
- Міграції в базу staging через CI: прогін CI на `main` https://github.com/lukian-tomochko/leave-portal/actions/runs/46016755890 — job `db-deploy-staging` друкує «Ціль міграцій: проєкт nvahbpcqsouhzqmwqctm (staging)», крок «Apply migrations» — success; файл https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.github/workflows/ci.yml#L128
- Workflow smoke деплою: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.github/workflows/smoke.yml#L133 — адреса з `client_payload.url`, заголовок `x-vercel-protection-bypass` із секрету `VERCEL_AUTOMATION_BYPASS_SECRET`
- Зелений smoke на деплой staging: https://github.com/lukian-tomochko/leave-portal/actions/runs/46017295066 — у журналі health з `"appEnv":"staging"` і `supabaseRef` staging
- Smoke на деплой одного PR-preview: https://github.com/lukian-tomochko/leave-portal/actions/runs/45888028444 — preview з PR, у журналі `supabaseRef` staging
- Доступ PM до staging: спільне посилання Vercel (Standard Protection, посилання й акаунти надіслано в приват); runbook: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/docs/runbook.md#де-що
- Реліз `v1.0.0`: https://github.com/lukian-tomochko/leave-portal/releases/tag/v1.0.0
- Нотатки за категоріями: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.github/release.yml
- Прод на тегу: SHA тегу `v1.0.0` і `sha` у `/api/health` проду збігаються: b0e493bc9f4ff1816d23d4cbf000749925da83dc
- Приймання PM перед тегом: https://github.com/lukian-tomochko/leave-portal/issues/100#issuecomment-7221636323 — «Приймання релізу (@mkozlovskyi-ops, 2026-11-27, повторно): … коміт `b0e493b`, сценарій ✅ — прийнято». Перше приймання того ж дня https://github.com/lukian-tomochko/leave-portal/issues/100#issuecomment-7220650969 було ❌ (крок 3 → відхилення https://github.com/lukian-tomochko/leave-portal/issues/104); виправлено в https://github.com/lukian-tomochko/leave-portal/pull/105, тег поставлено лише після повторного приймання

### Сигнали (трек B)

- Крок алерту: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.github/workflows/smoke.yml#L175 — `if: failure()`, токен із Secrets, без токена попередження, а не падіння
- Навмисно червоний прогін: https://github.com/lukian-tomochko/leave-portal/actions/runs/45796825238
- Знімок алерту: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/docs/ops/alert-red-smoke.png — видно, що впало (job deploy), середовище, адресу й посилання на прогін; токена й chat id немає
- Хто отримує алерт: Telegram-група команди: розробник, PM
- Трекер помилок: власний onRequestError: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/src/instrumentation.ts#L8
- Дані моделі в трекері: власний onRequestError без заголовків, cookie, тіла й рядка запиту, без `error.message` (береться лише назва помилки, digest і до п'яти рядків стеку): https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/src/lib/error-alert.ts#L6 і #L31
- Подія навмисної помилки: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/docs/ops/error-event.png — маршрут `/ops/error-probe` і стек, staging
- Тестовий маршрут прибрано чи закрито: https://github.com/lukian-tomochko/leave-portal/pull/89 — маршрут лише для ролі адміністратора (решта отримує 401/403)
- Журнал `llm.call`: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/src/lib/ai/llm-log.ts#L41 — поля: модель, затримка, токени, стан, fallback, версія промпту, id запиту
- Тест «без тексту користувача»: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/tests/llm-log.test.ts і зелений прогін CI https://github.com/lukian-tomochko/leave-portal/actions/runs/46016755890

### Інцидент-дріл (трек C)

- Сценарій: хибне значення `NEXT_PUBLIC_SUPABASE_URL` (адреса неіснуючого проєкту) лише для гілки `staging` у Vercel; без міграцій і без видалення даних, прод і база проду не зачеплені
- Сигнал і час: червоний smoke https://github.com/lukian-tomochko/leave-portal/actions/runs/45892661504; деплой із поломкою готовий о 22:39:46, алерт відправлено о 22:40:04 (Київ, за серверним часом) — від поломки до алерту 18 с
- Профіль «лише пропонує»: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.claude/settings.incident.json (окремий файл профілю в `.claude/`; інструменти запису MCP Vercel і Supabase заборонені поіменно, постійні правила — в `.claude/settings.json`)
- MCP-конфіг: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.mcp.incident.json — Vercel і Supabase з `read_only=true` і `project_ref` staging, без токенів (у цій сесії розбору Supabase MCP не підключали)
- Стенограма розбору: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/docs/incidents/2026-11-26-staging-supabase-url-rca.md
- Виправлення людиною, не агентом: видалення гілкового значення змінної у Vercel і перебудова `staging` (зміна налаштування, а не коду; агент мав лише читання, спроба запису дістала відмову): розділ «Усунення» у https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/docs/incidents/2026-11-26-staging-supabase-url.md і зелений прогін https://github.com/lukian-tomochko/leave-portal/actions/runs/45899217085
- Postmortem: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/docs/incidents/2026-11-26-staging-supabase-url.md
- Runbook: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/docs/runbook.md
- Дія з postmortem → перевірка: https://github.com/lukian-tomochko/leave-portal/pull/94 (smoke звіряє `supabaseRef` із очікуваним ref середовища, тести `tests/scripts/expected-ref.test.ts`) і https://github.com/lukian-tomochko/leave-portal/pull/95 (прод-smoke пропускає статус від коміту, що вже не HEAD `main`); нова перевірка працює в зеленому прогоні https://github.com/lukian-tomochko/leave-portal/actions/runs/45907302069
- Розбір PM: https://github.com/lukian-tomochko/leave-portal/pull/99#issuecomment-7220224519 — підтвердив докази й відсутність пошуку винних, дав зауваження; відповідь — зміни postmortem і runbook у https://github.com/lukian-tomochko/leave-portal/pull/103 (крок 0 «спершу зупинити шкоду», неробочий час, скільки чекати після Resume, вплив мовою користувача)

### Ланцюг постачання (трек D)

- `dependabot.yml`: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.github/dependabot.yml — npm і github-actions, щотижня, cooldown, групи
- Dependabot запускався: https://github.com/lukian-tomochko/leave-portal/pull/79
- `.npmrc`: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.npmrc — з `min-release-age`
- Дії за SHA: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.github/workflows/smoke.yml — `uses: …@<повний sha> # v…`
- Push protection і CodeQL: перший прогін CodeQL https://github.com/lukian-tomochko/leave-portal/actions/runs/45887747188; push protection увімкнено в налаштуваннях репозиторію
- `next` ≥ 16.4.5: https://github.com/lukian-tomochko/leave-portal/pull/78 — злитий, CI зелений, версія 16.4.6

## Бонус

### Розробник

- Варіант: резервна копія і відновлення
- Workflow: https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/.github/workflows/backup.yml — вручну (`workflow_dispatch`) і за розкладом (щодня), усі дії за SHA
- Прогін: https://github.com/lukian-tomochko/leave-portal/actions/runs/45992700674 — job `dump` і `restore-drill` зелені (`scope=public`)
- Резервна копія: шифрування й строк зберігання: gpg симетричне, AES256, фраза з секрету `BACKUP_PASSPHRASE` середовища `production`; артефакт `supabase-backup` містить лише `*.gpg` і зберігається 7 днів (`retention-days: 7`); дамп у відкритому вигляді — ніде (ні в git, ні в артефакті, ні в журналі: журнал показує лише розміри файлів і кількості рядків)
- Резервна копія: тренування відновлення: прогін https://github.com/lukian-tomochko/leave-portal/actions/runs/45992700674 (одноразова локальна база на раннері, кількість рядків копії й відновленої бази збіглася в усіх таблицях `public`) і розділ «Відновлення» https://github.com/lukian-tomochko/leave-portal/blob/b0e493bc9f4ff1816d23d4cbf000749925da83dc/docs/runbook.md#10-резервна-копія-і-відновлення. Чесно: перше тренування з `scope=all` впало на завантаженні даних схеми `auth` (https://github.com/lukian-tomochko/leave-portal/actions/runs/45987265413), тому тренуємо й покладаємось на `scope=public`; користувачів після відновлення створюємо заново
- Асистент чергового: чернетка розбору: —
- Асистент чергового: межі, ліміти й вартість: —

### PM

—
