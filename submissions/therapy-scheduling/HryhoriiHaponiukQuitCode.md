# Здача WS11

Скорочення: WS (workshop — заняття курсу), PM (project manager — менеджер проєкту), SHA (Secure Hash Algorithm — тут: 40-символьний ідентифікатор коміту), URL (Uniform Resource Locator — адреса), SLO (Service Level Objective — ціль рівня сервісу), RPO/RTO (Recovery Point / Recovery Time Objective — скільки даних можна втратити і скільки продукт може не працювати), UAT (User Acceptance Testing — приймальне тестування), CI (continuous integration — автоматичні перевірки кожної зміни), PR (pull request — запит на злиття змін), MCP (Model Context Protocol — спосіб дати агенту інструменти зовнішнього сервісу).

## Дані

- Роль: pm
- Команда: therapy-scheduling
- GitHub-логін: HryhoriiHaponiukQuitCode
- Ваше репо: https://github.com/HryhoriiHaponiukQuitCode/therapy-scheduling-spec
- SHA: 0bedb11a79441a2cc036bb6a7025420f2338ef90
- Репо вимог PM: https://github.com/HryhoriiHaponiukQuitCode/therapy-scheduling-spec
- Прод-URL: —
- Staging-URL: —

## PM

### Операційні вимоги (трек A)

- Сховище: docs/operations.md
- Операційні вимоги: https://github.com/HryhoriiHaponiukQuitCode/therapy-scheduling-spec/blob/0bedb11a79441a2cc036bb6a7025420f2338ef90/docs/operations.md
- SLO:
  - SLO-1: працівник практики відкриває застосунок, входить і бачить свої дані — ≥ 95% прогонів `smoke` на прод зелені за 28 днів — історія workflow `smoke` у GitHub Actions репо розробника (після кожного випуску й щодня) — бюджет 5%, ≈ 2 червоні прогони з ~40.
  - SLO-2: помічник правильно заповнює чернетку картки з типового звернення — у кожному прогоні 10 еталонних звернень (GC-01…GC-10) ≥ 8 правильних — звіт прогону в репо розробника, щотижня й перед випуском, що змінює помічника — бюджет 2 з 10; стан «assistant unavailable» при вичерпаному ліміті не рахуємо (штатний, AC-INTAKE-08).
  - SLO-3: про збій на проді команда дізнається раніше за працівників — від поломки до алерту ≤ 15 хв у 100% інцидентів — T1 − T0 у postmortem — бюджет 0 (кожне порушення — дія в postmortem).
- Матриця серйозності: SEV1–3, по 2 приклади з нашого продукту (пауза бази проду 23–25.11, терапевт бачить чужі картки; «assistant unavailable», не зберігається статус страхування; зламаний staging у дрілах 22.11 і 25.11, хибна тривога про прод), хто реагує і як швидко: https://github.com/HryhoriiHaponiukQuitCode/therapy-scheduling-spec/blob/0bedb11a79441a2cc036bb6a7025420f2338ef90/docs/operations.md#2-матриця-серйозності-sev13
- RPO і RTO: RPO — не більше 1 доби змін; на Supabase Free копій немає, тож після збою бази відновлювати нічого (для курсу прийнятно: дані синтетичні). RTO — 4 год у робочий час; відкат і Resume project — хвилини, але дріл 25.11 показав, що розбір може стояти годинами.
- Хто отримує алерт: Telegram-група розробника: розробник цього проду й PM (PM ще не в усіх групах — відкрита дія з postmortem 22.11); помилки сервера — Sentry розробника, де він є.
- Після курсу: пауза — лишаємо безкоштовні тарифи для показу й будимо базу перед кожним показом; справжніх даних без Pro і резервних копій не вносимо.

### Приймання релізу (трек B)

- Сценарій demo day для UAT: планувальник входить → вставляє текст звернення й отримує чернетку картки → перевіряє поля й створює картку → позначає статус страхування «Verified» → утримує вільний слот терапевта за клієнтом → підтверджує утримання → терапевт входить і бачить підтверджений сеанс і лише своїх клієнтів (https://github.com/HryhoriiHaponiukQuitCode/therapy-scheduling-spec/blob/0bedb11a79441a2cc036bb6a7025420f2338ef90/docs/demo-day.md#1-сценарій-показу-з-часом)
- UAT на staging:
  - @maryan-qc: staging https://therapy-scheduling-git-staging-mandrushchak-4349s-projects.vercel.app, сценарій ✅, підпис https://github.com/maryan-qc/therapy-scheduling/issues/75#issuecomment-7175074613
  - @SnatchPlay: staging https://therapy-scheduling-git-staging-snatchplays-projects.vercel.app, сценарій ✅, підпис https://github.com/SnatchPlay/therapy-scheduling/pull/102#issuecomment-7185938555
  - @pavmys: не прийнято: staging https://therapy-scheduling-git-staging-pavlom2901-9007s-projects.vercel.app пише в базу проду (клієнт, створений на staging, з'явився на проді https://therapy-scheduling-three.vercel.app), а окрема база staging — умова WS11. Сценарій demo day на staging пройшов (7 з 7 кроків); розробнику повідомлено, що виправити; «реліз-кандидат на staging» не надходив, 2026-11-26
  - @markkern-hyra: не прийнято: «реліз-кандидат на staging» не надходив, гілки staging немає, 2026-11-26
- Відхилення від вимог: відхилень немає (staging @maryan-qc і @SnatchPlay: усі 7 кроків сценарію пройшли; у @SnatchPlay — дрібне зауваження про `role="alert"` для повідомлень про успіх, у підписі)
- Розбір postmortem і runbook:
  - @maryan-qc: https://github.com/maryan-qc/therapy-scheduling/pull/73#issuecomment-7174684592
  - @SnatchPlay: https://github.com/SnatchPlay/therapy-scheduling/pull/100#issuecomment-7174684081
  - @pavmys: не прийнято: postmortem не надходив, 2026-11-26
  - @markkern-hyra: не прийнято: postmortem не надходив, 2026-11-25
- Повідомлення клієнту про інцидент: https://github.com/HryhoriiHaponiukQuitCode/therapy-scheduling-spec/blob/0bedb11a79441a2cc036bb6a7025420f2338ef90/docs/incident-message.md — за postmortem @SnatchPlay (дріл 22.11 на staging)

### Готовність до demo day і вартість (трек C)

- Чекліст demo day: https://github.com/HryhoriiHaponiukQuitCode/therapy-scheduling-spec/blob/0bedb11a79441a2cc036bb6a7025420f2338ef90/docs/demo-day.md
- Точки відмови з планом Б: прод не відкривається, база на паузі, вичерпано ліміт моделей чи модель не відповідає, помічник склав неправильну чернетку, утримання прострочилось чи слот зайнятий, інтернет у залі не працює
- Записка про вартість: https://github.com/HryhoriiHaponiukQuitCode/therapy-scheduling-spec/blob/0bedb11a79441a2cc036bb6a7025420f2338ef90/docs/cost-note.md
- Цифри станом на: 09.11.2026 (Supabase Branching — 17.11.2026); перевірено 25.11.2026

## Розробник

—

## Бонус

—
