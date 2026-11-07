# IT Helpdesk — Requirements Specification

---

## 0. Customer Requirements

Компанії потрібна невелика внутрішня система служби підтримки для централізованого обліку та обробки звернень співробітників щодо проблем із технікою, доступом і програмним забезпеченням. Співробітник повинен мати можливість швидко створити заявку, зазначивши тему та опис проблеми. Спеціалісти IT-відділу переглядають вхідні звернення, призначають відповідального виконавця, встановлюють пріоритет задачі, змінюють статус у міру виконання робіт і залишають коментарі до заявки для уточнення деталей. У майбутньому система повинна автоматично визначати категорію поломки та її пріоритет за текстом звернення.

---

## 1. Discovery / Problem Analysis

**Вхід:** Customer Request

**Вихід:** Problem Statement + Product Goals + Stakeholders + Primary Users + Known Constraints + Assumptions + Open Questions

### Problem Statement

Компанія не має централізованого інструменту для прийому та обробки звернень співробітників щодо технічних проблем. Заявки надходять через електронну пошту, месенджери та в усній формі, що призводить до втрати звернень, непрозорого стану виконання та відсутності аналітики щодо навантаження на IT-відділ. Необхідна внутрішня система Service Desk, яка дозволить співробітникам швидко оформити заявку, а IT-спеціалістам — структуровано її опрацювати, відстежити та закрити.

### Product Business Goal

Скоротити час від реєстрації звернення до його призначення та вирішення шляхом централізованого обліку і обробки заявок в єдиній системі.

### Stakeholders

**Primary**
- Employee (Співробітник)
- IT Specialist (IT-спеціаліст)

**Secondary**
- IT Manager / керівник IT-відділу
- System Administrator

Secondary stakeholders мають інтерес до результатів або належної роботи системи, але не є основними користувачами та не беруть безпосередньої участі в основних сценаріях використання MVP.

### Primary Users

- Employee — будь-який штатний працівник компанії, який має технічну проблему і потребує допомоги IT-відділу;
- IT Specialist — фахівець IT-відділу, який переглядає вхідні заявки, призначає виконавця, встановлює пріоритет, змінює статус і залишає коментарі.

### Constraints

- KC-01. Система призначена для внутрішнього використання в межах компанії.
- KC-02. Перший реліз має бути простим і швидким у розробці.
- KC-03. Система повинна працювати як web application.
- KC-04. Користувачами є лише співробітники компанії, а не зовнішні клієнти.
- KC-05. У першій версії не планується інтеграція з Active Directory, OAuth або іншими системами автентифікації.
- KC-06. AI-функціональність не є обов'язковою для першого релізу.
- KC-07. Рішення має бути придатним для подальшого розширення AI-функціональністю.

### Assumptions

- A-01. Припускаємо, що всі користувачі системи є штатними співробітниками компанії.
- A-02. Припускаємо, що кількість користувачів невелика.
- A-03. Припускаємо, що одночасно працюватиме невелика кількість користувачів.
- A-04. Припускаємо, що для заявки достатньо одного відповідального виконавця.
- A-05. Припускаємо, що заявка має один поточний пріоритет.
- A-06. Припускаємо, що коментарів достатньо для взаємодії між Employee та IT Specialist в межах заявки.
- A-07. Припускаємо, що мобільний застосунок для першої версії не потрібний.
- A-08. Припускаємо, що AI-функції будуть рекомендаційними, а не автоматично змінюватимуть категорію або пріоритет.

### Open Questions та відповіді

- Чи потрібна автентифікація? Так. В системі є заздалегідь визначені користувачі з двома ролями — Employee та IT Specialist.
- Хто може створювати заявку? Будь-який автентифікований Employee.
- Хто може змінювати пріоритет? IT Specialist може встановлювати та змінювати priority.
- Які є пріоритети? Low, Medium, High, Critical. До встановлення пріоритет має значення Not Set.
- Який пріоритет отримує заявка під час створення? Значення не встановлене (Not Set).
- Які статуси? Open, In Progress, Resolved, Closed.
- Хто може призначати виконавця на заявку? IT Specialist.
- Хто може змінювати статус? IT Specialist, відповідальний за заявку.
- Чи може Employee переглядати заявки інших? Ні — Employee бачить лише власні заявки.
- Чи може Employee коментувати заявки інших співробітників? Ні — Employee може залишати коментарі лише до власних заявок.
- Що означає «важлива» заявка? Priority = High або Critical.
- Чи потрібна історія змін? Ні.

---

## 2. Product Vision + Initial Product Scope

**Вхід:** Problem Statement + Product Goals + Stakeholders + Primary Users + Constraints + Assumptions

**Вихід:** Product Vision + Initial Product Scope + Initial Product Capabilities

### Product Vision

IT Helpdesk — внутрішня вебсистема для співробітників і IT-відділу компанії, призначена для реєстрації, відстеження та координації вирішення технічних звернень.

### Initial Product Scope

**In Scope:**

- реєстрація та відстеження заявок протягом їх життєвого циклу;
- організація відповідальності та пріоритетності роботи над заявками;
- взаємодія між Employee та IT Specialist у межах конкретної заявки;
- допомога у визначенні заявок, що потребують першочергової уваги;
- інтелектуальна підтримка: автоматичні рекомендації щодо категорії та пріоритету заявки.

**Out of Scope на поточному етапі:**

- Customer support portal
- Billing
- Active Directory / OAuth / SSO
- Email / Push-сповіщення
- SLA-моніторинг та аналітичні дашборди
- Mobile application
- Інтеграції з зовнішніми системами (Jira, Slack тощо)

### Initial Product Capabilities

| ID | Product Capability | Що означає на цьому рівні |
|---|---|---|
| C-01 | Ticket Registration & Repository | Реєструвати звернення та зберігати їх у єдиному місці для подальшого перегляду |
| C-02 | Ticket Lifecycle Tracking | Відстежувати стан заявки протягом її життєвого циклу |
| C-03 | Work Assignment & Prioritization | Визначати відповідального та відносну важливість заявки |
| C-04 | Team Collaboration | Підтримувати взаємодію між Employee та IT Specialist навколо конкретної заявки |
| C-05 | Ticket Triage & Focus | Допомагати IT-відділу швидко визначати заявки, що потребують першочергової уваги |
| C-06 | AI-assisted Triage | Допомагати автоматично класифікувати заявки та рекомендувати їхній пріоритет |

---

## 3. Stakeholder Needs & Stakeholder Requirements

**Вхід:** Problem Statement + Stakeholders + Primary Users + Product Vision + Initial Product Scope + Initial Product Capabilities + Constraints + Assumptions + Open Questions

**Вихід:** Stakeholder Needs (-> Stakeholder Requirements) + Business Rules known at this level + Stakeholder Constraints + Refined Product Scope + Remaining Open Questions

1. SR-01 — Register ticket. Співробітник повинен мати можливість зареєструвати технічне звернення в системі.
2. SR-02 — View tickets. Співробітник повинен мати можливість переглядати свої заявки; IT Specialist — усі заявки системи.
3. SR-03 — Assign responsibility. IT Specialist повинен мати можливість призначити відповідального виконавця на заявку.
4. SR-04 — Prioritize tickets. IT Specialist повинен мати можливість визначати відносну важливість заявки.
5. SR-05 — Track progress. Користувачі повинні бачити поточний стан роботи над заявкою.
6. SR-06 — Discuss ticket. Користувачі повинні мати можливість обговорювати заявку за допомогою коментарів.
7. SR-07 — Change ticket status. IT Specialist, відповідальний за заявку, повинен мати можливість змінювати її статус.
8. SR-08 — Identify important tickets. IT-відділ повинен мати можливість швидко знаходити важливі невирішені заявки.
9. SR-09 — Attach files to a ticket. Користувач повинен мати можливість прикріплювати файли до заявки, щоб надавати скріншоти, журнали помилок та інші матеріали.
10. SR-10 — Automatic ticket categorization. Система повинна допомагати користувачу визначати категорію звернення.
11. SR-11 — Automatic priority recommendation. Система повинна допомагати користувачу визначати пріоритет звернення.

### Business Rules

BR-01 — Authorized users  
Системою можуть користуватися лише автентифіковані співробітники компанії. У першій версії система має заздалегідь визначених користувачів із двома ролями: Employee та IT Specialist.

BR-02 — Ticket creation  
Будь-який автентифікований Employee може створити заявку.

BR-02.1 — Ticket category  
При створенні заявки Employee обирає категорію звернення. Допустимі значення: Hardware, Software, Access. Категорія є обов'язковим полем. IT Specialist може змінити категорію після створення заявки.

BR-03 — Initial ticket status  
Під час створення нового ticket система автоматично встановлює status = Open.

BR-04 — Initial ticket priority  
Під час створення нового ticket його priority не визначений: priority = Not Set.

BR-05 — Allowed priority values  
Після встановлення priority повинно мати одне з таких значень: Low, Medium, High, Critical. До встановлення priority допускається стан Not Set.

BR-06 — Priority management  
Встановлювати та змінювати priority може лише IT Specialist.

BR-07 — Allowed status values  
Status ticket може мати одне з таких значень: Open, In Progress, Resolved, Closed.

BR-08 — Ticket status management  
Змінювати статус заявки може лише IT Specialist. Employee не має права змінювати статус жодної заявки, навіть власної. Серед IT Specialist'ів змінювати статус може лише той, кого призначено виконавцем.

BR-08.1 — Status transitions: Open → In Progress, In Progress → Resolved, Resolved → Closed. Інші переходи не дозволені.

BR-09 — Important ticket  
Ticket вважається важливим, якщо його priority має значення High або Critical.

BR-09.1 — Ticket зі статусом Resolved вважається вирішеним, але ще не закритим. До переходу в Closed така заявка залишається активною та може відображатися серед Important Tickets відповідно до її priority.

BR-10 — Ticket deletion  
Tickets не видаляються із системи.

BR-11 — Change history  
У першій версії системи окрема користувацька історія змін ticket не передбачається.

BR-12 — AI recommendations  
AI-функціональність, якщо вона реалізована, має рекомендаційний характер: система може рекомендувати категорію та пріоритет ticket, але не повинна автоматично змінювати ці значення без дії користувача.

---

## 4. Prioritization of Needs + Candidate MVP

**Вхід:** Product Goals + Refined Product Scope + Stakeholder Requirements + Business Value + Dependencies + Risks + Constraints + Rough technical estimates

**Вихід:** Prioritized Stakeholder Requirements + Candidate MVP + Deferred Requirements + Future Requirements + Prioritization Rationale

### Prioritized Stakeholder Requirements

| Requirement | Priority | Rough Effort |
|---|---|---|
| SR-01 Register ticket | Must | Small |
| SR-02 View tickets | Must | Small |
| SR-03 Assign responsibility | Must | Small |
| SR-04 Prioritize tickets | Must | Small |
| SR-05 Track progress | Must | Small |
| SR-06 Discuss ticket | Should | Medium |
| SR-07 Change ticket status | Must | Small |
| SR-08 Identify important tickets | Should | Small |
| SR-09 Attach files to a ticket | Should | Large |
| SR-10 Automatic ticket categorization | Won't for now | Large / uncertain |
| SR-11 Automatic priority recommendation | Won't for now | Large / uncertain |

### Candidate MVP

Must + Should (ті з Should, що плануємо дозволити собі): з SR-01 по SR-09.

### Future / Deferred Requirements

SR-10 та SR-11.

---

## 5. Detailed User Stories & Acceptance Criteria

**Вхід:** Prioritized Stakeholder Requirements + Candidate MVP + Known Constraints + Relevant Assumptions + Open Questions + Primary Users

**Вихід:** Detailed FR або User Stories + Acceptance Criteria

### Epic 1 — Ticket Management

Як компанія, ми хочемо реєструвати та відстежувати технічні звернення співробітників в єдиній системі.

---

**Story US-01 — Create Ticket**

As an Employee,  
I want to create a ticket with a subject and description,  
So that a problem is recorded and can be tracked by the IT team.

Acceptance Criteria:

- AC-01.1. Користувач може ввести: Subject, Description, Category.
- AC-01.2. Subject є обов'язковим.
- AC-01.3. Description є обов'язковим.
- AC-01.4. Category є обов'язковим; допустимі значення: Hardware, Software, Access.
- AC-01.5. Після створення заявка отримує: ID, status = Open, created_at, author.
- AC-01.6. Після створення ticket його priority не встановлено (Not Set).

```gherkin
Scenario: Successful ticket creation
Given the user is authenticated
And the user is on the "New Ticket" page
When the user enters the subject "Printer not working"
And enters the description "The HP printer in room 204 does not respond"
And selects the category "Hardware"
And clicks "Create"
Then a new ticket should be created
And the ticket status should be "Open"
And the ticket should have a unique ID
And the ticket should contain the current user as its author

Scenario: Ticket creation fails when Category is not selected
Given the user is creating a new ticket
When the user fills in the Subject and Description
But does not select a Category
And clicks "Create"
Then the ticket should not be created
And the system should display "Category is required"

Scenario: Ticket creation fails when Subject is empty
Given the user is creating a new ticket
When the user leaves the Subject field empty
And clicks "Create"
Then the ticket should not be created
And the system should display "Subject is required"
```

---

**Story US-02 — View Tickets**

As a user,  
I want to view registered tickets,  
So that I can see which problems have been reported and inspect their current details.

Acceptance Criteria:

- AC-02.1 — View ticket list. Автентифікований Employee може переглянути список лише своїх заявок. IT Specialist бачить усі заявки системи.
- AC-02.2 — Ticket isolation. Employee не має доступу до заявок інших співробітників навіть при прямому переході за URL.
- AC-02.3 — Ticket information in the list. Для кожного ticket у списку відображаються щонайменше: ID, Subject, Category, Status, Priority, Assignee (якщо призначений).
- AC-02.4 — View ticket details. Користувач може вибрати ticket зі списку та відкрити сторінку його деталей.
- AC-02.5 — Ticket details. На сторінці ticket відображаються: ID, Subject, Description, Category, Status, Priority, Assignee (якщо призначений), Author, Created at.
- AC-02.6 — Empty list. Якщо заявок немає, система відображає порожній список або повідомлення про відсутність tickets.

---

**Story US-03 — Assign Ticket to the Responsible IT Specialist**

As an IT Specialist,  
I want to assign a ticket to a team member,  
So that it is clear who is responsible for its resolution.

Acceptance Criteria:

- AC-03.1. Можна вибрати одного assignee.
- AC-03.2. Assignee повинен бути користувачем із роллю IT Specialist.
- AC-03.3. Призначати виконавця може лише IT Specialist.
- AC-03.4. Поточний assignee відображається у заявці.

---

**Story US-04 — Set Ticket Priority**

As an IT Specialist,  
I want to set the priority of a ticket,  
So that the team can determine the order of work.

Acceptance Criteria:

- AC-04.1. Допустимі значення встановленого priority: Low, Medium, High, Critical.
- AC-04.2. Встановлювати та змінювати priority може лише IT Specialist.

---

**Story US-05 — Track Ticket Progress**

As a user,  
I want to see the current status of a ticket,  
So that I can understand the current progress of work on the issue.

Acceptance Criteria:

- AC-05.1. Поточний status відображається для кожного ticket.
- AC-05.2. Status відображається на сторінці ticket.
- AC-05.3. Status відображається у списку tickets.

---

**Story US-06 — Add Comment**

As a user,  
I want to add comments to a ticket,  
So that I can discuss the problem with the IT Specialist.

Acceptance Criteria:

- AC-06.1. Comment text cannot be empty.
- AC-06.2. Added comment is stored and shown for the ticket.
- AC-06.3. Comment author is stored and displayed.
- AC-06.4. Creation date/time is stored and displayed.
- AC-06.5. Failed save does not create a comment and produces an error.
- AC-06.6. Employee може залишати коментарі лише до власних заявок. Спроба залишити коментар до чужої заявки через прямий запит повертає помилку доступу.

---

**Story US-07 — Update Ticket Status**

As an IT Specialist responsible for a ticket,  
I want to change the status of the ticket,  
So that the team can see the current state of work on the issue.

Acceptance Criteria:

- AC-07.1. Нова заявка створюється зі статусом Open.
- AC-07.2. Допустимі статуси: Open, In Progress, Resolved, Closed.
- AC-07.3. Після зміни статусу система зберігає нове значення та відображає його при наступному перегляді заявки.
- AC-07.4. Employee не може змінювати статус жодної заявки. IT Specialist, який не є виконавцем заявки, також не може змінити її статус.
- AC-07.5. Система не дозволяє встановити значення статусу, якого немає у визначеному переліку.
- AC-07.6. Перехід Resolved → In Progress не дозволений. На відміну від деяких систем, повторне відкриття заявки не передбачено.

```gherkin
Scenario: Invalid status transition is blocked
Given I am logged in as IT Specialist
And ticket #15 has status "Resolved"
When I attempt to set the status to "Open"
Then the option "Open" is not available in the status dropdown
And the status of ticket #15 remains "Resolved"

Scenario: Employee cannot change ticket status
Given I am logged in as Employee
And I am viewing my own ticket #7 with status "Open"
When I attempt to change the status to "In Progress"
Then the status control is not available
And the status of ticket #7 remains "Open"
```

---

**Story US-08 — View Important Open Tickets**

As a user,  
I want to quickly see important open tickets,  
So that I can focus on issues that require immediate attention.

Acceptance Criteria:

- AC-08.1. Заявка з priority = High і статусом, відмінним від Closed, відображається у списку Important Tickets.
- AC-08.2. Заявка з priority = Critical і статусом, відмінним від Closed, відображається у списку Important Tickets.
- AC-08.3. Заявки з priority = Low або Medium не відображаються у списку Important Tickets.
- AC-08.4. Заявки зі status = Closed не відображаються незалежно від priority.

---

**Story US-09 — Attach Files to a Ticket**

As a user,  
I want to attach files to a ticket,  
So that I can provide screenshots, logs, and other information needed to investigate the problem.

Acceptance Criteria:

- AC-09.1 — Upload attachment. Автентифікований користувач, який має доступ до ticket, може завантажити файл і прикріпити його до цього ticket.
- AC-09.2 — Persistent storage. Після успішного завантаження attachment повинен залишатися доступним після перезапуску застосунку та повторного входу користувача.
- AC-09.3 — File size limit. Якщо розмір файлу перевищує встановлений максимально допустимий розмір, система не повинна завантажувати файл і повинна повідомити користувача про перевищення ліміту.

```gherkin
Given the maximum attachment size is defined by the system
When the user uploads a file that exceeds the limit
Then the file is rejected
And the user sees "Maximum attachment size exceeded"
```

- AC-09.4 — Antivirus scanning. Кожен завантажений файл повинен бути перевірений на шкідливий вміст до того, як він стане доступним іншим користувачам.

```gherkin
Given the user uploads an attachment
When the upload is completed
Then the file is scanned for malware
And the file is not available for download until the scan succeeds
```

- AC-09.5 — Infected file. Файл, визначений як шкідливий, не повинен ставати доступним для завантаження, повинен бути позначений як відхилений, а користувач — отримати відповідне повідомлення.
- AC-09.6 — Download integrity. Користувач повинен отримати саме той attachment, який був успішно завантажений до ticket, без неконтрольованої модифікації його вмісту.

**Dependencies:**  
US-08 (View Important Open Tickets) → US-04 (Set Ticket Priority), US-07 (Update Ticket Status)

---

## 6. Quality Attributes + Security Requirements (NFR)

**Вхід:** Functional requirements + Business constraints

**Вихід:** Non-functional / Quality / Security Requirements

- NFR-01. Response time — сторінки системи повинні завантажуватися своєчасно при нормальному навантаженні. Перевірка: ручне або автоматизоване вимірювання часу відповіді.
- NFR-02. Authentication required — доступ до будь-якої сторінки системи, крім сторінки входу, можливий лише для автентифікованих користувачів. Перевірка: спроба відкрити захищені URL без сесії повинна призводити до перенаправлення на login.
- NFR-03. HTTPS only — уся передача даних між клієнтом і сервером має відбуватися виключно по HTTPS. Перевірка: перевірка наявності SSL/TLS-сертифікату та налаштувань сервера.
- NFR-04. Backup of all data — усі дані системи повинні регулярно резервуватися. Перевірка: наявність та відновлення з резервної копії.

---

## 7. Feasibility, Estimation & Risk Assessment

**Вхід:** Candidate MVP + Detailed FR (або User Stories + AC) + NFR + Business Rules + Constraints + Relevant Assumptions + Known dependencies

**Вихід:** Rough Effort Assessment + Key Risks + Technical Feasibility Assessment + Recommended Scope Adjustments

Попередня оцінка показала, що вимоги US-01–US-08 можуть бути реалізовані в межах першого релізу без складної додаткової інфраструктури.

US-09 — Attach files to a ticket має значно вищу складність, оскільки потребує:
- file storage;
- file size limits;
- malware scanning;
- backup.

### Основні ризики

- R-01: спосіб реалізації authentication потребує уточнення.
- R-02: для майбутніх AI-функцій може бути недостатньо навчальних даних.
- R-03: реалізація attachments суттєво збільшує складність першого релізу.

### Technical Feasibility

Реалізація US-01–US-08 оцінюється як технічно здійсненна для невеликої web application із заздалегідь визначеними користувачами та стандартними CRUD-операціями.

### Recommended Scope Adjustment

Перенести US-09 — Attach files to a ticket за межі першого релізу.

---

## 8. MVP / Release Scope

**Release MVP = US-01..US-08**

**Deferred:**
- US-09 — Attach files to a ticket

**Future:**
- SR-10 — Automatic ticket categorization
- SR-11 — Automatic priority recommendation

### Updated NFRs

- NFR-01. Response time
- NFR-02. Authentication required
- NFR-03. HTTPS only
- NFR-04. Backup of all data
