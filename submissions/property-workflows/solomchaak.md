# Здача WS11

## Дані

- Роль: pm
- Команда: property-workflows
- GitHub-логін: solomchaak
- Ваше репо: https://github.com/solomchaak/property-workflows-spec
- SHA: f287b4160a281167acd17a251c86961c353f35f5
- Репо вимог PM: https://github.com/solomchaak/property-workflows-spec
- Прод-URL: —

## PM

### Операційні вимоги (трек A)

- Сховище: docs/operations.md
- Операційні вимоги: https://github.com/solomchaak/property-workflows-spec/blob/f287b4160a281167acd17a251c86961c353f35f5/docs/operations.md
- SLO: SLO-1: застосунок відкривається, і дані на місці — щоденний smoke проду зелений 27 днів із 28 — Actions → `smoke` (schedule) — 1 червоний день; SLO-2: після кожного оновлення застосунок працює — 95% деплоїв на прод зелені з першого прогону — Actions → `smoke` (деплой) — 1 червоний із 20; SLO-3: AI-помічник правильно читає документи — щотижня ≥ 8 з 10 golden cases, кейси 9 і 10 обов'язкові — `evals/reports/` — 1 тиждень нижче порогу
- Матриця серйозності: SEV1–3, по 2 приклади, хто реагує і як швидко — https://github.com/solomchaak/property-workflows-spec/blob/f287b4160a281167acd17a251c86961c353f35f5/docs/operations.md#2-матриця-серйозності-sev13
- RPO і RTO: RPO — не більше 1 доби, але на Supabase Free резервних копій немає (щоденну копію в CI має один розробник); RTO — 4 год у робочий час, база на паузі — Resume project і кілька хвилин
- Хто отримує алерт: група Telegram команди — розробник проду й PM; клієнту пише PM протягом 2 год на SEV1–2
- Після курсу: пауза — продукт лишається на безкоштовних тарифах для показу, базу будять перед кожним показом

### Приймання релізу (трек B)

- Сценарій demo day для UAT: https://github.com/solomchaak/property-workflows-spec/blob/f287b4160a281167acd17a251c86961c353f35f5/docs/demo-day.md#1-сценарій-показу-з-часом
- UAT на staging: @yuriivynar: staging https://property-workflows-git-staging-yvynar-8355.vercel.app, сценарій не прогнано — прийнято без тестування (PM передав завдання із запізненням, 28.11 — останній день, коли PM може перевірити; доступ до захищеного staging не встигли відкрити), підпис https://github.com/yuriivynar/property-workflows/issues/130#issuecomment-7240533233; @dmudryk: staging https://property-workflows-git-staging-bravo16.vercel.app, сценарій не прогнано — прийнято без тестування, підпис https://github.com/dmudryk/property-workflows/pull/79#issuecomment-7240533597; @OleksandrMartynov: staging https://property-workflows-git-staging-flambs-projects.vercel.app, сценарій не прогнано — прийнято без тестування, підпис https://github.com/OleksandrMartynov/property-workflows/pull/123#issuecomment-7240533928; @vhrytsyndyshynquitcode, @ylehenkyi-qc, @vsmrnk: «реліз-кандидат на staging» не надходив — прийнято без тестування за рішенням PM (завдання передано із запізненням), 28.11
- Відхилення від вимог: відхилень немає
- Розбір postmortem і runbook: https://github.com/yuriivynar/property-workflows/pull/118#issuecomment-7229534519, https://github.com/dmudryk/property-workflows/pull/78#issuecomment-7240354345, https://github.com/OleksandrMartynov/property-workflows/pull/115#issuecomment-7240355011; @vhrytsyndyshynquitcode, @ylehenkyi-qc, @vsmrnk — postmortem не надходив, 28.11
- Повідомлення клієнту про інцидент: https://github.com/solomchaak/property-workflows-spec/blob/f287b4160a281167acd17a251c86961c353f35f5/docs/incident-message.md — за postmortem @yuriivynar

### Готовність до demo day і вартість (трек C)

- Чекліст demo day: https://github.com/solomchaak/property-workflows-spec/blob/f287b4160a281167acd17a251c86961c353f35f5/docs/demo-day.md
- Точки відмови з планом Б: основний прод не відкривається, база на паузі, вичерпано ліміт моделей, вхід не проходить через ліміт входів з однієї мережі, підрядник бачить зайве
- Записка про вартість: https://github.com/solomchaak/property-workflows-spec/blob/f287b4160a281167acd17a251c86961c353f35f5/docs/cost-note.md
- Цифри станом на: 09.11.2026 (Branching — 17.11.2026), за гайдом вартості курсу; перевірено для записки 28.11.2026

## Розробник

—

## Бонус

—
