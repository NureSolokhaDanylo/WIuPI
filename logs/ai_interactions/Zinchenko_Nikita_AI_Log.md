# AI Interaction Log — Учасник №3: Зінченко Нікіта (Етапи G5, F1 та Ручний F2)

Журнал синхронізовано зі зведеним журналом [`logs/AI_Interaction_Log.md`](../AI_Interaction_Log.md) і журналом дефектів [`logs/Defect_and_Change_Log.md`](../Defect_and_Change_Log.md).

---

## Сесія 1: Етап G5 — Компіляція самодостатнього контракту `05_Figma_Brief_v1.0.md`

### Таблиця сесії G5

| Поле | Зафіксовані дані сесії |
| :--- | :--- |
| **Date / operator / step** | 07.11.2026 / **Зінченко Нікіта** (Оператор G5) та **Долгополов Ярослав** (Рецензент Контролю C) / Етап **G5 + Контроль C** |
| **Tool / plan / mode** | Chat UI • Модель: **Gemini 3.8 Flash** • Режим міркування: **Reasoning: Medium** |
| **Context / version** | Затверджені документи [`docs/02_Decision_Log_and_Screen_Map_v1.0.md`](../../docs/02_Decision_Log_and_Screen_Map_v1.0.md) та [`docs/03_Screen_Contracts_and_Traceability_v1.0.md`](../../docs/03_Screen_Contracts_and_Traceability_v1.0.md) (після Контролю B). |
| **Prompt / response** | Текст запиту 1.1 наведено нижче.<br>• **Первинна чернетка брифу ДО Контролю C (`v0.9`):** [`docs/drafts/05_Figma_Brief_v0.9.md`](../../docs/drafts/05_Figma_Brief_v0.9.md)<br>• **Виправлений фінальний бриф ПІСЛЯ Контролю C (`v1.0`):** [`docs/05_Figma_Brief_v1.0.md`](../../docs/05_Figma_Brief_v1.0.md) |
| **Decision / evidence** | **Перевірили (Контроль C):** Долгополов Ярослав та Зінченко Нікіта у текстовому редакторі.<br>• **Прийнято:** структуру та повноту англомовного контракту для Figma Agent.<br>• **Відхилено / виправлено вручну в `.md`-файлі (див. [`Defect_and_Change_Log.md`](../Defect_and_Change_Log.md)):**<br>1) `DEF-BRIEF-01`: у [`05_Figma_Brief_v0.9.md`](../../docs/drafts/05_Figma_Brief_v0.9.md) у рядку `TCK-005` виправлено випадкове слово `Normal` на `Medium` (згідно з `BR-05` та `AC-04.1`);<br>2) `DEF-BRIEF-02`: вилучено `Not Set` із дужок біля `Priority dropdown` у описі `S-03` та додано суворе обмеження `Low, Medium, High, Critical ONLY`. |
| **Limit / fallback** | Текстовий запит, 0 файлових завантажень. Виправлення `v0.9 → v1.0` виконано вручну у текстовому редакторі без витрати додаткового AI-запиту. |

### Запит 1.1 (Prompt G5 — Компіляція англомовного Figma Brief для IT Helpdesk)

```text
Скомпілюй один self-contained англомовний Figma Design implementation brief (05_Figma_Brief_v1.0.md) із погоджених Screen Contracts для системи IT Helpdesk. Це не новий UX-дизайн і не переказ усієї історії обговорення.
Обов'язкові секції:
1) Purpose and scope (US-01..US-08, 4 тестові користувачі: Employee Olena Koval, Employee Maksym Bondar, IT Specialist Ihor Melnyk, IT Specialist Anna Savchenko);
2) Design constraints (1440px desktop, grayscale low-fidelity, editable Figma-native layers, Auto Layout, reusable components, REF-01 outside product frames, no decorative page icon placeholders or bulk checkboxes);
3) Canonical screens/states with exact IDs (S-01, S-02-A All Tickets IT View, S-02-B Important Tickets, S-02-C Empty, S-02-D My Tickets Employee View, M-01 Create Ticket Modal, S-03 Ticket Details for TCK-003);
4) Canonical dataset TCK-001..TCK-008;
5) M-01 inputs (Subject, Category: Hardware/Software/Access, Description) and exact validation messages ("Subject is required", "Category is required", "Description is required");
6) S-03 controls, role permissions (IT Specialist can change Category, Assignee, Priority; ONLY current assigned IT Specialist can change Status; Employee sees read-only values and only their own tickets), Priority rules (Not Set is display-only badge, selectable options are Low, Medium, High, Critical), Status rules (ONLY 3 transitions: Open->In Progress, In Progress->Resolved, Resolved->Closed; Resolved->In Progress is strictly PROHIBITED per AC-07.6);
7) REF-01 state reference (4 role contexts including 403 Access Denied for Employee on another user's ticket, 4 status states, comment validation and save failure states);
8) Navigation intent (annotated only per D-08 — do not wire interactive prototype connections);
9) Explicit exclusions (no attachments US-09, no AI hints SR-10/11, no delete, no history);
10) Completion report requirements.
Виведи лише чистий текст Markdown.
```

---

## Сесія 2: Етап F1 — Основна генерація у Figma Agent та Ручний маршрут F2 після ліміту

### Таблиця сесії F1 / Ручного F2

| Поле | Зафіксовані дані сесії |
| :--- | :--- |
| **Date / operator / step** | 07.11.2026 / **Зінченко Нікіта** / Етап **F1 → Ручний F2-огляд та правки після ліміту** |
| **Tool / plan / mode** | Figma Design • **Figma Agent (Starter Plan, Beta)** |
| **Context / version** | Прикріплений файл [`docs/05_Figma_Brief_v1.0.md`](../../docs/05_Figma_Brief_v1.0.md) + короткий Launcher F1 (1140 символів). |
| **Prompt / response** | Текст лаунчера F1 та дослівна відповідь Figma Agent наведені нижче.<br>• **Збережений файл `.fig` ДО правок:** [`IT Helpdesk - UX_UI MVP.fig`](../../IT%20Helpdesk%20-%20UX_UI%20MVP.fig)<br>• **Скріншоти ДО правок:** [`screenshots/before_review/`](../../screenshots/before_review/)<br>• **Збережений файл `.fig` ПІСЛЯ правок:** [`IT Helpdesk - UX_UI MVP Edited.fig`](../../IT%20Helpdesk%20-%20UX_UI%20MVP%20Edited.fig)<br>• **Скріншоти ПІСЛЯ правок:** [`screenshots/after_review/`](../../screenshots/after_review/) |
| **Decision / evidence** | **Перевірили (F2-чекліст):** Зінченко Нікіта та Долгополов Ярослав.<br>• **Прийнято:** усі 7 продуктових фреймів (`S-01`, `S-02-A..D`, `M-01`, `S-03`), референс `REF-01`, 28 master-компонентів та 180 інстансів. Усі бізнес-правила специфікації виконано з першої спроби.<br>• **Виявлено та виправлено вручну (`MAN-CHG-01..03` / `DEF-FIG-01..03`):** див. таблицю ручного Change Report нижче. |
| **Limit / fallback** | Одразу після генерації `F1` з'явилося повідомлення про вичерпання денного ліміту кредитів Starter Plan. Перевірку за чек-листом `F2`, точкові правки в шарах Figma та `Change Report` виконано вручну. |

### Запит 2.1 (F1 Launcher, відправлений у Figma Agent)

```text
Use the attached 05_Figma_Brief_v1.0.md as the sole approved design contract for IT Helpdesk. Read the complete file before creating any layers.

Create editable low-fidelity grayscale desktop wireframes (1440px) on this single page using native Figma layers, Auto Layout, and reusable local components (Button, Input/Select, Badges, Ticket Row). Keep exact frame IDs: S-01, S-02-A (All Tickets - IT View), S-02-B (Important Tickets), S-02-C (Empty), S-02-D (My Tickets - Employee Olena Koval View), M-01 (Create Ticket Modal), S-03 (Ticket Details for TCK-003), and REF-01 outside product frames.

Strictly enforce IT Helpdesk rules:
1) Mandatory Category (Hardware, Software, Access) in M-01 and editable by IT Specialist on S-03;
2) Priority levels: Not Set (display badge only) and selectable Low, Medium, High, Critical;
3) Only 3 status transitions (Open->In Progress, In Progress->Resolved, Resolved->Closed); Resolved->In Progress is PROHIBITED;
4) Only the assigned IT Specialist can change Status; Employee sees read-only attributes and only their own tickets;
5) Important Tickets = High/Critical AND Status != Closed (keep Resolved TCK-003 and TCK-007 included).

Do not add decorative page icons, empty square placeholders, checkboxes, attachments (US-09), AI hints, delete buttons, or interactive prototype wires. In this same response, output a completion report listing created frames, reusable components, and unverified backend rules.
```

### Дослівна відповідь Figma Agent після генерації F1

```text
Created editable grayscale wireframes at 1440px on this page, using native layers and Auto Layout.

Created frames
S-01, S-02-A, S-02-B, S-02-C, S-02-D, M-01, S-03, and REF-01, outside product frames, containing four permission contexts, four lifecycle states, unassigned restrictions, validation/errors, option inventories, creation defaults, and navigation annotations.

Reusable components
Created 28 local component masters and 180 linked instances across six families: Button, Input/Select, Status Badge, Priority Badge, Category Badge, and Ticket Row.

Coverage and limitations
All requested visual states are represented. Important Tickets includes resolved TCK-003 and TCK-007; Olena’s list contains only her four tickets. No prototype connections were added.

These are static wireframes—not backend enforcement. They do not verify database persistence, server-side ticket/comment isolation (AC-02.2/AC-06.6), assignment permissions, lifecycle enforcement, filtering logic, authentication sessions (NFR-02), response time (NFR-01), HTTPS (NFR-03), or backups (NFR-04).
```

---

## Сесія 3: Таблиця ручних правок за чек-листом F2 (Ручний Change Report після ліміту F1)

| CHANGE-ID / DEF-ID | Frame / Layer у Figma | Скріншот ДО правок (`before_review`) | Що зроблено вручну (Оператор: Зінченко Нікіта) | Скріншот ПІСЛЯ правок (`after_review`) | Пов'язана вимога |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`MAN-CHG-01`**<br>(`DEF-FIG-01`) | `REF-01 / Validation & error state cards / S-03 — Failed to save comment / Comment text` | [`before_review/REF-01 — Reviewer & State Reference.png`](../../screenshots/before_review/REF-01%20—%20Reviewer%20&%20State%20Reference.png) (текст `I / c / a / n / n` розбитий вертикально) | Змінено горизонтальний ресайзинг текстового шару всередині інпута з `Fixed width` на `Fill container`, очищено зламаний текст. | [`after_review/REF-01 — Reviewer & State Reference.png`](../../screenshots/after_review/REF-01%20—%20Reviewer%20&%20State%20Reference.png) | `AC-06.5` (Читабельність стану помилки) |
| **`MAN-CHG-02`**<br>(`DEF-FIG-02`) | `M-01 — Create Ticket Modal Overlay / Background Header / User Badge` | [`before_review/M-01 — Create Ticket Modal Overlay.png`](../../screenshots/before_review/M-01%20—%20Create%20Ticket%20Modal%20Overlay.png) (у шапці `Ihor Melnyk (IT Specialist)` та `All Tickets`) | Замінено текст бейджа на фоновому шарі `M-01` на `"Logged in as: Olena Koval (Employee)"` та заголовок таблиці на `"My Tickets"`. | [`after_review/M-01 — Create Ticket Modal Overlay.png`](../../screenshots/after_review/M-01%20—%20Create%20Ticket%20Modal%20Overlay.png) | `BR-02`, `US-01` (Актор створення — `Employee`) |
| **`MAN-CHG-03`**<br>(`DEF-FIG-03`) | `REF-01 / Bottom Master Components Group` | [`before_review/REF-01 — Reviewer & State Reference.png`](../../screenshots/before_review/REF-01%20—%20Reviewer%20&%20State%20Reference.png) (непідписані master-компоненти внизу) | Додано службовий заголовок секції `"Local Component Masters (System Layer)"` над групою master-компонентів унизу `REF-01`. | [`after_review/REF-01 — Reviewer & State Reference.png`](../../screenshots/after_review/REF-01%20—%20Reviewer%20&%20State%20Reference.png) | Розділ 8 і 12 методички (Організація `REF-01`) |
