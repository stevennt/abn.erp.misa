# M12 - Payroll (Tien luong)

**Category:** HR  
**Priority:** P3  
**Reference Images:** assets/image_20.png, assets/image_50.png, assets/image_51.png, assets/image_63.png  
**Dependencies:** M01 (Organization), M02 (Employee), M03 (Contract), M06 (Allowance), M13 (Time Tracking), M15 (Performance Evaluation)  
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Payroll module manages end-to-end salary calculation, payslip generation, tax declaration, insurance deduction, and compensation reporting for all employees. It integrates with time tracking (attendance, overtime, leave), performance evaluation (bonus calculations), contract data (base salary), and allowance policies to produce accurate, compliant payroll runs on configurable cycles.

### Personas
| Persona | Role | Primary Use |
|---|---|---|
| Payroll Specialist | HR staff | Run payroll cycles, review calculations, resolve discrepancies, send payslips |
| Chief Accountant | Finance | Approve payroll totals, review tax/insurance declarations, audit reports |
| CFO / Finance Director | Executive | View aggregated salary costs, budget variance, income distribution analytics |
| Department Manager | Manager | View team salary summaries (aggregate only), approve bonus recommendations |
| Employee | End user | View personal payslip, income history, request corrections |
| System Administrator | IT | Configure salary components, tax tables, bonus schemes, automation rules |

### Dependencies
| Dependency | Module | Integration Point |
|---|---|---|
| Employee master data | M02 | Employee list, department, position, status |
| Contract salary | M03 | Base salary, probation salary, salary grade |
| Allowances | M06 | Fixed and variable allowance amounts |
| Timesheet | M13 | Working days, overtime hours, unpaid leave, late arrivals |
| Performance results | M15 | Performance bonus multipliers |
| Tax rates | System config | PIT progressive tax table, dependent deductions |
| Insurance rates | M17 | SI/HI/UI contribution percentages |

### Business Rules
1. **Payroll Cycle**: Monthly payroll runs from the 1st to the last day of each calendar month. Payment date configurable per company policy (default: 5th of following month).
2. **Salary Calculation Formula**: `Gross = Base Salary + Allowances + Bonuses + Overtime Pay + Other Income`. `Net = Gross - SI Employee - HI Employee - UI Employee - PIT - Other Deductions`.
3. **PIT (Personal Income Tax)**: Calculated using progressive tax brackets per Vietnam tax law. Deductibles: employee self-deduction (11M VND/month), dependent deduction (4.4M VND/month per dependent).
4. **Insurance Contributions**: SI 8% (employee), HI 1.5% (employee), UI 1% (employee). Employer contributions: SI 17.5%, HI 3%, UI 1%. Capped at 20x base salary floor.
5. **Overtime Pay**: Normal day 150%, weekend 200%, holiday 300% of hourly base rate.
6. **Unpaid Leave Deduction**: `Deduction = (Base Salary / Standard Working Days) * Unpaid Leave Days`.
7. **Probation Salary**: Minimum 85% of contracted salary per Vietnam Labor Code.
8. **Salary Confidentiality**: Employees can only view their own payslips. Managers see aggregated data only. Payroll specialists see all raw data.
9. **Lock Period**: Once a payroll period is finalized and payments are confirmed, no modifications allowed. Corrections require a new adjustment record in a subsequent period.
10. **Bonus Scheme Types**: Sales commission (percentage of revenue), Kicker bonus (target achievement multiplier), Excellent bonus (performance rating-based), Sympathy/Celebration (fixed amounts for events).
11. **Retroactive Adjustments**: Salary changes mid-period are prorated automatically based on effective date.

---

## 2. Screen Inventory

### 2.1 Payroll Dashboard
**Route:** `/payroll/dashboard`  
**Purpose:** Executive overview of total payroll costs, income distribution, and key metrics.

| UI Component | Type | Description |
|---|---|---|
| Total Salary KPI | Card | Total gross salary for current period (e.g., 1,250 Billion VND) |
| PIT KPI | Card | Total personal income tax withheld (e.g., 50 Billion VND) |
| Insurance KPI | Card | Total insurance contributions (e.g., 50 Billion VND) |
| Salary Level Chart | Bar chart | Employee count by salary bands: >30M (50), 20-30M (~150), 10-20M (~200), <10M (~150) |
| Income Structure | Pie chart | Base 54.6%, Sales bonus 28.6%, Kicker 14.3%, Excellent 1.8%, Sympathy 0.7% |
| Avg Income by Office | Bar chart | HN ~18M, HCM ~14M, CT 15M, BMT ~11M, DN ~10M |
| Period Selector | Dropdown | Select payroll month/year |
| Refresh Button | Button | Recalculate dashboard data |

**User Interactions:**
- Click period selector to switch payroll view.
- Click any chart segment to drill down to filtered employee list.
- Export dashboard as PDF.

**Data Displayed:** Aggregated salary totals, tax, insurance, distribution charts, office-level averages.

### 2.2 Payroll Period List
**Route:** `/payroll/periods`  
**Purpose:** Manage payroll processing cycles.

| UI Component | Type | Description |
|---|---|---|
| Period Table | Data table | Period name, month/year, status, employee count, total salary, actions |
| Status Filter | Dropdown | Draft, Calculating, Review, Approved, Paid, Locked |
| Create Button | Button | Start new payroll period |
| Search Box | Input | Filter by period name |
| Bulk Actions | Dropdown | Calculate all, Approve all, Export |

**User Interactions:**
- Click period row to open detail view.
- Create new period triggers employee enrollment and base data snapshot.
- Bulk calculate runs payroll for all enrolled employees.
- Approve locks the period and triggers payment instructions.

**Data Displayed:** All payroll periods with status, counts, totals.

### 2.3 Employee Payroll Detail
**Route:** `/payroll/periods/:id/employees/:empId`  
**Purpose:** View and edit individual employee payroll for a specific period.

| UI Component | Type | Description |
|---|---|---|
| Employee Info Header | Card | Name, code, department, position, contract salary |
| Income Section | Table | Base salary, allowances, bonuses, overtime, other income with amounts |
| Deduction Section | Table | SI, HI, UI, PIT, advance payments, other deductions |
| Net Pay | Display | Final take-home amount |
| Edit Button | Button | Toggle editable mode for components |
| History Tab | Tab | Previous period comparisons |
| Notes Field | Textarea | Adjustment notes |

**User Interactions:**
- Toggle edit mode to manually adjust component values.
- View history to compare with prior months.
- Add notes explaining adjustments.
- Recalculate button triggers formula re-computation.

**Data Displayed:** Full salary breakdown for one employee in one period.

### 2.4 Income Summary by Position
**Route:** `/payroll/reports/income-by-position`  
**Purpose:** Analyze average income grouped by job position (img_51).

| UI Component | Type | Description |
|---|---|---|
| Position Table | Data table | Position name, average income (e.g., CEO 4,853K, BA 7,833K, Deputy CEO 4,443K) |
| Sort Controls | Header clicks | Sort by position name or average income |
| Period Filter | Date range | Select analysis period |
| Export Button | Button | Export to Excel |

**User Interactions:**
- Sort table by any column.
- Filter by date range.
- Export for further analysis.

**Data Displayed:** Position-level average income aggregation.

### 2.5 Payslip Management
**Route:** `/payroll/payslips`  
**Purpose:** Generate, send, and track employee payslips.

| UI Component | Type | Description |
|---|---|---|
| Payslip List | Table | Employee, period, status (sent/unsent), send date |
| Send Button | Button | Email/send selected payslips |
| Preview | Modal | Payslip preview in PDF format |
| Unsent Reminder | Alert | Count of unsent payslips |
| Bulk Send | Button | Send all pending payslips |

**User Interactions:**
- Preview payslip before sending.
- Bulk send to all employees in period.
- View delivery status.

**Data Displayed:** Payslip distribution status, employee payslip records.

### 2.6 Reminders and Alerts
**Route:** `/payroll/reminders`  
**Purpose:** Highlight payroll issues requiring attention.

| UI Component | Type | Description |
|---|---|---|
| Unsent Payslips | Alert list | Employees without payslips for current period |
| Not in Insurance | Alert list | Active employees missing insurance enrollment |
| Irregular Insurance | Alert list | Insurance salary mismatches vs contract salary |
| Dismiss Button | Button | Clear individual alerts |

**User Interactions:**
- Click alert to navigate to affected employee list.
- Dismiss resolved alerts.
- Configure reminder rules in settings.

**Data Displayed:** Active payroll exceptions and compliance issues.

### 2.7 Tax Declaration
**Route:** `/payroll/tax-declarations`  
**Purpose:** Prepare and submit PIT declarations to tax authority.

| UI Component | Type | Description |
|---|---|---|
| Declaration Form | Form | Period, total employees, total PIT, filing status |
| Employee Detail | Table | Per-employee PIT breakdown |
| Export 05/KK-TNCN | Button | Generate tax authority form |
| Submit Status | Display | Filed/Pending/Rejected |

**User Interactions:**
- Generate declaration from approved payroll period.
- Export standardized tax form.
- Track submission status.

**Data Displayed:** PIT declaration data, submission status.

### 2.8 Insurance Declaration
**Route:** `/payroll/insurance-declarations`  
**Purpose:** Prepare insurance contribution reports for social insurance agency.

| UI Component | Type | Description |
|---|---|---|
| Declaration Summary | Cards | Total SI, HI, UI contributions (employee + employer) |
| Employee Breakdown | Table | Per-employee insurance base and contributions |
| Export Form | Button | Generate SI agency form |

**User Interactions:**
- Generate from approved payroll.
- Export for submission.

**Data Displayed:** Insurance contribution declarations.

### 2.9 Payroll Reports
**Route:** `/payroll/reports`  
**Purpose:** Generate standard and custom payroll reports.

| UI Component | Type | Description |
|---|---|---|
| Report Selector | Dropdown | Salary summary, tax report, insurance report, bonus report, comparison |
| Date Range | Date picker | Report period |
| Department Filter | Multi-select | Filter by departments |
| Generate Button | Button | Run report |
| Report Output | Table/Chart | Report results |
| Export | Button | PDF/Excel export |

**User Interactions:**
- Select report type and parameters.
- Generate and review.
- Export in desired format.

**Data Displayed:** Various payroll analytics and summaries.

### 2.10 Salary Component Configuration
**Route:** `/payroll/settings/components`  
**Purpose:** Define salary components (A/B/C categories).

| UI Component | Type | Description |
|---|---|---|
| Component List | Table | Code, name, category (A/B/C), taxable, insurable, formula |
| Add/Edit Form | Modal | Component details |
| Category A | Display | Base salary, position allowance, seniority |
| Category B | Display | Overtime, performance bonus, sales commission |
| Category C | Display | Sympathy, celebration, welfare payments |

**User Interactions:**
- Create/edit/delete components.
- Configure whether each component is taxable and/or subject to insurance.
- Define calculation formulas.

**Data Displayed:** All salary component definitions.

### 2.11 Bonus Scheme Configuration
**Route:** `/payroll/settings/bonus-schemes`  
**Purpose:** Configure bonus calculation rules.

| UI Component | Type | Description |
|---|---|---|
| Scheme List | Table | Name, type, formula, status |
| Scheme Form | Modal | Name, type (fixed/percentage/tiered), parameters |
| Test Calculator | Tool | Preview bonus calculation for sample employee |

**User Interactions:**
- Create bonus schemes.
- Link schemes to positions or departments.
- Test calculations.

**Data Displayed:** Bonus scheme definitions and test results.

---

## 3. Feature Specifications

### 3.1 Payroll Period Management
**Description:** Create and manage monthly payroll processing cycles with status tracking from draft through locked.

**User Stories:**
- As a Payroll Specialist, I can create a new payroll period for a specific month to begin salary calculations.
- As a Payroll Specialist, I can view all periods and their statuses to track processing progress.
- As a Chief Accountant, I can approve a completed payroll period to authorize payments.

**Acceptance Criteria:**
1. Creating a period enrolls all active employees as of the period end date.
2. Base salary, allowances, and contract data are snapshotted at period creation.
3. Status transitions follow: Draft -> Calculating -> Review -> Approved -> Paid -> Locked.
4. Only users with `payroll.period.approve` permission can transition to Approved.
5. Locked periods cannot be modified.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Unique period | No duplicate month/year | "A payroll period already exists for {month}/{year}" |
| Active employees | At least one active employee | "No active employees found for this period" |
| Prerequisite check | Previous period locked or skipped | "Previous period {month}/{year} must be finalized first" |
| Date range | Valid month/year | "Invalid period date" |

**Edge Cases:**
- Employee hired mid-period: prorate base salary from hire date.
- Employee terminated mid-period: prorate base salary through termination date.
- Employee on unpaid leave: deduct proportional amount.
- Multiple contracts for one employee: use primary contract for payroll.
- Retroactive salary change: create adjustment in current period.

### 3.2 Salary Calculation Engine
**Description:** Automated computation of gross and net pay based on configured components, policies, and employee data.

**User Stories:**
- As a Payroll Specialist, I can run bulk salary calculation for all employees in a period.
- As a Payroll Specialist, I can manually adjust individual component values with audit notes.
- As a system, I automatically calculate overtime pay from timesheet data.

**Acceptance Criteria:**
1. Calculation applies formula: Gross = Sum(Income Components) - Sum(Deduction Components).
2. PIT uses progressive tax brackets with configured deductibles.
3. Insurance percentages applied to insurance salary base (capped at 20x floor).
4. Overtime hours from M13 multiplied by hourly rate and applicable multiplier.
5. Manual adjustments require a reason note and are logged.
6. Recalculation is idempotent - running twice produces same result.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Non-negative net | Net pay >= 0 | "Net pay cannot be negative for employee {code}" |
| Valid overtime | Overtime hours <= legal max (300h/year) | "Overtime exceeds legal limit for {employee}" |
| PIT brackets | Current tax tables loaded | "Tax table not configured for year {year}" |
| Insurance cap | Insurance salary <= 20x base floor | "Insurance salary exceeds maximum cap" |

**Edge Cases:**
- Zero-salary period (full month unpaid leave).
- Employee with no tax obligation (income below threshold).
- Multiple income sources for one employee.
- Rounding differences between gross-sum and net-calculation.

### 3.3 Payslip Generation and Distribution
**Description:** Generate individual payslips and distribute to employees via email or portal.

**User Stories:**
- As a Payroll Specialist, I can generate payslips for all employees in an approved period.
- As a Payroll Specialist, I can send payslips via email to employees.
- As an Employee, I can view and download my payslips from the portal.
- As a Payroll Specialist, I can see which payslips have not been sent.

**Acceptance Criteria:**
1. Payslip generated only for Approved or Paid periods.
2. Payslip contains all income and deduction line items with totals.
3. Email delivery tracked (sent, delivered, bounced).
4. Unsent payslip reminder generated automatically.
5. Employee portal shows payslip history sorted by period.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Period status | Period must be Approved or Paid | "Cannot generate payslips for {status} period" |
| Email required | Employee has valid email | "No email configured for employee {code}" |

**Edge Cases:**
- Terminated employee: payslip for final period still generated.
- Employee without email: flag for manual distribution.
- Duplicate sends: system prevents re-sending same payslip without confirmation.

### 3.4 Tax Declaration (PIT)
**Description:** Prepare and export Personal Income Tax declarations per Vietnam tax authority requirements.

**User Stories:**
- As a Chief Accountant, I can generate a PIT declaration from an approved payroll period.
- As a Chief Accountant, I can export the 05/KK-TNCN form for tax authority submission.
- As a Chief Accountant, I can track declaration submission status.

**Acceptance Criteria:**
1. Declaration aggregates PIT from all employees in period.
2. Export follows standard 05/KK-TNCN form format.
3. Includes employee details: name, tax code, income, deductions, PIT.
4. Submission status tracked: Pending, Filed, Accepted, Rejected.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Period approved | Period must be Approved or later | "Cannot declare tax for {status} period" |
| Tax tables loaded | Tax bracket configuration exists | "Tax tables missing for {year}" |

**Edge Cases:**
- Foreign employees with different tax treatment.
- Mid-year tax residency changes.
- Dependent count changes mid-year.

### 3.5 Insurance Declaration
**Description:** Prepare insurance contribution reports for social insurance agency reconciliation.

**User Stories:**
- As a Payroll Specialist, I can generate insurance contribution reports from approved payroll.
- As a Payroll Specialist, I can export insurance declaration forms.

**Acceptance Criteria:**
1. Report includes SI (8% employee + 17.5% employer), HI (1.5% + 3%), UI (1% + 1%).
2. Per-employee breakdown with insurance salary base.
3. Export format compatible with SI agency requirements.

**Edge Cases:**
- Employee not yet enrolled in insurance.
- Insurance salary different from actual salary.
- Retroactive insurance adjustments.

### 3.6 Bonus Scheme Management
**Description:** Configure and apply bonus calculation schemes for different employee groups.

**User Stories:**
- As a Payroll Specialist, I can create bonus schemes with different calculation types.
- As a Payroll Specialist, I can assign bonus schemes to positions or departments.
- As a system, I automatically calculate bonuses during payroll run based on assigned schemes.

**Acceptance Criteria:**
1. Scheme types: Fixed amount, Percentage of base, Tiered (based on target achievement), Performance-based.
2. Schemes linked to positions, departments, or individual employees.
3. Sales commission calculated from revenue data (integration with Sales module).
4. Kicker bonus calculated from target achievement percentage.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid formula | Formula parses correctly | "Invalid bonus formula: {detail}" |
| Required fields | All required fields filled | "Missing required field: {field}" |

**Edge Cases:**
- Employee eligible for multiple schemes: apply all or highest (configurable).
- Scheme effective date in future: not applied until effective.
- Deactivated scheme: no longer applied but historical data preserved.

### 3.7 Payroll Adjustments
**Description:** Record manual adjustments to payroll with full audit trail.

**User Stories:**
- As a Payroll Specialist, I can create an adjustment record for an employee in a period.
- As a Chief Accountant, I can review all adjustments before approving payroll.

**Acceptance Criteria:**
1. Adjustment requires: employee, period, component, amount, reason.
2. Adjustment amount can be positive (addition) or negative (deduction).
3. All adjustments logged with creator, timestamp, and approval status.
4. Adjustment reflected in recalculated payroll totals.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Period not locked | Period status != Locked | "Cannot adjust locked period" |
| Reason required | Reason text non-empty | "Adjustment reason is required" |
| Amount limit | |adjustment| <= configurable threshold or requires approval | "Adjustment exceeds threshold, requires manager approval" |

**Edge Cases:**
- Adjustment after period locked: create in next period with retroactive flag.
- Competing adjustments: last-write-wins with audit log of all changes.

### 3.8 Payroll Analytics Dashboard
**Description:** Visual analytics for salary costs, distribution, and trends (img_20/50/51).

**User Stories:**
- As a CFO, I can view total salary costs, PIT, and insurance for any period.
- As a CFO, I can see salary distribution by level bands and office location.
- As a CFO, I can analyze income structure breakdown across component types.
- As a CFO, I can compare average income by position.

**Acceptance Criteria:**
1. Dashboard shows total salary, PIT, insurance as KPI cards.
2. Salary level bar chart with configurable bands.
3. Income structure pie chart showing component percentages.
4. Office comparison bar chart with average income.
5. Position income table with sortable columns.
6. All charts filterable by period and department.

**Edge Cases:**
- No data for selected period: show empty state message.
- Very large organizations: aggregate data loading with pagination.
- Data latency: dashboard refreshed from materialized view, not live calculation.

### 3.9 Deduction Rule Management
**Description:** Configure deduction rules for advances, loans, garnishments, and other withholdings.

**User Stories:**
- As a Payroll Specialist, I can set up recurring deduction rules for employees.
- As a Payroll Specialist, I can track remaining balance on installment deductions.

**Acceptance Criteria:**
1. Deduction types: Advance repayment, Loan installment, Garnishment, Other.
2. Recurring deductions applied automatically each period until balance exhausted.
3. Balance tracking with remaining amount displayed.
4. One-time deductions applied for single period only.

**Edge Cases:**
- Employee terminated before deduction complete: final paycheck deduction or write-off.
- Deduction exceeds net pay: cap at maximum allowable percentage.

---

## 4. Database Design

### ERD (ASCII)
```
+----------------+       +-------------------+       +-------------------+
| PayrollPeriod  |1-----*| EmployeePayroll   |1-----*| SalaryComponent   |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| period_name    |       | period_id (FK)    |       | component_id (PK) |
| month          |       | employee_id (FK)  |       | code              |
| year           |       | base_salary       |       | name              |
| status         |       | gross_pay         |       | category          |
| start_date     |       | net_pay           |       | is_taxable        |
| end_date       |       | pit_amount        |       | is_insurable      |
| payment_date   |       | si_amount         |       | formula           |
| approved_by    |       | hi_amount         |       | is_active         |
| approved_at    |       | ui_amount         |       | created_at        |
| locked_at      |       | total_deductions  |       | updated_at        |
| created_at     |       | status            |       +-------------------+
| updated_at     |       | created_at        |
+----------------+       | updated_at        |
                         +-------------------+
                                |
                    1----------*|
                                v
              +-------------------+       +-------------------+
              | Payslip           |       | PayrollAdjustment |
              +-------------------+       +-------------------+
              | id (PK)           |       | id (PK)           |
              | employee_payroll  |       | period_id (FK)    |
              | status            |       | employee_id (FK)  |
              | send_method       |       | component_id (FK) |
              | sent_at           |       | amount            |
              | delivery_status   |       | direction         |
              | pdf_url           |       | reason            |
              | created_at        |       | created_by (FK)   |
              +-------------------+       | status            |
                                          | created_at        |
                                          +-------------------+

+----------------+       +-------------------+       +-------------------+
| SalaryPolicy   |       | TaxDeclaration    |       | InsuranceDecl.    |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| policy_type    |       | period_id (FK)    |       | period_id (FK)    |
| effective_from |       | total_employees   |       | total_si_emp      |
| effective_to   |       | total_pit         |       | total_si_employer |
| parameters     |       | filing_status     |       | total_hi_emp      |
| is_active      |       | filed_at          |       | total_hi_employer |
+----------------+       | export_file_url   |       | total_ui_emp      |
                         +-------------------+       | total_ui_employer |
                                                     | status            |
                                                     +-------------------+

+----------------+       +-------------------+
| BonusScheme    |       | DeductionRule     |
+----------------+       +-------------------+
| id (PK)        |       | id (PK)           |
| name           |       | rule_type         |
| scheme_type    |       | employee_id (FK)  |
| formula        |       | amount            |
| target_type    |       | remaining_balance |
| target_id      |       | frequency         |
| is_active      |       | start_period      |
+----------------+       | end_period        |
                         +-------------------+
```

### Table Schemas

#### payroll_period
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| period_name | VARCHAR(100) | NOT NULL | Display name (e.g., "Payroll - April 2026") |
| month | INT | NOT NULL, CHECK 1-12 | Payroll month |
| year | INT | NOT NULL | Payroll year |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft/calculating/review/approved/paid/locked |
| start_date | DATE | NOT NULL | Period start (1st of month) |
| end_date | DATE | NOT NULL | Period end (last day of month) |
| payment_date | DATE | NULL | Actual/scheduled payment date |
| approved_by | UUID | FK -> users.id, NULL | Approver user |
| approved_at | TIMESTAMPTZ | NULL | Approval timestamp |
| locked_at | TIMESTAMPTZ | NULL | Lock timestamp |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### employee_payroll
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| period_id | UUID | FK -> payroll_period.id, NOT NULL | Payroll period |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| base_salary | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Contract base salary |
| allowance_total | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Total allowances |
| bonus_total | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Total bonuses |
| overtime_pay | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Overtime compensation |
| other_income | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Other income items |
| gross_pay | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Total gross pay |
| si_employee | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Social insurance employee portion |
| hi_employee | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Health insurance employee portion |
| ui_employee | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Unemployment insurance employee portion |
| pit_amount | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Personal income tax |
| other_deductions | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Other deductions |
| total_deductions | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Sum of all deductions |
| net_pay | DECIMAL(15,2) | NOT NULL, DEFAULT 0 | Final take-home pay |
| overtime_hours | DECIMAL(8,2) | NOT NULL, DEFAULT 0 | Total overtime hours |
| unpaid_leave_days | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Unpaid leave days |
| working_days | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Actual working days |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/calculated/verified |
| notes | TEXT | NULL | Payroll notes |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### salary_component_value
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_payroll_id | UUID | FK -> employee_payroll.id, NOT NULL | Employee payroll record |
| component_id | UUID | FK -> salary_component.id, NOT NULL | Component definition |
| amount | DECIMAL(15,2) | NOT NULL | Component amount |
| is_auto_calculated | BOOLEAN | NOT NULL, DEFAULT true | System-calculated vs manual |
| notes | TEXT | NULL | Notes for this component |

#### salary_component
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Component code (e.g., BASE, OT, ALLOW_POS) |
| code | VARCHAR(20) | UNIQUE, NOT NULL | Short code |
| name | VARCHAR(100) | NOT NULL | Display name |
| category | VARCHAR(1) | NOT NULL | A (base) / B (variable) / C (welfare) |
| is_taxable | BOOLEAN | NOT NULL, DEFAULT true | Subject to PIT |
| is_insurable | BOOLEAN | NOT NULL, DEFAULT true | Subject to insurance |
| formula | TEXT | NULL | Calculation formula (if auto) |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### salary_policy
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| policy_type | VARCHAR(30) | NOT NULL | tax_table/insurance_rate/minimum_wage |
| effective_from | DATE | NOT NULL | Policy start date |
| effective_to | DATE | NULL | Policy end date (NULL = ongoing) |
| parameters | JSONB | NOT NULL | Policy-specific parameters |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |

#### payslip
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_payroll_id | UUID | FK -> employee_payroll.id, NOT NULL | Employee payroll record |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft/generated/sent/delivered |
| send_method | VARCHAR(20) | NOT NULL, DEFAULT 'email' | email/portal/print |
| sent_at | TIMESTAMPTZ | NULL | Send timestamp |
| delivery_status | VARCHAR(20) | NULL | delivered/bounced/pending |
| pdf_url | VARCHAR(500) | NULL | Stored payslip PDF |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### payroll_adjustment
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| period_id | UUID | FK -> payroll_period.id, NOT NULL | Payroll period |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| component_id | UUID | FK -> salary_component.id, NOT NULL | Affected component |
| amount | DECIMAL(15,2) | NOT NULL | Adjustment amount (positive/negative) |
| direction | VARCHAR(10) | NOT NULL | add/deduct |
| reason | TEXT | NOT NULL | Reason for adjustment |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| approved_by | UUID | FK -> users.id, NULL | Approver (if required) |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/approved/rejected |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### tax_declaration
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| period_id | UUID | FK -> payroll_period.id, NOT NULL | Payroll period |
| total_employees | INT | NOT NULL | Number of employees with PIT |
| total_pit | DECIMAL(15,2) | NOT NULL | Total PIT amount |
| filing_status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft/prepared/filed/accepted/rejected |
| filed_at | TIMESTAMPTZ | NULL | Submission timestamp |
| export_file_url | VARCHAR(500) | NULL | Generated form file |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### insurance_declaration
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| period_id | UUID | FK -> payroll_period.id, NOT NULL | Payroll period |
| total_si_employee | DECIMAL(15,2) | NOT NULL | Employee SI total |
| total_si_employer | DECIMAL(15,2) | NOT NULL | Employer SI total |
| total_hi_employee | DECIMAL(15,2) | NOT NULL | Employee HI total |
| total_hi_employer | DECIMAL(15,2) | NOT NULL | Employer HI total |
| total_ui_employee | DECIMAL(15,2) | NOT NULL | Employee UI total |
| total_ui_employer | DECIMAL(15,2) | NOT NULL | Employer UI total |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft/prepared/submitted |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### bonus_scheme
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(100) | NOT NULL | Scheme name |
| scheme_type | VARCHAR(30) | NOT NULL | fixed/percentage/tiered/performance |
| formula | JSONB | NOT NULL | Calculation parameters |
| target_type | VARCHAR(30) | NOT NULL | all/department/position/individual |
| target_id | UUID | NULL (FK varies) | Target entity reference |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### deduction_rule
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| rule_type | VARCHAR(30) | NOT NULL | advance/loan/garnishment/other |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| amount | DECIMAL(15,2) | NOT NULL | Per-period deduction amount |
| total_amount | DECIMAL(15,2) | NULL | Total obligation (for installments) |
| remaining_balance | DECIMAL(15,2) | NULL | Remaining to deduct |
| frequency | VARCHAR(20) | NOT NULL, DEFAULT 'monthly' | monthly/one-time |
| start_period_id | UUID | FK -> payroll_period.id, NOT NULL | First period to apply |
| end_period_id | UUID | FK -> payroll_period.id, NULL | Last period to apply |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |

### All Tables Summary
| Table | Purpose | Key Relationships |
|---|---|---|
| payroll_period | Payroll cycle management | Parent of EmployeePayroll |
| employee_payroll | Per-employee payroll per period | Links Period, Employee, Components |
| salary_component_value | Individual component amounts | Links EmployeePayroll, Component |
| salary_component | Component definitions | Referenced by component_value |
| salary_policy | Tax tables, insurance rates | Standalone configuration |
| payslip | Payslip generation & delivery | Links EmployeePayroll |
| payroll_adjustment | Manual adjustments | Links Period, Employee, Component |
| tax_declaration | PIT declarations | Links PayrollPeriod |
| insurance_declaration | Insurance declarations | Links PayrollPeriod |
| bonus_scheme | Bonus calculation rules | Linked to EmployeePayroll via calculation |
| deduction_rule | Recurring/one-time deductions | Links Employee, PayrollPeriod |

---

## 5. API Specifications

### JSON Response Format
```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 150
  }
}
```

### Error Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "A payroll period already exists for 4/2026",
    "details": [
      { "field": "month", "message": "Duplicate period for this month/year" }
    ]
  }
}
```

### Endpoints Summary
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| GET | `/api/payroll/periods` | List all payroll periods | payroll.period.read |
| POST | `/api/payroll/periods` | Create new payroll period | payroll.period.create |
| GET | `/api/payroll/periods/:id` | Get period detail | payroll.period.read |
| PATCH | `/api/payroll/periods/:id/status` | Update period status | payroll.period.approve |
| POST | `/api/payroll/periods/:id/calculate` | Run salary calculation | payroll.period.calculate |
| GET | `/api/payroll/periods/:id/employees` | List employees in period | payroll.period.read |
| GET | `/api/payroll/periods/:id/employees/:empId` | Get employee payroll detail | payroll.period.read |
| PUT | `/api/payroll/periods/:id/employees/:empId` | Update employee payroll | payroll.period.edit |
| POST | `/api/payroll/periods/:id/employees/:empId/recalculate` | Recalculate employee | payroll.period.calculate |
| GET | `/api/payroll/payslips` | List payslips | payroll.payslip.read |
| POST | `/api/payroll/payslips/generate` | Generate payslips for period | payroll.payslip.create |
| POST | `/api/payroll/payslips/:id/send` | Send payslip to employee | payroll.payslip.send |
| GET | `/api/payroll/payslips/:id/preview` | Preview payslip PDF | payroll.payslip.read |
| GET | `/api/payroll/dashboard` | Dashboard analytics | payroll.dashboard.read |
| GET | `/api/payroll/reports/income-by-position` | Income by position report | payroll.report.read |
| POST | `/api/payroll/tax-declarations` | Create tax declaration | payroll.tax.create |
| GET | `/api/payroll/tax-declarations/:id/export` | Export tax form | payroll.tax.read |
| POST | `/api/payroll/insurance-declarations` | Create insurance declaration | payroll.insurance.create |
| GET | `/api/payroll/adjustments` | List adjustments | payroll.adjustment.read |
| POST | `/api/payroll/adjustments` | Create adjustment | payroll.adjustment.create |
| PATCH | `/api/payroll/adjustments/:id/status` | Approve/reject adjustment | payroll.adjustment.approve |
| GET | `/api/payroll/components` | List salary components | payroll.settings.read |
| POST | `/api/payroll/components` | Create component | payroll.settings.write |
| PUT | `/api/payroll/components/:id` | Update component | payroll.settings.write |
| DELETE | `/api/payroll/components/:id` | Deactivate component | payroll.settings.write |
| GET | `/api/payroll/bonus-schemes` | List bonus schemes | payroll.settings.read |
| POST | `/api/payroll/bonus-schemes` | Create bonus scheme | payroll.settings.write |
| PUT | `/api/payroll/bonus-schemes/:id` | Update bonus scheme | payroll.settings.write |
| GET | `/api/payroll/deduction-rules` | List deduction rules | payroll.settings.read |
| POST | `/api/payroll/deduction-rules` | Create deduction rule | payroll.settings.write |
| GET | `/api/payroll/reminders` | Get payroll reminders | payroll.period.read |

### Endpoint Details

#### POST /api/payroll/periods
**Request Body:**
```json
{
  "month": 4,
  "year": 2026,
  "period_name": "Payroll - April 2026",
  "payment_date": "2026-05-05"
}
```
**Response:** 201 Created with payroll_period object.

#### POST /api/payroll/periods/:id/calculate
**Description:** Trigger salary calculation for all employees in the period. Runs asynchronously for large employee counts. Returns a job ID for status polling.
**Response:**
```json
{
  "success": true,
  "data": {
    "job_id": "calc-job-123",
    "status": "queued",
    "employee_count": 550
  }
}
```

#### PATCH /api/payroll/periods/:id/status
**Request Body:**
```json
{
  "status": "approved",
  "note": "All calculations verified, ready for payment"
}
```
**Response:** Updated payroll_period object with new status.

#### GET /api/payroll/dashboard
**Query Parameters:** `period_id` (required), `department_id` (optional)
**Response:**
```json
{
  "success": true,
  "data": {
    "total_salary": 1250000000000,
    "total_pit": 50000000000,
    "total_insurance": 50000000000,
    "salary_levels": [
      { "range": ">30M", "count": 50 },
      { "range": "20-30M", "count": 150 },
      { "range": "10-20M", "count": 200 },
      { "range": "<10M", "count": 150 }
    ],
    "income_structure": [
      { "component": "Base", "percentage": 54.6 },
      { "component": "Sales Bonus", "percentage": 28.6 },
      { "component": "Kicker", "percentage": 14.3 },
      { "component": "Excellent", "percentage": 1.8 },
      { "component": "Sympathy", "percentage": 0.7 }
    ],
    "avg_by_office": [
      { "office": "HN", "avg_income": 18000000 },
      { "office": "HCM", "avg_income": 14000000 },
      { "office": "CT", "avg_income": 15000000 },
      { "office": "BMT", "avg_income": 11000000 },
      { "office": "DN", "avg_income": 10000000 }
    ]
  }
}
```

---

## 6. Permissions Matrix

### Roles
| Role | Description |
|---|---|
| HR Director | Full access to all payroll functions |
| Payroll Specialist | Create/edit/calculate payroll, send payslips |
| Chief Accountant | Approve payroll, view all reports, tax declarations |
| CFO | Dashboard access, aggregate reports only |
| Department Manager | Team aggregate salary view (no individual data) |
| Employee | View own payslips only |
| System Admin | Configuration only (components, policies, schemes) |

### Permission Matrix (CRUD)
| Permission | HR Director | Payroll Spec. | Chief Acct. | CFO | Dept. Mgr. | Employee | Sys Admin |
|---|---|---|---|---|---|---|---|
| payroll.period.create | C | C | - | - | - | - | - |
| payroll.period.read | CRUD | CRUD | R | R | R (own dept) | - | - |
| payroll.period.calculate | C | C | - | - | - | - | - |
| payroll.period.approve | C | - | C | - | - | - | - |
| payroll.period.edit | CRUD | CRUD | R | - | - | - | - |
| payroll.payslip.create | C | C | - | - | - | - | - |
| payroll.payslip.read | CRUD | CRUD | R | - | - | R (own) | - |
| payroll.payslip.send | C | C | - | - | - | - | - |
| payroll.adjustment.create | CRUD | C | CRUD | - | - | - | - |
| payroll.adjustment.approve | C | - | C | - | - | - | - |
| payroll.tax.create | C | - | C | - | - | - | - |
| payroll.tax.read | R | R | R | R | - | - | - |
| payroll.insurance.create | C | C | C | - | - | - | - |
| payroll.report.read | R | R | R | R | R (own dept) | - | - |
| payroll.dashboard.read | R | R | R | R | - | - | - |
| payroll.settings.read | R | R | R | - | - | - | R |
| payroll.settings.write | C | - | - | - | - | - | C |

### Row-Level Security
| Rule | Description |
|---|---|
| Employee self-service | Employees can only access payslips where employee_id = current_user.employee_id |
| Department manager | Managers can only see aggregated data for employees in their department chain |
| Payroll specialist | Full access to all payroll data within assigned company/tenant |
| CFO | Access to aggregated dashboard and reports; cannot edit individual records |
| Cross-tenant isolation | All queries scoped to tenant_id (multi-tenant architecture) |

---

## 7. State Machine / Workflows

### Payroll Period Status Transitions
```
                          +----------+
                  +------>|  DRAFT   |
                  |       +----------+
                  |            |
                  |     calculate()
                  |            v
                  |       +------------+
                  |       | CALCULATING|
                  |       +------------+
                  |            |
                  |       calculated
                  |            v
                  |       +----------+
                  |       |  REVIEW  |<---------+
                  |       +----------+          |
                  |            |                |
                  |      approve()       recalculate()
                  |            |                |
                  |            v                |
                  |       +----------+          |
                  |       | APPROVED |          |
                  |       +----------+          |
                  |            |                |
                  |       confirm_pay()         |
                  |            |                |
                  |            v                |
                  |       +----------+          |
                  |       |  PAID    |          |
                  |       +----------+          |
                  |            |                |
                  |       lock()                |
                  |            |                |
                  |            v                |
                  |       +----------+          |
                  +-------|  LOCKED  |---------+
                          +----------+
```

### Status Transition Table
| From Status | To Status | Trigger | Required Role | Side Effects |
|---|---|---|---|---|
| draft | calculating | calculate() | Payroll Specialist | Snapshot employee data, begin calculation job |
| calculating | review | calculation complete | System (auto) | Notify Payroll Specialist |
| review | calculating | recalculate() | Payroll Specialist | Re-run calculation for all or specific employees |
| review | approved | approve() | Chief Accountant | Lock edits, generate payslips, create payment instructions |
| approved | paid | confirm_pay() | Chief Accountant | Record payment date, notify employees |
| paid | locked | lock() | HR Director / System (auto after 30 days) | Finalize, prevent all edits |
| locked | - | - | - | No transitions allowed |

### Approval Workflow
1. **Payroll Specialist** creates period and initiates calculation.
2. **System** calculates salary for all enrolled employees automatically.
3. **Payroll Specialist** reviews calculated results, resolves discrepancies, creates adjustments if needed.
4. **Chief Accountant** reviews totals, tax, insurance, and approves.
5. **System** generates payslips and payment instructions upon approval.
6. **Chief Accountant** confirms payment execution.
7. **System** auto-locks period after configurable grace period (default 30 days).

### Payslip Distribution Workflow
```
  +-------+     generate()     +-----------+     send()      +---------+
  | DRAFT | -----------------> | GENERATED | --------------> |  SENT   |
  +-------+                    +-----------+                 +---------+
                                                               |
                                                    delivery confirmation
                                                               v
                                                          +-----------+
                                                          | DELIVERED |
                                                          +-----------+
```

---

## 8. Reports & Analytics

### 8.1 Payroll Summary Report
**Dimensions:** Period, Department, Office, Position Level  
**Metrics:** Total gross, total net, total PIT, total insurance, employee count, average salary  
**Filters:** Period range, department, office, employee status

### 8.2 Salary Distribution Report
**Dimensions:** Salary bands (>30M, 20-30M, 10-20M, <10M)  
**Metrics:** Employee count per band, percentage of total  
**Filters:** Period, department, office

### 8.3 Income Structure Report
**Dimensions:** Component category (A/B/C), specific component  
**Metrics:** Total amount per component, percentage of gross, per-employee average  
**Filters:** Period, department

### 8.4 Office Comparison Report
**Dimensions:** Office location  
**Metrics:** Average income, median income, total payroll, employee count  
**Filters:** Period, department

### 8.5 Position Income Report
**Dimensions:** Position/job title  
**Metrics:** Average income, min, max, standard deviation, employee count  
**Filters:** Period range

### 8.6 Tax Report (PIT)
**Dimensions:** Employee, tax bracket  
**Metrics:** PIT per employee, total PIT, deductible amounts, dependent count  
**Filters:** Period, department

### 8.7 Insurance Contribution Report
**Dimensions:** Employee, insurance type (SI/HI/UI)  
**Metrics:** Employee contribution, employer contribution, insurance salary base  
**Filters:** Period, department

### 8.8 Bonus Report
**Dimensions:** Bonus scheme, department, position  
**Metrics:** Total bonus, average bonus, employee count receiving bonus  
**Filters:** Period, bonus type

### 8.9 Payroll Trend Report
**Dimensions:** Month-over-month  
**Metrics:** Total salary trend, average salary trend, headcount trend  
**Filters:** Year range, department

### 8.10 Payroll Variance Report
**Dimensions:** Employee, component  
**Metrics:** Current vs prior period amount, variance amount, variance percentage  
**Filters:** Period, department, minimum variance threshold

### Report Summary Table
| Report | Purpose | Primary Users | Export Formats |
|---|---|---|---|
| Payroll Summary | Overall period summary | CFO, Chief Acct., HR Director | PDF, Excel |
| Salary Distribution | Salary band analysis | CFO, HR Director | PDF, Excel |
| Income Structure | Component breakdown | CFO, Payroll Specialist | PDF, Excel |
| Office Comparison | Location cost analysis | CFO, HR Director | PDF, Excel |
| Position Income | Position-level analysis | HR Director, CFO | PDF, Excel |
| Tax Report | PIT filing support | Chief Acct. | PDF, Excel, 05/KK |
| Insurance Report | Insurance reconciliation | Payroll Specialist | PDF, Excel |
| Bonus Report | Bonus analysis | CFO, HR Director | PDF, Excel |
| Payroll Trend | Historical trends | CFO, HR Director | PDF, Excel |
| Payroll Variance | Period-over-period changes | Payroll Specialist, Chief Acct. | PDF, Excel |

---

## 9. Integration Points

### External APIs
| Integration | Direction | Purpose | Frequency |
|---|---|---|---|
| Vietnam Tax Authority API | Outbound | PIT declaration submission | Monthly |
| Social Insurance Agency API | Outbound | Insurance contribution filing | Monthly |
| Bank Payment API | Outbound | Salary payment transfer instructions | Per payroll period |
| M03 Contract Service | Inbound | Base salary, contract status | On period creation, on change |
| M13 Time Tracking Service | Inbound | Working days, overtime, leave | On calculation |
| M15 Performance Service | Inbound | Performance ratings for bonus | On calculation |
| M06 Allowance Service | Inbound | Allowance amounts | On calculation |
| M17 Insurance Service | Inbound | Insurance enrollment status, rates | On calculation |
| Email Service | Outbound | Payslip delivery | Per payslip |
| Notification Service | Outbound | Payroll status notifications | On state changes |

### Webhooks
| Event | Trigger | Payload |
|---|---|---|
| payroll.period.created | New period created | period_id, month, year, employee_count |
| payroll.period.calculated | Calculation complete | period_id, success_count, failure_count |
| payroll.period.approved | Period approved | period_id, total_salary, approved_by |
| payroll.period.paid | Payment confirmed | period_id, payment_date |
| payroll.period.locked | Period locked | period_id, locked_at |
| payslip.sent | Payslip sent to employee | payslip_id, employee_id, send_method |
| payroll.adjustment.created | Adjustment recorded | adjustment_id, employee_id, amount |
| tax_declaration.filed | Tax declaration submitted | declaration_id, total_pit |

### Events Published
| Event | Description | Consumers |
|---|---|---|
| PayrollPeriodStatusChanged | Any status change | Notification Service, Audit Log |
| SalaryCalculated | Individual employee calculated | Payslip Generator |
| PayslipGenerated | Payslip ready for delivery | Email Service, Employee Portal |
| TaxDeclarationPrepared | Tax declaration ready | Chief Accountant notification |
| InsuranceDeclarationPrepared | Insurance declaration ready | SI specialist notification |
| PayrollAnomalyDetected | Unusual salary variance detected | Payroll Specialist notification |

---

## 10. Technical Notes

### Performance
- **Bulk calculation** for 5,000+ employees must complete within 15 minutes using async job processing.
- **Dashboard queries** use materialized views refreshed every 5 minutes during active payroll periods.
- **Payslip PDF generation** batched in groups of 50, processed asynchronously.
- **Database indexing**: Composite indexes on (period_id, employee_id), (status, created_at) for employee_payroll.

### Caching
- **Tax tables** cached in application memory, invalidated on policy change.
- **Salary component definitions** cached with 1-hour TTL.
- **Dashboard aggregates** cached with 5-minute TTL during active periods, 1-hour TTL for historical.
- **Employee master data** cached at period creation to ensure calculation consistency.

### File Handling
- **Payslip PDFs** stored in object storage (S3-compatible) with signed URLs, 2-year retention.
- **Tax/Insurance export files** stored with 5-year retention for audit compliance.
- **File naming convention**: `payslip_{employee_code}_{period}_{timestamp}.pdf`.

### Localization
- **Currency**: VND (Vietnamese Dong), formatted with thousand separators (e.g., 18,000,000).
- **Date format**: DD/MM/YYYY.
- **Language**: Vietnamese (primary), English (secondary for multinational deployments).
- **Tax terminology**: Follows Vietnam General Department of Taxation standards.
- **Number formatting**: Decimal separator is comma or period based on locale setting.

### Mobile Support
- **Employee payslip view** responsive on mobile browsers.
- **Push notifications** for payslip availability (via mobile app).
- **Manager dashboard** viewable on tablet (read-only aggregates).
- **Payroll processing** requires desktop (complex data entry, not mobile-optimized).

### Security
- **Salary data encrypted** at rest (AES-256) and in transit (TLS 1.3).
- **Audit logging** on all create/update/delete operations for payroll data.
- **Access logging** on all payslip views (who viewed whose payslip and when).
- **Data retention**: Payroll records retained for 10 years per Vietnam accounting regulations.
- **GDPR/Vietnam data privacy**: Employee consent for electronic payslip delivery.

### Job Processing
- **Salary calculation** uses background job queue (Redis/Bull or RabbitMQ).
- **Job retry policy**: 3 retries with exponential backoff for transient failures.
- **Dead letter queue** for failed calculations requiring manual intervention.
- **Progress reporting**: Calculation job reports progress percentage for UI display.

### Concurrency
- **Optimistic locking** on payroll_period with version column.
- **Row-level locks** on employee_payroll during calculation to prevent concurrent edits.
- **Idempotent calculation**: Running calculation multiple times produces same result.
