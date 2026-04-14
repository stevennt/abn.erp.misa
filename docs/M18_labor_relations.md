# M18 - Labor Relations (Quan he lao dong)

**Category:** HR  
**Priority:** P3  
**Reference Images:** assets/image_35.png, assets/image_37.png, assets/image_63.png  
**Dependencies:** M01 (Organization), M02 (Employee), M12 (Payroll), M14 (Recruitment), M17 (Social Insurance)  
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Labor Relations module manages the complete employee lifecycle from contract creation through employment events including appointments, transfers, resignations, dismissals, commendations, incidents, and labor disputes. It serves as the central repository for employee profiles and contracts, tracking contract types (Probation 5%, Indefinite 40%, Fixed-term 34%, Seasonal 20%), managing employment status changes, and maintaining comprehensive records of all employment-related events. The module also provides resignation analysis with reason categorization and workforce planning support.

### Personas
| Persona | Role | Primary Use |
|---|---|---|
| HR Administrator | HR staff | Manage employee profiles, contracts, employment events |
| HR Director | Executive | Approve contracts, review workforce analytics, handle disputes |
| Department Manager | Manager | Request transfers, initiate commendations/incidents, view team contracts |
| Employee | End user | View own contract, profile information |
| Legal Counsel | Legal | Review labor disputes, contract compliance, incident documentation |
| System Administrator | IT | Configure contract types, event templates, workflow rules |

### Dependencies
| Dependency | Module | Integration Point |
|---|---|---|
| Organization structure | M01 | Department hierarchy, position definitions |
| Employee master data | M02 | Employee profiles, personal information |
| Payroll | M12 | Contract salary, severance calculations |
| Recruitment | M14 | New hire handoff, contract creation from offer |
| Social Insurance | M17 | Insurance enrollment tied to contract status |

### Business Rules
1. **Contract Types**: Probation (5% of contracts) - trial period with reduced rights, Indefinite (40%) - no fixed end date, Fixed-term (34%) - specified duration (typically 12-36 months), Seasonal (20%) - temporary/seasonal work.
2. **Probation Period**: Maximum 60 days for management positions, 30 days for college-level, 6 days for other positions per Vietnam Labor Code.
3. **Contract Renewal**: Fixed-term contracts renewed before expiration. After 2 consecutive fixed-term contracts, must convert to indefinite (unless specific exceptions).
4. **Contract Amendment**: Requires mutual agreement. Salary changes, position changes, department changes documented via amendment.
5. **Transfer**: Employee moves between departments/positions/locations. New contract or amendment issued. Seniority preserved.
6. **Resignation**: Employee-initiated termination. Notice period: 45 days for indefinite, 30 days for fixed-term. Resignation reasons tracked: Benefits (60), Work environment (35), Adaptation (30), Career change (25), Performance (20).
7. **Dismissal**: Employer-initiated termination. Requires valid cause per Labor Code (repeated violations, theft, disclosure, etc.) or organizational restructuring. Severance pay required.
8. **Appointment**: Promotion or position assignment. Documented with appointment decision letter.
9. **Commendation**: Formal recognition for achievements. Types: Completion of duties, Innovation, Dedication, Outstanding contribution.
10. **Incident**: Disciplinary events. Types: Warning, Reprimand, Demotion, Dismissal. Progressive discipline documented.
11. **Labor Dispute**: Conflicts between employee and employer. Tracked with resolution status.
12. **Contract Expiration Alert**: System alerts 60/30/15 days before fixed-term contract expiration.
13. **Seniority Calculation**: Cumulative service time across all contracts with the company. Used for severance, leave accrual, and benefits.

---

## 2. Screen Inventory

### 2.1 Employee Database (Profiles)
**Route:** `/labor-relations/employees`  
**Purpose:** Central employee profile database with contract information (img_35).

| UI Component | Type | Description |
|---|---|---|
| Employee List | Data table | Employee code, name, department, position, contract type, status |
| Profile View | Detail view | Personal info, contracts, employment events, documents |
| Contract Summary | Card | Active contract details, expiration date, seniority |
| Search | Input | Search by name, code, department |
| Filters | Multi-select | Department, contract type, status |
| Add Employee | Button | New employee profile |

**User Interactions:**
- Search and filter employee database.
- View detailed employee profiles.
- Access contract information.
- Add new employees.

**Data Displayed:** Employee profiles with contract and event history.

### 2.2 HR Overview Dashboard
**Route:** `/labor-relations/overview`  
**Purpose:** Navigation hub with workforce analytics (img_37).

| UI Component | Type | Description |
|---|---|---|
| Left Navigation | Menu | Profiles, Contracts, Appointments, Dismissals, Transfers, Resignations, Commendations, Incidents, Planning, Reports, Settings |
| Workforce Summary | Cards | Total employees, new hires this month, departures, open contracts |
| Contract Distribution | Pie chart | Probation 5%, Indefinite 40%, Fixed-term 34%, Seasonal 20% |
| Upcoming Expirations | List | Contracts expiring within 60 days |
| Recent Events | Timeline | Recent appointments, transfers, resignations |
| Quick Actions | Buttons | New contract, New event, Export report |

**User Interactions:**
- Navigate to any labor relations sub-module.
- View workforce summary and contract analytics.
- Access upcoming expiration alerts.
- Quick action shortcuts.

**Data Displayed:** Workforce overview, contract distribution, upcoming actions.

### 2.3 Contract Management
**Route:** `/labor-relations/contracts`  
**Purpose:** Create, manage, and track employment contracts.

| UI Component | Type | Description |
|---|---|---|
| Contract List | Table | Employee, contract type, start date, end date, salary, status |
| Contract Form | Modal | Employee, type, term, salary, position, department, benefits |
| Status Badge | Badge | Active, Expired, Terminated, Pending renewal |
| Renewal Alert | Alert | Contracts approaching expiration |
| Template Selector | Dropdown | Contract templates by type |
| Generate PDF | Button | Generate signed contract document |

**User Interactions:**
- Create new contracts from templates.
- Renew expiring contracts.
- Amend existing contracts.
- Generate contract PDFs for signature.

**Data Displayed:** Contract inventory with status and expiration tracking.

### 2.4 Appointment Management
**Route:** `/labor-relations/appointments`  
**Purpose:** Manage employee promotions and position appointments.

| UI Component | Type | Description |
|---|---|---|
| Appointment List | Table | Employee, new position, effective date, decision number, status |
| Appointment Form | Modal | Employee, new position, department, salary change, effective date |
| Decision Letter | Button | Generate appointment decision |
| Approval Workflow | Display | Approval chain and status |

**User Interactions:**
- Create appointment records.
- Generate decision letters.
- Track approval workflow.

**Data Displayed:** Appointment records with decision details.

### 2.5 Dismissal Management
**Route:** `/labor-relations/dismissals`  
**Purpose:** Manage employer-initiated terminations with compliance tracking.

| UI Component | Type | Description |
|---|---|---|
| Dismissal List | Table | Employee, reason, effective date, severance, status |
| Dismissal Form | Modal | Employee, reason category, justification, effective date, severance calc |
| Severance Calculator | Tool | Auto-calculate severance based on seniority and salary |
| Compliance Checklist | Checklist | Legal compliance verification |
| Notification | Button | Generate termination notice |

**User Interactions:**
- Create dismissal records with reason and justification.
- Calculate severance pay.
- Verify legal compliance.
- Generate termination notices.

**Data Displayed:** Dismissal records with severance and compliance data.

### 2.6 Transfer Management
**Route:** `/labor-relations/transfers`  
**Purpose:** Manage employee transfers between departments, positions, or locations.

| UI Component | Type | Description |
|---|---|---|
| Transfer List | Table | Employee, from dept/position, to dept/position, effective date, status |
| Transfer Form | Modal | Employee, source, destination, reason, salary adjustment, effective date |
| Decision Letter | Button | Generate transfer decision |
| Impact Analysis | Display | Salary change, reporting change, location change |

**User Interactions:**
- Create transfer requests.
- Approve transfers.
- Generate transfer decisions.
- Review impact on salary and reporting.

**Data Displayed:** Transfer records with before/after comparison.

### 2.7 Resignation Management
**Route:** `/labor-relations/resignations`  
**Purpose:** Process and analyze employee resignations.

| UI Component | Type | Description |
|---|---|---|
| Resignation List | Table | Employee, resignation date, notice period end, reason, status |
| Resignation Form | Modal | Employee, reason category, details, last working date |
| Reason Analysis | Chart | Resignation reasons: Benefits 60, Work env 35, Adaptation 30, Career 25, Performance 20 |
| Exit Checklist | Checklist | Handover, asset return, final pay, insurance decrease |
| Exit Interview | Form | Exit interview notes and feedback |

**User Interactions:**
- Record resignation with reason.
- Track notice period and handover.
- Conduct exit interview.
- Analyze resignation patterns.

**Data Displayed:** Resignation records with reason analytics.

### 2.8 Commendation Management
**Route:** `/labor-relations/commendations`  
**Purpose:** Record and manage employee commendations and recognition.

| UI Component | Type | Description |
|---|---|---|
| Commendation List | Table | Employee, type, reason, date, reward, status |
| Commendation Form | Modal | Employee, type, achievement description, reward type |
| Reward Calculator | Tool | Suggested reward based on commendation type |
| Certificate Generator | Button | Generate commendation certificate |

**User Interactions:**
- Create commendation records.
- Assign rewards.
- Generate certificates.

**Data Displayed:** Commendation records with reward details.

### 2.9 Incident Management
**Route:** `/labor-relations/incidents`  
**Purpose:** Track disciplinary incidents and actions.

| UI Component | Type | Description |
|---|---|---|
| Incident List | Table | Employee, incident type, date, action taken, status |
| Incident Form | Modal | Employee, incident description, type, witnesses, action |
| Progressive Discipline | Timeline | Previous incidents and actions for employee |
| Action Form | Dropdown | Warning, Reprimand, Demotion, Dismissal |

**User Interactions:**
- Record disciplinary incidents.
- Apply progressive discipline actions.
- Review incident history.

**Data Displayed:** Incident records with progressive discipline tracking.

### 2.10 Labor Dispute Management
**Route:** `/labor-relations/disputes`  
**Purpose:** Track and resolve labor disputes.

| UI Component | Type | Description |
|---|---|---|
| Dispute List | Table | Parties, issue, filing date, status, resolution |
| Dispute Form | Modal | Employee(s), employer rep, issue description, claims |
| Timeline | Timeline | Dispute events: filing, mediation, arbitration, resolution |
| Document List | List | Supporting documents, legal opinions |
| Resolution Form | Modal | Resolution type, terms, agreement |

**User Interactions:**
- Record new disputes.
- Track dispute progress through resolution stages.
- Record outcomes and agreements.

**Data Displayed:** Dispute records with resolution tracking.

### 2.11 Workforce Planning
**Route:** `/labor-relations/planning`  
**Purpose:** Workforce planning and headcount management.

| UI Component | Type | Description |
|---|---|---|
| Headcount Plan | Table | Department, planned headcount, current, gap, action |
| Projection Chart | Chart | Headcount projection over time |
| Attrition Forecast | Display | Expected departures based on contract expirations and trends |
| Hiring Plan | List | Planned hires by department and position |

**User Interactions:**
- Review headcount plan vs actual.
- Project future workforce needs.
- Plan for expected attrition.

**Data Displayed:** Workforce planning data and projections.

### 2.12 Labor Relations Reports
**Route:** `/labor-relations/reports`  
**Purpose:** Generate standard and custom labor relations reports.

| UI Component | Type | Description |
|---|---|---|
| Report Selector | Dropdown | Contract summary, turnover analysis, resignation reasons, seniority report, dispute summary |
| Resignation Analysis | Bar chart | Reason distribution with trends |
| Date Range | Date picker | Report period |
| Export | Button | PDF/Excel export |

**User Interactions:**
- Select and generate reports.
- Analyze workforce trends.
- Export for management review.

**Data Displayed:** Labor relations analytics and compliance reports.

---

## 3. Feature Specifications

### 3.1 Employee Profile Management
**Description:** Maintain comprehensive employee profiles with personal, professional, and employment data.

**User Stories:**
- As an HR Administrator, I can create and maintain employee profiles with complete personal and professional information.
- As an HR Administrator, I can view an employee's complete employment history including all contracts and events.
- As an Employee, I can view my own profile information.

**Acceptance Criteria:**
1. Profile includes: personal info (name, DOB, ID, contact), education, work history, emergency contacts, bank details.
2. Employment history: all contracts, positions, departments, salary history, events.
3. Document storage: ID copies, diplomas, certificates, contracts, decisions.
4. Profile linked to contracts, events, payroll, insurance records.
5. Profile accessible to authorized personnel only.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Required fields | All mandatory fields filled | "Missing required field: {field}" |
| Valid ID | National ID format correct | "Invalid ID format" |
| Unique employee code | No duplicate codes | "Employee code already exists" |

**Edge Cases:**
- Employee name change (marriage): historical records preserved, alias tracked.
- Dual nationality: both nationalities recorded.
- Profile merged from prior system: data migration with audit trail.

### 3.2 Contract Lifecycle Management
**Description:** Create, renew, amend, and terminate employment contracts.

**User Stories:**
- As an HR Administrator, I can create new employment contracts from templates.
- As an HR Administrator, I can renew expiring contracts before they expire.
- As an HR Administrator, I can amend contracts for salary or position changes.
- As a system, I can alert HR about contracts approaching expiration.

**Acceptance Criteria:**
1. Contract types: Probation, Fixed-term, Indefinite, Seasonal.
2. Contract includes: employee, type, term (start/end), position, department, salary, working hours, benefits, conditions.
3. Templates for each contract type with pre-filled standard clauses.
4. Contract PDF generation with company letterhead.
5. Renewal workflow: alert at 60/30/15 days, renewal form, new contract generation.
6. Amendment records linked to original contract.
7. Contract status: Draft -> Signed -> Active -> Expired/Terminated.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid term | End date > start date (for fixed-term) | "End date must be after start date" |
| Salary >= minimum | Salary >= minimum wage | "Salary below minimum wage" |
| Probation limit | Probation period within legal max | "Probation period exceeds legal maximum" |
| No overlap | Employee has no overlapping active contracts | "Employee already has an active contract" |

**Edge Cases:**
- Fixed-term contract expires without renewal: auto-terminate, trigger offboarding.
- Second consecutive fixed-term: system recommends conversion to indefinite.
- Contract signed but employee never starts: void contract with reason.

### 3.3 Appointment (Promotion) Management
**Description:** Manage employee promotions and position appointments.

**User Stories:**
- As an HR Administrator, I can create appointment records for employee promotions.
- As a Manager, I can request appointments for team members.
- As a system, I can generate appointment decision letters.

**Acceptance Criteria:**
1. Appointment includes: employee, new position, new department, effective date, salary adjustment, decision number.
2. Approval workflow: Manager request -> HR review -> Director approval.
3. Decision letter generated with official format.
4. Appointment triggers: contract amendment, salary update, reporting change.
5. Appointment history tracked per employee.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid position | Position exists and is available | "Position not available for appointment" |
| Approval chain | All approvals obtained | "Missing approval from {role}" |
| Salary within band | New salary within position band | "Salary exceeds position salary band" |

**Edge Cases:**
- Appointment rejected: record preserved with rejection reason.
- Multiple appointments for same employee: latest supersedes prior.
- Appointment with no salary change: allowed, noted in record.

### 3.4 Dismissal Management
**Description:** Process employer-initiated terminations with legal compliance tracking.

**User Stories:**
- As an HR Administrator, I can create dismissal records with reason and justification.
- As an HR Administrator, I can calculate severance pay based on seniority.
- As an HR Director, I can review and approve dismissals before execution.
- As a system, I can verify legal compliance before dismissal is finalized.

**Acceptance Criteria:**
1. Dismissal includes: employee, reason category, justification, effective date, severance amount, decision number.
2. Reason categories: Repeated violations, Theft/fraud, Confidentiality breach, Restructuring, Performance, Other.
3. Severance calculation: based on seniority and average salary per Labor Code.
4. Compliance checklist: notice period, union consultation (if applicable), severance calculation verified.
5. Dismissal triggers: contract termination, insurance decrease, final payroll, asset handover.
6. Approval required from HR Director and Legal (for cause dismissals).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid reason | Reason category selected with details | "Select dismissal reason and provide justification" |
| Severance calculated | Severance amount computed | "Severance must be calculated before approval" |
| Compliance verified | All checklist items passed | "Compliance checklist incomplete" |
| Approval obtained | Required approvals secured | "Missing required approval from {role}" |

**Edge Cases:**
- Protected employee (pregnant, on sick leave): dismissal restricted, legal review required.
- Mass layoff (restructuring): additional regulatory requirements (notification to labor authority).
- Dismissal contested: dispute record created, dismissal paused pending resolution.

### 3.5 Transfer Management
**Description:** Manage employee transfers between departments, positions, or locations.

**User Stories:**
- As a Manager, I can request a transfer for an employee.
- As an HR Administrator, I can process transfer requests with impact analysis.
- As a system, I can generate transfer decision letters and update employee records.

**Acceptance Criteria:**
1. Transfer includes: employee, source dept/position/location, destination dept/position/location, reason, effective date, salary adjustment.
2. Approval workflow: Source manager -> Destination manager -> HR -> Director.
3. Impact analysis: salary change, new manager, new location, contract amendment needed.
4. Transfer decision letter generated.
5. Seniority preserved across transfer.
6. Transfer history tracked per employee.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Both managers approve | Source and destination managers agree | "Transfer requires approval from both managers" |
| Position available | Destination position not filled | "Destination position already filled" |
| Valid effective date | Date in future | "Effective date must be in the future" |

**Edge Cases:**
- Transfer with relocation: additional benefits (moving allowance, housing) configured.
- Employee refuses transfer: record refusal, explore alternatives.
- Cross-office transfer: tax and insurance implications assessed.

### 3.6 Resignation Management
**Description:** Process employee resignations with reason tracking and exit procedures.

**User Stories:**
- As an Employee, I can submit my resignation with reason and notice period.
- As an HR Administrator, I can process resignations and manage the exit procedure.
- As an HR Director, I can analyze resignation patterns by reason category.

**Acceptance Criteria:**
1. Resignation includes: employee, reason category, details, resignation date, notice period end, last working date.
2. Reason categories: Benefits (60), Work environment (35), Adaptation (30), Career change (25), Performance (20), Other.
3. Multiple reasons per resignation allowed (ranked).
4. Notice period enforced: 45 days (indefinite), 30 days (fixed-term).
5. Exit checklist: handover, asset return, final pay calculation, insurance decrease, contract termination.
6. Exit interview conducted and recorded.
7. Resignation triggers: contract termination, payroll finalization, insurance decrease (M17), access revocation.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Notice period | Minimum notice period respected | "Notice period must be at least {days} days" |
| Reason provided | At least one reason category | "Provide resignation reason" |
| Last date valid | Last working date >= resignation date | "Last working date must be on or after resignation date" |

**Edge Cases:**
- Resignation withdrawn: employee retracts before last working date, record withdrawal.
- Immediate resignation (no notice): record breach, potential penalty.
- Counter-offer accepted: resignation cancelled, retention note added.

### 3.7 Commendation Management
**Description:** Record employee commendations and recognition.

**User Stories:**
- As a Manager, I can nominate an employee for commendation.
- As an HR Administrator, I can process commendations and assign rewards.
- As a system, I can generate commendation certificates.

**Acceptance Criteria:**
1. Commendation types: Completion of duties, Innovation, Dedication, Outstanding contribution.
2. Commendation includes: employee, type, achievement description, date, reward type, reward value.
3. Reward types: Certificate, Bonus, Promotion consideration, Public recognition.
4. Certificate generated with company letterhead and signatures.
5. Commendation history tracked per employee (used for performance evaluation).
6. Approval workflow: Manager nomination -> HR review -> Director approval.

**Edge Cases:**
- Team commendation: multiple employees recognized together.
- Commendation linked to bonus: integration with payroll bonus calculation.
- Multiple commendations in period: all recorded, may influence evaluation.

### 3.8 Incident Management
**Description:** Track disciplinary incidents and progressive discipline actions.

**User Stories:**
- As a Manager, I can report a disciplinary incident involving an employee.
- As an HR Administrator, I can track incident history and apply progressive discipline.
- As an HR Director, I can review incident patterns across the organization.

**Acceptance Criteria:**
1. Incident types: Attendance violation, Performance failure, Policy violation, Misconduct, Safety violation.
2. Disciplinary actions: Warning (verbal/written), Reprimand, Demotion, Dismissal.
3. Progressive discipline: system tracks prior incidents and recommends escalation.
4. Incident record: employee, incident type, description, date, witnesses, action taken.
5. Incident acknowledged by employee (signature or record of notification).
6. Appeal process: employee can contest incident within configurable timeframe.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Evidence attached | Supporting evidence provided | "Attach supporting evidence for incident" |
| Progressive discipline | Action level appropriate for history | "Action exceeds progressive discipline level for this incident count" |
| Timely recording | Incident recorded within 30 days | "Incident must be recorded within 30 days of occurrence" |

**Edge Cases:**
- Incident leads to dismissal: link to dismissal workflow, compliance verification.
- Incident contested: dispute record created, disciplinary action paused.
- Incident expunged: after configurable period (e.g., 12 months clean record), flag as expired.

### 3.9 Labor Dispute Management
**Description:** Track and resolve labor disputes between employees and employer.

**User Stories:**
- As an HR Administrator, I can record a labor dispute with parties and issues.
- As Legal Counsel, I can track dispute progress through resolution stages.
- As an HR Director, I can monitor dispute resolution outcomes.

**Acceptance Criteria:**
1. Dispute includes: employee(s), employer representative, issue description, claims, filing date, status.
2. Resolution stages: Filed -> Mediation -> Arbitration -> Court -> Resolved.
3. Timeline tracking: key events, deadlines, decisions.
4. Document storage: legal filings, evidence, opinions, agreements.
5. Outcome recording: resolution type, terms, financial settlement.
6. Dispute linked to related incidents, dismissals, or grievances.

**Edge Cases:**
- Class action (multiple employees): group dispute with individual sub-records.
- Dispute settled out of court: settlement agreement recorded.
- Dispute lost by employer: financial liability recorded, process improvement triggered.

### 3.10 Contract Expiration Alerts
**Description:** Automated alerts for approaching contract expirations.

**User Stories:**
- As an HR Administrator, I receive alerts when contracts are approaching expiration.
- As a system, I can automatically generate renewal reminders at configurable intervals.

**Acceptance Criteria:**
1. Alerts at configurable intervals: 60, 30, 15 days before expiration.
2. Alert includes: employee, contract type, expiration date, recommended action.
3. Renewal dashboard showing all upcoming expirations.
4. Batch renewal: select multiple contracts for renewal.
5. Non-renewal: record decision not to renew, trigger offboarding.

**Edge Cases:**
- Contract renewed early (before alert): alert suppressed.
- Employee on notice of termination: alert suppressed, offboarding already in progress.
- Auto-renewal clause: contract auto-renewed per terms, alert confirms renewal.

---

## 4. Database Design

### ERD (ASCII)
```
+----------------+       +-------------------+       +-------------------+
| LaborContract  |1-----*| ContractAmendment |       | Employee          |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| employee_id    |       | contract_id (FK)  |       +-------------------+
| contract_type  |       | amendment_type    |
| start_date     |       | change_description|
| end_date       |       | effective_date    |
| salary         |       | new_salary        |
| position       |       +-------------------+
| department     |
| status         |
| signed_date    |
+----------------+
       |
       | 1
       |
       v
+-------------------+       +-------------------+       +-------------------+
| Appointment       |       | Dismissal         |       | Transfer          |
+-------------------+       +-------------------+       +-------------------+
| id (PK)           |       | id (PK)           |       | id (PK)           |
| employee_id (FK)  |       | employee_id (FK)  |       | employee_id (FK)  |
| new_position      |       | reason            |       | from_department   |
| new_department    |       | justification     |       | to_department     |
| effective_date    |       | effective_date    |       | effective_date    |
| decision_number   |       | severance_amount  |       | salary_adjustment |
| status            |       | status            |       | status            |
+-------------------+       +-------------------+       +-------------------+

+-------------------+       +-------------------+       +-------------------+
| Resignation       |       | ResignationReason |       | Commendation      |
+-------------------+       +-------------------+       +-------------------+
| id (PK)           |       | id (PK)           |       | id (PK)           |
| employee_id (FK)  |       | employee_id (FK)  |       | employee_id (FK)  |
| reason_category   |       | category          |       | comm_type         |
| resignation_date  |       | rank              |       | description       |
| notice_end        |       | description       |       | reward_type       |
| last_working_date |       +-------------------+       | reward_value      |
| status            |                                     | date              |
+-------------------+                                     +-------------------+

+-------------------+       +-------------------+       +-------------------+
| Incident          |       | LaborDispute      |       | PlanningRecord    |
+-------------------+       +-------------------+       +-------------------+
| id (PK)           |       | id (PK)           |       | id (PK)           |
| employee_id (FK)  |       | employee_ids      |       | department_id     |
| incident_type     |       | issue             |       | planned_headcount |
| description       |       | filing_date       |       | current_headcount |
| action_taken      |       | status            |       | gap               |
| date              |       | resolution        |       | action_plan       |
| witnesses         |       | resolution_date   |       | period            |
+-------------------+       +-------------------+       +-------------------+
```

### Table Schemas

#### labor_contract
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| contract_type | VARCHAR(20) | NOT NULL | probation/fixed_term/indefinite/seasonal |
| start_date | DATE | NOT NULL | Contract start date |
| end_date | DATE | NULL | Contract end date (NULL for indefinite) |
| salary | DECIMAL(12,2) | NOT NULL | Contract salary |
| position_id | UUID | FK -> positions.id, NOT NULL | Position |
| department_id | UUID | FK -> departments.id, NOT NULL | Department |
| working_hours | DECIMAL(4,2) | NOT NULL, DEFAULT 8 | Daily working hours |
| benefits | TEXT | NULL | Benefits description |
| conditions | TEXT | NULL | Special conditions |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft/signed/active/expired/terminated |
| signed_date | DATE | NULL | Contract signing date |
| template_id | UUID | FK -> contract_template.id, NULL | Source template |
| document_url | VARCHAR(500) | NULL | Signed contract PDF |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### contract_amendment
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| contract_id | UUID | FK -> labor_contract.id, NOT NULL | Parent contract |
| amendment_type | VARCHAR(30) | NOT NULL | salary_change/position_change/department_change/other |
| change_description | TEXT | NOT NULL | Description of changes |
| effective_date | DATE | NOT NULL | Amendment effective date |
| old_salary | DECIMAL(12,2) | NULL | Previous salary |
| new_salary | DECIMAL(12,2) | NULL | New salary |
| old_position_id | UUID | FK -> positions.id, NULL | Previous position |
| new_position_id | UUID | FK -> positions.id, NULL | New position |
| document_url | VARCHAR(500) | NULL | Amendment document |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### contract_template
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(100) | NOT NULL | Template name |
| contract_type | VARCHAR(20) | NOT NULL | Applicable contract type |
| content | TEXT | NOT NULL | Template content with placeholders |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### appointment
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| new_position_id | UUID | FK -> positions.id, NOT NULL | New position |
| new_department_id | UUID | FK -> departments.id, NOT NULL | New department |
| effective_date | DATE | NOT NULL | Appointment effective date |
| salary_adjustment | DECIMAL(12,2) | NULL | Salary change amount |
| new_salary | DECIMAL(12,2) | NULL | New salary after appointment |
| decision_number | VARCHAR(50) | NULL | Official decision number |
| reason | TEXT | NOT NULL | Reason for appointment |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/approved/rejected/executed |
| approved_by | UUID | FK -> users.id, NULL | Approver |
| approved_at | TIMESTAMPTZ | NULL | Approval timestamp |
| decision_url | VARCHAR(500) | NULL | Decision letter PDF |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### dismissal
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| reason_category | VARCHAR(50) | NOT NULL | violation/theftware/restructuring/performance/other |
| justification | TEXT | NOT NULL | Detailed justification |
| effective_date | DATE | NOT NULL | Termination effective date |
| severance_amount | DECIMAL(12,2) | NOT NULL | Severance pay amount |
| seniority_months | DECIMAL(6,2) | NOT NULL | Employee seniority at dismissal |
| decision_number | VARCHAR(50) | NULL | Official decision number |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/approved/executed/contested |
| approved_by | UUID | FK -> users.id, NULL | Approver |
| approved_at | TIMESTAMPTZ | NULL | Approval timestamp |
| legal_review | BOOLEAN | NOT NULL, DEFAULT false | Legal review completed |
| notice_url | VARCHAR(500) | NULL | Termination notice PDF |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### transfer
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| from_department_id | UUID | FK -> departments.id, NOT NULL | Source department |
| to_department_id | UUID | FK -> departments.id, NOT NULL | Destination department |
| from_position_id | UUID | FK -> positions.id, NOT NULL | Source position |
| to_position_id | UUID | FK -> positions.id, NOT NULL | Destination position |
| from_location | VARCHAR(200) | NULL | Source location |
| to_location | VARCHAR(200) | NULL | Destination location |
| reason | TEXT | NOT NULL | Transfer reason |
| effective_date | DATE | NOT NULL | Transfer effective date |
| salary_adjustment | DECIMAL(12,2) | NULL | Salary change |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/source_approved/dest_approved/hr_approved/executed |
| decision_url | VARCHAR(500) | NULL | Transfer decision PDF |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### resignation
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| reason_primary | VARCHAR(50) | NOT NULL | Primary resignation reason |
| reason_secondary | VARCHAR(50) | NULL | Secondary reason |
| reason_details | TEXT | NULL | Detailed explanation |
| resignation_date | DATE | NOT NULL | Date resignation submitted |
| notice_period_days | INT | NOT NULL | Required notice period |
| notice_end_date | DATE | NOT NULL | Notice period end |
| last_working_date | DATE | NOT NULL | Employee's last working day |
| exit_interview_done | BOOLEAN | NOT NULL, DEFAULT false | Exit interview completed |
| exit_interview_notes | TEXT | NULL | Exit interview summary |
| handover_complete | BOOLEAN | NOT NULL, DEFAULT false | Handover completed |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'submitted' | submitted/in_progress/completed/withdrawn |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### resignation_reason
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| resignation_id | UUID | FK -> resignation.id, NOT NULL | Parent resignation |
| category | VARCHAR(50) | NOT NULL | benefits/work_environment/adaptation/career_change/performance/other |
| rank | INT | NOT NULL | Reason priority (1 = primary) |
| description | TEXT | NULL | Detailed description |

#### commendation
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| comm_type | VARCHAR(50) | NOT NULL | completion/innovation/dedication/outstanding_contribution |
| description | TEXT | NOT NULL | Achievement description |
| commendation_date | DATE | NOT NULL | Date of commendation |
| reward_type | VARCHAR(30) | NULL | certificate/bonus/promotion/public_recognition |
| reward_value | DECIMAL(12,2) | NULL | Reward monetary value |
| decision_number | VARCHAR(50) | NULL | Decision number |
| certificate_url | VARCHAR(500) | NULL | Certificate PDF |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/approved/issued |
| approved_by | UUID | FK -> users.id, NULL | Approver |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### incident
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| incident_type | VARCHAR(50) | NOT NULL | attendance/performance/policy/misconduct/safety |
| description | TEXT | NOT NULL | Incident description |
| incident_date | DATE | NOT NULL | Date of incident |
| action_taken | VARCHAR(30) | NOT NULL | verbal_warning/written_warning/reprimand/demotion/dismissal |
| action_date | DATE | NOT NULL | Date action taken |
| witnesses | TEXT[] | NULL | Witness names/IDs |
| evidence_urls | TEXT[] | NULL | Evidence documents |
| employee_notified | BOOLEAN | NOT NULL, DEFAULT false | Employee notified of action |
| appealed | BOOLEAN | NOT NULL, DEFAULT false | Employee appealed |
| appeal_notes | TEXT | NULL | Appeal details |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'recorded' | recorded/active/expired/escalated |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### labor_dispute
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_ids | UUID[] | NOT NULL | Involved employee(s) |
| employer_rep | VARCHAR(200) | NOT NULL | Employer representative |
| issue | TEXT | NOT NULL | Dispute issue description |
| claims | TEXT | NULL | Employee claims |
| filing_date | DATE | NOT NULL | Dispute filing date |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'filed' | filed/mediation/arbitration/court/resolved/dismissed |
| resolution | TEXT | NULL | Resolution description |
| resolution_type | VARCHAR(30) | NULL | settlement/ruling/withdrawn |
| financial_settlement | DECIMAL(12,2) | NULL | Settlement amount |
| resolution_date | DATE | NULL | Resolution date |
| related_incident_id | UUID | FK -> incident.id, NULL | Related incident |
| documents | TEXT[] | NULL | Legal documents |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### planning_record
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| department_id | UUID | FK -> departments.id, NOT NULL | Department |
| planned_headcount | INT | NOT NULL | Target headcount |
| current_headcount | INT | NOT NULL | Current headcount |
| gap | INT | NOT NULL | Planned - Current |
| action_plan | TEXT | NULL | Planned actions to close gap |
| period_start | DATE | NOT NULL | Planning period start |
| period_end | DATE | NOT NULL | Planning period end |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

### All Tables Summary
| Table | Purpose | Key Relationships |
|---|---|---|
| labor_contract | Employment contracts | Links Employee |
| contract_amendment | Contract changes | Links LaborContract |
| contract_template | Contract templates | Referenced by LaborContract |
| appointment | Promotions/position changes | Links Employee |
| dismissal | Employer terminations | Links Employee |
| transfer | Employee transfers | Links Employee |
| resignation | Employee resignations | Links Employee, Parent of ResignationReason |
| resignation_reason | Resignation reason details | Links Resignation |
| commendation | Employee recognition | Links Employee |
| incident | Disciplinary events | Links Employee |
| labor_dispute | Legal disputes | Links Employee(s), Incident |
| planning_record | Workforce planning | Links Department |

---

## 5. API Specifications

### JSON Response Format
```json
{
  "success": true,
  "data": { ... },
  "meta": { "page": 1, "per_page": 20, "total": 500 }
}
```

### Error Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Contract expiration alert: 15 days remaining",
    "details": [
      { "field": "end_date", "message": "Contract expires on 30/04/2026" }
    ]
  }
}
```

### Endpoints Summary
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| GET | `/api/labor/employees` | List employee profiles | labor.employee.read |
| POST | `/api/labor/employees` | Create employee profile | labor.employee.create |
| GET | `/api/labor/employees/:id` | Get employee profile detail | labor.employee.read |
| GET | `/api/labor/contracts` | List contracts | labor.contract.read |
| POST | `/api/labor/contracts` | Create contract | labor.contract.create |
| PUT | `/api/labor/contracts/:id` | Update contract | labor.contract.update |
| POST | `/api/labor/contracts/:id/renew` | Renew contract | labor.contract.renew |
| POST | `/api/labor/contracts/:id/amend` | Create amendment | labor.contract.amend |
| GET | `/api/labor/contracts/expiring` | Get expiring contracts | labor.contract.read |
| GET | `/api/labor/appointments` | List appointments | labor.appointment.read |
| POST | `/api/labor/appointments` | Create appointment | labor.appointment.create |
| PATCH | `/api/labor/appointments/:id/status` | Approve/reject appointment | labor.appointment.approve |
| GET | `/api/labor/dismissals` | List dismissals | labor.dismissal.read |
| POST | `/api/labor/dismissals` | Create dismissal | labor.dismissal.create |
| PATCH | `/api/labor/dismissals/:id/status` | Approve dismissal | labor.dismissal.approve |
| GET | `/api/labor/transfers` | List transfers | labor.transfer.read |
| POST | `/api/labor/transfers` | Create transfer | labor.transfer.create |
| PATCH | `/api/labor/transfers/:id/status` | Approve transfer | labor.transfer.approve |
| GET | `/api/labor/resignations` | List resignations | labor.resignation.read |
| POST | `/api/labor/resignations` | Record resignation | labor.resignation.create |
| PATCH | `/api/labor/resignations/:id/status` | Update resignation status | labor.resignation.update |
| GET | `/api/labor/resignations/analysis` | Resignation reason analysis | labor.report.read |
| GET | `/api/labor/commendations` | List commendations | labor.commendation.read |
| POST | `/api/labor/commendations` | Create commendation | labor.commendation.create |
| GET | `/api/labor/incidents` | List incidents | labor.incident.read |
| POST | `/api/labor/incidents` | Record incident | labor.incident.create |
| GET | `/api/labor/disputes` | List labor disputes | labor.dispute.read |
| POST | `/api/labor/disputes` | Record dispute | labor.dispute.create |
| PATCH | `/api/labor/disputes/:id/status` | Update dispute status | labor.dispute.update |
| GET | `/api/labor/planning` | Workforce planning data | labor.planning.read |
| PUT | `/api/labor/planning` | Update planning | labor.planning.update |
| GET | `/api/labor/overview` | HR overview dashboard | labor.overview.read |

### Endpoint Details

#### GET /api/labor/overview
**Response:**
```json
{
  "success": true,
  "data": {
    "total_employees": 500,
    "new_hires_this_month": 12,
    "departures_this_month": 5,
    "open_contracts": 485,
    "contract_distribution": [
      { "type": "Probation", "percentage": 5, "count": 25 },
      { "type": "Indefinite", "percentage": 40, "count": 200 },
      { "type": "Fixed-term", "percentage": 34, "count": 170 },
      { "type": "Seasonal", "percentage": 20, "count": 100 }
    ],
    "upcoming_expirations": [
      { "employee": "Nguyen Van A", "contract_type": "Fixed-term", "expiry_date": "2026-05-15" }
    ],
    "recent_events": [
      { "type": "appointment", "employee": "Tran Thi B", "date": "2026-04-10" },
      { "type": "transfer", "employee": "Le Van C", "date": "2026-04-08" }
    ]
  }
}
```

#### GET /api/labor/resignations/analysis
**Response:**
```json
{
  "success": true,
  "data": {
    "total_resignations": 170,
    "by_reason": [
      { "category": "Benefits", "count": 60, "percentage": 35.3 },
      { "category": "Work environment", "count": 35, "percentage": 20.6 },
      { "category": "Adaptation", "count": 30, "percentage": 17.6 },
      { "category": "Career change", "count": 25, "percentage": 14.7 },
      { "category": "Performance", "count": 20, "percentage": 11.8 }
    ],
    "by_department": [
      { "department": "Sales", "count": 45 },
      { "department": "Engineering", "count": 38 }
    ],
    "by_tenure": [
      { "range": "< 6 months", "count": 30 },
      { "range": "6-12 months", "count": 45 },
      { "range": "1-2 years", "count": 55 },
      { "range": "> 2 years", "count": 40 }
    ]
  }
}
```

---

## 6. Permissions Matrix

### Roles
| Role | Description |
|---|---|
| HR Director | Full access to all labor relations functions, final approver |
| HR Administrator | Manage profiles, contracts, events, reports |
| Department Manager | View team data, request transfers/appointments, report incidents |
| Employee | View own profile and contract |
| Legal Counsel | View disputes, incidents, compliance data |

### Permission Matrix (CRUD)
| Permission | HR Director | HR Admin | Dept. Mgr. | Employee | Legal |
|---|---|---|---|---|---|
| labor.employee.read | CRUD | CRUD | R (own dept) | R (own) | R |
| labor.employee.create | C | C | - | - | - |
| labor.contract.read | CRUD | CRUD | R (own dept) | R (own) | R |
| labor.contract.create | C | C | - | - | - |
| labor.contract.renew | C | C | - | - | - |
| labor.contract.amend | C | C | - | - | - |
| labor.appointment.read | CRUD | CRUD | R (own dept) | R (own) | R |
| labor.appointment.create | C | C | C (own dept) | - | - |
| labor.appointment.approve | C | - | - | - | - |
| labor.dismissal.read | CRUD | CRUD | R (own dept) | R (own) | R |
| labor.dismissal.create | C | C | - | - | - |
| labor.dismissal.approve | C | - | - | - | - |
| labor.transfer.read | CRUD | CRUD | R (own dept) | R (own) | R |
| labor.transfer.create | C | C | C (own dept) | - | - |
| labor.resignation.read | CRUD | CRUD | R (own dept) | R (own) | R |
| labor.resignation.create | C | C | - | C (own) | - |
| labor.commendation.read | CRUD | CRUD | R (own dept) | R (own) | R |
| labor.commendation.create | C | C | C (own dept) | - | - |
| labor.incident.read | CRUD | CRUD | R (own dept) | R (own) | R |
| labor.incident.create | C | C | C (own dept) | - | - |
| labor.dispute.read | CRUD | R | - | R (own) | CRUD |
| labor.dispute.create | C | C | - | - | C |
| labor.planning.read | R | R | R (own dept) | - | R |
| labor.planning.update | C | C | - | - | - |
| labor.overview.read | R | R | R (own dept) | - | R |

### Row-Level Security
| Rule | Description |
|---|---|
| Employee self-service | Employees only see their own profile and contract |
| Department scoping | Managers only see data for their department employees |
| Confidentiality | Dismissal and incident data restricted to authorized roles |
| Cross-tenant isolation | All queries scoped to tenant_id |

---

## 7. State Machine / Workflows

### Contract Status Transitions
```
  +-------+     sign()      +--------+     start_date     +--------+
  | DRAFT | ---------------> | SIGNED | -----------------> | ACTIVE |
  +-------+                 +--------+                    +--------+
                                                             |    |
                                                    expire()|    |terminate()
                                                             |    |
                                                             v    v
                                                       +--------+  +-----------+
                                                       |EXPIRED |  |TERMINATED |
                                                       +--------+  +-----------+
```

### Contract Status Table
| From Status | To Status | Trigger | Required Role | Side Effects |
|---|---|---|---|---|
| draft | signed | sign() | HR Administrator | Generate PDF, record signed date |
| signed | active | start_date reached | System (auto) | Activate contract, notify employee |
| active | expired | end_date reached | System (auto) | Trigger renewal alert or offboarding |
| active | terminated | terminate() | HR Administrator | Trigger offboarding, insurance decrease |

### Resignation Workflow
```
  +-----------+     submit()      +-------------+    notice_period     +-------------+
  |  DRAFT    | ----------------> |  SUBMITTED  | -------------------> | IN PROGRESS |
  +-----------+                   +-------------+  (notice period)     +-------------+
                                                                             |
                                                                    exit_procedures
                                                                             |
                                                                             v
                                                                      +-----------+
                                                                      | COMPLETED |
                                                                      +-----------+
```

### Dismissal Approval Workflow
```
  +---------+     create()     +---------+    legal_review()    +----------+    approve()    +----------+
  |  DRAFT  | ----------------> | PENDING | -------------------> | REVIEWED | ---------------> | APPROVED |
  +---------+                  +---------+                      +----------+                 +----------+
                                                                                                    |
                                                                                           execute()
                                                                                                    |
                                                                                                    v
                                                                                             +-----------+
                                                                                             | EXECUTED  |
                                                                                             +-----------+
```

### Transfer Approval Workflow
```
  +---------+     create()     +-----------------+    source_appro()    +-----------------+
  |  DRAFT  | ----------------> | PENDING SOURCE  | -------------------> | PENDING DEST    |
  +---------+                  +-----------------+                      +-----------------+
                                                                              |
                                                                     dest_appro()
                                                                              |
                                                                              v
                                                                       +-------------+   execute()   +----------+
                                                                       | HR APPROVED | -----------> | EXECUTED |
                                                                       +-------------+              +----------+
```

### Progressive Discipline Workflow
```
  +------------------+    incident #1     +-----------------+    incident #2     +------------+
  | No prior record  | -----------------> | Verbal Warning  | -----------------> | Written    |
  +------------------+                    +-----------------+                    | Warning    |
                                                                                +------------+
                                                                                     |
                                                                              incident #3
                                                                                     |
                                                                                     v
                                                                              +------------+
                                                                              | Reprimand  |
                                                                              +------------+
                                                                                   |
                                                                            incident #4
                                                                                   |
                                                                                   v
                                                                            +------------+
                                                                            | Demotion/  |
                                                                            | Dismissal  |
                                                                            +------------+
```

---

## 8. Reports & Analytics

### 8.1 Contract Summary Report
**Dimensions:** Contract type, Department, Status  
**Metrics:** Count per type, active vs expired, average salary by type  
**Filters:** Department, status, date range

### 8.2 Turnover Analysis Report
**Dimensions:** Department, Period, Reason  
**Metrics:** Resignation count, dismissal count, turnover rate, average tenure at departure  
**Filters:** Period, department

### 8.3 Resignation Reason Report
**Dimensions:** Reason category, Department, Tenure band  
**Metrics:** Count per reason, percentage, trend (img_63: Benefits 60, Work env 35, Adaptation 30, Career 25, Performance 20)  
**Filters:** Date range, department, source of hire

### 8.4 Seniority Report
**Dimensions:** Department, Tenure band  
**Metrics:** Employee count, average tenure, seniority distribution  
**Filters:** Department

### 8.5 Contract Expiration Report
**Dimensions:** Month, Department  
**Metrics:** Contracts expiring, renewal rate, non-renewal count  
**Filters:** Upcoming period (30/60/90 days)

### 8.6 Incident Summary Report
**Dimensions:** Incident type, Department, Action taken  
**Metrics:** Incident count, action distribution, repeat offender count  
**Filters:** Date range, department

### 8.7 Dispute Summary Report
**Dimensions:** Dispute status, Resolution type  
**Metrics:** Active disputes, resolved count, average resolution time, financial impact  
**Filters:** Date range

### 8.8 Workforce Planning Report
**Dimensions:** Department, Period  
**Metrics:** Planned vs actual headcount, gap, hiring needs, attrition forecast  
**Filters:** Period, department

### Report Summary Table
| Report | Purpose | Primary Users | Export Formats |
|---|---|---|---|
| Contract Summary | Contract inventory | HR Admin, HR Director | PDF, Excel |
| Turnover Analysis | Departure trends | HR Director, CFO | PDF, Excel |
| Resignation Reason | Exit analytics | HR Director | PDF, Excel |
| Seniority Report | Workforce experience | HR Admin, CFO | PDF, Excel |
| Contract Expiration | Renewal planning | HR Admin | PDF, Excel |
| Incident Summary | Discipline tracking | HR Director, Manager | PDF, Excel |
| Dispute Summary | Legal exposure | HR Director, Legal | PDF, Excel |
| Workforce Planning | Headcount management | HR Director, CFO | PDF, Excel |

---

## 9. Integration Points

### External APIs
| Integration | Direction | Purpose | Frequency |
|---|---|---|---|
| M02 Employee Service | Inbound/Outbound | Employee profile sync | Real-time (event-driven) |
| M01 Organization Service | Inbound | Department/position data | Real-time (event-driven) |
| M12 Payroll Service | Outbound | Salary changes, termination triggers | On event |
| M14 Recruitment | Inbound | New hire handoff from offer acceptance | On hire confirmed |
| M17 Social Insurance | Outbound | Labor increase/decrease triggers | On contract start/termination |
| Email Service | Outbound | Notifications, decision letters | On event |
| Document Generator | Internal | Contract PDF, decision letter generation | On create |
| Calendar Service | Outbound | Contract expiration reminders, event scheduling | On schedule |

### Webhooks
| Event | Trigger | Payload |
|---|---|---|
| contract.created | New contract created | contract_id, employee_id, type, start_date |
| contract.expiring | Contract nearing expiration | contract_id, employee_id, days_remaining |
| contract.terminated | Contract terminated | contract_id, employee_id, reason |
| resignation.submitted | Resignation received | resignation_id, employee_id, last_working_date |
| dismissal.approved | Dismissal approved | dismissal_id, employee_id, effective_date |
| transfer.executed | Transfer completed | transfer_id, employee_id, from, to |
| incident.recorded | Incident logged | incident_id, employee_id, type, action |

### Events Published
| Event | Description | Consumers |
|---|---|---|
| ContractStatusChanged | Contract state change | Payroll (M12), Insurance (M17) |
| EmployeeDeparture | Resignation or dismissal | Payroll (M12), Insurance (M17), IT (access revocation) |
| ContractRenewed | Contract renewed | Payroll (M12), Insurance (M17) |
| TransferExecuted | Employee transferred | Payroll (M12), Organization structure |

---

## 10. Technical Notes

### Performance
- **Employee profile search**: Full-text search across name, code, department; results in <500ms for 10,000 employees.
- **Contract expiration queries**: Indexed on (end_date, status); alerts generated in <1 second.
- **Bulk contract generation**: Generate 100+ contracts from templates within 2 minutes.
- **Database indexing**: Index on (employee_id) for all event tables, (end_date, status) for expiration queries, (contract_type, status) for distribution.

### Caching
- **Contract templates** cached with 1-hour TTL.
- **Overview dashboard** cached with 5-minute TTL.
- **Resignation reason analytics** cached with 1-hour TTL.
- **Employee profile data** cached with 2-minute TTL.

### File Handling
- **Signed contracts** stored in object storage with signed URLs, 10-year retention.
- **Decision letters** (appointment, dismissal, transfer) stored with 10-year retention.
- **Incident evidence** stored with 5-year retention.
- **File naming convention**: `contract_{employee_code}_{version}_{timestamp}.pdf`.

### Localization
- **Language**: Vietnamese (primary), English (secondary).
- **Currency**: VND.
- **Date format**: DD/MM/YYYY.
- **Legal terminology**: Vietnam Labor Code standard terms.
- **Contract templates**: Bilingual support for international employees.

### Mobile Support
- **Employee profile view** responsive on mobile.
- **Contract view** mobile-optimized.
- **Event creation** requires desktop (complex forms).
- **Push notifications** for contract expiration alerts, event approvals.

### Security
- **Employee data** encrypted at rest (AES-256).
- **Contracts and decision letters** tamper-evident (hash stored).
- **Audit logging**: All create/update/delete operations logged with user identity.
- **Data retention**: Employment records retained for 10 years per Vietnam regulations.
- **Compliance**: Vietnam Labor Code 2019, Personal Data Protection regulations.

### Document Generation
- **Contract templates** use placeholder system: `{{employee_name}}`, `{{salary}}`, `{{position}}`, etc.
- **PDF generation** with company letterhead, signatures, seals.
- **Version tracking**: Each document generation creates a new version.
- **E-signature support**: Digital signature integration for remote signing.

### Alert System
- **Contract expiration**: Configurable intervals (60/30/15 days).
- **Probation end**: Alert 7 days before probation period ends.
- **Notice period**: Alert when resignation notice period is ending.
- **Channels**: Email + push notification + in-app alert.
- **Escalation**: Unhandled alerts escalated to HR Director after configurable period.

### Seniority Calculation
- **Cumulative**: All periods of employment summed, including gaps if re-hired.
- **Precision**: Calculated in months and days.
- **Use cases**: Severance pay, leave accrual, benefits eligibility.
- **Auto-recalculation**: Recalculated on any employment event.
