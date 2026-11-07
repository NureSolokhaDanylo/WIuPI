# 04. Перевірка плану до запуску Figma (Етап G4 + Контроль B)

* **Проєкт:** IT Helpdesk (Release MVP `US-01..US-08`)
* **Оператор G4 (AI Review):** Долгополов Ярослав (модель: `4 Argon, Reasoning: High`)
* **Рецензент Контролю B (Human Review):** Солоха Данило
* **Дата перевірки:** 07.11.2026
* **Перевірені документи:** `source/requirements.md`, `01_Input_Pack_v1.0.md`, `02_Decision_Log_and_Screen_Map_v1.0.md`, `03_Screen_Contracts_and_Traceability_v1.0.md`
* **Підсумковий вердикт:** `PLAN READY` (після усунення 3 дефектів чернетки `DEF-PLAN-01..03`)

## 1. Матриця покриття вимог у плані (Coverage Matrix — 37 AC)

Під час звірки з вихідною специфікацією `requirements.md` підтверджено, що всі 8 включених User Stories та всі 37 Acceptance Criteria мають явне місце в `Screen Contracts` та `Traceability Matrix`:

| User Story | Критерії (AC ID) | Статус покриття у плані v1.0 | Доказ у плані (Screen / Control / Test ID) |
| :--- | :--- | :---: | :--- |
| **US-01 Create Ticket** | `AC-01.1`, `AC-01.2`, `AC-01.3`, `AC-01.4`, `AC-01.5`, `AC-01.6` | `COVERED (6/6)` | `M-01` (`C-05..C-09`), `S-03` (автоматичні поля), `REF-01` (3 стани валідації включно з `"Category is required"`), тести `T-01`, `T-02`. |
| **US-02 View Tickets** | `AC-02.1`, `AC-02.2`, `AC-02.3`, `AC-02.4`, `AC-02.5`, `AC-02.6` | `COVERED (6/6)` | `S-02-A` (IT All), `S-02-D` (Employee Isolated), `S-02-C` (Empty), `S-03` (9 полів деталей), `REF-01` (`403 Access Denied`), тести `T-01`, `T-08`, `T-09`. |
| **US-03 Assign Ticket** | `AC-03.1`, `AC-03.2`, `AC-03.3`, `AC-03.4` | `COVERED (4/4)` | `S-03` (`C-13, C-14`), `REF-01` (4 рольові контексти, список лише з `IT Specialist`), тест `T-03`. |
| **US-04 Set Ticket Priority** | `AC-04.1`, `AC-04.2` | `COVERED (2/2)` | `S-03` (`C-15, C-16`: опції `Low, Medium, High, Critical` без `Not Set`), `REF-01`, тест `T-04`. |
| **US-05 Track Ticket Progress** | `AC-05.1`, `AC-05.2`, `AC-05.3` | `COVERED (3/3)` | Колонка `Status` у `S-02-A/B/D` та поле `Status` у `S-03`, тест `T-05`. |
| **US-06 Add Comment** | `AC-06.1`, `AC-06.2`, `AC-06.3`, `AC-06.4`, `AC-06.5`, `AC-06.6` | `COVERED (6/6)` | Секція `Comments` у `S-03` (`C-19, C-20`) + стани валідації, помилки збереження та ізоляції чужих заявок у `REF-01`, тести `T-07`, `T-09`. |
| **US-07 Update Ticket Status** | `AC-07.1`, `AC-07.2`, `AC-07.3`, `AC-07.4`, `AC-07.5`, `AC-07.6` | `COVERED (6/6)` | `S-03` (`C-17, C-18`), матриця з **3 переходів** без `Resolved → In Progress` у `REF-01`, тести `T-05`, `T-06`. |
| **US-08 View Important Open Tickets** | `AC-08.1`, `AC-08.2`, `AC-08.3`, `AC-08.4` | `COVERED (4/4)` | `S-02-B` (рівно 4 заявки: `TCK-001, 002, 003, 007`; виключення `Closed` та `Low/Medium/Not Set`), тест `T-08`. |

## 2. Дефекти, виявлені під час перевірки чернетки плану (`v0.1` → `v1.0`)

Під час критичного прогону промпту **G4** (модель `4 Argon, Reasoning: High`) та перехресного рев'ю **Контролю B** (рецензент: **Солоха Данило**) у первинних чернетках [`docs/drafts/02_Screen_Map_v0.1.md`](./drafts/02_Screen_Map_v0.1.md) та [`docs/drafts/03_Screen_Contracts_v0.1.md`](./drafts/03_Screen_Contracts_v0.1.md) було знайдено й виправлено 3 дефекти:

| DEF-ID | Клас і критичність | Джерело вимоги | Де виявлено у чернетці `v0.1` | Опис проблеми | Як виправлено у `v1.0` |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **DEF-PLAN-01** | Вимоговий дефект (`Major`) | `BR-08.1`, `AC-07.6` | `02_Screen_Map_v0.1` (Таблиця переходів) та `03_Screen_Contracts_v0.1` (Матриця статусів) | Модель `Gemini 3.8 Flash` при генерації чернетки G2 за шаблоном стандартних баг-трекерів додала 4-й перехід `Resolved → In Progress` (повторне відкриття заявки в роботу), хоча в `AC-07.6` нашої специфікації прямо вказано: *«Перехід Resolved → In Progress не дозволений. Повторне відкриття заявки не передбачено»*. | Вилучено перехід `Resolved → In Progress` з таблиці переходів і матриці статусів. У стані `Resolved` єдиним дозволеним переходом залишено `Resolved → Closed`. |
| **DEF-PLAN-02** | Вимоговий дефект (`Major`) | `AC-01.4`, Gherkin `US-01` | `03_Screen_Contracts_v0.1` (Контракт `M-01`) | У переліку станів валідації модального вікна `M-01` модель вказала лише помилки для порожніх `Subject` та `Description`, забувши обов'язковий Gherkin-сценарій `US-01`: *"Ticket creation fails when Category is not selected -> display 'Category is required'"*. | Додано обов'язковий стан валідації `"Category is required"` у контракт `M-01` та секцію `REF-01`. |
| **DEF-PLAN-03** | Дефект повноти представлення (`Minor`) | `AC-02.1`, `AC-02.2`, `AC-06.6` | `02_Screen_Map_v0.1` (Опис `S-02`) | У чернетці G2 було описано лише загальний список `All Tickets` (як для `IT Specialist`), але не було показано, як візуально в макеті демонструється ізоляція списку для ролі `Employee` (`My Tickets`) та блокування доступу до чужої заявки. | Додано демонстраційний стан `S-02-D (My Tickets — Employee Isolated View)` для Олени Коваль (`TCK-001, 003, 005, 006`) та картку `403 Access Denied` у `REF-01`. |

## 3. Перевірка блокувальних питань перед генерацією Figma

* Усі рішення `D-01..D-08` мають статус `APPROVED`.
* Жодних відкритих блокерів (`UNRESOLVED Q-ID`) не залишилося.
* Технічні вимоги `NFR-01` (Response time), `NFR-03` (HTTPS only), `NFR-04` (Backup) коректно позначені як такі, що не створюють фейкових екранів у макеті.

## 4. Запис Контролю B (Затвердження людиною)

* **Reviewer:** Солоха Данило (Учасник №1)
* **Author of Plan:** Долгополов Ярослав (Учасник №2)
* **Date:** 07.11.2026
* **Reviewed Version:** `02_Decision_Log_and_Screen_Map_v1.0.md`, `03_Screen_Contracts_and_Traceability_v1.0.md`
* **Accepted Decisions:** `D-01` – `D-08`
* **Open Blockers:** `0 (None)`
* **Decision:** **PLAN READY — дозволено компіляцію `05_Figma_Brief_v1.0.md` (етап G5).**
