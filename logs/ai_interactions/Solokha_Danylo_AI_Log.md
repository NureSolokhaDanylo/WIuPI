# AI Interaction Log — Учасник №1: Солоха Данило (Етапи G1 та G6)

Журнал синхронізовано зі зведеним журналом [`logs/AI_Interaction_Log.md`](../AI_Interaction_Log.md) і журналом дефектів [`logs/Defect_and_Change_Log.md`](../Defect_and_Change_Log.md).

---

## Сесія 1: Етап G1 — Формування UX/UI Design Input Pack

### Таблиця сесії G1

| Поле | Зафіксовані дані сесії |
| :--- | :--- |
| **Date / operator / step** | 07.11.2026 / **Солоха Данило** / Етап **G1 + Контроль A** (Запит 1.1 — первинна вибірка `v0.1`; Запит 1.2 — точкове доопрацювання до `v1.0`) |
| **Tool / plan / mode** | Chat UI • Модель: **Gemini 3.8 Flash** • Режим міркування: **Reasoning: Medium** |
| **Context / version** | Прикріплений файл вихідної специфікації [`source/requirements.md`](../../source/requirements.md) (`IT Helpdesk — Requirements Specification`, розділи `§0`–`§8`). Жодних сторонніх UI-прикладів не додано. |
| **Prompt / response** | Тексти запитів 1.1 та 1.2 наведено нижче.<br>• **Первинний результат ДО правок (`v0.1`):** [`docs/drafts/01_Input_Pack_v0.1.md`](../../docs/drafts/01_Input_Pack_v0.1.md)<br>• **Виправлений результат ПІСЛЯ Контролю A (`v1.0`):** [`docs/01_Input_Pack_v1.0.md`](../../docs/01_Input_Pack_v1.0.md) |
| **Decision / evidence** | **Перевірили (Контроль A):** Зінченко Нікіта та Солоха Данило (ручна посторінкова звірка з `requirements.md`).<br>• **Прийнято:** межі Release MVP (`US-01..US-08`, 37 AC), виключення `US-09` (Deferred) та `SR-10/11` (Future), правила статусів і пріоритетів (`Low, Medium, High, Critical`).<br>• **Відхилено / виправлено (див. [`Defect_and_Change_Log.md`](../Defect_and_Change_Log.md)):**<br>1) `DEF-G1-01`: у [`01_Input_Pack_v0.1.md`](../../docs/drafts/01_Input_Pack_v0.1.md) модель скоротила `BR-02.1` і пропустила право `IT Specialist` змінювати `Category` після створення заявки;<br>2) `DEF-G1-02`: не винесено в `Q-ID` розбіжність між `SR-08` (`IT-відділ`) та `US-08` (`user`) — додано як `Q-02` / `D-07`;<br>3) `DEF-G1-03`: розділено візуальний стан `403 Access Denied` та серверну авторизацію URL/API для `AC-02.2` і `AC-06.6`. |
| **Limit / fallback** | Використано 1 завантаження файлу (`requirements.md`). Лімітів не перевищено. |

### Запит 1.1 (Prompt G1, адаптований під IT Helpdesk)

```text
Ти — аналітик вимог. Створи UX/UI Design Input Pack виключно з прикріпленої актуальної специфікації IT Helpdesk (requirements.md). Екрани ще не проєктуй.
Спочатку назви прочитаний файл та підтвердь, які його розділи доступні. Якщо матеріал неповний, не роби вигляд, що прочитано все.
Визнач Release MVP за остаточним Release Scope (§8), а не за Candidate MVP (§4). Не трактуй пізніший розділ як автоматичне скасування бізнес-правил: суперечності або прогалини винеси окремо.

Сформуй:
1. Product purpose — до 3 речень; primary actors (Employee, IT Specialist), secondary stakeholders та межі першого релізу.
2. Included US; Deferred (US-09); Future (SR-10, SR-11); explicitly out of scope.
3. Для кожної включеної US (US-01..US-08) — ID, actor, мета, усі AC зі збереженням змісту та вихідних ідентифікаторів (разом має бути 37 AC), а також важливі деталі з Gherkin-сценаріїв.
4. UI-релевантні BR (BR-01..BR-12): права ролей Employee vs IT Specialist, обов'язкове поле Category (Hardware, Software, Access), початкові значення (Open, Not Set), дозволені переходи статусів (зверни увагу на заборону Resolved -> In Progress), правило Important Tickets, коментарі, заборонені дії (видалення, історія змін).
5. Relevant NFR (NFR-01..NFR-04); окремо вкажи вимоги, які не перевіряються статичним макетом.
6. Q-ID: конфлікти, прогалини та питання, що потребують рішення людини (Decision Log).

Для кожного твердження вкажи Source: розділ + US/AC/BR або точний пункт. Познач тип: SOURCE / DERIVED / PROPOSED / UNRESOLVED.
Збережи всі негативні умови: ізоляція заявок Employee (бачить і коментує лише власні), відсутні права на зміну статусу для Employee та непризначеного IT Specialist, заборона переходу Resolved -> In Progress, помилка збереження коментаря, виключення Closed із Important Tickets. Розрізняй Not Set як відсутність priority і встановлений рівень priority (Low, Medium, High, Critical). Не прирівнюй Resolved до Closed.
Закінчи Source Check: кожна included US -> усі її AC -> місце у Pack; перелік excluded вимог із причиною; список unresolved питань. Не оголошуй Pack погодженим.
```

### Запит 1.2 (Уточнювальний промпт після Контролю A для отримання `01_Input_Pack_v1.0.md`)

```text
Під час ручного Контролю A у чернетці 01_Input_Pack_v0.1.md виявлено 3 неточності порівняно з requirements.md:
1) У BR-02.1 пропущено друге речення: "IT Specialist може змінити категорію після створення заявки". Додай це правило до BR-02.1 і винеси питання Q-01 щодо розміщення контрола зміни Category на екрані деталей.
2) У SR-08 вказано "IT-відділ", а в US-08 — "As a user", при цьому AC-02.1 обмежує Employee лише власними заявками. Зафіксуй це як Q-02.
3) Для AC-02.2 та AC-06.6 (заборона прямого переходу за URL та прямого запиту на коментування чужої заявки) явно розділи візуальний стан відмови доступу (403 Access Denied) і серверну авторизацію, яка не доводиться статичним макетом.
Онови документ і вбудуй погоджені командою рішення D-01..D-08 як версію 01_Input_Pack_v1.0 APPROVED.
```

---

## Сесія 2: Етап G6 — Фінальний аудит доказів макета за скріншотами

### Таблиця сесії G6

| Поле | Зафіксовані дані сесії |
| :--- | :--- |
| **Date / operator / step** | 07.11.2026 / **Солоха Данило** / Етап **G6 + Контроль D** (Фінальний аудит доказів після ручних правок у Figma) |
| **Tool / plan / mode** | Chat UI • Модель: **4 Argon** • Режим міркування: **Reasoning: High** |
| **Context / version** | Текст [`docs/03_Screen_Contracts_and_Traceability_v1.0.md`](../../docs/03_Screen_Contracts_and_Traceability_v1.0.md) + експортовані з Figma PNG-скріншоти фінального макета з папки [`screenshots/after_review/`](../../screenshots/after_review/) (`S-01..S-03`, `M-01`, `REF-01`). |
| **Prompt / response** | Текст запиту 2.1 наведено нижче.<br>• **Скріншоти ДО правок:** [`screenshots/before_review/`](../../screenshots/before_review/)<br>• **Скріншоти ПІСЛЯ правок:** [`screenshots/after_review/`](../../screenshots/after_review/)<br>• **Збережений звіт аудиту G6:** [`docs/06_Figma_Prompts_and_Review_Report_v1.0.md`](../../docs/06_Figma_Prompts_and_Review_Report_v1.0.md) (Частина 2) |
| **Decision / evidence** | **Перевірили (Контроль D):** Солоха Данило, Долгополов Ярослав, Зінченко Нікіта.<br>• **Прийнято:** статус `PASS` для 35 візуально перевірюваних AC, статус `PARTIAL` для `AC-02.2` та `AC-06.6` (у [`REF-01 — Reviewer & State Reference.png`](../../screenshots/after_review/REF-01%20—%20Reviewer%20&%20State%20Reference.png) присутня картка `403 Access Denied`, але блокування прямого URL потребує серверного тесту), статус `NOT VERIFIABLE` для `NFR-01..04`.<br>• **Підтверджено усунення дефектів макета:** `DEF-FIG-01` (текст у картці `Failed to save comment`), `DEF-FIG-02` (шапка `Olena Koval` у [`M-01 — Create Ticket Modal Overlay.png`](../../screenshots/after_review/M-01%20—%20Create%20Ticket%20Modal%20Overlay.png)), `DEF-FIG-03` (підпис `Local Component Masters` унизу `REF-01`).<br>• **Рішення щодо UX-спостережень:** `UX-01` залишено за дизайном брифу, `UX-02` зафіксовано як примітку в `REF-01`. |
| **Limit / fallback** | Використано 2 завантаження PNG-зображень (сумарно 3/3 за добу в межах ліміту). |

### Запит 2.1 (Prompt G6 — Аудит доказів макета IT Helpdesk)

```text
Перевір фінальний макет IT Helpdesk за погодженими вимогами (03_Screen_Contracts_and_Traceability_v1.0.md) і прикріпленими PNG-скріншотами з папки screenshots/after_review/ (S-01..S-03, M-01 та REF-01). Не вважай текстовий звіт Figma Agent доказом виконаної правки — оцінюй лише те, що реально видно на зображеннях.
За умовами проєкту (D-08) інтерактивний клікабельний прототип не створювався, переходи анотовані в REF-01.

Спочатку переліч видимі Screen/State IDs на скріншотах і підтвердь, чи чітко читається текст.
Частина A — вимоги. Для кожного з 37 included AC (US-01..US-08) та UI-релевантних BR/NFR заповни таблицю:
Source | Expected | Screen/State | Observed evidence | Result (PASS / PARTIAL / FAIL / NOT VERIFIABLE) | Remaining implementation test.
Окремо перевір:
- чи є обов'язковий селект Category (Hardware, Software, Access) у M-01 та редагований Category для IT Specialist у S-03;
- чи прибрано Not Set із випадаючих опцій селекта Priority (залишено лише Low, Medium, High, Critical);
- чи контрол Status на S-03 є візуально disabled для непризначеного IT Specialist (Ігор Мельник на заявці TCK-003 Анни Савченко) та read-only для Employee;
- чи у стані Resolved дозволено лише перехід у Closed (без Resolved -> In Progress згідно з AC-07.6);
- чи у S-02-B (Important Tickets) присутні TCK-001, TCK-002, TCK-003 (High+Resolved), TCK-007 (Critical+Resolved) і відсутня TCK-004 (Closed+Critical);
- чи показана ізоляція списку Employee у S-02-D та картка 403 Access Denied у REF-01.
Статичний disabled control або картка 403 не доводять серверну авторизацію; намальований коментар не доводить збереження в БД. NFR-01..04 не позначай PASS за картинкою.

Частина B — дай конкретні UX-спостереження щодо читабельності, підписів і запобігання помилкам.
Частина C — підсумковий висновок щодо готовності дизайну та перелік тестів, що переходять на етап програмування.
```
