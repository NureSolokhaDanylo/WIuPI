# 01. UX/UI Design Input Pack — IT Helpdesk (v0.1 DRAFT)

* **Проєкт:** IT Helpdesk (внутрішня вебсистема служби підтримки)
* **Вихідне джерело `[S1]`:** `source/requirements.md` (зафіксована версія від 07.11.2026)
* **Автор вибірки (Оператор G1):** Солоха Данило
* **Використана модель:** Gemini 3.8 Flash (Reasoning: Medium)
* **Статус документа:** `v0.1 DRAFT` (первинна AI-вибірка до проведення ручного Контролю A та до погодження Decision Log)

## Підтвердження прочитаного джерела
Прочитано повний файл `requirements.md` (`IT Helpdesk — Requirements Specification`). Доступні всі 9 розділів документа:
* `§0` Customer Requirements
* `§1` Discovery / Problem Analysis (Problem Statement, Product Business Goal, Stakeholders, Primary Users, Constraints `KC-01..KC-07`, Assumptions `A-01..A-08`, Open Questions)
* `§2` Product Vision + Initial Product Scope (`C-01..C-06`, In Scope / Out of Scope)
* `§3` Stakeholder Needs & Stakeholder Requirements (`SR-01..SR-11`) та Business Rules (`BR-01..BR-12`)
* `§4` Prioritization of Needs + Candidate MVP
* `§5` Detailed User Stories & Acceptance Criteria (`US-01..US-09` + Gherkin-сценарії)
* `§6` Quality Attributes + Security Requirements (`NFR-01..NFR-04`)
* `§7` Feasibility, Estimation & Risk Assessment (`R-01..R-03`, Recommended Scope Adjustment)
* `§8` MVP / Release Scope + Updated NFRs

## 1. Product Purpose, Actors та межі першого релізу

* **Product Purpose (`SOURCE: §1, §2`):** IT Helpdesk — внутрішня вебсистема компанії для централізованої реєстрації, відстеження та координації вирішення технічних звернень співробітників (щодо техніки, програмного забезпечення та доступу). Мета продукту — скоротити час від реєстрації звернення до його призначення та вирішення, усунувши втрату заявок у пошті й месенджерах.
* **Primary Users / Actors (`SOURCE: §1 Primary Users, §3 BR-01`):**
  1. **Employee (Співробітник):** будь-який автентифікований штатний працівник компанії, який створює технічні заявки, переглядає список та деталі власних заявок і залишає коментарі до власних заявок.
  2. **IT Specialist (IT-спеціаліст):** фахівець IT-відділу, який переглядає усі заявки системи, призначає відповідального виконавця (`Assignee`), встановлює/змінює пріоритет (`Priority`), залишає коментарі та — якщо він є призначеним виконавцем (`Assignee`) — змінює статус заявки (`Status`).
* **Secondary Stakeholders (`SOURCE: §1 Stakeholders`):** `IT Manager / керівник IT-відділу`, `System Administrator` — не беруть безпосередньої участі в основних сценаріях MVP і не мають окремих екранів.
* **Межі першого релізу (`SOURCE: §1 KC-01..07, §8`):** Внутрішній desktop web-застосунок для співробітників компанії із заздалегідь визначеними користувачами, без мобільного застосунку, без інтеграції з Active Directory / OAuth / Jira / Slack та без AI-рекомендацій у першому релізі.

## 2. Межі Scope (Release MVP vs Deferred / Future / Out of Scope)

| Категорія | Елементи специфікації | Джерело та причина |
| :--- | :--- | :--- |
| **Included in Release MVP** | **`US-01` – `US-08`** (відповідають `SR-01` – `SR-08`): створення заявки, перегляд списку та деталей, призначення виконавця, встановлення пріоритету, відстеження статусу, коментарі, перегляд важливих відкритих заявок. | `SOURCE: §8 MVP / Release Scope` (остаточний обсяг релізу після оцінки ризиків у `§7`). |
| **Deferred (Відкладено після оцінки ризиків)** | **`US-09` (`SR-09`) — Attach files to a ticket** (`AC-09.1..AC-09.6`: завантаження файлів, ліміт розміру, антивірусне сканування, persistent storage). | `SOURCE: §7, §8 Deferred` (перенесено за межі першого релізу через високу складність реалізації сховища та malware scanning — ризик `R-03`, хоча в `§4` входило до Candidate MVP). |
| **Future Requirements** | **`SR-10`** (Automatic ticket categorization), **`SR-11`** (Automatic priority recommendation) та пов'язане правило **`BR-12`**. | `SOURCE: §4, §8 Future` (`Won't for now`, ризик `R-02` щодо нестачі навчальних даних). У `§5` User Stories для них не створювалися. |
| **Explicitly Out of Scope** | Зовнішній клієнтський портал, білінг, інтеграція з Active Directory / OAuth / SSO (`KC-05`), Email/Push-сповіщення, SLA-моніторинг та аналітичні дашборди, мобільний застосунок (`A-07`), інтеграції з Jira/Slack (`§2`), видалення заявок (`BR-10`), історія змін (`BR-11`), повторне відкриття заявки `Resolved → In Progress` (`AC-07.6`). | `SOURCE: §1 KC-04/05, A-07; §2 Out of Scope; §3 BR-10, BR-11; §5 AC-07.6`. |

## 3. Включені User Stories та всі Acceptance Criteria (37 AC)

Усі критерії нижче мають статус `SOURCE` (`§5 Detailed User Stories & Acceptance Criteria`). Збережено оригінальні ідентифікатори.

### US-01 — Create Ticket
* **Actor:** `Employee`
* **Goal:** Створити заявку з темою (`Subject`), описом (`Description`) та категорією (`Category`), щоб проблема була зафіксована та передана в роботу IT-команді.
* **Acceptance Criteria (6 AC):**
  * **`AC-01.1`** (`SOURCE: §5 US-01`): Користувач може ввести: `Subject`, `Description`, `Category`.
  * **`AC-01.2`** (`SOURCE: §5 US-01`): `Subject` є обов'язковим. *(У Gherkin US-01 наведено текст помилки: `"Subject is required"`).*
  * **`AC-01.3`** (`SOURCE: §5 US-01`): `Description` є обов'язковим.
  * **`AC-01.4`** (`SOURCE: §5 US-01, §3 BR-02.1`): `Category` є обов'язковим; допустимі значення: `Hardware`, `Software`, `Access`. *(У Gherkin US-01 наведено текст помилки: `"Category is required"`).*
  * **`AC-01.5`** (`SOURCE: §5 US-01, §3 BR-03`): Після створення заявка автоматично отримує: `ID`, `status = Open`, `created_at`, `author` (поточний користувач).
  * **`AC-01.6`** (`SOURCE: §5 US-01, §3 BR-04`): Після створення ticket його `priority` не встановлено (`Not Set`).

### US-02 — View Tickets
* **Actor:** `user` (`Employee` та `IT Specialist`)
* **Goal:** Переглядати зареєстровані заявки та відкривати їхні поточні деталі.
* **Acceptance Criteria (6 AC):**
  * **`AC-02.1`** (`SOURCE: §5 US-02, §3 SR-02`): Автентифікований `Employee` може переглянути список лише своїх заявок. `IT Specialist` бачить усі заявки системи.
  * **`AC-02.2`** (`SOURCE: §5 US-02`): `Employee` не має доступу до заявок інших співробітників.
  * **`AC-02.3`** (`SOURCE: §5 US-02`): Для кожного ticket у списку відображаються щонайменше: `ID`, `Subject`, `Category`, `Status`, `Priority`, `Assignee` (якщо призначений).
  * **`AC-02.4`** (`SOURCE: §5 US-02`): Користувач може вибрати ticket зі списку та відкрити сторінку його деталей.
  * **`AC-02.5`** (`SOURCE: §5 US-02`): На сторінці деталей ticket відображаються: `ID`, `Subject`, `Description`, `Category`, `Status`, `Priority`, `Assignee` (якщо призначений), `Author`, `Created at`.
  * **`AC-02.6`** (`SOURCE: §5 US-02`): Якщо заявок немає, система відображає порожній список або повідомлення про відсутність tickets.

### US-03 — Assign Ticket to the Responsible IT Specialist
* **Actor:** `IT Specialist`
* **Goal:** Призначити заявку члену IT-команди, щоб було зрозуміло, хто відповідає за її вирішення.
* **Acceptance Criteria (4 AC):**
  * **`AC-03.1`** (`SOURCE: §5 US-03, §1 A-04`): Можна вибрати одного `assignee`.
  * **`AC-03.2`** (`SOURCE: §5 US-03`): `Assignee` повинен бути користувачем із роллю `IT Specialist`.
  * **`AC-03.3`** (`SOURCE: §5 US-03, §1 Open Questions`): Призначати виконавця може лише `IT Specialist`.
  * **`AC-03.4`** (`SOURCE: §5 US-03`): Поточний `assignee` відображається у заявці.

### US-04 — Set Ticket Priority
* **Actor:** `IT Specialist`
* **Goal:** Встановлювати пріоритет заявки для визначення порядку роботи.
* **Acceptance Criteria (2 AC):**
  * **`AC-04.1`** (`SOURCE: §5 US-04, §3 BR-05`): Допустимі значення встановленого `priority`: `Low`, `Medium`, `High`, `Critical`.
  * **`AC-04.2`** (`SOURCE: §5 US-04, §3 BR-06`): Встановлювати та змінювати `priority` може лише `IT Specialist`.

### US-05 — Track Ticket Progress
* **Actor:** `user` (`Employee` та `IT Specialist`)
* **Goal:** Бачити поточний статус заявки для розуміння прогресу роботи.
* **Acceptance Criteria (3 AC):**
  * **`AC-05.1`** (`SOURCE: §5 US-05`): Поточний `status` відображається для кожного ticket.
  * **`AC-05.2`** (`SOURCE: §5 US-05`): `Status` відображається на сторінці деталей ticket.
  * **`AC-05.3`** (`SOURCE: §5 US-05`): `Status` відображається у списку tickets.

### US-06 — Add Comment
* **Actor:** `user` (`Employee` та `IT Specialist`)
* **Goal:** Додавати коментарі до заявки для обговорення проблеми.
* **Acceptance Criteria (6 AC):**
  * **`AC-06.1`** (`SOURCE: §5 US-06`): Текст коментаря не може бути порожнім (`Comment text cannot be empty`).
  * **`AC-06.2`** (`SOURCE: §5 US-06`): Доданий коментар зберігається та відображається у заявці.
  * **`AC-06.3`** (`SOURCE: §5 US-06`): Автор коментаря (`Comment author`) зберігається та відображається.
  * **`AC-06.4`** (`SOURCE: §5 US-06`): Дата і час створення коментаря (`Creation date/time`) зберігаються та відображаються.
  * **`AC-06.5`** (`SOURCE: §5 US-06`): Помилка збереження (`Failed save`) не створює коментар і виводить повідомлення про помилку.
  * **`AC-06.6`** (`SOURCE: §5 US-06`): `Employee` може залишати коментарі лише до власних заявок.

### US-07 — Update Ticket Status
* **Actor:** `IT Specialist responsible for a ticket` (призначений виконавець)
* **Goal:** Змінювати статус заявки відповідно до стану виконання робіт.
* **Acceptance Criteria (6 AC):**
  * **`AC-07.1`** (`SOURCE: §5 US-07, §3 BR-03`): Нова заявка створюється зі статусом `Open`.
  * **`AC-07.2`** (`SOURCE: §5 US-07, §3 BR-07`): Допустимі статуси: `Open`, `In Progress`, `Resolved`, `Closed`.
  * **`AC-07.3`** (`SOURCE: §5 US-07`): Після зміни статусу система зберігає нове значення та відображає його при наступному перегляді заявки.
  * **`AC-07.4`** (`SOURCE: §5 US-07, §3 BR-08`): `Employee` не може змінювати статус жодної заявки. `IT Specialist`, який не є виконавцем (`assignee`) заявки, також не може змінити її статус.
  * **`AC-07.5`** (`SOURCE: §5 US-07`): Система не дозволяє встановити значення статусу, якого немає у визначеному переліку.
  * **`AC-07.6`** (`SOURCE: §5 US-07, §3 BR-08.1`): Перехід `Resolved → In Progress` не дозволений. Повторне відкриття заявки (Reopen) не передбачено.

### US-08 — View Important Open Tickets
* **Actor:** `user`
* **Goal:** Швидко бачити важливі не закриті заявки, які потребують першочергової уваги.
* **Acceptance Criteria (4 AC):**
  * **`AC-08.1`** (`SOURCE: §5 US-08, §3 BR-09/09.1`): Заявка з `priority = High` і `status != Closed` відображається у списку `Important Tickets`.
  * **`AC-08.2`** (`SOURCE: §5 US-08, §3 BR-09/09.1`): Заявка з `priority = Critical` і `status != Closed` відображається у списку `Important Tickets`.
  * **`AC-08.3`** (`SOURCE: §5 US-08`): Заявки з `priority = Low` або `Medium` не відображаються у списку `Important Tickets`. *(За сукупністю `BR-04`, `BR-09` та `AC-08.1/08.2` заявки з `priority = Not Set` також не входять до `Important Tickets` — тип `DERIVED`).*
  * **`AC-08.4`** (`SOURCE: §5 US-08`): Заявки зі `status = Closed` не відображаються у `Important Tickets` незалежно від `priority`.

## 4. UI-релевантні Business Rules (BR)

| ID | Правило та його вплив на UX/UI | Тип | Джерело |
| :--- | :--- | :--- | :--- |
| **BR-01** | Системою користуються лише автентифіковані співробітники з наперед визначеними ролями `Employee` та `IT Specialist`. | `SOURCE` | `§3 BR-01`, `§1 Open Questions` |
| **BR-02** | Створювати заявку (`Create Ticket`) може будь-який автентифікований `Employee`. | `SOURCE` | `§3 BR-02`, `§5 US-01` |
| **BR-02.1** | При створенні заявки `Employee` обов'язково обирає `Category` (`Hardware`, `Software`, `Access`). | `SOURCE` | `§3 BR-02.1`, `§5 AC-01.4` |
| **BR-03** | Початковий статус нової заявки: `status = Open` (встановлюється автоматично, не є полем вводу при створенні). | `SOURCE` | `§3 BR-03`, `§5 AC-01.5` |
| **BR-04** | Початковий пріоритет нової заявки: `priority = Not Set` (відсутність встановленого пріоритету). | `SOURCE` | `§3 BR-04`, `§5 AC-01.6` |
| **BR-05** | Допустимі значення встановленого пріоритету: `Low`, `Medium`, `High`, `Critical`. До встановлення допускається стан `Not Set`. | `SOURCE` | `§3 BR-05`, `§5 AC-04.1` |
| **BR-06** | Встановлювати та змінювати `priority` може виключно `IT Specialist`. | `SOURCE` | `§3 BR-06`, `§5 AC-04.2` |
| **BR-07** | Допустимі значення статусу: `Open`, `In Progress`, `Resolved`, `Closed`. | `SOURCE` | `§3 BR-07`, `§5 AC-07.2` |
| **BR-08** | Змінювати `status` може лише той `IT Specialist`, якого призначено поточним виконавцем (`Assignee`) цієї заявки. `Employee` та непризначений `IT Specialist` не можуть змінювати статус. | `SOURCE` | `§3 BR-08`, `§5 AC-07.4` |
| **BR-08.1** | Дозволені лише переходи статусів: `Open → In Progress`, `In Progress → Resolved`, `Resolved → Closed`. Інші переходи не дозволені. | `SOURCE` | `§3 BR-08.1`, `§5 AC-07.6` |
| **BR-09 / BR-09.1** | `Important Ticket`: `Priority IN {High, Critical} AND Status != Closed`. Статус `Resolved` означає вирішену, але ще не закриту заявку, тому `High/Critical + Resolved` залишається у списку `Important Tickets`. | `SOURCE` | `§3 BR-09, BR-09.1`, `§5 US-08` |
| **BR-10** | Заявки не видаляються із системи (жодних кнопок `Delete Ticket` в UI). | `SOURCE` | `§3 BR-10` |
| **BR-11** | Окрема користувацька історія змін (`Change History` / `Audit Log`) у першому релізі не передбачається. | `SOURCE` | `§3 BR-11` |

## 5. Non-Functional Requirements (NFR) та межі верифікації макетом

| ID | Вимога | Як враховується в UX/UI макеті | Чи перевіряється макетом? |
| :--- | :--- | :--- | :--- |
| **NFR-01** | **Response time** — своєчасне завантаження сторінок при нормальному навантаженні. | Лаконічний інтерфейс без важких дашбордів. | **Ні (`NOT VERIFIABLE` у макеті):** перевіряється лише на працюючому застосунку. |
| **NFR-02** | **Authentication required** — доступ до всіх сторінок, крім входу, лише для автентифікованих користувачів (`KC-05`: без AD/OAuth у v1). | Наявність межі автентифікації та відображення поточного користувача. | **Частково:** перевіряється у застосунку при спробі входу. |
| **NFR-03** | **HTTPS only** — передача даних виключно по HTTPS (SSL/TLS). | Не потребує окремого продуктового екрана чи перемикача в UI. | **Ні (`NOT VERIFIABLE` у макеті):** перевіряється конфігурацією сервера. |
| **NFR-04** | **Backup of all data** — регулярне резервне копіювання даних. | Не створює користувацьких кнопок чи екранів у MVP. | **Ні (`NOT VERIFIABLE` у макеті):** перевіряється інфраструктурними тестами відновлення. |

## 6. Відкриті питання, прогалини та неоднозначності джерела (Q-ID — UNRESOLVED)

| Q-ID | Статус | Опис питання / прогалини у специфікації `[S1]` | Потрібне рішення людини (Decision Log) |
| :--- | :---: | :--- | :--- |
| **Q-01** | `UNRESOLVED` | **Початковий стан та перепризначення `Assignee`:** У `US-03` зазначено, що `IT Specialist` може призначити виконавця на заявку, але явно не уточнено, як відображається відсутність виконавця відразу після створення заявки та чи дозволено перепризначати вже призначену заявку іншому `IT Specialist`. | Потрібно зафіксувати відображення `Unassigned` та можливість перепризначення `Assignee`. |
| **Q-02** | `UNRESOLVED` | **Відображення недоступних контролів для `Employee` vs `IT Specialist non-assignee`:** У Gherkin `US-07` для `Employee` вказано *"Then the status control is not available"*, а для `IT Specialist non-assignee` діє заборона `AC-07.4`. Потрібно узгодити єдиний візуальний патерн (read-only чи disabled). | Потрібно погодити UX-подання рольових обмежень на сторінці деталей. |
| **Q-03** | `UNRESOLVED` | **Механізм підтвердження змін на сторінці деталей:** Специфікація не визначає, чи змінюються `Assignee`, `Priority`, `Status` миттєво при виборі зі списку, чи через кнопку підтвердження `Apply`. | Потрібно обрати патерн застосування змін. |
| **Q-04** | `UNRESOLVED` | **Представлення `Not Set` для `Priority` (`BR-04`, `BR-05`):** Як візуально відрізнити початкову відсутність пріоритету від вибраних значень і чи можна скинути пріоритет назад у `Not Set`. | Потрібно зафіксувати UI-подання `Not Set` та перелік опцій dropdown. |
| **Q-05** | `UNRESOLVED` | **Чи може `IT Specialist` сам створювати заявку (`BR-02`, `US-01`):** У `BR-02` та `US-01` актором створення вказано `Employee`, але за `A-01` усі користувачі є штатними співробітниками компанії. | Потрібно уточнити актора для демонстрації створення заявки. |

## 7. Source Check (Первинна перевірка покриття джерела `[S1]`)

| User Story | Кількість AC у `[S1]` | Перелік усіх AC ID з вихідної специфікації | Місце в Input Pack v0.1 | Статус |
| :--- | :---: | :--- | :--- | :---: |
| **US-01 Create Ticket** | 6 | `AC-01.1`, `AC-01.2`, `AC-01.3`, `AC-01.4`, `AC-01.5`, `AC-01.6` | Розділ 3 (US-01), Розділ 4 (`BR-02..04`) | Включено |
| **US-02 View Tickets** | 6 | `AC-02.1`, `AC-02.2`, `AC-02.3`, `AC-02.4`, `AC-02.5`, `AC-02.6` | Розділ 3 (US-02) | Включено |
| **US-03 Assign Ticket** | 4 | `AC-03.1`, `AC-03.2`, `AC-03.3`, `AC-03.4` | Розділ 3 (US-03), Розділ 6 (`Q-01`) | Включено |
| **US-04 Set Ticket Priority** | 2 | `AC-04.1`, `AC-04.2` | Розділ 3 (US-04), Розділ 4 (`BR-05, BR-06`) | Включено |
| **US-05 Track Ticket Progress** | 3 | `AC-05.1`, `AC-05.2`, `AC-05.3` | Розділ 3 (US-05), Розділ 4 (`BR-07`) | Включено |
| **US-06 Add Comment** | 6 | `AC-06.1`, `AC-06.2`, `AC-06.3`, `AC-06.4`, `AC-06.5`, `AC-06.6` | Розділ 3 (US-06) | Включено |
| **US-07 Update Ticket Status** | 6 | `AC-07.1`, `AC-07.2`, `AC-07.3`, `AC-07.4`, `AC-07.5`, `AC-07.6` | Розділ 3 (US-07), Розділ 4 (`BR-08, BR-08.1`) | Включено |
| **US-08 View Important Open Tickets** | 4 | `AC-08.1`, `AC-08.2`, `AC-08.3`, `AC-08.4` | Розділ 3 (US-08), Розділ 4 (`BR-09, BR-09.1`) | Включено |
| **РАЗОМ (Release MVP)** | **37 AC** | **Усі 37 критеріїв `US-01..US-08` перенесені до вибірки** | Розділи 3–6 | **DRAFT — ОЧІКУЄ КОНТРОЛЮ A** |
