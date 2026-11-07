# IT Helpdesk — Approved Low-Fidelity Design Brief v1.0

## 1. Purpose and Scope
Internal desktop web service desk application (`IT Helpdesk`) for company employees and the IT department to register, track, prioritize, assign, and resolve technical support tickets.
Include Release MVP `US-01` to `US-08` only: create ticket with category, view ticket lists (with strict role isolation), view ticket details, assign IT Specialist, set priority, change category, update status along a strict 3-step lifecycle, add comments, and view important non-closed tickets.
Use four fictional predefined users across two system roles (`BR-01`, `D-02`):
- `Employee A`: Olena Koval
- `Employee B`: Maksym Bondar
- `IT Specialist 1`: Ihor Melnyk
- `IT Specialist 2`: Anna Savchenko

`Assignee` is a ticket-specific relationship assigned exclusively to an `IT Specialist` (`AC-03.2`), not a separate global role.

## 2. Design Constraints and Output Format
Create editable native Figma layers on a single working page (`1440px` desktop frames). Use grayscale, low-fidelity wireframes, clear typographic hierarchy, reusable local components (`Button`, `Input/Select`, `Status Badge`, `Priority Badge`, `Category Badge`, `Ticket Row`), and Auto Layout where practical.
Preserve exact Screen/State IDs in frame titles (`S-01`, `S-02-A`, `S-02-B`, `S-02-C`, `S-02-D`, `M-01`, `S-03`, `REF-01`).
Place all reviewer annotations, permission matrices, status transition tables, and error/validation examples OUTSIDE product frames inside the dedicated `REF-01` reference section.
Do NOT add decorative page icons, empty square icon placeholders next to headings, row-selection checkboxes, bulk actions, or search/filter bars (`D-01`).
Create the canonical frames plus compact state examples in `REF-01`; do NOT generate 16 full-screen copies for every role and status combination.
Prototype navigation is annotated only (`D-08`); do NOT wire interactive prototype connections or implement backend logic.

## 3. Canonical Screens and States

### S-01 — Authentication Boundary [BR-01; NFR-02; KC-05; D-02]
A neutral boundary frame labeled `"IT Helpdesk — Authentication entry (mechanism TBD; predefined roles: Employee, IT Specialist)"`. Do not invent password inputs, registration forms, OAuth buttons, or SSO controls. The main product demonstration starts from `S-02` as an already authenticated user.

### S-02 — Tickets List [US-02; US-05; US-08; D-01; D-07]
Top header displays application title `"IT Helpdesk"`, active user badge (e.g., `"Logged in as: Ihor Melnyk (IT Specialist)"` or `"Olena Koval (Employee)"`), and primary action button `"+ Create Ticket"`.
Navigation tabs below header:
- For `IT Specialist`: `"All Tickets"` and `"Important Tickets"`.
- For `Employee`: `"My Tickets"` and `"Important Tickets"` (filtered strictly to the employee's own tickets per `AC-02.1`).

Table columns in every list state must show at least (`AC-02.3`, `AC-05.3`):
`ID` | `Subject` | `Category` | `Status` | `Priority` | `Assignee` (show `"Unassigned"` when empty).
Clicking a ticket `Subject` or row opens `S-03 Ticket Details`. No row-selection checkboxes or delete buttons.

Represent four list states using separate demonstration frames of this ONE product screen:
1. `S-02-A — Tickets List (All Tickets — IT Specialist View)`: Shows all 8 sample tickets (`TCK-001` to `TCK-008`).
2. `S-02-B — Tickets List (Important Tickets — IT Specialist View)`: Filtered view of the same dataset where `(Priority is High or Critical) AND Status != Closed` (`AC-08.1..08.4`, `BR-09`, `BR-09.1`). Shows `TCK-001`, `TCK-002`, `TCK-003` (`High + Resolved`), and `TCK-007` (`Critical + Resolved`) only.
3. `S-02-C — Tickets List (Empty State)`: Shows empty table state with clear message `"No tickets registered yet. Click '+ Create Ticket' to submit a support request"` (`AC-02.6`).
4. `S-02-D — Tickets List (My Tickets — Employee Olena Koval View)`: Demonstrates ticket isolation (`AC-02.1`). Shows only Olena Koval's 4 tickets (`TCK-001`, `TCK-003`, `TCK-005`, `TCK-006`). Tickets created by Maksym Bondar are hidden.

### Canonical Sample Dataset (Use consistently across all frames)
- `TCK-001` | `"VPN connection drops every 10 minutes"` | Category: `Access` | Status: `Open` | Priority: `Critical` | Author: `Olena Koval` | Assignee: `Ihor Melnyk` -> Included in Important: **YES**
- `TCK-002` | `"Accounting database timeout on export"` | Category: `Software` | Status: `In Progress` | Priority: `High` | Author: `Maksym Bondar` | Assignee: `Ihor Melnyk` -> Included in Important: **YES**
- `TCK-003` | `"Shared folder permissions missing for HR"` | Category: `Access` | Status: `Resolved` | Priority: `High` | Author: `Olena Koval` | Assignee: `Anna Savchenko` -> Included in Important: **YES** (`Resolved` is not `Closed` per `BR-09.1`)
- `TCK-004` | `"Main office router failure in building B"` | Category: `Hardware` | Status: `Closed` | Priority: `Critical` | Author: `Maksym Bondar` | Assignee: `Ihor Melnyk` -> Included in Important: **NO** (`Closed` excluded per `AC-08.4`)
- `TCK-005` | `"Printer not working in room 204"` | Category: `Hardware` | Status: `Open` | Priority: `Medium` | Author: `Olena Koval` | Assignee: `Unassigned` -> Included in Important: **NO** (`Medium` excluded per `AC-08.3`)
- `TCK-006` | `"Need Adobe Acrobat license installed"` | Category: `Software` | Status: `Open` | Priority: `Not Set` | Author: `Olena Koval` | Assignee: `Unassigned` -> Included in Important: **NO** (`Not Set` excluded)
- `TCK-007` | `"Laptop screen flickering after BIOS update"` | Category: `Hardware` | Status: `Resolved` | Priority: `Critical` | Author: `Maksym Bondar` | Assignee: `Ihor Melnyk` -> Included in Important: **YES**
- `TCK-008` | `"Keyboard key stuck on workstation #14"` | Category: `Hardware` | Status: `In Progress` | Priority: `Low` | Author: `Maksym Bondar` | Assignee: `Anna Savchenko` -> Included in Important: **NO** (`Low` excluded per `AC-08.3`)

### M-01 — Create Ticket Modal Overlay [US-01; BR-02..04; D-01; D-04]
Modal dialog centered over `S-02`.
Editable inputs (ONLY these three fields are user-editable at creation per `AC-01.1..01.4`):
1. `Subject *` (required single-line text input).
2. `Category *` (required dropdown select; default placeholder `"Select category..."`; valid options: `Hardware`, `Software`, `Access` only per `BR-02.1`, `AC-01.4`).
3. `Description *` (required multiline textarea).
Actions: `Create` (primary button) and `Cancel` (secondary button).

Do NOT include inputs for `Status`, `Priority`, `Assignee`, `Author`, `ID`, or `Created at` inside `M-01`. Upon valid creation, the system automatically sets `ID`, `Status = Open` (`BR-03`, `AC-01.5`), `Priority = Not Set` (`BR-04`, `AC-01.6`), `Assignee = Unassigned` (`D-04`), `Author = current user`, and `Created at = current timestamp`.
Represent validation states in `REF-01` (or compact modal state cards):
- Missing Subject: `"Subject is required"` (exact Gherkin copy from `US-01`).
- Missing Category: `"Category is required"` (exact Gherkin copy from `US-01`).
- Missing Description: `"Description is required"` (`AC-01.3`).
Invalid submission never creates a ticket.

### S-03 — Ticket Details [US-02..US-07; BR-02.1; BR-05..08.1; D-03..D-06]
Provide navigation link `"← Return to Tickets List"` at top left.
Display all 9 required ticket attributes (`AC-02.5`, `AC-03.4`, `AC-05.2`):
`ID`, `Subject`, `Description`, `Category`, `Status`, `Priority`, `Assignee`, `Author`, `Created at`.
Keep `Category`, `Assignee`, `Priority`, and `Status` controls cleanly grouped without decorative `"Control Actions Area"` headings. Each editable dropdown for `IT Specialist` has an adjacent `Apply` button (`D-06`), and applying a change keeps the user on `S-03`.

Canonical `S-03` frame scenario:
Show `IT Specialist 1 (Ihor Melnyk)` viewing `TCK-003` (`"Shared folder permissions missing for HR"`, `Category = Access`, `Status = Resolved`, `Priority = High`, `Author = Olena Koval`, `Assignee = Anna Savchenko`).
In this canonical frame:
- `Category` dropdown (`Hardware`, `Software`, `Access`) + `Apply` is **ENABLED** (`BR-02.1`: IT Specialist can change category after creation).
- `Assignee` dropdown (`Ihor Melnyk (IT Specialist)`, `Anna Savchenko (IT Specialist)`) + `Apply` is **ENABLED** (`US-03`: only IT Specialists can be selected as assignee).
- `Priority` dropdown (`Low`, `Medium`, `High`, `Critical`) + `Apply` is **ENABLED** (`US-04`, `BR-06`).
- `Status` dropdown (`Resolved`) + `Apply` is **DISABLED** with helper note `"Only the assigned IT Specialist (Anna Savchenko) can change status"` (`BR-08`, `AC-07.4`, `D-05`).

### Priority Rules [BR-04; BR-05; AC-04.1; AC-04.2; D-03]
Display states: `Not Set`, `Low`, `Medium`, `High`, `Critical`.
Selectable dropdown options for `IT Specialist`: `Low`, `Medium`, `High`, `Critical` ONLY. `Not Set` represents the initial absence of priority (`BR-04`), never a selectable dropdown option or reset action (`D-03`).

### Status Rules and Strict 3-Transition Lifecycle [BR-07; BR-08; BR-08.1; US-07; AC-07.4; AC-07.6]
Allowed status values: `Open`, `In Progress`, `Resolved`, `Closed`.
Only the `IT Specialist` who is the **current Assignee** of the ticket can change its `Status` (`BR-08`, `AC-07.4`). `Employee` cannot change status on any ticket, even their own (for `Employee`, status control is not available / shown as read-only badge per Gherkin `US-07` and `D-05`). If a ticket is `Unassigned`, status cannot be changed until an `IT Specialist` is assigned.
Allowed status transitions (ONLY 3 transitions exist per `BR-08.1` and `AC-07.6`):
1. `Open -> In Progress`
2. `In Progress -> Resolved`
3. `Resolved -> Closed`
**CRITICAL RULE (`AC-07.6`):** Transition `Resolved -> In Progress` (reopening a ticket) and `Resolved -> Open` are **STRICTLY PROHIBITED**. When a ticket is in `Resolved`, the only selectable next status for the assigned IT Specialist is `Closed`. When a ticket is in `Closed`, there are no outgoing status transitions (`disabled`).

### Comments Section on S-03 [US-06; AC-06.1..06.6]
Show existing saved comments list: each entry displays `Comment author` (name + role), `Creation date/time`, and `Comment text` (`AC-06.2..06.4`).
Show `"Add Comment"` form: multiline `Comment text` input and `"Add Comment"` button.
In `REF-01`, show compact comment state examples:
- Empty comment validation: `"Comment text cannot be empty"` (`AC-06.1`), no comment added.
- Save failure state: error banner `"Failed to save comment. Please try again."` (`AC-06.5`), failed comment is NOT added to the saved list.

## 4. REF-01 — Reviewer & State Reference Section (Outside Product Frames)
Place `REF-01` clearly outside the product frames on the canvas. Include:
1. **Four Role & Relationship Context Cards for `S-03` Controls:**
   - **Context 1 — `Employee (Own Ticket, e.g., Olena Koval on TCK-001)`:** Can view details and add comments; `Category`, `Assignee`, `Priority`, and `Status` are read-only badges (`status control not available` per Gherkin `US-07`, `D-05`).
   - **Context 2 — `Employee (Another Employee's Ticket, e.g., Olena Koval requesting TCK-002 via direct URL)`:** Blocked with `"403 Access Denied — Employees can only view and comment on their own tickets"` (`AC-02.2`, `AC-06.6`).
   - **Context 3 — `IT Specialist (Non-assignee, e.g., Ihor Melnyk on TCK-003 assigned to Anna Savchenko)`:** `Category`, `Assignee`, `Priority` controls ENABLED; `Status` control DISABLED (`AC-07.4`).
   - **Context 4 — `IT Specialist (Current Assignee, e.g., Anna Savchenko on TCK-003)`:** `Category`, `Assignee`, `Priority`, AND `Status` controls ENABLED.
2. **All Four Status Control States (for Current Assignee):**
   - `Open` -> dropdown offers `In Progress` only.
   - `In Progress` -> dropdown offers `Resolved` only.
   - `Resolved` -> dropdown offers `Closed` only (explicitly note: `In Progress` and `Open` are absent per `AC-07.6`).
   - `Closed` -> control disabled (`"Closed — no further transitions"`).
3. **Validation & Error State Cards:**
   - `M-01` validation states (`"Subject is required"`, `"Category is required"`, `"Description is required"`).
   - `S-03` comment validation (`"Comment text cannot be empty"`) and save error (`"Failed to save comment"`).

## 5. Navigation Intent Annotations [D-01; D-07; D-08]
Annotate navigation flow in `REF-01` (do not wire interactive prototype links):
- `S-02` tabs switch between `All/My Tickets` and `Important Tickets`.
- Clicking `+ Create Ticket` on `S-02` opens `M-01`.
- Clicking `Cancel` on `M-01` closes overlay and returns to prior `S-02` state without changes.
- Valid `Create` on `M-01` opens `S-03` with new ticket defaults (`Open`, `Not Set`, `Unassigned`).
- Clicking ticket `Subject` on `S-02` opens `S-03` for that ticket.
- Clicking `← Return to Tickets List` on `S-03` returns to the previously active `S-02` tab.

## 6. Explicit Exclusions
Do NOT include: file attachments or upload dropzones (`US-09` is deferred), AI category/priority suggestions (`SR-10/SR-11` are future scope), ticket deletion (`BR-10`), change history / audit log (`BR-11`), `Resolved -> In Progress` reopen action (`AC-07.6`), subject/description editing after creation, search bars, column sorting, pagination, bulk row checkboxes, user management screens, SLA timers, notifications, or separate screens for assignment/priority/status/comments.

## 7. Completion Report Requirement
In the same Agent response after generating the frames, report:
1) Created frames and `REF-01` state blocks;
2) Reusable local components created;
3) Any requirements or states not represented;
4) Explicit limitations (confirm that static wireframes do not verify database persistence, server-side URL isolation `AC-02.2`/`AC-06.6`, authentication sessions `NFR-02`, response time `NFR-01`, HTTPS `NFR-03`, or backups `NFR-04`).
