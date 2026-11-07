# AI Interaction Log — Учасник №2: Долгополов Ярослав (Етапи G2, G3, G4)

Журнал синхронізовано зі зведеним журналом [`logs/AI_Interaction_Log.md`](../AI_Interaction_Log.md), журналом рішень [`logs/Decision_Log.md`](../Decision_Log.md) і журналом дефектів [`logs/Defect_and_Change_Log.md`](../Defect_and_Change_Log.md).

---

## Сесія 1: Етап G2 — Побудова системи екранів (Screen Map)

### Таблиця сесії G2

| Поле | Зафіксовані дані сесії |
| :--- | :--- |
| **Date / operator / step** | 07.11.2026 / **Долгополов Ярослав** / Етап **G2** (Побудова карти екранів та таблиці переходів) |
| **Tool / plan / mode** | Chat UI • Модель: **Gemini 3.8 Flash** • Режим міркування: **Reasoning: High** |
| **Context / version** | Вставлений текстом [`docs/01_Input_Pack_v1.0.md`](../../docs/01_Input_Pack_v1.0.md) + погоджений [`logs/Decision_Log.md`](../Decision_Log.md) (`D-01..D-08`). |
| **Prompt / response** | Текст запиту 1.1 наведено нижче.<br>• **Первинна чернетка ДО рев'ю (`v0.1`):** [`docs/drafts/02_Screen_Map_v0.1.md`](../../docs/drafts/02_Screen_Map_v0.1.md)<br>• **Виправлений документ ПІСЛЯ рев'ю (`v1.0`):** [`docs/02_Decision_Log_and_Screen_Map_v1.0.md`](../../docs/02_Decision_Log_and_Screen_Map_v1.0.md) |
| **Decision / evidence** | **Перевірили:** Долгополов Ярослав та Солоха Данило (під час G4 / Контролю B).<br>• **Прийнято:** структуру з 3 продуктових екранів (`S-01`, `S-02`, `S-03`), модального вікна `M-01` та позапродуктової секції `REF-01`, об'єднання всіх дій керування заявкою на `S-03`.<br>• **Відхилено / виправлено (див. [`Defect_and_Change_Log.md`](../Defect_and_Change_Log.md)):**<br>1) `DEF-PLAN-01`: у чернетці [`02_Screen_Map_v0.1.md`](../../docs/drafts/02_Screen_Map_v0.1.md) модель додала заборонений перехід `T-11b: Resolved → In Progress (Reopen)`, що порушувало `AC-07.6` — вилучено у `v1.0`;<br>2) `DEF-PLAN-03`: у `v0.1` були лише стани `S-02-A/B/C` — у `v1.0` додано стан `S-02-D (My Tickets — Employee Isolated View)`. |
| **Limit / fallback** | Текстовий запит, 0 файлових завантажень. Лімітів не перевищено. |

### Запит 1.1 (Prompt G2, адаптований під IT Helpdesk)

```text
Ти — UX architect. Використовуй погоджений Input Pack v1.0 для системи IT Helpdesk та наведений нижче Decision Log (D-01..D-08). Не відновлюй Deferred (US-09 Attachments) та Future (SR-10/SR-11 AI) функції.
Побудуй мінімальну систему екранів для Release MVP (US-01..US-08). Одна User Story не означає окремий екран: керування Category, Assignee, Priority, Status та Comments розміщується у спільних деталях заявки (S-03).

Для кожної поверхні/стану запиши:
Screen/State ID | Name | Purpose | Actors | Entry | Visible data | Inputs | Actions | Role/state conditions | Errors/empty states | Exit | Source/D-ID.
Використай канонічну структуру:
- S-01 Authentication boundary;
- S-02 Tickets List зі станами списку (All Tickets, Important Tickets, Empty);
- M-01 Create Ticket Modal (поля: Subject, Category, Description);
- S-03 Ticket Details;
- REF-01 Reviewer & State Reference поза продуктовими фреймами.

Сформуй таблицю переходів:
T-ID | From | Actor | Trigger | Preconditions | To | Visible result | Requirement/D-ID.
Дай текстові user flows для основних сценаріїв та приклади того, що НЕ слід додавати в макет IT Helpdesk. Врахуй D-08 (переходи анотуються без налаштування інтерактивного прототипу у Figma).
```

---

## Сесія 2: Етап G3 — Формалізація Screen Contracts та Traceability (37 AC)

### Таблиця сесії G3

| Поле | Зафіксовані дані сесії |
| :--- | :--- |
| **Date / operator / step** | 07.11.2026 / **Долгополов Ярослав** / Етап **G3** (Формалізація контрактів екранів, матриць прав/статусів, Traceability та тестів) |
| **Tool / plan / mode** | Chat UI • Модель: **4 Argon** • Режим міркування: **Reasoning: High** |
| **Context / version** | Текст [`docs/01_Input_Pack_v1.0.md`](../../docs/01_Input_Pack_v1.0.md) + [`logs/Decision_Log.md`](../Decision_Log.md) (`D-01..D-08`) + чернетка карти екранів [`docs/drafts/02_Screen_Map_v0.1.md`](../../docs/drafts/02_Screen_Map_v0.1.md). |
| **Prompt / response** | Текст запиту 2.1 наведено нижче.<br>• **Первинна чернетка ДО рев'ю (`v0.1`):** [`docs/drafts/03_Screen_Contracts_v0.1.md`](../../docs/drafts/03_Screen_Contracts_v0.1.md)<br>• **Виправлений документ ПІСЛЯ рев'ю (`v1.0`):** [`docs/03_Screen_Contracts_and_Traceability_v1.0.md`](../../docs/03_Screen_Contracts_and_Traceability_v1.0.md) |
| **Decision / evidence** | **Перевірили:** Долгополов Ярослав та Солоха Данило.<br>• **Прийнято:** матрицю прав доступу для 4 рольових контекстів, формальний предикат `Important Tickets`, тестовий набір `TCK-001..TCK-008`, матрицю простежуваності на 37 AC та тест-кейси `T-01..T-10`.<br>• **Відхилено / виправлено у `v1.0`:**<br>1) `DEF-PLAN-01`: із матриці переходів статусів у [`03_Screen_Contracts_v0.1.md`](../../docs/drafts/03_Screen_Contracts_v0.1.md) видалено заборонений перехід `Resolved → In Progress (Reopen)`;<br>2) `DEF-PLAN-02`: у контракт контрола `C-06 (Category *)` та тест `T-02` додано пропущений у `v0.1` стан валідації `"Category is required"` з Gherkin `US-01`;<br>3) `DEF-PLAN-03`: додано опис `S-02-D` та картки `403 Access Denied`. |
| **Limit / fallback** | Текстовий запит, 0 файлових завантажень. Лімітів не перевищено. |

### Запит 2.1 (Prompt G3, адаптований під IT Helpdesk)

```text
Підготуй Developer-Verifiable Screen Contracts для системи IT Helpdesk за погодженим планом. Не додавай нових продуктових вимог.
Для кожного screen/state запиши:
1. ID, призначення, actors, entry conditions.
2. Visible data: назва поля, обов'язковість, вигляд відсутнього значення (Not Set для Priority, Unassigned для Assignee).
3. Controls: C-ID (C-01..C-20), label, type, visible/enabled conditions для Employee vs IT Specialist (Non-assignee) vs IT Specialist (Current Assignee), trigger, результат, Source/D-ID.
4. Default, empty, validation та error states.

Окремо сформуй:
- Permission matrix для 4 контекстів: Employee (Author of own ticket); Employee (Other employee's ticket — blocked); IT Specialist (Non-assignee); IT Specialist (Current Assignee). Дії: view in list, open details, create ticket, comment, change category (BR-02.1), assign IT Specialist, change priority, change status.
- Повну матрицю status transitions між Open, In Progress, Resolved, Closed.
- Точний предикат Important Tickets: Priority IN {High, Critical} AND Status != Closed, та перевірочну таблицю на 8 тестових заявках TCK-001..TCK-008.
- Таблицю Traceability для всіх 37 AC (US-01..US-08): Source ID | UIR-ID | Screen/State | C-ID / Annotation | Test ID | Verification level.
- Опис 10 перевірочних тестів (T-01..T-10) із зазначенням того, що не можна довести статичним макетом.
```

---

## Сесія 3: Етап G4 — Критичне рев'ю плану перед Figma

### Таблиця сесії G4

| Поле | Зафіксовані дані сесії |
| :--- | :--- |
| **Date / operator / step** | 07.11.2026 / **Долгополов Ярослав** (Оператор G4) та **Солоха Данило** (Рецензент Контролю B) / Етап **G4 + Контроль B** |
| **Tool / plan / mode** | Chat UI • Модель: **4 Argon** • Режим міркування: **Reasoning: High** |
| **Context / version** | Повний текст [`source/requirements.md`](../../source/requirements.md) + чернетки [`docs/drafts/02_Screen_Map_v0.1.md`](../../docs/drafts/02_Screen_Map_v0.1.md) та [`docs/drafts/03_Screen_Contracts_v0.1.md`](../../docs/drafts/03_Screen_Contracts_v0.1.md). |
| **Prompt / response** | Текст запиту 3.1 наведено нижче.<br>• **Збережений звіт рев'ю G4 та рішення Контролю B:** [`docs/04_Plan_Review_v1.0.md`](../../docs/04_Plan_Review_v1.0.md) |
| **Decision / evidence** | **Перевірили (Контроль B):** Солоха Данило та Долгополов Ярослав.<br>• **Прийнято:** усі 3 знайдені зауваження (`DEF-PLAN-01`, `DEF-PLAN-02`, `DEF-PLAN-03`) внесено до фінальних документів [`docs/02_Decision_Log_and_Screen_Map_v1.0.md`](../../docs/02_Decision_Log_and_Screen_Map_v1.0.md) та [`docs/03_Screen_Contracts_and_Traceability_v1.0.md`](../../docs/03_Screen_Contracts_and_Traceability_v1.0.md).<br>• **Вердикт Контролю B:** `PLAN READY` (0 відкритих блокерів, дозволено перехід до G5). |
| **Limit / fallback** | Текстовий запит, 0 файлових завантажень. Лімітів не перевищено. |

### Запит 3.1 (Prompt G4 — Критичний рецензент плану)

```text
Перевір план UX/UI для IT Helpdesk як критичний рецензент. Не захищай попередні відповіді. Зістав вихідну специфікацію requirements.md, погоджені D-рішення (D-01..D-08), Input Pack v1.0, Screen Map v0.1 і Screen Contracts v0.1.
Пройди кожну included US (US-01..US-08) і кожен із 37 AC. Шукай: пропуски; зайві функції; неправильні права; незаконні status transitions (особливо перевір AC-07.6 щодо заборони Resolved -> In Progress); неправильне трактування Resolved або Not Set; пропущені стани валідації з Gherkin-сценаріїв (зокрема "Category is required" та "Subject is required"); перевір ізоляцію заявок Employee (AC-02.1, AC-02.2, AC-06.6).

Вихід 1 — матриця coverage для всіх 37 AC: COVERED / MISSING / AMBIGUOUS із доказом у плані.
Вихід 2 — DEF-ID | severity | source | plan location | проблема | мінімальне виправлення.
Вихід 3 — чи є блокувальні Q-ID перед генерацією Figma.
Вихід 4 — рекомендація PLAN READY / NOT READY.
```
