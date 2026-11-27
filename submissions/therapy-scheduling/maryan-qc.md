# Здача WS11

Скорочення: WS (workshop — заняття курсу), PM (project manager — менеджер проєкту), SHA (Secure Hash Algorithm — тут: 40-символьний ідентифікатор коміту), URL (Uniform Resource Locator — адреса), SLO (Service Level Objective — ціль рівня сервісу), RPO/RTO (Recovery Point / Recovery Time Objective — скільки даних можна втратити і скільки продукт може не працювати), UAT (User Acceptance Testing — приймальне тестування), CI (continuous integration — автоматичні перевірки кожної зміни), PR (pull request — запит на злиття змін), MCP (Model Context Protocol — спосіб дати агенту інструменти зовнішнього сервісу).

## Дані

- Роль: dev
- Команда: therapy-scheduling
- GitHub-логін: maryan-qc
- Ваше репо: https://github.com/maryan-qc/therapy-scheduling
- SHA: 1626419b59a1800b19c09b6558fa34981e132c4f
- Репо вимог PM: https://github.com/HryhoriiHaponiukQuitCode/therapy-scheduling-spec
- Прод-URL: https://therapy-scheduling-sigma.vercel.app
- Staging-URL: https://therapy-scheduling-git-staging-mandrushchak-4349s-projects.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://therapy-scheduling-sigma.vercel.app/api/health — `"db":"ok"`, `"sha"` = SHA вище
- Зелений прогін `smoke` на `main`: https://github.com/maryan-qc/therapy-scheduling/actions/runs/45975914627
- `accounts.zip` на SHA: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/accounts.zip
- Ролі тестових акаунтів:
  - адміністратор — admin@example.test, pm-admin@example.test;
  - планувальник — scheduler1@example.test, pm-scheduler@example.test;
  - терапевт — therapist1@example.test, pm-therapist@example.test.

  Решта акаунтів і паролі — лише в `accounts.zip`.
- Версія `next`: next@16.4.5

### Середовища й реліз (трек A)

- Гілка `staging`: https://github.com/maryan-qc/therapy-scheduling/tree/staging
- Зміни з `main` у `staging`: https://github.com/maryan-qc/therapy-scheduling/pull/74, https://github.com/maryan-qc/therapy-scheduling/pull/79
- Змінна лише для гілки `staging`: `APP_ENV` (Vercel: Preview, гілка `staging`) — поле `appEnv` у `/api/health`. На staging там `"staging"`, на проді й інших preview — `null`.
- Проєкт Supabase для staging: `therapy-scheduling-staging` — окремий проєкт в іншому акаунті Supabase, регіон eu-west-1.
  - supabaseRef staging: hudgoucpezodnvirccok;
  - supabaseRef проду: azsawyysyqzgjxtlufjk.
- Міграції в базу staging через CI: прогін https://github.com/maryan-qc/therapy-scheduling/actions/runs/45483522327.
  - У job `db-deploy-staging` запобіжник друкує «Ціль міграцій: проєкт hudgoucpezodnvirccok (staging), хост aws-1-eu-west-1.pooler.supabase.com».
  - Крок «Apply migrations» — success.
  - Файл: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/workflows/ci.yml#L131
- Workflow smoke деплою: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/workflows/smoke.yml#L123-L191
  - Адреса береться з `client_payload.url`: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/workflows/smoke.yml#L132.
  - Заголовок `x-vercel-protection-bypass` береться лише із секрету `VERCEL_AUTOMATION_BYPASS_SECRET`: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/workflows/smoke.yml#L155-L162.
  - Захист Vercel вимкнено заради доступу PM, тож секрету немає, і smoke перевіряє без обходу.
- Зелений smoke на деплой staging: https://github.com/maryan-qc/therapy-scheduling/actions/runs/45841343763 — гілка `staging`, коміт `cd4cc2f`; у журналі `"appEnv":"staging","supabaseRef":"hudgoucpezodnvirccok","db":"ok"`
- Smoke на деплой одного PR-preview: https://github.com/maryan-qc/therapy-scheduling/actions/runs/45848579955 — preview PR #80 (гілка `task/alerts-group`); у журналі `"appEnv":null,"supabaseRef":"hudgoucpezodnvirccok"`
- Доступ PM до staging: захист Vercel вимкнено. Він відкриває всі preview, а в застосунок PM входить тестовим акаунтом. Запис у runbook: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/docs/runbook.md#де-що (рядок «Доступ PM до staging»).
- Реліз `v1.0.0`: https://github.com/maryan-qc/therapy-scheduling/releases/tag/v1.0.0
- Нотатки за категоріями: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/release.yml
- Прод на тегу:
  - Тег `v1.0.0` → `32c03ceb3780f68734b3469cd429385561bb7982`, той самий коміт, який приймав PM. Тег поставлено 2026-11-26 о 09:46 UTC.
  - У той момент `/api/health` проду показував саме цей SHA. У прогоні smoke проду https://github.com/maryan-qc/therapy-scheduling/actions/runs/45648552772 видно `"sha":"32c03ceb3780f68734b3469cd429385561bb7982"`.
  - Після тегу в `main` злито лише документацію й бонус резервної копії: #78 (зауваження PM до postmortem і runbook), #80 (канал алерту), #81 (знімки), #82 і #83 (workflow `backup`, розділ 9 runbook). Тому зараз `/api/health` показує SHA вище.
  - Код застосунку той самий, що в `v1.0.0`: https://github.com/maryan-qc/therapy-scheduling/compare/v1.0.0...1626419b59a1800b19c09b6558fa34981e132c4f — змінено лише `docs/`, `.github/workflows/backup.yml`, `scripts/privileges.sql` і `.gitignore`; `src/`, `supabase/` і залежності — без змін.
  - Повертати прод на SHA тегу не стали. Тоді `/api/health` не показував би SHA `main` з D0, а щоденний smoke `prod`, який звіряє SHA з `main`, червонів би.
- Приймання PM перед тегом: https://github.com/maryan-qc/therapy-scheduling/issues/75#issuecomment-7175074613 — «Приймання релізу (…, 2026-11-25): staging …, сценарій ✅»

### Сигнали (трек B)

- Крок алерту: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/workflows/smoke.yml#L164-L191
  - `if: failure()`; токен і chat id — із GitHub Secrets.
  - Без секретів — `::warning`, а не падіння: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/workflows/smoke.yml#L178-L179.
  - Такий самий крок є в job `prod`: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/workflows/smoke.yml#L96-L121.
- Навмисно червоний прогін: https://github.com/maryan-qc/therapy-scheduling/actions/runs/45603275036
- Знімок алерту: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/docs/ops/alert-red-smoke-telegram.png — видно workflow і job, середовище, адресу й посилання на прогін; токена й chat id на знімку немає
- Хто отримує алерт: Telegram-група «therapy-scheduling: алерти» (з 2026-11-26; до того — особистий чат розробника з ботом).
  - Розробник — у групі; PM запрошено.
  - Доки PM не приєднався, про SEV1–SEV2 розробник пише йому особисто.
  - Запис у runbook: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/docs/runbook.md#хто-отримує-алерт-і-кого-кликати
- Трекер помилок: власний `onRequestError` → Telegram: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/src/instrumentation.ts
- Дані моделі в трекері: власний `onRequestError` не надсилає заголовків, cookie, тіла, рядка запиту і `error.message`. Лише маршрут, метод, назву помилки, digest, середовище, деплой і верхні рядки стеку:
  - https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/src/lib/error-alert.ts#L3-L4
  - https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/src/instrumentation.ts#L20
- Подія навмисної помилки: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/docs/ops/error-tracker-staging-telegram.png — маршрут `/api/ops/error-check` і стек; staging
- Тестовий маршрут прибрано чи закрито: https://github.com/maryan-qc/therapy-scheduling/pull/66. Маршрут лише для ролі адміністратора й лише на preview чи staging; на проді відповідає 404: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/src/app/api/ops/error-check/route.ts#L15-L20
- Журнал `llm.call`: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/src/lib/ai/llm-log.ts#L24-L40
  - Поля: модель, затримка, токени, вартість, стан, fallback, версія промпту, id запиту, маршрут.
  - Знімок рядка у Vercel Logs: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/docs/ops/llm-call-vercel-log.png
- Тест «без тексту користувача»: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/tests/llm-log.test.ts і зелений прогін CI https://github.com/maryan-qc/therapy-scheduling/actions/runs/45975844386

### Інцидент-дріл (трек C)

- Сценарій: коміт, що ламає сторінку входу лише на staging (PR #69 у `staging`).
  - Рядок `ERRORS[(error as string).toLowerCase()]` у `src/app/login/page.tsx` дає `TypeError` і 500 на `/login` без `?error=`.
  - Без міграцій і без видалення даних; прод і база проду не зачеплені.
  - Прогноз записано до поломки: https://github.com/maryan-qc/therapy-scheduling/issues/68
- Сигнал і час: червоний smoke деплою staging https://github.com/maryan-qc/therapy-scheduling/actions/runs/45606096374.
  - Поломка (деплой Ready) — 11:37:00 UTC, алерт — 11:37:13 UTC: від поломки до алерту 13 с.
  - На preview PR #69 сигнал спрацював ще раніше, о 11:34:11: https://github.com/maryan-qc/therapy-scheduling/actions/runs/45605627003
- Профіль «лише пропонує»: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.claude/settings.incident.json — окремий профіль за зразком ops-kit.
  - Інструменти запису MCP Vercel і Supabase заборонені поіменно.
  - Також заборонені редагування файлів, git-команди запису і читання `.env*`.
  - Постійні правила: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.claude/settings.json
- MCP-конфіг: Supabase MCP у розборі дрілу не підключали. Сервер був у конфігу, але не авторизований (`needs-auth`) і для розбору не знадобився — див. шапку стенограми. Агент працював через MCP Vercel. Файли, без токенів:
  - https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.mcp.json — постійний конфіг: Supabase з `read_only=true`, ref — зі змінної середовища;
  - https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.mcp.incident.json — конфіг профілю розбору: Vercel лише для команди й проєкту `therapy-scheduling`; Supabase — лише staging (`project_ref=hudgoucpezodnvirccok`) з `read_only=true`.
- Стенограма розбору: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/docs/incidents/2026-11-25-staging-login-500-rca.md — дослівно, 4 частини. Шапка: дата, Claude Code 2.1.332, модель `claude-opus-5-5`, режим `plan`, що агент міг і чого не міг.
- Виправлення людиною, не агентом: https://github.com/maryan-qc/therapy-scheduling/pull/70
- Postmortem: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/docs/incidents/2026-11-25-staging-login-500.md
- Runbook: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/docs/runbook.md
- Дія з postmortem → перевірка: https://github.com/maryan-qc/therapy-scheduling/pull/73
  - Тест `tests/login-page.test.tsx` рендерить `/login` без параметрів і з будь-яким `?error=`. На коді дрілу 2 з 3 сценаріїв червоні.
  - Зелений CI на `main`: https://github.com/maryan-qc/therapy-scheduling/actions/runs/45641574928
- Розбір PM: https://github.com/maryan-qc/therapy-scheduling/pull/73#issuecomment-7174684592
  - Зміни: https://github.com/maryan-qc/therapy-scheduling/pull/78 і https://github.com/maryan-qc/therapy-scheduling/pull/80.
  - Відповідь по кожному пункту: https://github.com/maryan-qc/therapy-scheduling/pull/73#issuecomment-7191326936

### Ланцюг постачання (трек D)

- `dependabot.yml`: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/dependabot.yml — npm і github-actions щотижня, cooldown 7 днів (мажорні — 30), групи
- Dependabot запускався:
  - https://github.com/maryan-qc/therapy-scheduling/pull/58 — безпекове оновлення `next` до 16.4.5, злито;
  - https://github.com/maryan-qc/therapy-scheduling/pull/60 — групове оновлення, свідомо відкладено до після `v1.0.0`.
- `.npmrc`: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.npmrc — `min-release-age=7`
- Дії за SHA: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/workflows/ci.yml#L51-L52 — `uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1`. Так закріплено всі `uses:` в усіх workflow.
- Push protection і CodeQL: перший прогін CodeQL (default setup): https://github.com/maryan-qc/therapy-scheduling/actions/runs/45407144046. Secret scanning і push protection увімкнено в налаштуваннях репо (Advanced Security).
- `next` ≥ 16.4.5: https://github.com/maryan-qc/therapy-scheduling/pull/58 — PR Dependabot, злито 2026-11-24, CI зелений; next@16.4.5

## Бонус

### Розробник

- Варіант: резервна копія і відновлення
- Workflow: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/.github/workflows/backup.yml — вручну і за розкладом (щодня ≈ 03:41 за Києвом); дії за SHA; Supabase CLI 2.120.0 з `package-lock.json`; секрети `SUPABASE_DB_URL` і `BACKUP_PASSPHRASE` — лише середовища `production` і лише в кроках, яким вони потрібні; запобіжник друкує, що копіюється саме прод-проєкт.
- Прогін: https://github.com/maryan-qc/therapy-scheduling/actions/runs/45971144280 — копія і тренування відновлення, обидва job зелені.
- Резервна копія: шифрування й строк зберігання:
  - дамп прод-бази (ролі, схема, дані, історія міграцій) одразу шифрується `gpg --symmetric --cipher-algo AES256` фразою `BACKUP_PASSPHRASE`, відкриті файли видаляються;
  - артефакт `supabase-backup` — один файл `backup-2026-11-27T0909.tgz.gpg`, `retention-days: 7`;
  - дамп у відкритому вигляді — ніде: ні в git (правила для локальних дампів у `.gitignore`), ні в артефакті, ні в журналі (у журналі — розміри файлів, назви таблиць і числа).
- Резервна копія: тренування відновлення: той самий прогін, job `restore-drill` — одноразова база з міграцій репо, завантаження даних (`scope=public`) і порівняння кількості рядків: 5 таблиць, усі збігаються (`clients` 46, `holds` 16, `profiles` 10, `slots` 1248, `therapists` 3), 2 хв 0 с. Розділ «Відновлення» в runbook: https://github.com/maryan-qc/therapy-scheduling/blob/1626419b59a1800b19c09b6558fa34981e132c4f/docs/runbook.md#9-резервна-копія-і-відновлення-бази
  - Чесно про межі: з `scope=all` завантаження впало на схемі `auth` (у локальному Supabase CLI 2.120.0 немає стовпця з копії хмарного проєкту) — https://github.com/maryan-qc/therapy-scheduling/actions/runs/45970519677. Тому за замовчуванням `public`; дані `auth.*` у копії є, але їх відновлення в CI не відрепетирувано.
- Асистент чергового: чернетка розбору: —
- Асистент чергового: межі, ліміти й вартість: —

### PM

—
