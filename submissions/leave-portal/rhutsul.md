# Здача WS6

## Дані

- Роль: dev
- Команда: leave-portal
- GitHub-логін: rhutsul
- Ваше репо: https://github.com/rhutsul/leave-portal
- SHA: 5acf33d254c27703db2196792aa707cf36a8deb6
- Репо вимог PM: https://github.com/mkozlovskyi-ops/course-staff-management-spec
- Прод-URL: https://leave-portal-olive.vercel.app

## PM

—

## Розробник

- Рядок «Вимоги:» з `docs/conformance.md` на SHA: Вимоги: https://github.com/mkozlovskyi-ops/course-staff-management-spec · джерело: docs · знімок 2026-10-21
- Перший PR «Оновлення вимог»: https://github.com/rhutsul/leave-portal/pull/3
- Журнал `docs/spec-reading.md`: https://github.com/rhutsul/leave-portal/blob/5acf33d254c27703db2196792aa707cf36a8deb6/docs/spec-reading.md
- Питання в репо PM (щонайменше 3, з них щонайменше 1 — власне issue): https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/1, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/2, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/3, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/4, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/8
- PR «Специфікація» з перекладом першого зрізу: https://github.com/rhutsul/leave-portal/pull/4 — зміна `add-leave-ws6`: кожен AC PM — сценарій з тим самим ID, archive у гілці, рядки матриці «відкладено (чекає реалізації)»
- Зміна `impl-<зріз>`: https://github.com/rhutsul/leave-portal/tree/5acf33d254c27703db2196792aa707cf36a8deb6/openspec/changes/archive/2026-10-21-impl-leave-ws6 (план — PR https://github.com/rhutsul/leave-portal/pull/5, оновлення плану — PR https://github.com/rhutsul/leave-portal/pull/7 і https://github.com/rhutsul/leave-portal/pull/13, archive — в останньому PR задачі https://github.com/rhutsul/leave-portal/pull/17)
- PR-и задач «Реалізація» (щонайменше 2): https://github.com/rhutsul/leave-portal/pull/9, https://github.com/rhutsul/leave-portal/pull/10, https://github.com/rhutsul/leave-portal/pull/11, https://github.com/rhutsul/leave-portal/pull/14, https://github.com/rhutsul/leave-portal/pull/16, https://github.com/rhutsul/leave-portal/pull/17 (запис екрана з проду — https://github.com/rhutsul/leave-portal/pull/17#issuecomment-6325012023)
- PR «Оновлення вимог» після уточнень PM: https://github.com/rhutsul/leave-portal/pull/6 (знімок 2026-10-20, відповіді на #1–#7), https://github.com/rhutsul/leave-portal/pull/8 (відповідь на #8), https://github.com/rhutsul/leave-portal/pull/12 (знімок 2026-10-21, відповіді на #9–#11), https://github.com/rhutsul/leave-portal/pull/15 (відповідь на #12)
- PR «Специфікація» зі змінами після уточнень: https://github.com/rhutsul/leave-portal/pull/7 (`clarify-leave-ws6`: ADDED 11 AC, MODIFIED), https://github.com/rhutsul/leave-portal/pull/13 (`clarify-leave-ws6-2`: ADDED AC-LEAVE-21, AC-DECIDE-23, AC-DECIDE-24, AC-BALANCE-21, AC-BALANCE-22, MODIFIED)
- AC, які ви реалізували: усі 67 AC зрізу — «реалізовано» в `docs/conformance.md` на SHA: AC-LEAVE-01…AC-LEAVE-21 (21), AC-DECIDE-01…AC-DECIDE-24 (24), AC-BALANCE-01…AC-BALANCE-22 (22); відкладених немає. Нові AC після уточнень PM (AC-LEAVE-17…21, AC-DECIDE-20…24, AC-BALANCE-17…22) — теж «реалізовано».

## Бонус E

`/opsx:verify` для зміни `impl-leave-ws6` (архів `openspec/changes/archive/2026-10-21-impl-leave-ws6`) проти коду на SHA.
Підсумок скіла: 15/15 задач; Spec Coverage, Requirement Mapping і Scenario Coverage — «не застосовно» (план з
`skip_specs: true`, дельт немає); покриття 67 сценаріїв `openspec/specs` підтверджує `npm run ac:coverage`
(67/67 «реалізовано», тест з AC ID на кожен AC); D1–D4, D6, D7 дотримано; 0 critical, 3 warning, 3 suggestion.

Розбір знахідок:

- **D5 «`access.ts`» не створено** — справжнє відхилення від плану, але свідоме: правило «хто бачить чиї записи й
  залишки» тримає база (view `my_leave_records`, функція `can_view_balances`), бо `security definer`-функція мусить
  перевіряти право сама, а копія в TS тестувала б копію. Задокументовано в
  [ADR 0002](https://github.com/rhutsul/leave-portal/blob/5acf33d254c27703db2196792aa707cf36a8deb6/docs/decisions/0002-my-leave-records-view.md)
  і [ADR 0003](https://github.com/rhutsul/leave-portal/blob/5acf33d254c27703db2196792aa707cf36a8deb6/docs/decisions/0003-balance-access-in-database.md);
  чого бракує — посилання на ці ADR у самому `design.md`. Висновок: verify ловить розбіжність план ↔ код, але не бачить
  ADR, тож рішення поза планом варто відбивати й у плані (окремим PR «Специфікація»).
- **Попередження працівнику не покрите тестом рівня server action** — справжня прогалина: тести AC-BALANCE-08/15/17
  перевіряють розрахунок і функцію бази, а не те, що action передає попередження на сторінку. Поведінку перевірено
  вручну в браузері; тест `submitLeaveRequest` (FormData → redirect) — follow-up, зафіксовано в треді PR #17.
- **`grant update (full_name)` для `authenticated`** — розбіжність, яку дала зміна вимог PM після реалізації (#12:
  ім'я змінює лише адміністратор), а не помилка зрізу: керування профілями — WS10, там і закрити `revoke`.
- Suggestions (подвійна формула частки дня в SQL і TS; обрізана колонка дій у My requests; D7 без AC ID) — прийнято
  як технічний борг: подвійна формула свідома (D4 — база сама рахує розподіл під блокуванням), її захищають тести з
  однаковими числами з AC в обох шарах.

Що з цього випливає: `ac:coverage` доводить, що в кожного AC є тест, а verify додає перевірку плану — і саме він показав
два місця, де реалізація пішла далі за `design.md`, і одну розбіжність, яку створила пізня зміна вимог.
