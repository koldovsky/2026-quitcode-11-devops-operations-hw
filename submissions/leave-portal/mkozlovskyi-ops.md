# Здача WS11

Скорочення: WS (workshop — заняття курсу), PM (project manager — менеджер проєкту), SHA (Secure Hash Algorithm — тут: 40-символьний ідентифікатор коміту), URL (Uniform Resource Locator — адреса), SLO (Service Level Objective — ціль рівня сервісу), RPO/RTO (Recovery Point / Recovery Time Objective — скільки даних можна втратити і скільки продукт може не працювати), UAT (User Acceptance Testing — приймальне тестування), CI (continuous integration — автоматичні перевірки кожної зміни), PR (pull request — запит на злиття змін), MCP (Model Context Protocol — спосіб дати агенту інструменти зовнішнього сервісу).

## Дані

- Роль: pm
- Команда: leave-portal
- GitHub-логін: mkozlovskyi-ops
- Ваше репо: https://github.com/mkozlovskyi-ops/course-staff-management-spec
- SHA: 9babda606a6412d1ac90709214124e229f6959cb
- Репо вимог PM: https://github.com/mkozlovskyi-ops/course-staff-management-spec
- Прод-URL: —
- Staging-URL: —

## PM

### Операційні вимоги (трек A)

- Сховище: docs/operations.md
- Операційні вимоги: https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/9babda606a6412d1ac90709214124e229f6959cb/docs/operations.md
- SLO:
  - SLO-1: зранку портал відкривається, вхід працює, дані на місці — щонайменше 27 зелених щоденних прогонів `smoke` із 28 — історія `smoke.yml` за розкладом у GitHub Actions кожної реалізації — бюджет: 1 червоний щоденний прогін за 28 днів
  - SLO-2: після оновлення порталу все працює, як раніше — щонайменше 19 із 20 деплоїв на прод із зеленим `smoke` з першого разу — прогони `smoke.yml` після деплою на прод — бюджет: 1 деплой із 20
- Матриця серйозності: SEV1–3, по 2 приклади з порталу на рівень (SEV1 — портал не відкривається / чужі залишки за прямим посиланням; SEV2 — хибне списання залишку / помічник календаря недоступний; SEV3 — текст не як у вимозі), хто реагує і як швидко (SEV1 — розробник протягом 2 год робочого часу, клієнту — того ж дня): https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/9babda606a6412d1ac90709214124e229f6959cb/docs/operations.md#2-матриця-серйозності-sev13
- RPO і RTO: RPO — не більше 1 доби змін, RTO — 4 години робочого часу. На безкоштовному Supabase резервних копій немає: ціль RPO досяжна лише в реалізації з власною щоденною копією в CI, в інших після втрати бази відновлювати нічого (дані зараз синтетичні); вночі й у вихідні ніхто не реагує
- Хто отримує алерт: червоний smoke — Telegram-група реалізації (розробник і PM) або issue «Червоний smoke» в репо розробника; клієнту пише PM того ж робочого дня при SEV1–SEV2
- Після курсу: пауза — портал лишається на безкоштовних тарифах для показів, за день до кожного показу PM перевіряє `/api/health` і просить розбудити базу

### Приймання релізу (трек B)

- Сценарій demo day для UAT: адміністратор вставляє текст оголошення → помічник календаря пропонує раннє закінчення → зберегти → закриття видно в календарі → працівник подає заявку на 2 дати, закритий день не приймається → менеджер погоджує → працівник бачить «Approved» і зменшений залишок → працівник за прямим посиланням відкриває чужі залишки й бачить відмову; https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/9babda606a6412d1ac90709214124e229f6959cb/docs/demo-day.md#1-сценарій-показу-з-часом
- UAT на staging:
  - @rhutsul: staging https://leave-portal-git-staging-rh-3ddc.vercel.app, сценарій ✅, підпис https://github.com/rhutsul/leave-portal/issues/142#issuecomment-7220944479
  - @lukian-tomochko: ❌ крок 3 → https://github.com/lukian-tomochko/leave-portal/issues/104, підпис https://github.com/lukian-tomochko/leave-portal/issues/100#issuecomment-7220650969
  - @SvitlanaHaiduchyk: не прийнято: повідомлення «реліз-кандидат на staging» не надходило, 2026-11-27
- Відхилення від вимог: https://github.com/lukian-tomochko/leave-portal/issues/104
- Розбір postmortem і runbook:
  - @rhutsul: https://github.com/rhutsul/leave-portal/pull/115#issuecomment-7220229635
  - @lukian-tomochko: https://github.com/lukian-tomochko/leave-portal/pull/99#issuecomment-7220224519
  - @SvitlanaHaiduchyk: не розібрано: запиту на розбір postmortem не надходило, 2026-11-27
- Повідомлення клієнту про інцидент: https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/9babda606a6412d1ac90709214124e229f6959cb/docs/incident-message.md — за postmortem @rhutsul (дріл 2026-11-20)

### Готовність до demo day і вартість (трек C)

- Чекліст demo day: https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/9babda606a6412d1ac90709214124e229f6959cb/docs/demo-day.md
- Точки відмови з планом Б: прод не відкривається, база на паузі, вичерпано ліміт моделей, вхід не проходить (ліміт входів з однієї мережі), дані на проді не ті для сценарію, під час показу видно чужі дані
- Записка про вартість: https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/9babda606a6412d1ac90709214124e229f6959cb/docs/cost-note.md
- Цифри станом на: 09.11.2026 (Supabase Branching — 17.11.2026), звірено з гайдом вартості 24.11.2026

## Розробник

—

## Бонус

—
