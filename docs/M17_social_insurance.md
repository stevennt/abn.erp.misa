# M17 - Social Insurance (Bao hiem xa hoi)

**Category:** HR  
**Priority:** P3  
**Reference Images:** assets/image_52.png, assets/image_53.png, assets/image_54.png  
**Dependencies:** M02 (Employee), M03 (Contract), M12 (Payroll)  
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Social Insurance module manages employee enrollment, contributions, and declarations for Social Insurance (SI), Health Insurance (HI), and Unemployment Insurance (UI) per Vietnam regulations. It handles labor increase/decrease procedures, contribution adjustments, retroactive collections, card reissuance, and generates required forms (600, 600a, 600c, 600d, 600o, 601, 605, 607, 608, 610, 612) for submission to the Social Insurance Agency. The module tracks declaration status (Not sent, Sent, Missing info, Rejected, Approved) and maintains comprehensive contribution history.

### Personas
| Persona | Role | Primary Use |
|---|---|---|
| Insurance Specialist | HR staff | Create declarations, manage procedures, submit to SI agency, track status |
| Payroll Specialist | HR staff | Retrieve insurance contribution data for payroll calculation |
| Chief Accountant | Finance | Review insurance declarations, verify contribution amounts, audit reports |
| Employee | End user | View personal insurance status, SI number, contribution history |
| HR Director | Executive | Monitor enrollment compliance, approve declarations |
| System Administrator | IT | Configure procedure codes, rate tables, form templates |

### Dependencies
| Dependency | Module | Integration Point |
|---|---|---|
| Employee master data | M02 | Employee info, ID, DOB, department for declarations |
| Contract salary | M03 | Insurance salary base |
| Payroll | M12 | Insurance contribution amounts per period |

### Business Rules
1. **Insurance Types**: Social Insurance (SI - 8% employee + 17.5% employer), Health Insurance (HI - 1.5% employee + 3% employer), Unemployment Insurance (UI - 1% employee + 1% employer).
2. **Insurance Salary Cap**: Maximum insurance salary = 20x government base salary floor. Minimum = government base salary floor.
3. **Enrollment Timing**: New employees enrolled within 30 days of start date. Coverage effective from start date.
4. **Labor Increase**: Register new employee with SI agency using Form 600a. Requires employee info, contract, salary.
5. **Labor Decrease**: Remove employee from SI when terminated. Requires termination decision, last working date.
6. **Contribution Adjustment**: When salary changes, update contribution base. Retroactive adjustments for delayed reporting.
7. **Procedure Codes**: 600 (general), 600a (employee list), 600c (labor increase), 600d (labor decrease), 600o (other), 601 (contribution adjustment), 605 (retroactive collection), 607 (card reissue), 608 (SI policy), 610 (HI policy), 612 (HI only).
8. **Procedure 600 Cases**: 9 distinct cases covering different enrollment scenarios.
9. **Declaration Status Flow**: Not sent -> Sent -> Approved (or Rejected/Missing info).
10. **SI Book**: Each employee receives an SI book number upon enrollment. Tracks lifetime contributions.
11. **HI Card**: Health insurance card issued upon enrollment, renewed annually.
12. **Salary Fund Summary**: Total enrolled employees x insurance salary = total salary fund for contribution calculation.

---

## 2. Screen Inventory

### 2.1 Insurance Records List
**Route:** `/insurance/records`  
**Purpose:** View and manage all insurance declaration records (img_52).

| UI Component | Type | Description |
|---|---|---|
| Records Table | Data table | Record type, status, employee count, submission date, result |
| Status Filter | Dropdown | Not sent/Sent/Missing info/Rejected/Approved |
| Record Types | Column | Labor increase, Labor decrease, Contribution adjustment, Retroactive collection, Card reissue |
| Status Badge | Color-coded | Red (Rejected), Yellow (Missing info), Blue (Sent), Green (Approved), Gray (Not sent) |
| Create Button | Button | New insurance declaration |
| Search | Input | Search by employee, record type |
| Bulk Actions | Dropdown | Submit selected, Export |

**User Interactions:**
- Filter records by status.
- Create new declaration record.
- Submit records to SI agency.
- View record detail with employee list.

**Data Displayed:** Insurance declaration records with status tracking.

### 2.2 Create Declaration Modal
**Route:** `/insurance/records/create`  
**Purpose:** Create new insurance declaration with procedure selection (img_53).

| UI Component | Type | Description |
|---|---|---|
| Category Selector | Radio group | SI Collection, Reissue SI, Reissue HI, SI Policy, HI Policy, HI only, Voluntary, COVID |
| Procedure Code | Dropdown | 600, 600c, 600d, 600o, 601, 605, 607, 608, 610, 612 |
| Case Selector | Dropdown | Procedure 600: 9 cases |
| Employee Selector | Multi-select | Employees for this declaration |
| Form Fields | Form | Procedure-specific fields |
| Attachments | File upload | Required documents |
| Submit Button | Button | Create declaration |

**User Interactions:**
- Select insurance category.
- Choose procedure code.
- Select applicable case (for procedure 600).
- Add employees and required documents.
- Submit to create declaration.

**Data Displayed:** Declaration creation form with procedure configuration.

### 2.3 Employee Insurance List
**Route:** `/insurance/employees`  
**Purpose:** View insurance enrollment status for all employees (img_54).

| UI Component | Type | Description |
|---|---|---|
| Employee Table | Data table | Employee, SI number, HI card number, ID, DOB, department, insurance salary |
| Form Indicator | Badge | Form 600a indicator |
| Selection Count | Display | "20 of 123 employees" selected |
| Summary Bar | Cards | Declared: 20, SI books: 20, HI cards: 15, Salary fund: 120M VND |
| Filter | Dropdowns | Department, enrollment status |
| Export | Button | Export employee insurance data |

**User Interactions:**
- View employee insurance enrollment status.
- Select employees for bulk declaration.
- Filter by department or status.
- View summary statistics.

**Data Displayed:** Employee insurance data: SI numbers, HI cards, salary fund summary.

### 2.4 Declaration Detail
**Route:** `/insurance/records/:id`  
**Purpose:** View detailed declaration with employee list and status.

| UI Component | Type | Description |
|---|---|---|
| Declaration Header | Card | Procedure code, status, submission date, result |
| Employee List | Table | Employees in declaration with individual status |
| Document List | List | Attached documents |
| Submission History | Timeline | Submission events and responses |
| Resubmit Button | Button | Resubmit if rejected or missing info |

**User Interactions:**
- Review declaration details.
- Track individual employee status.
- Resubmit with corrections if rejected.
- Download generated forms.

**Data Displayed:** Full declaration detail with submission history.

### 2.5 Contribution History
**Route:** `/insurance/contributions`  
**Purpose:** Track insurance contributions per employee over time.

| UI Component | Type | Description |
|---|---|---|
| Contribution Table | Data table | Employee, period, SI base, SI amount, HI amount, UI amount |
| Employee Filter | Dropdown | Filter by employee |
| Date Range | Date picker | Contribution period |
| Total Row | Summary | Total contributions |

**User Interactions:**
- View contribution history per employee.
- Filter by date range.
- Export for audit.

**Data Displayed:** Historical insurance contributions.

### 2.6 Insurance Reports
**Route:** `/insurance/reports`  
**Purpose:** Generate insurance reports for management and SI agency.

| UI Component | Type | Description |
|---|---|---|
| Report Selector | Dropdown | Enrollment summary, contribution summary, declaration status, salary fund |
| Date Range | Date picker | Report period |
| Generate | Button | Run report |
| Export Form | Button | Export SI agency form |
| Report Output | Table | Report results |

**User Interactions:**
- Select report type and parameters.
- Generate and review.
- Export for submission.

**Data Displayed:** Insurance analytics and compliance reports.

### 2.7 SI/ HI Card Management
**Route:** `/insurance/cards`  
**Purpose:** Manage SI book and HI card records.

| UI Component | Type | Description |
|---|---|---|
| Card List | Table | Employee, SI book number, HI card number, issue date, status |
| Reissue Request | Button | Request card reissue |
| Reissue Form | Modal | Reason, supporting documents |
| Status Filter | Dropdown | Active/Lost/Damaged/Reissued |

**User Interactions:**
- View SI/HI card inventory.
- Request reissue for lost or damaged cards.
- Track reissue status.

**Data Displayed:** SI book and HI card records.

---

## 3. Feature Specifications

### 3.1 Declaration Management
**Description:** Create, submit, and track insurance declarations with the Social Insurance Agency.

**User Stories:**
- As an Insurance Specialist, I can create a new insurance declaration with procedure code and category.
- As an Insurance Specialist, I can select employees to include in a declaration.
- As an Insurance Specialist, I can track declaration status from creation through approval.
- As an Insurance Specialist, I can resubmit rejected declarations with corrections.

**Acceptance Criteria:**
1. Declaration has: procedure code, category, case (if applicable), employee list, status, submission date.
2. Procedure codes: 600, 600a, 600c, 600d, 600o, 601, 605, 607, 608, 610, 612.
3. Categories: SI Collection, Reissue SI, Reissue HI, SI Policy, HI Policy, HI only, Voluntary, COVID.
4. Procedure 600 has 9 configurable cases.
5. Status flow: Not sent -> Sent -> Approved/Rejected/Missing info.
6. Required documents attached per procedure type.
7. Generated forms follow SI agency format.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Required documents | All documents attached | "Missing required document: {document}" |
| Valid procedure | Procedure code exists | "Invalid procedure code: {code}" |
| Employee enrolled | Employee has valid contract | "Employee {name} not eligible for insurance" |
| Salary within range | Insurance salary within min/max | "Insurance salary outside allowed range" |

**Edge Cases:**
- Employee already enrolled: prevent duplicate enrollment.
- Employee terminated: auto-trigger labor decrease procedure.
- Rejected declaration: correction workflow with highlighted issues.

### 3.2 Labor Increase Procedure
**Description:** Register new employees for social insurance enrollment.

**User Stories:**
- As an Insurance Specialist, I can register a new employee for SI/HI/UI enrollment.
- As an Insurance Specialist, I can generate Form 600a with employee details.
- As a system, I can validate employee information against SI agency requirements.

**Acceptance Criteria:**
1. Form 600a includes: employee name, ID, DOB, department, SI number (if returning), insurance salary.
2. Employee info validated: ID format, DOB for age eligibility, contract status.
3. SI book number assigned (new or existing from prior employment).
4. HI card generated upon approval.
5. Submission to SI agency tracked with status.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid ID | National ID format correct | "Invalid ID number format" |
| Age eligible | Employee >= 15 years old | "Employee below minimum age for insurance" |
| Contract active | Active contract exists | "No active contract for employee" |

**Edge Cases:**
- Returning employee with existing SI book: use existing number, continue contribution history.
- Foreign employee: different insurance rules apply.
- Part-time employee: may have different enrollment requirements.

### 3.3 Labor Decrease Procedure
**Description:** Remove employees from insurance upon termination or resignation.

**User Stories:**
- As an Insurance Specialist, I can process labor decrease for terminated employees.
- As an Insurance Specialist, I can generate the required decrease form.
- As a system, I can auto-suggest decrease when employee status changes to terminated.

**Acceptance Criteria:**
1. Decrease form includes: employee info, last working date, reason, final contribution period.
2. Auto-suggested when employee contract ends or is terminated.
3. Final contribution calculated through last working date.
4. SI book preserved (employee retains for future employment).
5. HI card validity ends per SI agency rules.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid termination | Termination decision exists | "No termination record for employee" |
| Last date set | Last working date defined | "Last working date required" |
| Not already processed | No prior decrease for employee | "Decrease already processed for this employee" |

**Edge Cases:**
- Employee re-hired: labor increase procedure re-triggered.
- Contract expiration vs termination: same decrease procedure, different reason codes.
- Decrease submitted after deadline: retroactive processing with penalty.

### 3.4 Contribution Adjustment
**Description:** Update insurance contribution base when salary changes.

**User Stories:**
- As an Insurance Specialist, I can adjust the insurance salary base for employees.
- As a system, I can auto-detect salary changes and suggest adjustments.
- As an Insurance Specialist, I can process retroactive adjustments for delayed reporting.

**Acceptance Criteria:**
1. Adjustment triggered by: salary change, policy change, correction.
2. Procedure code 601 for contribution adjustment.
3. Retroactive adjustment (605) for missed periods.
4. New contribution calculated from effective date.
5. Difference calculated for retroactive periods.
6. Adjustment form generated and submitted.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Salary changed | New salary differs from current | "No salary change to adjust" |
| Within cap | New salary within min/max cap | "Salary exceeds insurance cap" |
| Effective date valid | Date within reasonable range | "Effective date too far in past" |

**Edge Cases:**
- Multiple salary changes: use most recent, retroactive for prior changes.
- Adjustment rejected by SI agency: correction and resubmission.
- Retroactive beyond allowed period: partial adjustment with note.

### 3.5 Card Reissuance
**Description:** Request replacement SI books or HI cards for lost, damaged, or expired documents.

**User Stories:**
- As an Insurance Specialist, I can request SI book or HI card reissuance.
- As an Insurance Specialist, I can track reissuance request status.
- As an Employee, I can report lost or damaged insurance documents.

**Acceptance Criteria:**
1. Reissuance types: Lost, Damaged, Expired (HI card), Information change.
2. Procedure code 607 for card reissue.
3. Request includes: employee info, reason, supporting documents (police report for lost, damaged card for damaged).
4. Status tracking: Requested -> Submitted -> Processing -> Received.
5. Old card/book number voided, new number assigned.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid reason | Reason category selected | "Select a reason for reissuance" |
| Supporting docs | Required documents attached | "Missing required document for {reason}" |
| Not already requested | No pending request | "Reissuance request already pending" |

**Edge Cases:**
- Employee needs emergency HI card: expedited processing available.
- Multiple reissues for same employee: flag for review.
- SI book full: request new book, old book archived.

### 3.6 Insurance Salary Fund Tracking
**Description:** Track total insurance salary fund for contribution calculation.

**User Stories:**
- As an Insurance Specialist, I can view the total salary fund for all enrolled employees.
- As a Chief Accountant, I can verify the salary fund matches payroll data.
- As a system, I can auto-calculate the salary fund from enrolled employee salaries.

**Acceptance Criteria:**
1. Salary fund = Sum of insurance salaries for all enrolled employees.
2. Displayed in declaration summary (e.g., "120M VND salary fund").
3. Reconciled with payroll data monthly.
4. Variance flagged for review.
5. Fund breakdown by insurance type (SI, HI, UI).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Positive fund | Total > 0 | "No enrolled employees with insurance salary" |
| Matches payroll | Variance within tolerance | "Salary fund differs from payroll by {amount}" |

**Edge Cases:**
- Mid-month enrollment/termination: prorate salary fund.
- Salary adjustment pending: use current salary until adjustment approved.

### 3.7 Procedure Code Management
**Description:** Configure and manage insurance procedure codes and their requirements.

**User Stories:**
- As a System Administrator, I can configure procedure codes with their requirements.
- As an Insurance Specialist, I can view procedure code requirements when creating declarations.

**Acceptance Criteria:**
1. Procedure codes: 600 (general), 600a (employee list), 600c (labor increase), 600d (labor decrease), 600o (other), 601 (adjustment), 605 (retroactive), 607 (reissue), 608 (SI policy), 610 (HI policy), 612 (HI only).
2. Each code has: name, description, required documents, applicable categories, case definitions.
3. Procedure 600: 9 configurable cases.
4. Requirements displayed during declaration creation.

**Edge Cases:**
- New procedure codes added by SI agency: admin updates configuration.
- Procedure code deprecated: existing declarations preserved, no new creation.

---

## 4. Database Design

### ERD (ASCII)
```
+----------------+       +-------------------+       +-------------------+
| SIRecord       |1-----*| SIDeclaration     |       | Employee          |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| procedure_code |       | record_id (FK)    |       +-------------------+
| category       |       | employee_id (FK)  |
| case_type      |       | si_number         |
| status         |       | hi_number         |
| submission_date|       | insurance_salary  |
| result         |       | enrollment_date   |
| created_by     |       | status            |
+----------------+       | form_data         |
                         | documents         |
                         | individual_status |
                         +-------------------+

+----------------+       +-------------------+       +-------------------+
| SIContribution |       | SIProcedure       |       | SIType            |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| employee_id    |       | code              |       | name              |
| period         |       | name              |       | rate_employee     |
| si_base        |       | description       |       | rate_employer     |
| si_amount      |       | required_docs     |       | min_salary        |
| hi_amount      |       | cases_json        |       | max_salary_mult   |
| ui_amount      |       | is_active         |       +-------------------+
| total_amount   |
| created_at     |       +-------------------+
+----------------+

+----------------+       +-------------------+       +-------------------+
| SIDeclaration  |       | SIReport          |       | SIHistory         |
+----------------+       +-------------------+       +-------------------+
| (see above)    |       | id (PK)           |       | id (PK)           |
                 |       | report_type       |       | employee_id       |
                 |       | period            |       | event_type        |
                 |       | data_json         |       | event_date        |
                 |       | generated_at      |       | details           |
                 |       +-------------------+       +-------------------+

+----------------+       +-------------------+
| HIRecord       |       | UIRecord          |
+----------------+       +-------------------+
| id (PK)        |       | id (PK)           |
| employee_id    |       | employee_id       |
| hi_card_number |       | ui_number         |
| issue_date     |       | enrollment_date   |
| expiry_date    |       | status            |
| status         |       +-------------------+
+----------------+
```

### Table Schemas

#### si_record
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| procedure_code | VARCHAR(10) | NOT NULL | 600/600a/600c/600d/600o/601/605/607/608/610/612 |
| procedure_name | VARCHAR(200) | NOT NULL | Procedure display name |
| category | VARCHAR(30) | NOT NULL | si_collection/reissue_si/reissue_hi/si_policy/hi_policy/hi_only/voluntary/covid |
| case_type | VARCHAR(50) | NULL | Specific case (procedure 600 has 9 cases) |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'not_sent' | not_sent/sent/missing_info/rejected/approved |
| submission_date | DATE | NULL | Date submitted to SI agency |
| result_date | DATE | NULL | Date of agency response |
| result_notes | TEXT | NULL | Agency response notes |
| employee_count | INT | NOT NULL, DEFAULT 0 | Number of employees in declaration |
| salary_fund | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Total insurance salary |
| form_file_url | VARCHAR(500) | NULL | Generated form file |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### si_declaration
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| record_id | UUID | FK -> si_record.id, NOT NULL | Parent record |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| si_number | VARCHAR(50) | NULL | Social insurance book number |
| hi_number | VARCHAR(50) | NULL | Health insurance card number |
| insurance_salary | DECIMAL(12,2) | NOT NULL | Insurance salary base |
| enrollment_date | DATE | NULL | Insurance enrollment date |
| individual_status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/submitted/approved/rejected |
| rejection_reason | TEXT | NULL | Individual rejection reason |
| form_data | JSONB | NOT NULL | Form-specific data fields |
| documents | TEXT[] | NULL | Attached document URLs |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### si_contribution
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| period_month | INT | NOT NULL | Contribution month |
| period_year | INT | NOT NULL | Contribution year |
| si_base | DECIMAL(12,2) | NOT NULL | Insurance salary base |
| si_employee | DECIMAL(12,2) | NOT NULL | SI employee portion (8%) |
| si_employer | DECIMAL(12,2) | NOT NULL | SI employer portion (17.5%) |
| hi_employee | DECIMAL(12,2) | NOT NULL | HI employee portion (1.5%) |
| hi_employer | DECIMAL(12,2) | NOT NULL | HI employer portion (3%) |
| ui_employee | DECIMAL(12,2) | NOT NULL | UI employee portion (1%) |
| ui_employer | DECIMAL(12,2) | NOT NULL | UI employer portion (1%) |
| total_employee | DECIMAL(12,2) | NOT NULL | Total employee portion |
| total_employer | DECIMAL(12,2) | NOT NULL | Total employer portion |
| total_amount | DECIMAL(12,2) | NOT NULL | Grand total |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### si_procedure
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| code | VARCHAR(10) | UNIQUE, NOT NULL | Procedure code |
| name | VARCHAR(200) | NOT NULL | Procedure name |
| description | TEXT | NULL | Procedure description |
| required_documents | TEXT[] | NOT NULL | Required document list |
| cases_json | JSONB | NULL | Case definitions (for procedure 600) |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |

#### si_type
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(50) | NOT NULL | SI/HI/UI |
| rate_employee | DECIMAL(5,2) | NOT NULL | Employee contribution rate (%) |
| rate_employer | DECIMAL(5,2) | NOT NULL | Employer contribution rate (%) |
| min_salary | DECIMAL(12,2) | NOT NULL | Minimum insurance salary |
| max_salary_multiplier | DECIMAL(5,2) | NOT NULL, DEFAULT 20 | Max = N x base salary floor |
| effective_from | DATE | NOT NULL | Rate effective date |
| effective_to | DATE | NULL | Rate end date |

#### si_declaration_history
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| event_type | VARCHAR(30) | NOT NULL | enrollment/decrease/adjustment/reissue |
| event_date | DATE | NOT NULL | Event date |
| record_id | UUID | FK -> si_record.id, NULL | Related record |
| details | JSONB | NOT NULL | Event details |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### hi_record
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| hi_card_number | VARCHAR(50) | NOT NULL | HI card number |
| issue_date | DATE | NOT NULL | Card issue date |
| expiry_date | DATE | NOT NULL | Card expiry date |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active/lost/damaged/expired/reissued |
| reissue_count | INT | NOT NULL, DEFAULT 0 | Number of reissues |

#### ui_record
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| ui_number | VARCHAR(50) | NOT NULL | UI registration number |
| enrollment_date | DATE | NOT NULL | Enrollment date |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active/terminated |
| termination_date | DATE | NULL | UI termination date |

#### si_report
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| report_type | VARCHAR(30) | NOT NULL | enrollment/contribution/declaration_status/salary_fund |
| period_start | DATE | NOT NULL | Report start date |
| period_end | DATE | NOT NULL | Report end date |
| data_json | JSONB | NOT NULL | Report data |
| generated_by | UUID | FK -> users.id, NOT NULL | Generator |
| generated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Generation timestamp |
| export_file_url | VARCHAR(500) | NULL | Exported file |

### All Tables Summary
| Table | Purpose | Key Relationships |
|---|---|---|
| si_record | Declaration records | Parent of SIDeclaration |
| si_declaration | Per-employee declaration detail | Links SIRecord, Employee |
| si_contribution | Monthly contribution records | Links Employee |
| si_procedure | Procedure code definitions | Referenced by SIRecord |
| si_type | Insurance rate definitions | Standalone configuration |
| si_declaration_history | Insurance event history | Links Employee, SIRecord |
| hi_record | Health insurance card records | Links Employee |
| ui_record | Unemployment insurance records | Links Employee |
| si_report | Generated reports | Standalone report data |

---

## 5. API Specifications

### JSON Response Format
```json
{
  "success": true,
  "data": { ... },
  "meta": { "page": 1, "per_page": 20, "total": 20 }
}
```

### Error Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Missing required document: Employment contract copy",
    "details": [
      { "field": "documents", "message": "Employment contract copy is required for procedure 600c" }
    ]
  }
}
```

### Endpoints Summary
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| GET | `/api/insurance/records` | List declaration records | insurance.record.read |
| POST | `/api/insurance/records` | Create declaration | insurance.record.create |
| GET | `/api/insurance/records/:id` | Get record detail | insurance.record.read |
| PATCH | `/api/insurance/records/:id/status` | Update record status | insurance.record.update |
| POST | `/api/insurance/records/:id/submit` | Submit to SI agency | insurance.record.submit |
| GET | `/api/insurance/records/:id/declarations` | List employee declarations | insurance.record.read |
| GET | `/api/insurance/employees` | List employee insurance data | insurance.employee.read |
| GET | `/api/insurance/employees/:id` | Get employee insurance detail | insurance.employee.read |
| POST | `/api/insurance/employees/:id/enroll` | Enroll employee | insurance.employee.enroll |
| POST | `/api/insurance/employees/:id/decrease` | Process labor decrease | insurance.employee.decrease |
| GET | `/api/insurance/contributions` | List contributions | insurance.contribution.read |
| GET | `/api/insurance/contributions/:employeeId` | Employee contribution history | insurance.contribution.read |
| POST | `/api/insurance/contributions/adjust` | Adjust contribution base | insurance.contribution.adjust |
| GET | `/api/insurance/cards` | List SI/HI cards | insurance.card.read |
| POST | `/api/insurance/cards/:employeeId/reissue` | Request card reissue | insurance.card.reissue |
| GET | `/api/insurance/procedures` | List procedure codes | insurance.settings.read |
| GET | `/api/insurance/rates` | Get current insurance rates | insurance.settings.read |
| GET | `/api/insurance/reports/:type` | Generate insurance report | insurance.report.read |
| GET | `/api/insurance/summary` | Insurance summary (fund, counts) | insurance.report.read |

### Endpoint Details

#### POST /api/insurance/records
**Request Body:**
```json
{
  "procedure_code": "600c",
  "category": "si_collection",
  "case_type": "case_1",
  "employee_ids": ["emp-001", "emp-002"],
  "documents": ["url-to-contract.pdf", "url-to-id.pdf"],
  "submission_date": "2026-04-15"
}
```
**Response:** 201 Created with si_record object.

#### GET /api/insurance/summary
**Response:**
```json
{
  "success": true,
  "data": {
    "total_employees": 123,
    "declared": 20,
    "si_books": 20,
    "hi_cards": 15,
    "salary_fund": 120000000,
    "by_status": {
      "not_sent": 5,
      "sent": 3,
      "missing_info": 2,
      "rejected": 1,
      "approved": 9
    }
  }
}
```

---

## 6. Permissions Matrix

### Roles
| Role | Description |
|---|---|
| HR Director | Full access to all insurance functions |
| Insurance Specialist | Create declarations, submit, track status, manage cards |
| Payroll Specialist | Read contribution data for payroll |
| Chief Accountant | Review declarations, verify amounts, audit reports |
| Employee | View own insurance status |

### Permission Matrix (CRUD)
| Permission | HR Director | Insurance Spec. | Payroll Spec. | Chief Acct. | Employee |
|---|---|---|---|---|---|
| insurance.record.read | CRUD | CRUD | R | R | R (own) |
| insurance.record.create | C | C | - | - | - |
| insurance.record.update | CRUD | CRUD | - | R | - |
| insurance.record.submit | C | C | - | - | - |
| insurance.employee.read | CRUD | CRUD | R | R | R (own) |
| insurance.employee.enroll | C | C | - | - | - |
| insurance.employee.decrease | C | C | - | - | - |
| insurance.contribution.read | CRUD | CRUD | R | R | R (own) |
| insurance.contribution.adjust | C | C | - | - | - |
| insurance.card.read | CRUD | CRUD | R | R | R (own) |
| insurance.card.reissue | C | C | - | - | - |
| insurance.settings.read | R | R | R | R | - |
| insurance.report.read | R | R | R | R | - |

### Row-Level Security
| Rule | Description |
|---|---|
| Employee self-service | Employees only see their own insurance data |
| Specialist access | Full access within assigned company/tenant |
| Cross-tenant isolation | All queries scoped to tenant_id |

---

## 7. State Machine / Workflows

### Declaration Status Transitions
```
  +-----------+     submit()     +------+    agency_response    +--------------+
  | NOT SENT  | ----------------> | SENT | -------------------> |   APPROVED   |
  +-----------+                  +------+                       +--------------+
                                    |                                ^
                          agency_response                     agency_response
                                    |                                |
                                    v                                |
                          +---------------+                  +--------------+
                          | MISSING INFO  | ---resubmit()--> |    SENT      |
                          +---------------+                  +--------------+
                                    |
                                    v
                          +----------+
                          | REJECTED |
                          +----------+
```

### Status Transition Table
| From Status | To Status | Trigger | Required Role | Side Effects |
|---|---|---|---|---|
| not_sent | sent | submit() | Insurance Specialist | Generate forms, submit to SI agency, set submission date |
| sent | approved | agency_response (approved) | System (auto) | Update employee SI numbers, activate coverage |
| sent | missing_info | agency_response (missing) | System (auto) | Flag missing items, notify specialist |
| sent | rejected | agency_response (rejected) | System (auto) | Flag rejection reasons, notify specialist |
| missing_info | sent | resubmit() | Insurance Specialist | Attach missing documents, resubmit |
| rejected | sent | resubmit() | Insurance Specialist | Correct errors, resubmit |

### Labor Increase Workflow
```
  +--------------------+     validate()      +-----------------+    generate_form()   +----------+
  | Employee Eligible  | -------------------> | Info Validated  | -------------------> | Form 600a|
  +--------------------+                     +-----------------+                      +----------+
                                                                                            |
                                                                                      submit()
                                                                                            |
                                                                                            v
                                                                                     +-------------+
                                                                                     | SENT to SI  |
                                                                                     +-------------+
                                                                                            |
                                                                                   agency_response
                                                                                            |
                                                                                            v
                                                                                     +-------------+
                                                                                     | APPROVED    |
                                                                                     | SI# assigned|
                                                                                     +-------------+
```

---

## 8. Reports & Analytics

### 8.1 Enrollment Summary Report
**Dimensions:** Department, Insurance type  
**Metrics:** Enrolled count, SI books issued, HI cards issued, pending enrollment  
**Filters:** Department, enrollment status

### 8.2 Contribution Summary Report
**Dimensions:** Period, Insurance type, Department  
**Metrics:** Employee portion, employer portion, total, per-employee breakdown  
**Filters:** Period range, department

### 8.3 Declaration Status Report
**Dimensions:** Declaration type, Status  
**Metrics:** Count per status, processing time, rejection rate  
**Filters:** Date range, procedure code

### 8.4 Salary Fund Report
**Dimensions:** Period, Department  
**Metrics:** Total salary fund, per-employee salary, average salary  
**Filters:** Period, department

### 8.5 Card Reissuance Report
**Dimensions:** Card type, Reason  
**Metrics:** Reissue count, by reason, by employee  
**Filters:** Date range

### 8.6 Compliance Report
**Dimensions:** Employee, Enrollment status  
**Metrics:** Not enrolled, enrolled, coverage gaps, late enrollment  
**Filters:** Department, period

### Report Summary Table
| Report | Purpose | Primary Users | Export Formats |
|---|---|---|---|
| Enrollment Summary | Coverage overview | Insurance Spec., HR Director | PDF, Excel |
| Contribution Summary | Contribution audit | Chief Acct., Payroll | PDF, Excel |
| Declaration Status | Processing tracking | Insurance Spec. | PDF, Excel |
| Salary Fund | Fund verification | Chief Acct. | PDF, Excel |
| Card Reissuance | Card management | Insurance Spec. | PDF, Excel |
| Compliance | Regulatory compliance | HR Director, Chief Acct. | PDF, Excel |

---

## 9. Integration Points

### External APIs
| Integration | Direction | Purpose | Frequency |
|---|---|---|---|
| Vietnam SI Agency API/Portal | Outbound | Declaration submission | Per declaration |
| SI Agency Portal | Inbound | Status check, result retrieval | Daily (poll) |
| M02 Employee Service | Inbound | Employee data for declarations | Real-time (event-driven) |
| M03 Contract Service | Inbound | Salary data for insurance base | On salary change |
| M12 Payroll Service | Inbound/Outbound | Contribution data exchange | Monthly |
| Email Service | Outbound | Status notifications | On event |

### Webhooks
| Event | Trigger | Payload |
|---|---|---|
| declaration.submitted | Declaration sent to agency | record_id, procedure_code, employee_count |
| declaration.approved | Agency approves | record_id, approved_employees |
| declaration.rejected | Agency rejects | record_id, rejection_reasons |
| employee.enrolled | Employee enrolled in SI | employee_id, si_number, hi_number |
| employee.decreased | Employee removed from SI | employee_id, last_contribution_period |
| card.reissued | New card issued | employee_id, card_type, new_number |

### Events Published
| Event | Description | Consumers |
|---|---|---|
| DeclarationStatusChanged | Status update | Notification Service, Audit Log |
| EmployeeEnrolled | New SI enrollment | Payroll Service (M12) |
| ContributionRateChanged | Insurance rate update | Payroll Service (M12), Notification |

---

## 10. Technical Notes

### Performance
- **Declaration generation**: Generate forms for 100+ employees within 30 seconds.
- **Contribution calculation**: Calculate monthly contributions for all employees within 2 minutes.
- **Summary dashboard**: Pre-aggregated queries, <1 second response.
- **Database indexing**: Index on (procedure_code, status) for record filtering, (employee_id, period) for contributions.

### Caching
- **Insurance rates** cached with 24-hour TTL (changes infrequently).
- **Procedure code definitions** cached with 1-hour TTL.
- **Summary statistics** cached with 5-minute TTL.

### File Handling
- **Declaration forms** stored in object storage with signed URLs, 10-year retention (regulatory requirement).
- **Supporting documents** stored with 10-year retention.
- **Form naming convention**: `{procedure_code}_{record_id}_{timestamp}.pdf`.

### Localization
- **Language**: Vietnamese (primary).
- **Currency**: VND.
- **Date format**: DD/MM/YYYY.
- **Form terminology**: Vietnam Social Insurance Agency standard terms.
- **Procedure codes**: Official SI agency codes.

### Mobile Support
- **Employee insurance status view** responsive on mobile.
- **Declaration management** requires desktop (complex forms).
- **Push notifications** for declaration status changes.

### Security
- **Insurance data** encrypted at rest (AES-256).
- **SI numbers** considered sensitive personal data.
- **Audit logging**: All declaration operations logged.
- **Data retention**: Insurance records retained for 10 years per Vietnam regulations.
- **Compliance**: Vietnam Social Insurance Law, Personal Data Protection regulations.

### Form Generation
- **Form templates** stored as structured templates (JSON schema + layout).
- **Dynamic field population** from employee and declaration data.
- **PDF generation** using template engine.
- **Form versioning**: Templates versioned per SI agency updates.

### Validation Engine
- **Pre-submission validation**: All required fields, documents, employee eligibility checked before submit.
- **SI agency validation rules**: Configurable rule set matching agency requirements.
- **Auto-correction suggestions**: For common validation failures (e.g., ID format).
