# Здача WS11

## Дані

- Роль: dev
- Команда: school-purchasing
- GitHub-логін: YarynaPankivv
- Ваше репо: https://github.com/YarynaPankivv/school-purchasing
- SHA: 048f58ebab9caf074c0ac88c9de4ecd247d68df4
- Репо вимог PM: https://github.com/ValiaBihun/2026-quitcode-dream-team
- Прод-URL: https://school-purchasing.vercel.app
- Staging-URL: https://school-purchasing-git-staging-ypankiv-7566s-projects.vercel.app

## PM

—

## Розробник

### D0 — умова прийняття

- `/api/health`: https://school-purchasing.vercel.app/api/health — `"db":"ok"`, `sha` = `048f58ebab9caf074c0ac88c9de4ecd247d68df4` (SHA `main`, `staging` і тегу `v1.1.0`)
- Зелений прогін `smoke` на `main`: https://github.com/YarynaPankivv/school-purchasing/actions/runs/45973483617
- `accounts.zip` на SHA: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/accounts.zip
- Ролі тестових акаунтів: адміністратор — admin@example.test, pm-admin@example.test; працівник (подає заявки) — member1@example.test, member2@example.test, pm-requester@example.test, pm-empty@example.test (без заявок); схвалювач відділу — approver@example.test, pm-approver@example.test. Паролі — лише в accounts.zip
- Версія `next`: next@16.4.5 (`package-lock.json` на SHA)

### Середовища й реліз (трек A)

- Гілка `staging`: https://github.com/YarynaPankivv/school-purchasing/tree/staging
- Зміни з `main` у `staging`: лише fast-forward з `main` (`git merge --ff-only origin/main`); поточний коміт staging https://github.com/YarynaPankivv/school-purchasing/commit/048f58ebab9caf074c0ac88c9de4ecd247d68df4 (PR https://github.com/YarynaPankivv/school-purchasing/pull/118; staging, `main` і прод на одному SHA)
- Змінна лише для гілки `staging`: `APP_ENV=staging` — поле `appEnv` у `/api/health` (на проді `production`, на PR-preview `preview`)
- Проєкт Supabase для staging: school-purchasing-staging; supabaseRef staging: nrapwkzhqplxvwgittly; supabaseRef проду: rgpngqhjkawryldpyxzl
- Міграції в базу staging через CI: прогін CI на main https://github.com/YarynaPankivv/school-purchasing/actions/runs/45973423722 — job `db-deploy-staging` друкує «Ціль міграцій: проєкт nrapwkzhqplxvwgittly (staging)», крок «Apply migrations» — success; файл https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.github/workflows/ci.yml#L127
- Workflow smoke деплою: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.github/workflows/smoke.yml — адреса з `client_payload.url`, заголовок `x-vercel-protection-bypass` із секрету `VERCEL_AUTOMATION_BYPASS_SECRET`
- Зелений smoke на деплой staging: https://github.com/YarynaPankivv/school-purchasing/actions/runs/45975591746 (SHA 048f58e) — у журналі `"appEnv":"staging"`, `supabaseRef` staging; раніший прогін https://github.com/YarynaPankivv/school-purchasing/actions/runs/44920287009
- Smoke на деплой одного PR-preview: https://github.com/YarynaPankivv/school-purchasing/actions/runs/45160317993 — у журналі `"appEnv":"preview"`, `supabaseRef` staging
- Доступ PM до staging: спільне посилання Vercel (Shareable Link), запасний шлях — виняток Deployment Protection; https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/docs/runbook.md#7-доступ-pm-до-staging
- Релізи: `v1.1.0` https://github.com/YarynaPankivv/school-purchasing/releases/tag/v1.1.0 (поточний, на SHA 048f58e) і `v1.0.0` https://github.com/YarynaPankivv/school-purchasing/releases/tag/v1.0.0 (попередній)
- Нотатки за категоріями: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.github/release.yml
- Прод на тегу: SHA тегу `v1.1.0` = `048f58ebab9caf074c0ac88c9de4ecd247d68df4` = `sha` у `/api/health` проду (`git rev-parse v1.1.0^{commit}`)
- Приймання PM перед тегом: https://github.com/YarynaPankivv/school-purchasing/pull/118#issuecomment-7219202905 (staging на 048f58e, 27.11.2026; до цього `v1.0.0` — https://github.com/YarynaPankivv/school-purchasing/pull/112#issuecomment-7079526517, staging на f9f6c06). Після приймання `v1.1.0` до `main` нічого не зливали

### Сигнали (трек B)

- Крок алерту: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.github/workflows/smoke.yml#L187 (job `deploy`; для проду — #L92): `if: failure()`, токен і chat id із Secrets, без токена — попередження, а не падіння
- Навмисно червоний прогін: https://github.com/YarynaPankivv/school-purchasing/actions/runs/44921240295
- Знімок алерту: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/docs/ops/telegram-alert.png
- Хто отримує алерт: Telegram-група «School Purchasing Alerts»: розробник і PM
- Трекер помилок: власний `onRequestError` — https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/src/instrumentation.ts
- Дані моделі в трекері: власний `onRequestError`: без заголовків, cookie і тексту запиту (у повідомленні лише маршрут, середовище, деплой і стек)
- Подія навмисної помилки: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/docs/ops/telegram-server-error.png — preview, маршрут і стек
- Тестовий маршрут прибрано чи закрито: прибрано в межах https://github.com/YarynaPankivv/school-purchasing/pull/111 (у `main` його немає)
- Журнал `llm.call`: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/src/lib/ai/llm-log.ts — модель, затримка, токени, стан (`ok / needs_info / refused / unavailable`), fallback, версія промпту, id запиту
- Тест «без тексту користувача»: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/tests/llm-log.test.ts і зелений прогін CI https://github.com/YarynaPankivv/school-purchasing/actions/runs/45973423722

### Інцидент-дріл (трек C)

- Сценарій: хибне значення `NEXT_PUBLIC_SUPABASE_URL` (змінено один символ ref) лише для гілки `staging`; без міграцій і без видалення даних, прод і база проду не зачеплені
- Сигнал і час: червоний smoke https://github.com/YarynaPankivv/school-purchasing/actions/runs/44921240295; деплой зі зламаною змінною готовий о 15:49:40 UTC, алерт о 15:50:00 — від поломки до алерту 20 секунд
- Профіль «лише пропонує»: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.claude/settings.incident.json (разом із постійними правилами `.claude/settings.json`); інструменти запису MCP Vercel і Supabase заборонені поіменно
- MCP-конфіг: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.mcp.json — Vercel і Supabase (`read_only=true`, `project_ref` з змінної `SUPABASE_PROJECT_REF` — проєкт staging), без токенів; для розбору інциденту — окремий https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.mcp.incident.json (Vercel і Supabase staging, `read_only=true`)
- Стенограма розбору: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/docs/incidents/2026-11-21-supabase-url-rca.md
- Виправлення людиною, не агентом: виправлення не в git, тож PR чи коміту з ним немає: людина видалила перевизначення змінної для гілки `staging` у Vercel і зробила Redeploy того самого коміту https://github.com/YarynaPankivv/school-purchasing/commit/ea4897db4492dc4929cf890b1ae0e5cac591c707 (16:03:27 → зелений деплой 16:04:15 UTC). Доказ: зелений smoke після Redeploy https://github.com/YarynaPankivv/school-purchasing/actions/runs/44922526753 і хронологія в розділі «Усунення» postmortem; дія з postmortem — https://github.com/YarynaPankivv/school-purchasing/pull/110
- Postmortem: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/docs/incidents/2026-11-21-supabase-url-typo.md
- Runbook: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/docs/runbook.md
- Дія з postmortem → перевірка: https://github.com/YarynaPankivv/school-purchasing/pull/110 — smoke звіряє `supabaseRef` з очікуваним для середовища і називає змінну (`scripts/lib/supabase-ref.mjs`, тест `tests/scripts/smoke-supabase-ref.test.ts`); друга дія — `tests/runbook.test.ts` (розділи runbook не зникають), PR https://github.com/YarynaPankivv/school-purchasing/pull/114; прогін CI https://github.com/YarynaPankivv/school-purchasing/actions/runs/45973423722
- Розбір PM: https://github.com/YarynaPankivv/school-purchasing/pull/110#issuecomment-7079560965 (6 зауважень); відповідь і зміни — https://github.com/YarynaPankivv/school-purchasing/pull/114; повторний розбір PM: прийнято, 27.11.2026 — https://github.com/YarynaPankivv/school-purchasing/pull/110#issuecomment-7214298280

### Ланцюг постачання (трек D)

- `dependabot.yml`: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.github/dependabot.yml — `npm` і `github-actions`, щотижня, `cooldown`, групи
- Dependabot запускався: https://github.com/YarynaPankivv/school-purchasing/pull/98 (злито 21.11.2026)
- `.npmrc`: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.npmrc — `min-release-age=7`
- Дії за SHA: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.github/workflows/smoke.yml — `uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1`
- Push protection і CodeQL: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/docs/ops/push-protection.png, https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/docs/ops/codeql-setup.webp і прогін CodeQL https://github.com/YarynaPankivv/school-purchasing/actions/runs/45161329364
- `next` ≥ 16.4.5: https://github.com/YarynaPankivv/school-purchasing/pull/98 — Dependabot, `next` 16.4.3 → 16.4.5, злитий, CI зелений

## Бонус

### Розробник

- Варіант: резервна копія і відновлення
- Workflow: https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/.github/workflows/backup.yml
- Прогін: https://github.com/YarynaPankivv/school-purchasing/actions/runs/45159259730 (`scope=public`, `dump` і `restore-drill` зелені). Нічний прогін за розкладом після виправлення (PR https://github.com/YarynaPankivv/school-purchasing/pull/115, за розкладом тепер `scope=public`) 27.11: https://github.com/YarynaPankivv/school-purchasing/actions/runs/45979474558 — зелений; ручний прогін після виправлення https://github.com/YarynaPankivv/school-purchasing/actions/runs/45835026310. Перший прогін зі `scope=all` https://github.com/YarynaPankivv/school-purchasing/actions/runs/45158746772 упав на завантаженні `auth.*` (передбачено в runbook, розділ 9)
- Резервна копія: шифрування й строк зберігання: `gpg --symmetric --cipher-algo AES256`, ключ — секрет `BACKUP_PASSPHRASE`; артефакт `supabase-backup` лише зашифрований (`*.gpg`), `retention-days: 7`; дамп у відкритому вигляді — ніде (ні в git, ні в артефакті, ні в журналі)
- Резервна копія: тренування відновлення: прогін https://github.com/YarynaPankivv/school-purchasing/actions/runs/45159259730 — job `restore-drill` порівняв кількість рядків 9 таблиць `public` (у копії = відновлено, усі «ok»: ai_call_log 0, department_approvers 2, department_members 8, departments 4, profiles 8, request_history 13, request_lines 35, requests 31, user_permissions 2); розділ «Відновлення» https://github.com/YarynaPankivv/school-purchasing/blob/048f58ebab9caf074c0ac88c9de4ecd247d68df4/docs/runbook.md#9-відновлення-резервна-копія-бази
- Асистент чергового: чернетка розбору: —
- Асистент чергового: межі, ліміти й вартість: —

### PM

—
