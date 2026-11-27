# Здача WS11

## Дані

- Роль: pm
- Команда: school-purchasing
- GitHub-логін: ValiaBihun
- Ваше репо: https://github.com/ValiaBihun/2026-quitcode-dream-team
- SHA: b150c0921c01f6cce3e4bf8d767ed3154bcae725
- Репо вимог PM: https://github.com/ValiaBihun/2026-quitcode-dream-team
- Прод-URL: —
- Staging-URL: —

## PM

### Операційні вимоги (трек A)

- Сховище: issue «Операційні вимоги»
- Операційні вимоги: https://github.com/ValiaBihun/2026-quitcode-dream-team/issues/83
- SLO: SLO-1: застосунок відкривається, вхід працює, база не «заснула» — щоденний smoke проду зелений щонайменше 27 днів із 28 — міряємо щоденним прогоном smoke у GitHub Actions (`/`, `/login`, `/api/health`) — бюджет 1 червоний день за 28. SLO-2: AI-помічник пропонує рядки — 95% запитів «Suggest lines» за 28 днів без стану «недоступний» — міряємо за таблицею `ai_calls` (рядок на запит, без тексту опису, зберігається не менше 35 днів), доба за `Europe/Kyiv` — бюджет 5% запитів. SLO-3 (необов'язково): після рішення схвалювача автор бачить новий статус і запис в історії — кожен реліз за 28 днів вийшов із зеленим кроком UAT «рішення збережено» — міряємо підписом приймання в PR релізу — бюджет 0 релізів із червоним кроком
- Матриця серйозності: SEV1–3, по 2 приклади, хто реагує і як швидко — https://github.com/ValiaBihun/2026-quitcode-dream-team/issues/83 (розділ 2)
- RPO і RTO: RPO — не більше 1 доби змін, RTO — 4 години в робочий час; на безкоштовному тарифі автоматичних резервних копій немає, тож без власного дампа бази після збою відновлювати нічого, а база на паузі піднімається вручну («Resume project») за кілька хвилин
- Хто отримує алерт: Telegram-група команди: розробник і PM; клієнту за шаблоном `docs/incident-message.md` пише PM протягом 2 годин для SEV1 і SEV2
- Після курсу: пауза — продукт лишається на безкоштовних тарифах для показів, платні тарифи розглянемо, якщо клієнт вирішить запускати його в роботу

### Приймання релізу (трек B)

- Сценарій demo day для UAT: 6 кроків із 8 хв — гість бачить лише вхід; працівник подає закупівлю з двома рядками і бачить номер; AI-помічник пропонує рядки й відмовляє на сторонній запит; схвалювач частково схвалює заявку; працівник бачить «Partially approved» та історію; схвалювач повертає заявку на доопрацювання, працівник виправляє й подає знову — https://github.com/ValiaBihun/2026-quitcode-dream-team/blob/b150c0921c01f6cce3e4bf8d767ed3154bcae725/docs/demo-day.md
- UAT на staging: @YarynaPankivv: staging https://school-purchasing-git-staging-ypankiv-7566s-projects.vercel.app, сценарій ✅, підпис https://github.com/YarynaPankivv/school-purchasing/pull/112#issuecomment-7107279615 (перший підпис 22.11: https://github.com/YarynaPankivv/school-purchasing/pull/112#issuecomment-7079526517). @mvorobets-blip: staging https://school-purchasing-git-staging-mvorobets-blip.vercel.app, сценарій ✅, підпис https://github.com/mvorobets-blip/school-purchasing/pull/100#issuecomment-7079792436. @kdeineko-tech: не прийнято: реліз-кандидат на staging не надходив, домашнє не виконано, станом на 24.11
- Відхилення від вимог: відхилень немає
- Розбір postmortem і runbook: @YarynaPankivv: https://github.com/YarynaPankivv/school-purchasing/pull/110#issuecomment-7079560965 (правки за розбором: https://github.com/YarynaPankivv/school-purchasing/pull/114; після виправлень — «Розбір PM: прийнято» 27.11: https://github.com/YarynaPankivv/school-purchasing/pull/110#issuecomment-7214298280). @mvorobets-blip: https://github.com/mvorobets-blip/school-purchasing/pull/107#issuecomment-7107635893 (після виправлень — «Розбір PM: прийнято» 24.11: https://github.com/mvorobets-blip/school-purchasing/pull/107#issuecomment-7128100726). @kdeineko-tech: не прийнято: postmortem не надходив, домашнє не виконано, станом на 24.11
- Повідомлення клієнту про інцидент: https://github.com/ValiaBihun/2026-quitcode-dream-team/blob/b150c0921c01f6cce3e4bf8d767ed3154bcae725/docs/incident-message.md — за postmortem @YarynaPankivv

### Готовність до demo day і вартість (трек C)

- Чекліст demo day: https://github.com/ValiaBihun/2026-quitcode-dream-team/blob/b150c0921c01f6cce3e4bf8d767ed3154bcae725/docs/demo-day.md
- Точки відмови з планом Б: прод не відкривається, база на паузі, вичерпано ліміт моделей, вхід не проходить з кількох пристроїв, немає інтернету в залі
- Записка про вартість: https://github.com/ValiaBihun/2026-quitcode-dream-team/blob/b150c0921c01f6cce3e4bf8d767ed3154bcae725/docs/cost-note.md
- Цифри станом на: 09.11.2026 (staging-гілка Supabase — 17.11.2026)

## Розробник

—

## Бонус

### Розробник

—

### PM

- Що зроблено: нотатки релізу v1.0.0 для клієнта
- Посилання: https://github.com/ValiaBihun/2026-quitcode-dream-team/blob/b150c0921c01f6cce3e4bf8d767ed3154bcae725/docs/release-notes-v1.0.0.md
- Висновок: Нотатки написані для директора школи, а не для розробників: що вміє продукт за ролями (працівник, схвалювач, адміністратор, гість), чого в цьому випуску свідомо немає і що варто знати про роботу (пауза бази, денний ліміт AI-помічника, відсутність резервних копій, строк відповіді клієнту). У розділі про історію рішень сказано лише те, що є в продукті зараз: запис про подання, а рішення схвалювача — як статус із причиною; повної історії не обіцяно. Складаючи нотатки, я звірила їх з іншими документами репо вимог і знайшла розбіжності, які виправила: демо показує 6 кроків, а в операційних вимогах їх 8 (додала примітку в `docs/demo-day.md`); повідомлення клієнту посилалося на стару версію postmortem (оновила посилання на версію після розбору PM); ціну Sentry в записці про вартість уточнила до «$26 на місяць при оплаті за рік». Висновок: нотатки клієнту варто писати останніми, після звірки решти документів, і обіцяти в них лише те, що вже є в продукті. Назви екранів і повідомлень лишено англійською, як у продукті, щоб клієнт упізнавав їх на показі.
