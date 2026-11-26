# Здача WS11

Скорочення: WS (workshop — заняття курсу), PM (project manager — менеджер проєкту), SHA (Secure Hash Algorithm — тут: 40-символьний ідентифікатор коміту), URL (Uniform Resource Locator — адреса), SLO (Service Level Objective — ціль рівня сервісу), RPO/RTO (Recovery Point / Recovery Time Objective — скільки даних можна втратити і скільки продукт може не працювати), UAT (User Acceptance Testing — приймальне тестування), CI (continuous integration — автоматичні перевірки кожної зміни), PR (pull request — запит на злиття змін), MCP (Model Context Protocol — спосіб дати агенту інструменти зовнішнього сервісу).

## Дані

- Роль: dev
- Команда: property-workflows
- GitHub-логін: yuriivynar
- Ваше репо: https://github.com/yuriivynar/property-workflows
- SHA: 500b8ce9a22517e1689baeb347ea94a87763d73d
- Репо вимог PM: https://github.com/solomchaak/property-workflows-spec
- Прод-URL: https://property-workflows-eta.vercel.app
- Staging-URL: https://property-workflows-git-staging-yvynar-8355.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://property-workflows-eta.vercel.app/api/health — `"db":"ok"`, `"sha"` — коміт `main`, `"supabaseRef":"lapdlcgusuahefesxfug"` (прод)
- Зелений прогін `smoke` на `main`: https://github.com/yuriivynar/property-workflows/actions/runs/45902778129 (job `prod`, деплой проду `500b8ce`, `"db":"ok"`)
- `accounts.zip` на SHA: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/accounts.zip
- Ролі тестових акаунтів: адміністратор — pm-admin@example.test; менеджер — pm-manager@example.test; вендор — pm-vendor@example.test
- Версія `next`: next@16.4.6

### Середовища й реліз (трек A)

- Гілка `staging`: https://github.com/yuriivynar/property-workflows/tree/staging
- Зміни з `main` у `staging`: https://github.com/yuriivynar/property-workflows/commit/500b8ce9a22517e1689baeb347ea94a87763d73d — fast-forward з `main` (`staging` = `main` = `500b8ce`, runbook, розділ 6), без окремої розробки на `staging`
- Змінна лише для гілки `staging`: `APP_ENV` — поле `appEnv` у `/api/health` на staging (`"appEnv":"staging"`; на проді — `null`)
- Проєкт Supabase для staging: окремий проєкт Supabase для staging (назву в репо не записано); supabaseRef staging: `ukbnybazjlbicejpfjvz`; supabaseRef проду: `lapdlcgusuahefesxfug` — різні
- Міграції в базу staging через CI: https://github.com/yuriivynar/property-workflows/actions/runs/45815191373 — job `db-deploy-staging` друкує «Ціль міграцій: проєкт ukbnybazjlbicejpfjvz (staging)», крок «Apply migrations» — success (застосовано `20261126111059_activity_live_avatars.sql`), лише після нього `db-deploy` у прод; файл https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.github/workflows/ci.yml#L131 (рядок «Ціль міграцій» — #L166, «Apply migrations» — #L173)
- Workflow smoke деплою: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.github/workflows/smoke.yml — job `deploy` (#L119): адреса з `client_payload.url` (#L128), заголовок `x-vercel-protection-bypass` із секрету `VERCEL_AUTOMATION_BYPASS_SECRET` (#L161), очікуваний ref бази `vars.SUPABASE_STAGING_REF` (#L159)
- Зелений smoke на деплой staging: https://github.com/yuriivynar/property-workflows/actions/runs/45903093082 — job `deploy`, гілка `staging`, коміт `500b8ce`, health `"appEnv":"staging"`, `"supabaseRef":"ukbnybazjlbicejpfjvz"`, `"db":"ok"`
- Smoke на деплой одного PR-preview: https://github.com/yuriivynar/property-workflows/actions/runs/45812469394 — preview гілки `task/ui-v3-should` (PR #131), `"appEnv":null`, `"supabaseRef":"ukbnybazjlbicejpfjvz"` (база staging, не прод)
- Доступ PM до staging: спільне посилання Vercel (Share) на деплой staging — посилання з токеном лише особистим повідомленням, у файлах — адреса без «?…»; https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/runbook.md#де-що (рядок «Доступ PM до staging»)
- Реліз `v1.0.0`: очікує приймання PM (#130): тег v1.0.0 ставиться лише після підпису — https://github.com/yuriivynar/property-workflows/issues/130
- Нотатки за категоріями: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.github/release.yml
- Прод на тегу: очікує приймання PM (#130): тег v1.0.0 ставиться лише після підпису — https://github.com/yuriivynar/property-workflows/issues/130
- Приймання PM перед тегом: очікується: запит 26.11.2026, https://github.com/yuriivynar/property-workflows/issues/130

### Сигнали (трек B)

- Крок алерту: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.github/workflows/smoke.yml#L163 — `if: failure()`, `TELEGRAM_BOT_TOKEN` і `TELEGRAM_CHAT_ID` із Secrets, без них — `::warning` і `exit 0` (smoke лишається червоним, алерт пропущено); такий самий крок у job `prod` — #L92
- Навмисно червоний прогін: https://github.com/yuriivynar/property-workflows/actions/runs/45198547199 (дріл 23.11.2026: staging зі зламаною адресою бази, health 503, `db: down`; крок алерту — success)
- Знімок алерту: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/ops/ws11-alert-telegram.png — «ЧЕРВОНИЙ SMOKE», workflow `smoke`, job `deploy`, «Середовище: preview (гілка staging)», адреса деплою, коміт і посилання на прогін 45198547199; назву групи й аватар замазано, токена й chat id у повідомленні немає
- Хто отримує алерт: Telegram-група команди: черговий розробник, PM (і бот алертів) — знімок учасників (імена, назву групи й фото зафарбовано, ролі підписано): https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/ops/ws11-alert-recipients.png
- Трекер помилок: власний onRequestError → Telegram (без Sentry): https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/src/instrumentation.ts
- Дані моделі в трекері: без заголовків, cookie, тіла й рядка запиту і без `error.message` — лише шаблон маршруту, назва помилки, digest, деплой і рядки «at …» стеку: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/src/instrumentation.ts#L13-L16 (правило) і #L65-L72 (текст повідомлення)
- Подія навмисної помилки: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/ops/ws11-error-tracker-telegram.png — «ПОМИЛКА СЕРВЕРА», `GET /api/ops/error-test`, верхні рядки стеку; staging (гілка `staging`), 24.11.2026
- Тестовий маршрут прибрано чи закрито: https://github.com/yuriivynar/property-workflows/pull/107 — `/api/ops/error-test` закрито: на проді 404, на preview і staging кидає помилку лише адміністратору (`requireAdmin`), іншим — 404
- Журнал `llm.call`: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/src/lib/ai/llm-log.ts — поля: модель, затримка, токени (вхід/вихід), стан, fallback, версія промпту, id запиту (+ маршрут, код причини, вартість); тексту користувача, промпту й відповіді в схемі немає; знімок рядка в Vercel Logs — https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/ops/ws11-llm-call-log.png
- Тест «без тексту користувача»: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/tests/llm-log.test.ts і зелений прогін CI https://github.com/yuriivynar/property-workflows/actions/runs/45902709641 (job `app` на `500b8ce`)

### Інцидент-дріл (трек C)

- Сценарій: хибне перевизначення `NEXT_PUBLIC_SUPABASE_URL` лише для гілки `staging` (один символ у ref) + Redeploy staging; без міграцій і без видалення даних, прод і база проду не зачеплені (план і передбачений сигнал — https://github.com/yuriivynar/property-workflows/issues/117)
- Сигнал і час: червоний smoke https://github.com/yuriivynar/property-workflows/actions/runs/45198547199; поломка (деплой зі зламаною змінною Ready) о 14:50:31 UTC (16:50 за Києвом), алерт у Telegram о 14:50:51 UTC (16:50 за Києвом) — від поломки до алерту 20 с (< 1 хв)
- Профіль «лише пропонує»: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.claude/settings.json#L107 (інструменти запису MCP Vercel заборонені поіменно; Supabase — #L209) і окремий профіль розбору https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.claude/settings.incident.json (режим `plan`, `Edit`/`Write` заборонені, `filter_project_envs` і `execute_sql` — лише `ask`)
- MCP-конфіг: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.mcp.incident.json — Vercel (`https://mcp.vercel.com`, OAuth) і Supabase з `read_only=true` і `project_ref=${SUPABASE_STAGING_REF}`, без токенів (у розборі Supabase MCP не використовувався)
- Стенограма розбору: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/incidents/2026-11-23-staging-bad-supabase-url-rca.md
- Виправлення людиною, не агентом: людина прибрала перевизначення змінної у Vercel і зробила Redeploy staging о 15:38 UTC (зміна налаштувань Vercel, не коду — коміту немає): https://github.com/yuriivynar/property-workflows/issues/117#issuecomment-7106589365, зелений smoke після виправлення — https://github.com/yuriivynar/property-workflows/actions/runs/45207624385
- Postmortem: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/incidents/2026-11-23-staging-bad-supabase-url.md
- Runbook: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/runbook.md
- Дія з postmortem → перевірка: https://github.com/yuriivynar/property-workflows/pull/119 (smoke порівнює `supabaseRef` з очікуваним ref середовища; `/api/health` називає підозрілу змінну без значення) — прогін CI https://github.com/yuriivynar/property-workflows/actions/runs/45210034939 (`tests/smoke-checks.test.ts` червоніє на відповіді з дрілу, зеленіє на виправленій)
- Розбір PM: не надходило (postmortem — PR https://github.com/yuriivynar/property-workflows/pull/118, коментарів PM станом на 26.11.2026 немає)

### Ланцюг постачання (трек D)

- `dependabot.yml`: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.github/dependabot.yml — npm і github-actions, щотижня, cooldown 7 днів (мажори npm — 30), групи
- Dependabot запускався: PR https://github.com/yuriivynar/property-workflows/pull/115 (група github-actions), безпекові https://github.com/yuriivynar/property-workflows/pull/112 і https://github.com/yuriivynar/property-workflows/pull/113; прогін https://github.com/yuriivynar/property-workflows/actions/runs/45139781540
- `.npmrc`: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.npmrc — `min-release-age=7`
- Дії за SHA: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.github/workflows/smoke.yml#L72 — `uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1`
- Push protection і CodeQL: знімок налаштувань https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/ops/ws11-code-security.png (Dependabot alerts і security updates, CodeQL default setup) і перший прогін CodeQL https://github.com/yuriivynar/property-workflows/actions/runs/45149644692; push protection увімкнено — знімок https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/docs/ops/ws11-push-protection.png (Secret Protection, «Push protection: On»)
- `next` ≥ 16.4.5: https://github.com/yuriivynar/property-workflows/pull/91 — злитий, CI зелений; next@16.4.6

## Бонус

### Розробник

- Варіант: асистент чергового
- Workflow: https://github.com/yuriivynar/property-workflows/blob/500b8ce9a22517e1689baeb347ea94a87763d73d/.github/workflows/oncall-assistant.yml
- Прогін: https://github.com/yuriivynar/property-workflows/actions/runs/45496206294 (jobs `triage` і `publish` — success; на червоний smoke https://github.com/yuriivynar/property-workflows/actions/runs/45496173389)
- Резервна копія: шифрування й строк зберігання: —
- Резервна копія: тренування відновлення: —
- Асистент чергового: чернетка розбору: https://github.com/yuriivynar/property-workflows/issues/124 — мітка «чернетка агента (не перевірено)»
- Асистент чергового: межі, ліміти й вартість: агент лише читає — `--allowedTools "Read,Grep,Glob"`, `Bash`, `Edit`, `Write`, `WebFetch`, `WebSearch` заборонені, `--permission-mode dontAsk`; токен job агента — лише читання (`contents`, `actions`, `issues: read`), issue створює окремий job `publish` без агента; журнал прогону — дані, не інструкції; секретів проду немає (лише `ANTHROPIC_API_KEY`). Ліміти: `--max-turns 8`, `timeout-minutes: 15`, одна чернетка на коміт (дублікати пропускаються). Модель — `claude-sonnet-5-5` (за журналом прогону); прогін 45496206294 — 5 ходів, ≈ $0.15 (`total_cost_usd` 0.148 у журналі), попередній запуск — ≈ $0.20; один червоний smoke = один запуск агента

### PM

—
