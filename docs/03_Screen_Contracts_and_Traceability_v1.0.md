# 03. Developer-Verifiable Screen Contracts & Traceability — IT Helpdesk (v1.0 APPROVED)

* **Проєкт:** IT Helpdesk (Release MVP `US-01..US-08`)
* **Автор контрактів (Оператор G3):** Долгополов Ярослав
* **Рецензент Контролю B:** Солоха Данило
* **Використана модель:** 4 Argon (Reasoning: High)
* **Статус документа:** `v1.0 APPROVED`

## 1. Матриця прав доступу за ролями та відношенням до заявки (Permission Matrix)

У системі є дві глобальні ролі (`Employee` та `IT Specialist`), а `Assignee` є відношенням конкретного `IT Specialist` до конкретної заявки (`AC-03.2`).

| Дія / Операція в системі | `Employee` (Автор власної заявки) | `Employee` (Чужа заявка іншого співробітника) | `IT Specialist` (Не є поточним `Assignee`) | `IT Specialist` (Поточний `Assignee` заявки) | Джерело / D-ID |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Перегляд у списку (`S-02`)** | Так (лише свої — `My Tickets`) | **Ні** (приховано зі списку) | Так (усі заявки компанії) | Так (усі заявки компанії) | `AC-02.1`, `SR-02`, `D-07` |
| **Відкриття деталей (`S-03`) за кліком або прямим URL** | Так | **Ні (403 Access Denied)** | Так | Так | `AC-02.1`, `AC-02.2`, `D-05` |
| **Створення нової заявки (`M-01`)** | Так | — | Так (як співробітник, основний актор — `Employee`) | — | `BR-02`, `US-01`, `D-01` |
| **Додавання коментаря (`S-03`)** | Так (лише до власної заявки) | **Ні (403 Access Denied)** | Так (до будь-якої заявки) | Так (до будь-якої заявки) | `US-06`, `AC-06.6` |
| **Зміна `Category` після створення (`S-03`)** | **Ні** (read-only badge) | **Ні** | **Так** (enabled dropdown + `Apply`) | **Так** (enabled dropdown + `Apply`) | `BR-02.1`, `D-04`, `D-05` |
| **Призначення / зміна `Assignee` (`S-03`)** | **Ні** (read-only text) | **Ні** | **Так** (вибір лише з `IT Specialist` + `Apply`) | **Так** (вибір лише з `IT Specialist` + `Apply`) | `US-03` (`AC-03.1..03.3`), `D-04` |
| **Встановлення / зміна `Priority` (`S-03`)** | **Ні** (read-only badge) | **Ні** | **Так** (`Low/Medium/High/Critical` + `Apply`) | **Так** (`Low/Medium/High/Critical` + `Apply`) | `BR-06`, `US-04` (`AC-04.1, 04.2`), `D-03` |
| **Зміна `Status` (`S-03`)** | **Ні** (read-only badge; контрол відсутній) | **Ні** | **Ні** (контрол `disabled` із поясненням) | **Так** (лише дозволені переходи за `BR-08.1` + `Apply`) | `BR-08`, `AC-07.4`, Gherkin `US-07`, `D-05` |

> **Важливе правило для `Unassigned` заявки:** Якщо у заявки `Assignee = Unassigned`, то жоден користувач (навіть `IT Specialist`) не є поточним виконавцем, тому контрол `Status` є `disabled` для всіх до моменту, поки `IT Specialist` не призначить виконавця через контрол `Assignee`.

## 2. Матриця переходів статусів (Status Transition Matrix)

Згідно з `BR-08.1` та `AC-07.6` у системі дозволено **рівно 3 переходи**. Повторне відкриття (`Resolved → In Progress` або `Resolved → Open`) **суворо заборонено**.

| Поточний статус (`Current Status`) ↓ \ Наступний статус → | `Open` | `In Progress` | `Resolved` | `Closed` | Доступні опції у селекті `Status` (для поточного `Assignee`) |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **`Open`** (початковий стан, `BR-03`) | *Поточний стан* | **ДОЗВОЛЕНО (1)** | Заборонено | Заборонено | Поточний: `Open` → Опція для вибору: **`In Progress`** |
| **`In Progress`** | Заборонено | *Поточний стан* | **ДОЗВОЛЕНО (2)** | Заборонено | Поточний: `In Progress` → Опція для вибору: **`Resolved`** |
| **`Resolved`** (активна, не закрита, `BR-09.1`) | Заборонено (Gherkin US-07) | **ЗАБОРОНЕНО (`AC-07.6`)** | *Поточний стан* | **ДОЗВОЛЕНО (3)** | Поточний: `Resolved` → Опція для вибору: **`Closed`** *(без `In Progress`!)* |
| **`Closed`** (фінальний стан) | Заборонено | Заборонено | Заборонено | *Поточний стан* | Немає вихідних переходів (селект `disabled` / `"Closed — final status"`) |

> **Локальність правила `Closed`:** Відсутність вихідних переходів зі статусу `Closed` стосується лише поля `Status`. Специфікація не вводить автоматичного блокування коментарів або зміни пріоритету для закритих заявок.

## 3. Формальний предикат `Important Tickets` та канонічний набір даних

* **Логічна формула (`BR-09`, `BR-09.1`, `AC-08.1..08.4`):**
  $$	ext{IsImportant}(t) \iff ig(t.	ext{Priority} \in \{	ext{High}, 	ext{Critical}\}ig) \land ig(t.	ext{Status} 
eq 	ext{Closed}ig)$$
* **Ізоляція за ролями (`AC-02.1`, `D-07`):**
  * Для `IT Specialist`: відображаються всі заявки $t$, де $	ext{IsImportant}(t) = 	ext{true}$.
  * Для `Employee`: відображаються лише заявки $t$, де $t.	ext{Author} == 	ext{currentUser} \land 	ext{IsImportant}(t) = 	ext{true}$.

### Канонічний набір тестових даних для всіх фреймів (`TCK-001` – `TCK-008`)

| ID | Subject | Category | Status | Priority | Author (Employee) | Assignee (IT Specialist) | Входить у `All Tickets` (IT)? | Входить у `Important Tickets` (IT)? | Входить у `My Tickets` (Олена Коваль)? | Пояснення для перевірки граничних умов |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **TCK-001** | VPN connection drops every 10 minutes | `Access` | `Open` | `Critical` | Олена Коваль (`Employee A`) | Ігор Мельник (`IT Spec 1`) | **Так** | **Так** | **Так** | `Critical` + `Open` → у важливих (`AC-08.2`). |
| **TCK-002** | Accounting database timeout on export | `Software` | `In Progress` | `High` | Максим Бондар (`Employee B`) | Ігор Мельник (`IT Spec 1`) | **Так** | **Так** | **Ні** (чужа) | `High` + `In Progress` → у важливих (`AC-08.1`). |
| **TCK-003** | Shared folder permissions missing for HR | `Access` | `Resolved` | `High` | Олена Коваль (`Employee A`) | Анна Савченко (`IT Spec 2`) | **Так** | **Так** | **Так** | **Граничний кейс 1 (`BR-09.1`):** `Resolved` ще не `Closed`, тому `High + Resolved` **залишається** в `Important Tickets`! |
| **TCK-004** | Main office router failure in building B | `Hardware` | `Closed` | `Critical` | Максим Бондар (`Employee B`) | Ігор Мельник (`IT Spec 1`) | **Так** | **Ні** | **Ні** (чужа) | **Граничний кейс 2 (`AC-08.4`):** Навіть `Critical` виключається, якщо `Status == Closed`. |
| **TCK-005** | Printer not working in room 204 | `Hardware` | `Open` | `Medium` | Олена Коваль (`Employee A`) | Unassigned | **Так** | **Ні** | **Так** | `Priority = Medium` не входить у важливі (`AC-08.3`). |
| **TCK-006** | Need Adobe Acrobat license installed | `Software` | `Open` | `Not Set` | Олена Коваль (`Employee A`) | Unassigned | **Так** | **Ні** | **Так** | **Граничний кейс 3 (`BR-04`):** Нова заявка з `Priority = Not Set` не перетворюється сама на `Medium` і не входить у `Important`. |
| **TCK-007** | Laptop screen flickering after BIOS update | `Hardware` | `Resolved` | `Critical` | Максим Бондар (`Employee B`) | Ігор Мельник (`IT Spec 1`) | **Так** | **Так** | **Ні** (чужа) | `Critical` + `Resolved` (`!= Closed`) → залишається у важливих (`AC-08.2`, `BR-09.1`). |
| **TCK-008** | Keyboard key stuck on workstation #14 | `Hardware` | `In Progress` | `Low` | Максим Бондар (`Employee B`) | Анна Савченко (`IT Spec 2`) | **Так** | **Ні** | **Ні** (чужа) | `Priority = Low` не входить у важливі (`AC-08.3`). |

## 4. Детальні контракти екранів та контролів (Screen & Control Contracts)

### 4.1. `S-01` — Authentication Boundary
* **Призначення:** Візуальне представлення межі автентифікації без вигадування парольної форми (`BR-01`, `NFR-02`, `D-02`).
* **Видимі елементи:** Заголовок `IT Helpdesk — Internal Service Desk`, інформаційний блок `Authentication Boundary — Mechanism TBD (Predefined users with roles: Employee, IT Specialist)`.
* **Перевірка:** У макеті перевіряється відсутність вигаданих полів `Password`, `Forgot password`, `Sign in with Google/Microsoft`; у реалізації перевіряється серверний редірект неавторизованих сесій (`NFR-02`).

### 4.2. `S-02` (`S-02-A`, `S-02-B`, `S-02-C`, `S-02-D`) — Tickets List
* **Призначення:** Перегляд списку заявок відповідно до ролі (`All Tickets` для IT, `My Tickets` для Employee) та фільтрація важливих не закритих заявок (`Important Tickets`).
* **Видимі поля рядка таблиці (усі обов'язкові за `AC-02.3`):**
  1. `ID` (наприклад, `TCK-001`)
  2. `Subject` (текстове посилання на деталі заявки)
  3. `Category` (`Hardware` | `Software` | `Access`)
  4. `Status` (`Open` | `In Progress` | `Resolved` | `Closed` — також покриває `AC-05.3`)
  5. `Priority` (`Not Set` | `Low` | `Medium` | `High` | `Critical`)
  6. `Assignee` (Ім'я `IT Specialist` або нейтральний підпис `Unassigned` для непризначених)
* **Контроли `S-02`:**
  * `C-01` (`+ Create Ticket` button): видима й активна; відкриває модальне вікно `M-01` (`T-04`).
  * `C-02` (`All Tickets` / `My Tickets` tab): активує повний список заявок, доступних поточному користувачу (`S-02-A` або `S-02-D`, `T-03`).
  * `C-03` (`Important Tickets` tab): активує відфільтрований список `S-02-B` за предикатом `Priority IN {High, Critical} AND Status != Closed` (`T-02`).
  * `C-04` (`Ticket Row / Subject Link`): відкриває `S-03` для обраної заявки (`T-08`).

### 4.3. `M-01` — Create Ticket Modal
* **Призначення:** Створення нової заявки співробітником (`US-01`).
* **Поля та контроли:**
  * `C-05` (`Subject *` — text input): обов'язкове поле (`AC-01.1`, `AC-01.2`). При порожньому значенні показує `"Subject is required"` (Gherkin `US-01`).
  * `C-06` (`Category *` — select dropdown): обов'язкове поле (`AC-01.1`, `AC-01.4`, `BR-02.1`). Початковий стан: placeholder `Select category...`. Доступні опції: `Hardware`, `Software`, `Access`. При спробі створення без вибору категорії показує `"Category is required"` (Gherkin `US-01`).
  * `C-07` (`Description *` — multiline textarea): обов'язкове поле (`AC-01.1`, `AC-01.3`). При порожньому значенні показує `"Description is required"`.
  * `C-08` (`Create` — primary button): валідує поля `C-05..C-07`; у разі успіху створює заявку (`T-07`).
  * `C-09` (`Cancel` — secondary button): закриває `M-01` без збереження (`T-05`).
* **Автоматично генеровані поля (НЕ показуються як інпути в `M-01`):** `ID`, `Status = Open` (`AC-01.5`, `BR-03`), `Priority = Not Set` (`AC-01.6`, `BR-04`), `Assignee = Unassigned` (`D-04`), `Author = current user` (`AC-01.5`), `Created at = current timestamp` (`AC-01.5`).

### 4.4. `S-03` — Ticket Details
* **Призначення:** Перегляд повної інформації про заявку (`AC-02.5`), зміна `Category`, `Assignee`, `Priority`, `Status` та робота з коментарями (`US-03..US-07`).
* **Видимі метадані заявки (усі 9 полів за `AC-02.5`):** `ID`, `Subject`, `Description`, `Category`, `Status` (`AC-05.2`), `Priority`, `Assignee` (`AC-03.4`), `Author`, `Created at`.
* **Контроли `S-03`:**
  * `C-10` (`← Return to Tickets List` link/button): повертає на попередній стан `S-02` (`T-13`, `D-07`).
  * `C-11` (`Category` dropdown + `C-12` `Apply` button):
    * Для `IT Specialist`: `enabled`, опції `Hardware`, `Software`, `Access` (`BR-02.1`, `D-04`, `D-06`).
    * Для `Employee`: відображається лише як read-only бейдж без селекта (`D-05`).
  * `C-13` (`Assignee` dropdown + `C-14` `Apply` button):
    * Для `IT Specialist`: `enabled`, дозволяє обрати одного виконавця виключно з ролі `IT Specialist` (`Ігор Мельник`, `Анна Савченко`) (`AC-03.1..03.3`, `D-04`, `D-06`).
    * Для `Employee`: read-only текст (`Ім'я виконавця` або `Unassigned`) (`AC-03.4`, `D-05`).
  * `C-15` (`Priority` dropdown + `C-16` `Apply` button):
    * Для `IT Specialist`: `enabled`, опції лише `Low`, `Medium`, `High`, `Critical` (без `Not Set` як вибіркової опції, `BR-05`, `AC-04.1`, `AC-04.2`, `D-03`, `D-06`). Якщо пріоритет ще не встановлено, до вибору відображається поточний стан `Not Set`.
    * Для `Employee`: read-only бейдж (`Not Set` / `Low` / `Medium` / `High` / `Critical`) (`D-05`).
  * `C-17` (`Status` dropdown + `C-18` `Apply` button):
    * Для `IT Specialist (Current Assignee)` за умови `Status != Closed`: `enabled`, містить лише дозволений наступний статус за `BR-08.1` (`Open → In Progress`, `In Progress → Resolved`, `Resolved → Closed`, без `Resolved → In Progress`, `AC-07.6`).
    * Для `IT Specialist (Current Assignee)` за умови `Status == Closed`: `disabled` (фінальний статус, переходів немає).
    * Для `IT Specialist (Non-assignee)` або якщо `Assignee == Unassigned`: `disabled` із підписом *"Only the assigned IT Specialist can change status"* (`BR-08`, `AC-07.4`, `D-05`).
    * Для `Employee`: контрол зміни статусу відсутній (`not available`), відображається лише read-only бейдж поточного статусу (`AC-07.4`, Gherkin `US-07`, `D-05`).
  * `C-19` (`Comment text` textarea + `C-20` `Add Comment` button):
    * Видимі та активні для `IT Specialist` (на будь-якій заявці) і для `Employee` (на власній заявці).
    * Список збережених коментарів під/над формою показує для кожного запису: `Comment text`, `Comment author`, `Creation date/time` (`AC-06.2..06.4`).
    * Стани помилок (показані у `REF-01`): 1) порожній коментар → `"Comment text cannot be empty"` (`AC-06.1`); 2) помилка збереження БД → банер `"Failed to save comment. Changes were not stored."` без додавання фейкового коментаря у список (`AC-06.5`).

## 5. Матриця простежуваності вимог (Traceability Matrix — 37 AC)

| Source ID | Короткий зміст вимоги | UIR-ID | Screen / State ID | Control ID / Annotation | Test ID | Verification Level (Рівень перевірки) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **AC-01.1** | Введення `Subject`, `Description`, `Category` при створенні | `UIR-01.1` | `M-01` | `C-05`, `C-06`, `C-07` | `T-01` | **Wireframe (PASS):** наявність 3 інпутів у `M-01`. |
| **AC-01.2** | `Subject` є обов'язковим (`"Subject is required"`) | `UIR-01.2` | `M-01`, `REF-01` | `C-05`, `C-08` (Validation state) | `T-02` | **Wireframe (PASS):** стан помилки валідації порожнього `Subject`. |
| **AC-01.3** | `Description` є обов'язковим | `UIR-01.3` | `M-01`, `REF-01` | `C-07`, `C-08` (Validation state) | `T-02` | **Wireframe (PASS):** стан помилки валідації порожнього `Description`. |
| **AC-01.4** | `Category` обов'язкова (`Hardware`, `Software`, `Access`; `"Category is required"`) | `UIR-01.4` | `M-01`, `REF-01` | `C-06`, `C-08` (Validation state) | `T-01`, `T-02` | **Wireframe (PASS):** 3 опції `Category` та стан помилки `"Category is required"`. |
| **AC-01.5** | Автоматичні `ID`, `status = Open`, `created_at`, `author` | `UIR-01.5` | `M-01` → `S-03` | Відсутність інпутів у `M-01`; метадані на `S-03` | `T-01` | **Wireframe + Implementation:** у макеті перевіряється відсутність полів у `M-01` та їх показ у `S-03`; генерація БД — у коді. |
| **AC-01.6** | Після створення `priority = Not Set` | `UIR-01.6` | `S-02-A/D`, `S-03` | Бейдж `Not Set` (`TCK-006`) | `T-01`, `T-04` | **Wireframe (PASS):** відображення `Not Set` для нової заявки. |
| **AC-02.1** | `Employee` бачить список лише своїх заявок; `IT Specialist` — усі | `UIR-02.1` | `S-02-A` (IT), `S-02-D` (Employee) | Порівняння рядків `S-02-A` (8 шт.) та `S-02-D` (4 шт. Олени Коваль) | `T-09` | **Wireframe + Backend:** у макеті показані обидва стани списку; фільтрація сесії — на сервері. |
| **AC-02.2** | Ізоляція: `Employee` не має доступу до чужих заявок за прямим URL | `UIR-02.2` | `REF-01` (403 State) | Картка `403 Access Denied` для `Employee` на чужій заявці | `T-09` | **Backend Implementation (Partial UI):** у `REF-01` показаний екран відмови; реальний захист URL — серверна авторизація. |
| **AC-02.3** | У списку показані: `ID`, `Subject`, `Category`, `Status`, `Priority`, `Assignee` | `UIR-02.3` | `S-02-A`, `S-02-B`, `S-02-D` | Колонки таблиці `S-02` | `T-01`, `T-08` | **Wireframe (PASS):** усі 6 колонок присутні у кожному стані списку. |
| **AC-02.4** | Вибір заявки зі списку відкриває сторінку деталей | `UIR-02.4` | `S-02` → `S-03` | `C-04` (`T-08` annotation) | `T-09` | **Wireframe Annotation:** позначено перехід `T-08` від `Subject` до `S-03`. |
| **AC-02.5** | На `S-03` показані: `ID`, `Subject`, `Description`, `Category`, `Status`, `Priority`, `Assignee`, `Author`, `Created at` | `UIR-02.5` | `S-03` | Блок деталей і метаданих `S-03` | `T-01` | **Wireframe (PASS):** усі 9 обов'язкових полів присутні на `S-03`. |
| **AC-02.6** | Порожній список або повідомлення за відсутності заявок | `UIR-02.6` | `S-02-C` | Empty state message у `S-02-C` | `T-09` | **Wireframe (PASS):** окремий стан фрейму `S-02-C`. |
| **AC-03.1** | Можна вибрати одного `assignee` | `UIR-03.1` | `S-03` | `C-13` (single-select dropdown) | `T-03` | **Wireframe (PASS):** контрол `Assignee` є single-select, не multi-select. |
| **AC-03.2** | `Assignee` має бути користувачем із роллю `IT Specialist` | `UIR-03.2` | `S-03`, `REF-01` | Опції `C-13`: лише Ігор Мельник, Анна Савченко | `T-03` | **Wireframe (PASS):** у списку `C-13` немає `Employee` (Олени Коваль чи Максима Бондаря). |
| **AC-03.3** | Призначати виконавця може лише `IT Specialist` (+ зміна `Category` за `BR-02.1`) | `UIR-03.3` | `S-03`, `REF-01` | `C-11..C-14` enabled для IT, read-only для Employee | `T-03` | **Wireframe + Backend:** візуальна доступність у `S-03`/`REF-01`; серверна перевірка в API. |
| **AC-03.4** | Поточний `assignee` відображається у заявці | `UIR-03.4` | `S-02`, `S-03` | Колонка `Assignee` та поле в `S-03` | `T-03` | **Wireframe (PASS):** видно ім'я виконавця або `Unassigned`. |
| **AC-04.1** | Допустимі значення встановленого `priority`: `Low`, `Medium`, `High`, `Critical` | `UIR-04.1` | `S-03`, `REF-01` | Опції `C-15` (рівно 4 значення) | `T-04` | **Wireframe (PASS):** у розкритому списку лише 4 опції, `Not Set` не є вибірковою опцією. |
| **AC-04.2** | Встановлювати та змінювати `priority` може лише `IT Specialist` | `UIR-04.2` | `S-03`, `REF-01` | `C-15, C-16` enabled для IT, read-only для Employee | `T-04` | **Wireframe + Backend:** перевірено у `S-03` та `REF-01`. |
| **AC-05.1** | Поточний `status` відображається для кожного ticket | `UIR-05.1` | `S-02`, `S-03` | Бейдж статусу | `T-05` | **Wireframe (PASS):** статус присутній для всіх заявок. |
| **AC-05.2** | `Status` відображається на сторінці деталей ticket | `UIR-05.2` | `S-03` | Поле/бейдж `Status` у `S-03` | `T-05` | **Wireframe (PASS):** відображено на `S-03`. |
| **AC-05.3** | `Status` відображається у списку tickets | `UIR-05.3` | `S-02-A/B/D` | Колонка `Status` у таблиці `S-02` | `T-05` | **Wireframe (PASS):** відображено в таблиці `S-02`. |
| **AC-06.1** | Текст коментаря не може бути порожнім | `UIR-06.1` | `S-03`, `REF-01` | `C-19, C-20` + стан валідації порожнього коментаря | `T-07` | **Wireframe (PASS):** приклад помилки `"Comment text cannot be empty"` у `REF-01`. |
| **AC-06.2** | Доданий коментар зберігається та відображається для заявки | `UIR-06.2` | `S-03` | Список збережених коментарів на `S-03` | `T-07` | **Wireframe + Backend:** відображення в UI показано; збереження в БД — реалізація. |
| **AC-06.3** | Автор коментаря зберігається та відображається | `UIR-06.3` | `S-03` | Підпис автора біля кожного коментаря | `T-07` | **Wireframe (PASS):** у кожному коментарі вказано ім'я та роль автора. |
| **AC-06.4** | Дата і час створення коментаря зберігаються та відображаються | `UIR-06.4` | `S-03` | Timestamp біля кожного коментаря | `T-07` | **Wireframe (PASS):** відображено формат `YYYY-MM-DD HH:MM`. |
| **AC-06.5** | Помилка збереження не створює коментар і виводить помилку | `UIR-06.5` | `REF-01` (`S-03` error example) | Стан `Comment Save Error` без нового запису в списку | `T-07` | **Wireframe + Backend:** візуальний стан у `REF-01`; транзакційність БД — у коді. |
| **AC-06.6** | `Employee` коментує лише власні заявки; прямий запит до чужої повертає помилку доступу | `UIR-06.6` | `S-03`, `REF-01` | Ізоляція `S-02-D` + картка `403 Access Denied` у `REF-01` | `T-07`, `T-09` | **Backend Implementation (Partial UI):** у `REF-01` задокументовано стан відмови доступу. |
| **AC-07.1** | Нова заявка створюється зі статусом `Open` | `UIR-07.1` | `M-01` → `S-03` | Початковий статус `Open` (`TCK-006`) | `T-01`, `T-05` | **Wireframe (PASS):** показано у `S-02`/`S-03` та правилах `M-01`. |
| **AC-07.2** | Допустимі статуси: `Open`, `In Progress`, `Resolved`, `Closed` | `UIR-07.2` | `S-02`, `S-03`, `REF-01` | 4 статуси у `REF-01` та таблиці `S-02` | `T-05` | **Wireframe (PASS):** представлені всі 4 статуси, включно з `In Progress` (`TCK-002`, `TCK-008`). |
| **AC-07.3** | Після зміни статусу система зберігає та відображає нове значення | `UIR-07.3` | `S-03` | `C-17, C-18` (`Apply`) | `T-05` | **Wireframe + Backend:** UI-поведінка описана; збереження — в БД. |
| **AC-07.4** | `Employee` та непризначений `IT Specialist` не можуть змінювати статус | `UIR-07.4` | `S-03`, `REF-01` | Канонічний `S-03` (IT non-assignee: `Status disabled`) + `REF-01` (Employee: read-only) | `T-06` | **Wireframe (PASS) + Backend:** явно показано обидва обмеження на `S-03` та в `REF-01`. |
| **AC-07.5** | Не можна встановити статус поза визначеним переліком | `UIR-07.5` | `S-03`, `REF-01` | Закритий перелік у селекті `C-17` | `T-05` | **Wireframe (PASS):** інпут статусу є строго обмеженим селектом. |
| **AC-07.6** | Перехід `Resolved → In Progress` **не дозволений** (без Reopen) | `UIR-07.6` | `REF-01` (Status Matrix) | У стані `Resolved` у селекті доступна лише опція `Closed` | `T-05` | **Wireframe (PASS):** у `REF-01` явно показано відсутність `In Progress` та `Open` для `Resolved`. |
| **AC-08.1** | `Priority = High` і `Status != Closed` → у `Important Tickets` | `UIR-08.1` | `S-02-B` | Рядки `TCK-002` (`High/In Progress`) та `TCK-003` (`High/Resolved`) | `T-08` | **Wireframe (PASS):** обидві заявки присутні у фреймі `S-02-B`. |
| **AC-08.2** | `Priority = Critical` і `Status != Closed` → у `Important Tickets` | `UIR-08.2` | `S-02-B` | Рядки `TCK-001` (`Critical/Open`) та `TCK-007` (`Critical/Resolved`) | `T-08` | **Wireframe (PASS):** обидві заявки присутні у фреймі `S-02-B`. |
| **AC-08.3** | `Priority = Low` або `Medium` (та `Not Set`) не входять у `Important Tickets` | `UIR-08.3` | `S-02-A` vs `S-02-B` | Відсутність `TCK-005` (`Medium`), `TCK-006` (`Not Set`), `TCK-008` (`Low`) у `S-02-B` | `T-08` | **Wireframe (PASS):** перевіряється порівнянням `S-02-A` та `S-02-B`. |
| **AC-08.4** | `Status = Closed` не відображається у `Important Tickets` незалежно від `Priority` | `UIR-08.4` | `S-02-A` vs `S-02-B` | Відсутність `TCK-004` (`Closed/Critical`) у `S-02-B` | `T-08` | **Wireframe (PASS):** `TCK-004` є в `S-02-A`, але виключена з `S-02-B`. |

## 6. Специфікація перевірочних тестів (Test Suite `T-01..T-10`)

| Test ID | Назва тесту | Актор та передумови | Дія / об'єкт перевірки | Очікуваний видимий результат у макеті | Що НЕ доводиться цим тестом у статичному макеті |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **T-01** | **Create Ticket & Defaults** | `Employee A (Олена Коваль)`, відкрито `M-01` | Перевірити поля `M-01` та стан нової заявки (`TCK-006`). | У `M-01` редагуються лише `Subject`, `Category` (`Hardware/Software/Access`), `Description`. Нова заявка має `Status=Open`, `Priority=Not Set`, `Assignee=Unassigned`, `Author`, `Created at`. | Фактичний `INSERT` у БД та автогенерація унікального `ID`. |
| **T-02** | **Create Validation States** | `Employee A`, відкрито `M-01` | Спроба натиснути `Create` без `Subject`, без `Category` або без `Description`. | Показані стани помилок `"Subject is required"`, `"Category is required"`, `"Description is required"`; заявка не створюється. | Серверна валідація HTTP-запиту. |
| **T-03** | **Assignment & Category Management** | `Employee` vs `IT Specialist` на `S-03` | Перевірити доступність і опції `Assignee` (`C-13`) та `Category` (`C-11`). | Для `Employee` — read-only. Для `IT Specialist` — `enabled` dropdown + `Apply`; у списку `Assignee` лише `IT Specialist` (Ігор Мельник, Анна Савченко). | Серверна перевірка ролі під час `PATCH /tickets/{id}`. |
| **T-04** | **Priority Management & Not Set** | `Employee` vs `IT Specialist` на `S-03` | Перевірити бейдж `Not Set` та опції селекта `Priority` (`C-15`). | Для `Employee` — read-only. Для `IT Specialist` — `enabled`, у списку лише `Low`, `Medium`, `High`, `Critical` (немає опції `Not Set`). | Збереження зміни пріоритету в БД. |
| **T-05** | **Strict 3 Status Transitions (No Reopen)** | `IT Specialist (Current Assignee)` на `S-03` та в `REF-01` | Перевірити дозволені переходи для всіх 4 статусів: `Open`, `In Progress`, `Resolved`, `Closed`. | Доступні лише `Open→In Progress`, `In Progress→Resolved`, `Resolved→Closed`. У стані `Resolved` перехід у `In Progress` або `Open` **відсутній** (`AC-07.6`). У `Closed` переходів немає. | Серверний кінцевий автомат статусів. |
| **T-06** | **Status Permission Restriction** | `IT Specialist 1 (Ігор Мельник)` відкриває `TCK-003` (`Assignee = Анна Савченко`) | Спробувати змінити `Status` чужої призначеної заявки або заявки для `Employee`. | На `S-03` для непризначеного `IT Specialist` селект `Status` і кнопка `Apply` є `disabled` із поясненням. Для `Employee` контрол зміни статусу відсутній (read-only badge). | Блокування прямого API-запиту на зміну статусу. |
| **T-07** | **Comments & Save Error Handling** | `Employee` (власна заявка) / `IT Specialist` на `S-03` та в `REF-01` | Перевірити збережені коментарі, порожній ввід і збій збереження. | Збережені коментарі мають текст, автора і дату/час. Порожній ввід дає помилку валідації. Збій збереження показує банер помилки без додавання коментаря до списку (`AC-06.5`). | Транзакційний rollback у БД при помилці збереження. |
| **T-08** | **Important Tickets Predicate** | `IT Specialist` у `S-02-A` та `S-02-B` | Зіставити набір `TCK-001..008` між `All Tickets` та `Important Tickets`. | У `S-02-B` присутні рівно `TCK-001`, `TCK-002`, `TCK-003` (`High+Resolved`!), `TCK-007` (`Critical+Resolved`!). Заявки `TCK-004` (`Closed/Critical`), `TCK-005` (`Medium`), `TCK-006` (`Not Set`), `TCK-008` (`Low`) відсутні. | Динамічний SQL-запит фільтрації на бекенді. |
| **T-09** | **Employee Ticket Isolation & Empty State** | `Employee A (Олена Коваль)` у `S-02-D`, `S-02-C` та `REF-01` | Перевірити список заявок `Employee`, стан `Empty` та спробу відкрити чужу заявку. | У `S-02-D` показані лише заявки Олени Коваль (`TCK-001, 003, 005, 006`). У `S-02-C` — повідомлення про порожній список. У `REF-01` — картка `403 Access Denied` при зверненні до чужої заявки (`AC-02.2`, `AC-06.6`). | Серверна перевірка власника заявки при зверненні за прямим URL. |
| **T-10** | **Scope & Exclusions Audit** | Рецензент оглядає всі фрейми макета | Перевірити відсутність позаскоупових елементів. | Немає кнопки `Delete`, блоку `Attachments` (`US-09`), AI-підказок (`SR-10/11`), `History` (`BR-11`), пошуку, чекбоксів рядків і декоративних квадратів біля заголовків. | — |
