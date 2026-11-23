# Здача WS11

Скорочення: WS (workshop — заняття курсу), PM (project manager — менеджер проєкту), SHA (Secure Hash Algorithm — тут: 40-символьний ідентифікатор коміту), URL (Uniform Resource Locator — адреса), SLO (Service Level Objective — ціль рівня сервісу), RPO/RTO (Recovery Point / Recovery Time Objective — скільки даних можна втратити і скільки продукт може не працювати), UAT (User Acceptance Testing — приймальне тестування), CI (continuous integration — автоматичні перевірки кожної зміни), PR (pull request — запит на злиття змін), MCP (Model Context Protocol — спосіб дати агенту інструменти зовнішнього сервісу).

## Дані

- Роль: dev
- Команда: property-workflows
- GitHub-логін: OleksandrMartynov
- Ваше репо: https://github.com/OleksandrMartynov/property-workflows
- SHA: 64077c669c3b1f8b043c6fa4cf9664bc2013a28e
- Репо вимог PM: https://github.com/solomchaak/property-workflows-spec
- Прод-URL: https://property-workflows-rho.vercel.app
- Staging-URL: https://property-workflows-git-staging-flambs-projects.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://property-workflows-rho.vercel.app/api/health — `"db":"ok"`, `"sha":"64077c669c3b1f8b043c6fa4cf9664bc2013a28e"` (= `main`)
- Зелений прогін `smoke` на `main`: https://github.com/OleksandrMartynov/property-workflows/actions/runs/45183685991
- `accounts.zip` на SHA: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/accounts.zip
- Ролі тестових акаунтів: адміністратор — admin@example.test, admin.c@example.test, pm-admin@example.test; менеджер — manager.a@example.test, manager.b@example.test, manager.c@example.test, manager.d@example.test, pm-manager@example.test; підрядник (vendor) — vendor.v@example.test, pm-vendor@example.test. Ті самі акаунти й паролі — і на staging
- Версія `next`: next@16.4.5

### Середовища й реліз (трек A)

- Гілка `staging`: https://github.com/OleksandrMartynov/property-workflows/tree/staging
- Зміни з `main` у `staging`: PR https://github.com/OleksandrMartynov/property-workflows/pull/123 (`main` → `staging`)
- Змінна лише для гілки `staging`: `APP_ENV` (Vercel → Preview → гілка `staging`) — поле `appEnv` у `/api/health` на staging (`"appEnv":"staging"`); на проді й PR-preview — `null`
- Проєкт Supabase для staging: «property-workflows-staging»; supabaseRef staging: otguzuprwoeerntjdgzu; supabaseRef проду: tdscfcilebnmjeapiems
- Міграції в базу staging через CI: прогін CI на main https://github.com/OleksandrMartynov/property-workflows/actions/runs/44552694434 — job `db-deploy-staging` друкує «Ціль міграцій: проєкт otguzuprwoeerntjdgzu (staging)», «Apply migrations» — success (усі 18 міграцій з нуля), далі `db-deploy` — «Ціль міграцій: проєкт tdscfcilebnmjeapiems (production)»; файл https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.github/workflows/ci.yml#L128
- Workflow smoke деплою: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.github/workflows/smoke.yml#L125 — адреса з `client_payload.url`, заголовок `x-vercel-protection-bypass` із секрету `VERCEL_AUTOMATION_BYPASS_SECRET`
- Зелений smoke на деплой staging: https://github.com/OleksandrMartynov/property-workflows/actions/runs/44916198210 — у журналі `"appEnv":"staging"`, `"supabaseRef":"otguzuprwoeerntjdgzu"`
- Smoke на деплой одного PR-preview: https://github.com/OleksandrMartynov/property-workflows/actions/runs/44930339621 (PR #124) — `"supabaseRef":"otguzuprwoeerntjdgzu"`, `"appEnv":null`
- Доступ PM до staging: спільне посилання Vercel (Share), захист Standard Protection лишається; runbook — https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/runbook.md#де-що
- Реліз `v1.0.0`: очікує приймання PM — допишу в цей PR після тегу
- Нотатки за категоріями: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.github/release.yml
- Прод на тегу: очікує приймання PM
- Приймання PM перед тегом: приймання PM не отримано на момент відкриття здачі: запит реліз-кандидата на staging надіслано 2026-11-23 (повідомлення PM); оновлю цей рядок після відповіді

### Сигнали (трек B)

- Крок алерту: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.github/workflows/smoke.yml#L89 (job `prod`) і #L153 (job `deploy`) — `if: failure()`, токен і chat id із Secrets, без них — попередження, а не падіння
- Навмисно червоний прогін: https://github.com/OleksandrMartynov/property-workflows/actions/runs/44122735090
- Знімок алерту: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/ops/alert-red-smoke.png — що впало, середовище й посилання на прогін; токена й chat id немає
- Хто отримує алерт: Telegram-група команди: розробник (черговий) — runbook https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/runbook.md#хто-отримує-алерт-і-кого-кликати
- Трекер помилок: власний `onRequestError` — https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/src/instrumentation.ts
- Дані моделі в трекері: власний onRequestError — без заголовків, cookie, тексту запиту й даних моделі: лише маршрут, метод, тип помилки й стек (https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/src/instrumentation.ts)
- Подія навмисної помилки: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/ops/error-tracker-staging.png — маршрут і стек; staging
- Тестовий маршрут прибрано чи закрито: https://github.com/OleksandrMartynov/property-workflows/pull/102 — `/api/ops/error-drill` лише для адміністратора, на проді — 404 (тест `tests/error-drill.test.ts`)
- Журнал `llm.call`: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/src/lib/ai/llm-log.ts — модель, затримка, токени, стан, fallback, версія промпту, id запиту
- Тест «без тексту користувача»: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/tests/llm-log.test.ts і зелений прогін CI https://github.com/OleksandrMartynov/property-workflows/actions/runs/45183610320

### Інцидент-дріл (трек C)

- Сценарій: перевизначення `NEXT_PUBLIC_SUPABASE_URL` лише для гілки `staging` з одним зміненим символом → Redeploy. Без міграцій і без видалення даних; прод не зачеплено
- Сигнал і час: червоний smoke деплою staging https://github.com/OleksandrMartynov/property-workflows/actions/runs/44497872998; поломка о 09:35:44 UTC, алерт у Telegram о 09:36:07 UTC (2026-11-19) — від поломки до алерту 23 с
- Профіль «лише пропонує»: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.claude/settings.incident.json (запуск `claude --settings .claude/settings.incident.json --permission-mode plan`) і щоденний https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.claude/settings.json — інструменти запису MCP Vercel і Supabase заборонені поіменно
- MCP-конфіг: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.mcp.json — Vercel (OAuth, без токенів) і Supabase з read_only=true; у розборі викликався лише Vercel
- Стенограма розбору: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/incidents/2026-11-19-staging-bad-supabase-url-rca.md — дослівно, із заголовком: дата, модель, профіль, що агент міг і чого не міг
- Виправлення людиною, не агентом: людина прибрала перевизначення у Vercel і зробила Redeploy (09:58:44–09:59:46 UTC); зелений smoke https://github.com/OleksandrMartynov/property-workflows/actions/runs/44501481804
- Postmortem: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/incidents/2026-11-19-staging-bad-supabase-url.md
- Runbook: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/runbook.md
- Дія з postmortem → перевірка: https://github.com/OleksandrMartynov/property-workflows/pull/114 (посилання на postmortem в описі) — тести `tests/health-route.test.ts` і `tests/scripts/smoke.test.ts` червоніють на зламаному стані дрілу; прогін CI https://github.com/OleksandrMartynov/property-workflows/actions/runs/45183610320
- Розбір PM: не надходило — запит розбору postmortem у https://github.com/OleksandrMartynov/property-workflows/pull/115 надіслано 2026-11-23; оновлю після відповіді

### Ланцюг постачання (трек D)

- `dependabot.yml`: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.github/dependabot.yml — npm і github-actions, щотижня, cooldown, групи
- Dependabot запускався: https://github.com/OleksandrMartynov/property-workflows/pull/103, https://github.com/OleksandrMartynov/property-workflows/pull/112
- `.npmrc`: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.npmrc — `min-release-age=7`
- Дії за SHA: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.github/workflows/ci.yml — напр. `uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1` (#100)
- Push protection і CodeQL: знімки https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/ops/code-security-push-protection.png (Push protection — On) і https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/ops/code-security-codeql.png (CodeQL — Default setup); перший прогін CodeQL https://github.com/OleksandrMartynov/property-workflows/actions/runs/44115601915
- `next` ≥ 16.4.5: PR Dependabot https://github.com/OleksandrMartynov/property-workflows/pull/103 — злитий, CI зелений; next@16.4.5

## Бонус

### Розробник

- Варіант: резервна копія і відновлення
- Workflow: https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/.github/workflows/backup.yml — щодня (`41 1 * * *`) і вручну; Supabase CLI 2.120.0 з package-lock.json; дії за SHA
- Прогін: https://github.com/OleksandrMartynov/property-workflows/actions/runs/44506250509
- Резервна копія: шифрування й строк зберігання: `gpg --symmetric` AES256 фразою `BACKUP_PASSPHRASE` (секрет середовища `production`), відкриті файли видаляються до завантаження; артефакт `supabase-backup` — лише `*.gpg`, 7 днів; журнал — лише розміри й кількості рядків. Дамп у відкритому вигляді — ніде
- Резервна копія: тренування відновлення: https://github.com/OleksandrMartynov/property-workflows/actions/runs/44506250509 — одноразова база з міграцій, 9 таблиць `public`, 348 рядків, «у копії = відновлено» в кожній (знімок https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/ops/backup-restore-drill.png); `scope=all` (з `auth.*`) не відновився — https://github.com/OleksandrMartynov/property-workflows/actions/runs/44504097040, тому типово `public` (#118). Розділ — https://github.com/OleksandrMartynov/property-workflows/blob/64077c669c3b1f8b043c6fa4cf9664bc2013a28e/docs/runbook.md#8-резервна-копія-і-відновлення
- Асистент чергового: чернетка розбору: —
- Асистент чергового: межі, ліміти й вартість: —

### PM

—
