# IT Helpdesk — UX/UI Design & Verification Repository

Репозиторій містить повний комплект артефактів проєктування, формалізації, генерації та верифікації low-fidelity UX/UI-макета для внутрішньої вебсистеми **IT Helpdesk** (Release MVP: `US-01..US-08`, 37 Acceptance Criteria). Роботу виконано за маршрутом «від специфікації до перевіреного UX/UI-макета» з використанням моделей **Gemini 3.8 Flash** і **4 Argon** (на різних рівнях міркування) та **Figma Agent** (Starter Plan).

## Склад бригади та розподіл відповідальності

| Учасник | Роль у проєкті | Основна зона відповідальності (етапи) | Роль рецензента (Peer Review) |
| :--- | :--- | :--- | :--- |
| **Солоха Данило** | Аналітик вимог та фінальний аудитор | **G1** (створення `Input Pack v0.1` → `v1.0`), **G6** (фінальний аудит макета за скріншотами `Review Report`) | **Контроль B** (перевірка `Screen Map` та `Screen Contracts` перед генерацією Figma) |
| **Долгополов Ярослав** | UX-архітектор та автор контрактів екранів | **Decision Log** (`D-01..D-08`), **G2** (`Screen Map`), **G3** (`Screen Contracts` + `Traceability` на 37 AC + тест-кейси `T-01..T-10`), **G4** (`Plan Review`) | **Контроль C** (перевірка самодостатності англомовного `Figma Brief v1.0`) |
| **Зінченко Нікіта** | Інженер Figma-брифа та верифікатор макета | **G5** (`05_Figma_Brief_v1.0.md`), **F1 + Ручний резервний маршрут після ліміту** (генерація в Figma Agent, ручний compliance review та точкові правки в Figma) | **Контроль A** (звірка повноти `Input Pack` із вихідною специфікацією `requirements.md`) |

## Посилання на Figma Design Files, відеодемонстрацію та скріншоти (ДО і ПІСЛЯ правок)

* **Figma File (View Access):** [IT Helpdesk - UX/UI MVP (Figma)](https://www.figma.com/design/aJ1vPM7Cig4MQZB6Ad0SVl/IT-Helpdesk---UX-UI-MVP?t=GPIQzaaOTqsOOqKk-1)
* **Відеозапис захисту / демонстрації (YouTube):** [Переглянути відео на YouTube](https://www.youtube.com/watch?v=PExLGBbTa7M)
* **Локальні файли Figma у репозиторії:**
  * **ДО правок (F1 Before Review):** [`IT Helpdesk - UX_UI MVP.fig`](./IT%20Helpdesk%20-%20UX_UI%20MVP.fig) та повний знімок [`IT Helpdesk - UX_UI MVP.png`](./IT%20Helpdesk%20-%20UX_UI%20MVP.png)
  * **ПІСЛЯ правок (F2 After Review):** [`IT Helpdesk - UX_UI MVP Edited.fig`](./IT%20Helpdesk%20-%20UX_UI%20MVP%20Edited.fig) та повний знімок [`IT Helpdesk - UX_UI MVP Edited.png`](./IT%20Helpdesk%20-%20UX_UI%20MVP%20Edited.png)
* **Пофреймові PNG-скріншоти (`S-01`, `S-02-A..D`, `M-01`, `S-03`, `REF-01`):**
  * **ДО правок (`before_review`):** [`screenshots/before_review/`](./screenshots/before_review/) (містять первинні дефекти генерації `DEF-FIG-01..03`)
  * **ПІСЛЯ правок (`after_review`):** [`screenshots/after_review/`](./screenshots/after_review/) (фінальна виправлена версія для аудиту G6)
* **Примітка щодо ліміту Figma Starter та прототипування (`D-08`):** Після першої повної генерації `F1` спрацював денний ліміт кредитів безкоштовного плану Figma Starter. Тому перевірка відповідності (F2-checklist), точкові правки та `Change Report` виконані за ручним резервним маршрутом. Інтерактивний клікабельний прототип за умовами завдання не вимагався (`D-08`).

## Навігація по артефактах репозиторію (Вихідні чернетки, Фінальні документи та 3 види журналів)

### 1. Специфікація та проєктні документи (`source/`, `docs/drafts/`, `docs/`)

| Шлях у репозиторії | Назва документа | Етап | Статус |
| :--- | :--- | :--- | :--- |
| [`source/requirements.md`](./source/requirements.md) | Вихідна специфікація вимог **IT Helpdesk** (незмінне джерело `[S1]`) | Крок 0 | Baseline |
| [`docs/drafts/01_Input_Pack_v0.1.md`](./docs/drafts/01_Input_Pack_v0.1.md) | Первинна AI-вибірка `Input Pack v0.1` **ДО виправлення** (`DEF-G1-01..03`) | G1 (Draft) | DRAFT v0.1 |
| [`docs/01_Input_Pack_v1.0.md`](./docs/01_Input_Pack_v1.0.md) | Виправлений UX/UI Design Input Pack + Source Check (8 US, 37 AC, BR-01..12, NFR-01..04, Q-01..06) | G1 + Контроль A | APPROVED v1.0 |
| [`docs/drafts/02_Screen_Map_v0.1.md`](./docs/drafts/02_Screen_Map_v0.1.md) | Первинна карта екранів та таблиця переходів **ДО виправлення** (`DEF-PLAN-01`, `DEF-PLAN-03`) | G2 (Draft) | DRAFT v0.1 |
| [`docs/02_Decision_Log_and_Screen_Map_v1.0.md`](./docs/02_Decision_Log_and_Screen_Map_v1.0.md) | Виправлений журнал рішень (`D-01..D-08`), карта екранів (`S-01..S-03`, `M-01`, `REF-01`), переходи (`T-01..T-13`) | G2 + Контроль B | APPROVED v1.0 |
| [`docs/drafts/03_Screen_Contracts_v0.1.md`](./docs/drafts/03_Screen_Contracts_v0.1.md) | Первинні контракти екранів **ДО виправлення** (`DEF-PLAN-01..03`) | G3 (Draft) | DRAFT v0.1 |
| [`docs/03_Screen_Contracts_and_Traceability_v1.0.md`](./docs/03_Screen_Contracts_and_Traceability_v1.0.md) | Виправлені контракти екранів, матриця прав, матриця 3 переходів статусів, Traceability (37 AC) та тести `T-01..T-10` | G3 + Контроль B | APPROVED v1.0 |
| [`docs/04_Plan_Review_v1.0.md`](./docs/04_Plan_Review_v1.0.md) | Критичний аудит плану до запуску Figma + підтвердження розробника | G4 + Контроль B | PLAN READY |
| [`docs/drafts/05_Figma_Brief_v0.9.md`](./docs/drafts/05_Figma_Brief_v0.9.md) | Первинна чернетка англомовного брифу **ДО виправлення** (`DEF-BRIEF-01..02`) | G5 (Draft) | DRAFT v0.9 |
| [`docs/05_Figma_Brief_v1.0.md`](./docs/05_Figma_Brief_v1.0.md) | Виправлений самодостатній англомовний контракт для Figma Agent (UTF-8 Markdown) | G5 + Контроль C | APPROVED v1.0 |
| [`docs/06_Figma_Prompts_and_Review_Report_v1.0.md`](./docs/06_Figma_Prompts_and_Review_Report_v1.0.md) | Лаунчер `F1`, звіт Figma Agent, ручний огляд за F2-чеклістом та фінальний аудит `G6` за скріншотами | F1, Ручний F2, G6 + Контроль D | COMPLETED |

### 2. Журнали (`logs/`)

| Шлях у репозиторії | Тип журналу | Опис вмісту | Статус |
| :--- | :--- | :--- | :--- |
| [`logs/Decision_Log.md`](./logs/Decision_Log.md) | **B1. Decision Log** | Табличний журнал погоджених UX-рішень людини (`D-01..D-08`: `PROPOSED` → `APPROVED`, джерела та змінені артефакти) | VERIFIED |
| [`logs/AI_Interaction_Log.md`](./logs/AI_Interaction_Log.md) | **B2. AI Interaction Log (Зведений)** | Зведена таблиця всіх звернень до ШІ по етапах `G1..G6`, `F1..F2` за всіма полями | VERIFIED |
| [`logs/ai_interactions/Solokha_Danylo_AI_Log.md`](./logs/ai_interactions/Solokha_Danylo_AI_Log.md) | **B2. AI Interaction Log (Учасник №1)** | Повний табличний лог та тексти промптів учасника №1 (Солоха Данило: етапи G1, G6) | VERIFIED |
| [`logs/ai_interactions/Dolhopolov_Yaroslav_AI_Log.md`](./logs/ai_interactions/Dolhopolov_Yaroslav_AI_Log.md) | **B2. AI Interaction Log (Учасник №2)** | Повний табличний лог та тексти промптів учасника №2 (Долгополов Ярослав: етапи G2, G3, G4) | VERIFIED |
| [`logs/ai_interactions/Zinchenko_Nikita_AI_Log.md`](./logs/ai_interactions/Zinchenko_Nikita_AI_Log.md) | **B2. AI Interaction Log (Учасник №3)** | Повний табличний лог, звіт Figma Agent та таблиця ручних правок учасника №3 (Зінченко Нікіта: G5, F1, F2) | VERIFIED |
| [`logs/Defect_and_Change_Log.md`](./logs/Defect_and_Change_Log.md) | **B3. Defect / Change Log** | Наскрізний табличний журнал перевірок, знайдених дефектів та доказів виправлення (`ДО` і `ПІСЛЯ`) по всіх етапах | VERIFIED |
