# Здача WS11

## Дані

- Роль: dev
- Команда: leave-portal
- GitHub-логін: SvitlanaHaiduchyk
- Ваше репо: https://github.com/SvitlanaHaiduchyk/course-staff-management
- SHA: 173434fae9c7ce434a5f0d88d58b0518bc3b79a1
- Репо вимог PM: https://github.com/mkozlovskyi-ops/course-staff-management-spec
- Прод-URL: https://course-staff-management.vercel.app
- Staging-URL: https://course-staff-management-git-staging-test11-af77.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://course-staff-management.vercel.app/api/health — `{"ok":true,"sha":"173434fae9c7ce434a5f0d88d58b0518bc3b79a1","env":"production","appEnv":"production","supabaseRef":"dolwlriqywfqhnstobei","db":"ok","reason":""}`; SHA збігається з `main`
- Зелений прогін `smoke` на `main`: https://github.com/SvitlanaHaiduchyk/course-staff-management/actions/runs/45366254393
- `accounts.zip` на SHA: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/accounts.zip
- Ролі тестових акаунтів: адміністратор — admin@example.test; відповідальний менеджер — manager1@example.test, manager2@example.test; працівник — teacher1@example.test, teacher2@example.test
- Версія `next`: next@16.4.6

### Середовища й реліз (трек A)

- Гілка `staging`: https://github.com/SvitlanaHaiduchyk/course-staff-management/tree/staging
- Зміни з `main` у `staging`: fast-forward злиття `main` у `staging` (`git merge --ff-only main`), окремої розробки в гілці немає. Станом на здачу `staging` показує `94857e183820c98157b8884019818ea060b3117d`, тобто відстає від `main` на коміти, злиті пізніше, — це видно в полі `sha` її `/api/health`
- Змінна лише для гілки `staging`: `APP_ENV` — поле `appEnv` у `/api/health`: на staging `"appEnv":"staging"` при системному `"env":"preview"`. Окрема змінна тут не примха: `VERCEL_ENV` на staging теж дорівнює `preview`, і без `APP_ENV` staging не відрізнити від прев'ю звичайного PR
- Проєкт Supabase для staging: окремий безкоштовний проєкт; supabaseRef staging — `euwjxwolcxhooqxzgsgp`, supabaseRef проду — `dolwlriqywfqhnstobei`; різні
- Міграції в базу staging через CI: https://github.com/SvitlanaHaiduchyk/course-staff-management/actions/runs/45359064981 — job `db-deploy-staging` друкує `Ціль міграцій: проєкт euwjxwolcxhooqxzgsgp (staging)`, висновок `success`, і лише після нього виконується `db-deploy` (прод); обидва jobs звіряють ref із рядка підключення зі змінними репо й падають, якщо рядок веде не туди. Файл: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/.github/workflows/ci.yml. База staging була порожня, тож усі міграції лягли в неї з нуля — це заодно перевірило, що історія міграцій відтворювана, а не лише «застосовується поверх наявної»
- Workflow smoke деплою: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/.github/workflows/smoke.yml — job `deploy` бере адресу з `client_payload.url`, заголовок `x-vercel-protection-bypass` — зі секрету `VERCEL_AUTOMATION_BYPASS_SECRET`
- Зелений smoke на деплой staging: https://github.com/SvitlanaHaiduchyk/course-staff-management/actions/runs/45371345865 — job `deploy`, у журналі `{"base":"https://course-staff-management-git-staging-test11-af77.vercel.app","target":"deploy","bypass":false,"root":307,"login":200,"health":{"status":200,"ok":true,"sha":"94857e18…","env":"preview","appEnv":"staging","supabaseRef":"euwjxwolcxhooqxzgsgp","db":"ok"}}`.
  **Чесно про цей доказ:** прогін **ручний** (`workflow_dispatch` з полем `url`), а не з події деплою. Vercel не надсилає `repository_dispatch` у це репо: серед 60 прогонів `smoke` жодного з такою подією немає — усі з `status` і `schedule`. Тобто job `deploy` робочий і перевіряє саме адресу конкретного деплою, але зв'язок «кожен деплой → smoke» не налаштований. Цього не зроблено
- Smoke на деплой одного PR-preview: **прогону smoke немає** — з тієї самої причини (немає `repository_dispatch`). Але вимога, заради якої цей прогін потрібен — «preview з PR ходить у базу staging, а не в прод» — виконана, і перевірена напряму.
  Знайдено це саме під час збирання здачі, і спершу було зламано: preview гілки `task/sentry` відповідав `{"appEnv":"preview","supabaseRef":"dolwlriqywfqhnstobei"}` — тобто **прод-база**. Причина: значення Supabase для staging задані у Vercel лише для гілки `staging`, а жовтневий запис із прод-значеннями (тип Secret, `Production and Preview`) лишився діяти на весь Preview.
  Виправлено 24.11: прод-записи перестворено як `Config` з областю **тільки Production** (тип Secret для `NEXT_PUBLIC_*` Vercel не дозволяє, а перемкнути наявний запис на Config не можна — лише видалити й створити), а зі staging-записів знято прив'язку до гілки, тож вони діють на **весь** Preview. `APP_ENV` навмисно лишився прив'язаним до гілки — саме він відрізняє staging від звичайного preview.
  Стан після виправлення й перезбірки без кешу (три середовища поруч):
  - прод — `{"appEnv":"production","supabaseRef":"dolwlriqywfqhnstobei","db":"ok"}`
  - preview гілки `task/sentry` — `{"appEnv":"preview","supabaseRef":"euwjxwolcxhooqxzgsgp","db":"ok"}`
  - staging — `{"appEnv":"staging","supabaseRef":"euwjxwolcxhooqxzgsgp","db":"ok"}`

  Це була не формальність рубрики: до виправлення будь-який експеримент у PR — зокрема сьогоднішні перевірки Sentry — писав у базу зі справжніми записами порталу
- Доступ PM до staging: захист деплоїв у Vercel вимкнено, тож staging (як і будь-який preview) відкривається за посиланням без входу. Це видно з журналу того самого прогону smoke: `"bypass":false` і `"login":200` — сторінка входу відповіла без секрету обходу. Записано в runbook, розділ «Staging»: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/docs/runbook.md
- Реліз `v1.0.0`: https://github.com/SvitlanaHaiduchyk/course-staff-management/releases/tag/v1.0.0
- Нотатки за категоріями: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/.github/release.yml
- Прод на тегу: **не збігається.** Тег `v1.0.0` стоїть на `423c35107040b47a7917211fe6f811c2d795f9eb`, прод показує `173434fae9c7ce434a5f0d88d58b0518bc3b79a1`. Після тегу в `main` злито два PR Dependabot (#94, #96) і трекер помилок (#97), а кожне злиття в `main` автоматично їде на прод
- Приймання PM перед тегом: **не отримано.** PM цього тижня не відповідав; реліз-кандидат на staging у чат команди не надсилався

### Сигнали (трек B)

- Крок алерту: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/.github/workflows/smoke.yml — крок «Алерт людині — issue в репозиторії» у кожному з jobs `prod` і `deploy`, `if: failure()`.
  Канал — **issue в цьому ж репозиторії**, а не Telegram. Три причини: власник репо одразу отримує сповіщення поштою й у GitHub; issue лишається записом інциденту, на який потім посилається postmortem, чого повідомлення в чаті не дає; дублікати не плодяться — якщо відкрите issue з тим самим заголовком уже є, додається коментар. Побічна вигода: цей канал не потребує жодного токена, тож і витікати нічому
- Навмисно червоний прогін: **навмисного немає.** Є справжній: https://github.com/SvitlanaHaiduchyk/course-staff-management/actions/runs/45363278473 — job `prod` упав о 09:14 24.11, бо прод ще показував попередній коміт, і крок алерту о 09:16 відкрив https://github.com/SvitlanaHaiduchyk/course-staff-management/issues/95 «Червоний smoke: production 94857e1». Тобто алерт доходить до людини — перевірено подією, а не задумом. Окремого навмисного прогону, якого вимагає рубрика, я не робила
- Знімок алерту: знімка немає, бо алерт і є записом — issue https://github.com/SvitlanaHaiduchyk/course-staff-management/issues/95. Секретів у ньому немає за побудовою: текст складається лише з назви середовища, скороченого SHA, адреси й посилання на прогін
- Хто отримує алерт: розробник (власник репо) — поштою й у GitHub. PM доступу до цього репо не має, тож до нього алерт не доходить
- Трекер помилок: Sentry —
  https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/src/instrumentation.ts (`register()` для Node і Edge плюс `onRequestError`),
  https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/src/instrumentation-client.ts,
  https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/src/app/global-error.tsx.
  Майстер `@sentry/wizard` навмисно не запускався: він додає демо-сторінку `/sentry-example-page`, яку завалили б власні ворота цього репо (`fallow audit` — мертвий код, `check:design` — сирі кольори), і файл із токеном завантаження source maps
- Дані моделі в трекері: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/src/lib/observability/sentry-options.ts — `dataCollection` вимикає поіменно `userInfo`, `cookies`, `httpHeaders`, `httpBodies`, `urlQueryParams` і `genAI: { inputs: false, outputs: false }`; останнє — саме вхід і вихід моделі.
  Варто сказати, **як** цей рядок з'явився. Спершу було написано `sendDefaultPii: false` — і рев'ю CodeRabbit показало, що в `@sentry/nextjs` 11 такого параметра вже немає: `grep -rl sendDefaultPii node_modules/@sentry/core/` не дає жодного файлу. TypeScript промовчав, бо об'єкт типізовано власним типом, тож зайві поля не перевіряються. Гарантія приватності була хибною при зелених тестах і зелених типах
- Подія навмисної помилки: **знімка події в інтерфейсі Sentry немає** — навмисної помилки на preview чи staging я не влаштовувала. Замість знімка — дві перевірки:
  - **доставка:** локальний прогін із реальним DSN, подія `cfa5c6caedc543039bf0d7ed678ccc3a`, відповідь ingest **HTTP 200**, `environment: "local"`;
  - **вміст:** https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/tests/observability-event.test.ts — справжній `Sentry.init` із синтетичним DSN і `beforeSend`, який забирає подію й повертає `null`. Перевірено, що в **повній** події немає ні коментаря заявки, ні `details` від Postgres, а лишаються `db 23514 at submit_leave_request`, теги `area/where/sqlState`, назва обмеження й порожні breadcrumbs
- Тестовий маршрут прибрано чи закрито: окремого маршруту в репо не з'являлося. Для перевірки доставки сторінку створено локально й видалено до коміту — у git її немає
- Журнал `llm.call`: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/src/lib/ai/llm-log.ts — один рядок JSON на виклик моделі: модель, затримка, токени, стан (`ok / needs_info / refused / unavailable`), чи був fallback, версія промпту, id запиту
- Тест «без тексту користувача»: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/tests/llm-log.test.ts і https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/tests/llm-log-assistant.test.ts; зелений прогін CI: https://github.com/SvitlanaHaiduchyk/course-staff-management/actions/runs/45359064981

### Інцидент-дріл (трек C)

- Сценарій: **дрілу на staging не було.** Замість нього розібрано справжній інцидент на проді 23.11.2026: прод не збирався близько двох годин, бо `@vitest/eslint-plugin` у **патч**-версії 1.6.28 підняв peer-залежність `typescript` до `>=6`. `npm ci` у CI ставить версії з lock-файлу й цього не бачив, а Vercel ставить залежності через `npm install`, який їх перерозв'язує, — і падав на ERESOLVE. Баз ніхто не чіпав: ні міграцій, ні видалення даних
- Сигнал і час: сигналом стала **червона перевірка Vercel на PR**, а не smoke. Прод лишався на попередньому деплої й відповідав `200`, тож smoke був зелений увесь час. Хронологія з часом — у postmortem. Це й записано як головний висновок: наш сигнал бачить «старе впало», але не бачить «нове не їде»
- Профіль «лише пропонує»: **окремого профілю для розбору немає.** Чинні обмеження агента — https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/.claude/settings.json: заборонено читати `.env*`, заборонено `supabase db push`, `db reset --linked`, `migration down --linked` і `migration repair`. Інструменти **запису** MCP Vercel і Supabase поіменно не заборонені: Vercel MCP у репо не підключений узагалі, а Supabase MCP підключений лише на читання (нижче). Вимоги рубрики «заборонені поіменно» це не виконує
- MCP-конфіг: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/.mcp.json — Supabase з `read_only=true` і `features=database,docs`; `project_ref` береться зі змінної оточення, у файлі значення немає. Vercel MCP не підключали
- Стенограма розбору: **немає.** Розбір справжнього інциденту вівся в сесії агента, але стенограму не збережено й не закомічено, а переказувати своїми словами рубрика прямо забороняє
- Виправлення людиною, не агентом: https://github.com/SvitlanaHaiduchyk/course-staff-management/pull/91 — закріплення `@vitest/eslint-plugin` на 1.6.27; рішення й злиття — людини
- Postmortem: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/docs/incidents/2026-11-23-prod-stuck-peer-bump.md — хронологія з часом, вплив мовою користувача, причина з доказом на кожне твердження, розділ «що НЕ було причиною» (перевірені й відкинуті гіпотези, зокрема `.npmrc`), що спрацювало і що ні, дії з власником.
  Там же дослівно записано внесок агента в затримку: агент радив мерджити PR попри червону перевірку Vercel, міркуючи за аналогією з учорашнім падінням, замість того щоб відкрити лог. Саме цей епізод став окремим розділом runbook «Червона перевірка на PR — читати лог, а не згадувати»
- Runbook: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/docs/runbook.md — червоний smoke; червона перевірка на PR; відкат і повернення автоматичного просування (Promote); база на паузі; ротація ключа; де журнали і скільки живуть; помилки застосунку (розділ 5a); staging; розбір з агентом; чого ніколи не робити
- Дія з postmortem → перевірка: https://github.com/SvitlanaHaiduchyk/course-staff-management/pull/93 — у job `app` додано крок «Залежності розв'язуються з нуля (шлях Vercel, не lockfile)» — `npm install --dry-run`. Він відтворює саме те, що робить Vercel, і чого не робить `npm ci`. Перевірка вже працювала на наступних PR Dependabot: https://github.com/SvitlanaHaiduchyk/course-staff-management/actions/runs/45366087259 (#94) і https://github.com/SvitlanaHaiduchyk/course-staff-management/actions/runs/45366135236 (#96)
- Розбір PM: не надходило

### Ланцюг постачання (трек D)

- `dependabot.yml`: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/.github/dependabot.yml — `npm` і `github-actions`, щотижня, `cooldown` (7 днів для патч- і мінорних, 30 для мажорних), група `npm-minor-and-patch`; окремі `ignore` з причинами: мажорний `supabase`, `@vitest/eslint-plugin >= 1.6.28` (наслідок інциденту), мажорний `@types/node`
- Dependabot запускався: https://github.com/SvitlanaHaiduchyk/course-staff-management/pull/85 (npm, зокрема `next` 16.4.6), https://github.com/SvitlanaHaiduchyk/course-staff-management/pull/84 (github-actions), https://github.com/SvitlanaHaiduchyk/course-staff-management/pull/94, https://github.com/SvitlanaHaiduchyk/course-staff-management/pull/96 — усі злиті
- `.npmrc`: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/.npmrc — `min-release-age=7`
- Дії за SHA: https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/173434fae9c7ce434a5f0d88d58b0518bc3b79a1/.github/workflows/smoke.yml — `actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1`, `actions/setup-node@949feb2413d6458794dcd2491c4babbbce0c15c1 # v7.1.0`; так само в `ci.yml`, `quality.yml`, `architecture.yml` і `e2e.yml`. Dependabot оновлює і SHA, і коментар версії — це видно з PR #84
- Push protection і CodeQL: обидва увімкнено в налаштуваннях репо. Доказ — прогони CodeQL: https://github.com/SvitlanaHaiduchyk/course-staff-management/actions/runs/45370378554 і https://github.com/SvitlanaHaiduchyk/course-staff-management/actions/runs/45366134851 (`Analyze (actions)` і `Analyze (javascript-typescript)`, обидва `success`); перевірка `Analyze` тепер висить на кожному PR. Знімка сторінки налаштувань немає
- `next` ≥ 16.4.5: https://github.com/SvitlanaHaiduchyk/course-staff-management/pull/85 — груповий PR Dependabot, `next` 16.4.5 → 16.4.6, злито, CI зелений

## Бонус

—
