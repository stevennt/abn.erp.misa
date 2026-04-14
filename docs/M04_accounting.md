# M04 Accounting Module Specification

## 1. Module Overview

### 1.1 Purpose

The Accounting Module (M04) is the financial backbone of the ABN ERP system. It provides comprehensive accounting capabilities including cash management, deposit tracking, accounts receivable, accounts payable, revenue tracking, expense management, profit calculation, inventory valuation, general ledger management, journal entries, tax declarations, financial reporting, budgeting, and financial analysis. The module ensures compliance with Vietnamese accounting standards (VAS) and supports real-time financial monitoring.

Key financial indicators: Revenue 65,338M (+15%), Expenses 51,212M (-6%), Profit 5,126M (+10%), Inventory value 640M. The module processes cash transactions totaling 7,500M, deposits of 1,000M, receivables of 5,082M, payables of 903M, with revenue accounts at 2,500M and expense accounts at 1,009M.

### 1.2 Personas

| Persona | Role | Key Responsibilities | Primary Screens | Access Level |
|---------|------|---------------------|-----------------|--------------|
| Chief Accountant | Financial oversight | Review all entries, approve journals, generate financial statements | Summary, Reports, FinancialAnalysis | Full |
| Cashier | Cash operations | Record cash receipts/payments, manage cash book, bank deposits | Cash, Deposit, BankAccount | Read/Write |
| AP Clerk | Payables management | Record supplier invoices, process payments, reconcile payables | Payable, Invoice, Purchasing | Read/Write |
| AR Clerk | Receivables management | Issue invoices, track collections, manage receivables | Receivable, Invoice, Sales | Read/Write |
| Tax Accountant | Tax compliance | Prepare tax declarations, e-invoice generation, tax reporting | TaxDeclaration, Invoice | Read/Write |
| Cost Accountant | Cost analysis | Inventory costing, cost allocation, profitability analysis | Costing, Warehouse, Reports | Read/Write |
| Financial Analyst | Strategic analysis | Budget variance, financial ratios, trend analysis | Budget, Reports, FinancialAnalysis | Read-only |
| Finance Manager | Department management | Budget approval, policy setting, audit review | Summary, Budget, Reports | Full |

### 1.3 Dependencies

| Dependency | Type | Direction | Description |
|-----------|------|-----------|-------------|
| M07 Purchasing | Upstream | Inbound | Purchase invoices drive AP entries and expense recording |
| M08 Sales | Upstream | Inbound | Sales invoices drive AR entries and revenue recording |
| M09 Inventory/Warehouse | Upstream | Inbound | Inventory movements trigger COGS and valuation entries |
| M05 Employee Management | Upstream | Inbound | Payroll data feeds into expense accounts |
| M10 Asset Management | Upstream | Inbound | Depreciation entries flow to expense accounts |
| M11 Contract Management | Upstream | Inbound | Contract payment schedules drive cash flow planning |
| Bank Integration | External | Bidirectional | Bank statement import and payment processing |
| Tax Authority System | External | Outbound | E-invoice submission and tax declaration filing |

### 1.4 Business Rules

1. **Double-Entry Principle**: Every transaction must have equal debits and credits; zero-balance enforced at posting time.
2. **Fiscal Year Lock**: Once a fiscal year is closed, no entries can be added or modified for that period.
3. **Sequential Numbering**: Journal entries must be numbered sequentially within each fiscal year; gaps require supervisor approval.
4. **Currency Handling**: Base currency is VND; foreign currency transactions require daily exchange rate conversion.
5. **Tax Calculation**: VAT is calculated on the gross amount unless marked as tax-exclusive; e-invoice QR code mandatory for all customer invoices.
6. **Receivable Recognition**: Revenue is recognized upon invoice issuance; collection status tracked separately.
7. **Payable Recognition**: Liability is recognized upon receipt of supplier invoice or goods receipt, whichever is earlier.
8. **Cash Reconciliation**: Daily cash count must match cash book balance; discrepancies require immediate investigation.
9. **Budget Enforcement**: Purchase orders exceeding budget require additional approval; warning at 80% threshold.
10. **Audit Trail**: All modifications to posted entries create an audit trail with user ID, timestamp, and reason.
11. **Chart of Accounts Structure**: Account codes follow hierarchical structure (e.g., 111-Cash, 112-Deposits, 131-Receivables, 331-Payables, 511-Revenue, 642-Expenses, 911-Profit).
12. **Depreciation Posting**: Asset depreciation automatically posts to expense accounts monthly.

---

## 2. Screen Inventory

### 2.1 Screen Routes and Purposes

| Route | Purpose | Personas | Priority |
|-------|---------|----------|----------|
| `/accounting/cash` | Record cash receipts and payments, view cash book | Cashier, Chief Accountant | Critical |
| `/accounting/deposit` | Manage bank deposits, interest calculation | Cashier, Finance Manager | High |
| `/accounting/receivable` | Track customer invoices and collections | AR Clerk, Chief Accountant | Critical |
| `/accounting/payable` | Track supplier invoices and payments | AP Clerk, Chief Accountant | Critical |
| `/accounting/invoice` | Create and manage all invoices (sales/purchase) | AR Clerk, Tax Accountant | Critical |
| `/accounting/purchasing` | Purchase-related accounting entries | AP Clerk, Cost Accountant | High |
| `/accounting/sales` | Sales-related accounting entries | AR Clerk, Cost Accountant | High |
| `/accounting/tools` | Accounting utilities, import/export, reconciliation | Chief Accountant | High |
| `/accounting/fixed-assets` | Asset depreciation, asset-related entries | Cost Accountant | Medium |
| `/accounting/tax` | Tax declarations, e-invoice management | Tax Accountant | Critical |
| `/accounting/costing` | Cost allocation, COGS calculation | Cost Accountant | High |
| `/accounting/summary` | Dashboard with key financial indicators | All personas | Critical |
| `/accounting/budget` | Budget planning, tracking, variance analysis | Finance Manager, Financial Analyst | High |
| `/accounting/reports` | Financial statements, management reports | Chief Accountant, Finance Manager | Critical |
| `/accounting/financial-analysis` | Ratio analysis, trend analysis, forecasting | Financial Analyst, Finance Manager | High |

### 2.2 UI Components by Screen

#### /accounting/summary (Dashboard)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Revenue KPI Card | Metric Card | Display total revenue 65,338M with +15% trend | Revenue summary query |
| Expense KPI Card | Metric Card | Display total expenses 51,212M with -6% trend | Expense summary query |
| Profit KPI Card | Metric Card | Display profit 5,126M with +10% trend | Profit calculation |
| Cash Balance Card | Metric Card | Display cash on hand 7,500M | CashBook current balance |
| Receivable Card | Metric Card | Outstanding receivables 5,082M | Receivable summary |
| Payable Card | Metric Card | Outstanding payables 903M | Payable summary |
| Inventory Value Card | Metric Card | Inventory value 640M | Inventory valuation |
| Revenue Trend Chart | Line Chart | Monthly revenue trend with YoY comparison | Historical revenue data |
| Expense Breakdown | Pie Chart | Expense categories distribution | ChartOfAccounts expenses |
| Cash Flow Chart | Bar Chart | Monthly cash inflow vs outflow | CashBook + DepositBook |
| Quick Actions Bar | Action Bar | New entry, import, report generation | N/A |
| Notification Panel | List | Overdue invoices, budget alerts, pending approvals | Alert system |

#### /accounting/cash

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Cash Account Selector | Dropdown | Select cash account (1111, 1112, etc.) | ChartOfAccounts |
| Transaction Grid | Data Table | List all cash transactions | CashBook |
| New Receipt Form | Modal Form | Record cash receipt | User input |
| New Payment Form | Modal Form | Record cash payment | User input |
| Daily Balance Summary | Summary Row | Opening + Receipts - Payments = Closing | CashBook calculations |
| Search/Filter Bar | Filter Panel | Filter by date, account, reference | User input |
| Export Button | Action Button | Export to Excel/PDF | CashBook data |
| Delete/Undo Button | Action Button | Void transaction (pre-posting only) | CashBook |

#### /accounting/receivable

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Customer Selector | Autocomplete | Select customer for new collection | Customer master |
| Invoice List | Data Table | Outstanding invoices by customer | Invoice + Receivable |
| Collection Form | Modal Form | Record payment received | User input |
| Aging Report Grid | Pivot Table | AR aging (0-30, 31-60, 61-90, 90+) | Receivable aging query |
| Customer Balance Card | Info Panel | Total balance, credit limit, overdue amount | Receivable summary |
| Apply Payment Button | Action Button | Allocate payment to specific invoices | Receivable |
| Write-off Button | Action Button | Write off bad debt (requires approval) | Receivable |

#### /accounting/payable

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Supplier Selector | Autocomplete | Select supplier for new payment | Supplier master |
| Invoice List | Data Table | Outstanding supplier invoices | Invoice + Payable |
| Payment Form | Modal Form | Record payment made to supplier | User input |
| Aging Report Grid | Pivot Table | AP aging analysis | Payable aging query |
| Supplier Balance Card | Info Panel | Total balance, payment terms, overdue | Payable summary |
| Batch Payment Button | Action Button | Process multiple payments at once | Payable |
| Import Invoice Button | Action Button | Import supplier invoices from CSV | File upload |

#### /accounting/tax

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Tax Period Selector | Dropdown | Select tax period (monthly/quarterly) | Fiscal calendar |
| VAT Declaration Form | Form | VAT input/output summary | Invoice data |
| E-Invoice Generator | Wizard | Generate e-invoice with QR code | Invoice + Tax rules |
| QR Code Preview | Image | Display e-invoice QR code | E-invoice service |
| Tax Filing History | Data Table | Historical tax declarations | TaxDeclaration |
| Submit Button | Action Button | Submit to tax authority portal | External API |
| Export XML Button | Action Button | Export in tax authority format | TaxDeclaration |

#### /accounting/budget

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Budget Year Selector | Dropdown | Select budget year | Budget master |
| Budget Entry Grid | Spreadsheet | Input budget amounts by account/month | Budget |
| Actual vs Budget Grid | Pivot Table | Compare actual spending to budget | Budget + GeneralLedger |
| Variance Chart | Bar Chart | Budget variance by department | Variance calculations |
| Approval Workflow | Timeline | Budget approval status | BudgetApproval |
| Import Budget Button | Action Button | Import budget from Excel | File upload |
| Lock Budget Button | Action Button | Lock budget after approval | Budget |

### 2.3 User Interactions

1. **Cash Receipt Entry**: Cashier clicks "New Receipt" -> fills form (date, account, amount, description, customer reference) -> system generates journal entry (Dr Cash, Cr Revenue/Receivable) -> saves to CashBook and GeneralLedger.
2. **Invoice Collection**: AR Clerk selects invoice -> clicks "Apply Payment" -> enters amount -> system allocates payment (oldest invoice first) -> updates Receivable status.
3. **Tax Declaration**: Tax Accountant selects period -> system auto-calculates VAT from invoices -> reviews adjustments -> generates XML -> submits to tax portal.
4. **Budget Variance Review**: Financial Analyst selects period -> views Actual vs Budget grid -> clicks variance bars for drill-down -> exports report for management.

### 2.4 Data Displayed

- Cash Book: Date, voucher number, reference, description, debit amount, credit amount, running balance
- Receivable/Payable: Invoice number, date, customer/supplier, original amount, paid amount, outstanding balance, due date, aging bucket
- Financial Reports: Balance Sheet, Income Statement, Cash Flow Statement with comparative periods
- Budget: Account code, account name, budget amount YTD, actual YTD, variance amount, variance percentage

---

## 3. Feature Specifications

### 3.1 Cash Book Management

**Description**: Record and track all cash transactions with automatic journal entry generation and running balance calculation.

**User Stories**:
- As a Cashier, I want to record cash receipts and payments quickly so that my cash book stays current.
- As a Chief Accountant, I want to review all cash entries before they are posted so that errors are caught early.
- As a Finance Manager, I want to see real-time cash balance so that I can make informed decisions.

**Acceptance Criteria**:
1. Cash receipt form captures: date, cash account, amount, currency, description, reference document, customer/supplier link.
2. System automatically generates journal entry with correct debit/credit accounts.
3. Running balance updates in real-time after each entry.
4. Daily cash total matches physical count before close of day.
5. Entries can be edited only before posting; posted entries require reversal.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-001 | Amount | Must be > 0 | "Amount must be greater than zero" |
| VR-002 | Date | Must be within open fiscal period | "Cannot post to a closed fiscal period" |
| VR-003 | Account | Must be valid cash account (111x) | "Invalid cash account selected" |
| VR-004 | Description | Required, max 255 chars | "Description is required" |
| VR-005 | Reference | Optional, max 100 chars | "Reference too long" |
| VR-006 | Currency | Must match account currency or provide rate | "Exchange rate required for foreign currency" |
| VR-007 | Balance | Cannot go negative for cash accounts | "Cash balance cannot be negative" |

**Edge Cases**:
- Negative cash balance detection: System warns and requires supervisor override.
- Foreign currency cash accounts: Exchange rate fluctuations create unrealized gain/loss entries.
- Backdated entries in closed period: Blocked with clear error message.
- Duplicate voucher numbers: System generates unique voucher numbers automatically.
- Partial payment receipt: System tracks remaining balance on original invoice.

### 3.2 General Ledger and Journal Entry Management

**Description**: Central repository of all accounting entries with full audit trail, posting controls, and period management.

**User Stories**:
- As a Chief Accountant, I want to create manual journal entries for adjustments and accruals.
- As a Chief Accountant, I want to review and approve journal entries before posting.
- As an auditor, I want to trace any GL entry back to its source document.

**Acceptance Criteria**:
1. Journal entry form supports unlimited lines with account, debit, credit, description.
2. System validates total debits equal total credits before saving.
3. Posting is a separate action requiring approval workflow.
4. Each entry links to source module (Sales, Purchasing, Manual, etc.).
5. Audit trail records all changes with user ID, timestamp, and reason.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-010 | Debits/Credits | Total debits = Total credits | "Entry is not balanced" |
| VR-011 | Account | Must exist in Chart of Accounts | "Account code does not exist" |
| VR-012 | Period | Must be open fiscal period | "Fiscal period is closed" |
| VR-013 | Description | Required per line | "Each line needs a description" |
| VR-014 | Amount | At least one line with amount > 0 | "Entry has no amount" |
| VR-015 | Posted entries | Cannot modify posted entries | "Posted entries cannot be modified" |

**Edge Cases**:
- Multi-currency journal entries: Each line can have different currency with automatic conversion.
- Reversing entries: System creates exact opposite entry in current period.
- Recurring entries: Template-based entries auto-generated on schedule.
- Split entries: Single transaction split across multiple cost centers.

### 3.3 Accounts Receivable

**Description**: Track customer invoices, payments, credit notes, and outstanding balances with aging analysis.

**User Stories**:
- As an AR Clerk, I want to apply customer payments to specific invoices so that balances are accurate.
- As a Chief Accountant, I want to see aging analysis of receivables so that I can assess collection risk.
- As a Finance Manager, I want to track DSO (Days Sales Outstanding) for performance monitoring.

**Acceptance Criteria**:
1. Each sales invoice creates an AR entry automatically.
2. Payment application supports partial payments and overpayments.
3. Aging report shows buckets: current, 1-30, 31-60, 61-90, 90+ days.
4. Credit notes reduce AR balance and can be applied to invoices.
5. Bad debt write-off requires manager approval.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-020 | Payment Amount | Cannot exceed outstanding balance | "Payment exceeds outstanding amount" |
| VR-021 | Customer | Must be active customer | "Customer account is inactive" |
| VR-022 | Invoice | Must be unpaid invoice | "Invoice already fully paid" |
| VR-023 | Credit Note | Cannot exceed original invoice | "Credit exceeds invoice amount" |

**Edge Cases**:
- Customer overpayment: Creates credit balance for future application.
- Invoice in foreign currency: Payment exchange rate difference creates gain/loss.
- Partial payment across multiple invoices: System allocates proportionally or by user selection.
- Write-off reversal: If customer pays after write-off, system reverses and applies payment.

### 3.4 Accounts Payable

**Description**: Track supplier invoices, payments, debit notes, and outstanding balances with payment scheduling.

**User Stories**:
- As an AP Clerk, I want to record supplier invoices and schedule payments so that we maintain good supplier relationships.
- As a Finance Manager, I want to see payment obligations by due date so that I can manage cash flow.
- As a Chief Accountant, I want to reconcile supplier statements with our records.

**Acceptance Criteria**:
1. Purchase invoices from M07 Purchasing auto-create AP entries.
2. Manual AP entry for non-PO invoices.
3. Payment scheduling with due date tracking and alerts.
4. Supplier statement reconciliation tool.
5. Early payment discount tracking.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-030 | Invoice Date | Must be within current or prior period | "Invoice date out of range" |
| VR-031 | Supplier | Must be active supplier | "Supplier account is inactive" |
| VR-032 | Amount | Must match PO amount within tolerance | "Amount exceeds PO by more than tolerance" |
| VR-033 | Payment Date | Cannot be before invoice date | "Payment date cannot precede invoice date" |

**Edge Cases**:
- Invoice received before goods: Create GRNI (Goods Received Not Invoiced) accrual.
- Payment in foreign currency: Exchange rate difference creates gain/loss.
- Supplier credit note: Reduces AP balance and can offset future payments.
- Duplicate invoice detection: System flags similar invoice numbers from same supplier.

### 3.5 Invoice Management and E-Invoice

**Description**: Create, manage, and issue invoices with e-invoice generation including QR code for tax compliance.

**User Stories**:
- As an AR Clerk, I want to generate customer invoices from sales orders so that billing is automated.
- As a Tax Accountant, I want e-invoices with QR codes so that they comply with tax regulations.
- As a Chief Accountant, I want to track invoice status (draft, issued, cancelled) for audit purposes.

**Acceptance Criteria**:
1. Invoice generation from sales order or manual entry.
2. E-invoice format complies with Vietnamese tax authority requirements.
3. QR code contains: invoice number, date, tax code, total amount, verification code.
4. Invoice cancellation requires reason and approval.
5. Batch invoice generation for multiple orders.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-040 | Invoice Number | Must be sequential within series | "Invoice number gap detected" |
| VR-041 | Tax Code | Must be valid customer tax code | "Invalid tax code" |
| VR-042 | VAT Rate | Must be valid rate (0, 5, 8, 10%) | "Invalid VAT rate" |
| VR-043 | Total | Line total + VAT = Grand total | "Totals do not reconcile" |

**Edge Cases**:
- Invoice modification after issuance: Must issue credit note + new invoice.
- E-invoice submission failure: Queue for retry with error notification.
- Bulk invoice generation: Rate limiting and error handling for large batches.
- Cancelled invoice reversal: Audit trail preserved, status updated.

### 3.6 Tax Declaration

**Description**: Prepare and submit tax declarations including VAT, CIT, PIT with auto-calculation from transaction data.

**User Stories**:
- As a Tax Accountant, I want the system to auto-calculate VAT from invoices so that I save time on preparation.
- As a Tax Accountant, I want to export declarations in the tax authority's format so that I can submit electronically.
- As a Chief Accountant, I want to review tax calculations before submission so that errors are prevented.

**Acceptance Criteria**:
1. VAT declaration auto-populated from sales and purchase invoices.
2. Input VAT vs output VAT reconciliation.
3. CIT calculation with adjustments for non-deductible expenses.
4. PIT calculation from payroll data (M05 integration).
5. Export in General Department of Taxation XML format.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-050 | Period | Must be valid tax period | "Tax period not found" |
| VR-051 | VAT Reconciliation | Input VAT + Output VAT = Net VAT | "VAT reconciliation failed" |
| VR-052 | Submission Date | Before legal deadline | "Submission deadline has passed" |
| VR-053 | Required Fields | All mandatory fields completed | "Missing required tax information" |

**Edge Cases**:
- Late submission: Calculate penalty automatically.
- Amendment after submission: Create supplementary declaration.
- Tax audit adjustment: Record adjustments with reference to audit report.

### 3.7 Budget Management

**Description**: Create, track, and analyze budgets with variance reporting and approval workflows.

**User Stories**:
- As a Finance Manager, I want to create annual budgets by account and department so that spending is controlled.
- As a Financial Analyst, I want to see budget vs actual variance in real-time so that I can identify issues early.
- As a Department Manager, I want to see my remaining budget so that I can plan spending.

**Acceptance Criteria**:
1. Budget creation by account, department, month.
2. Budget approval workflow with multiple levels.
3. Real-time budget consumption tracking.
4. Variance analysis with drill-down to transactions.
5. Budget transfer between accounts requires approval.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-060 | Budget Amount | Must be >= 0 | "Budget amount cannot be negative" |
| VR-061 | Account | Must be budget-controlled account | "Account is not budget-controlled" |
| VR-062 | Period | Must be within budget year | "Outside budget year" |
| VR-063 | Transfer | Total transfers cannot exceed threshold | "Transfer limit exceeded" |

**Edge Cases**:
- Mid-year budget revision: Version control with comparison to original.
- Budget carry-forward: Unspent budget rolls to next year (if policy allows).
- Zero-based budgeting: Each year starts from zero, no carry-forward.

### 3.8 Financial Analysis

**Description**: Financial ratio analysis, trend analysis, forecasting, and benchmarking.

**User Stories**:
- As a Financial Analyst, I want to calculate key financial ratios automatically so that I can focus on interpretation.
- As a Finance Manager, I want trend analysis with forecasting so that I can plan for the future.
- As the CEO, I want a financial health dashboard so that I can make strategic decisions.

**Acceptance Criteria**:
1. Pre-built ratio templates: liquidity, profitability, leverage, efficiency.
2. Trend analysis with customizable periods.
3. Forecasting based on historical trends.
4. Benchmarking against industry standards.
5. Export analysis to Excel and PDF.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-070 | Period | Minimum 2 periods for comparison | "Need at least 2 periods for analysis" |
| VR-071 | Data | Sufficient data for ratio calculation | "Insufficient data for selected ratio" |
| VR-072 | Forecast | Historical data minimum 12 months | "Need 12+ months for forecasting" |

**Edge Cases**:
- Missing data in some periods: Interpolation or exclusion options.
- Structural changes (mergers, divestitures): Adjusted comparability.
- Hyperinflation adjustment: Restate financials for inflation.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +-------------------+       +------------------+
| ChartOfAccounts|       |   JournalEntry    |       |    GeneralLedger |
+----------------+       +-------------------+       +------------------+
| PK account_code|<----->| PK entry_id       |<----->| PK gl_id         |
| account_name   |       | FK account_code   |       | FK entry_id      |
| account_type   |       | entry_number      |       | FK account_code  |
| parent_code    |       | entry_date        |       | debit_amount     |
| account_level  |       | description       |       | credit_amount    |
| is_active      |       | status            |       | balance          |
| currency       |       | created_by        |       | FK period_id     |
+----------------+       | posted_by         |       +------------------+
                         | posted_date       |              |
                         +-------------------+              |
                                 |                          |
                    +---------------------------+           |
                    |     JournalEntryLine      |           |
                    +---------------------------+           |
                    | PK line_id              |           |
                    | FK entry_id             |-----------+
                    | FK account_code         |
                    | debit_amount            |
                    | credit_amount           |
                    | description             |
                    | cost_center             |
                    +---------------------------+

+----------------+       +-------------------+       +------------------+
|   CashBook     |       |   DepositBook     |       |  BankAccount     |
+----------------+       +-------------------+       +------------------+
| PK cash_id     |       | PK deposit_id     |       | PK bank_id       |
| voucher_number |       | deposit_number    |       | bank_name        |
| FK account_code|       | FK bank_id        |       | account_number   |
| transaction_dt |       | deposit_date      |       | currency         |
| description    |       | amount            |       | current_balance  |
| debit_amount   |       | interest_rate     |       | FK bank_code     |
| credit_amount  |       | maturity_date     |       | is_active        |
| balance        |       | status            |       +------------------+
| FK created_by  |       | FK created_by     |
+----------------+       +-------------------+

+------------------+       +-------------------+       +------------------+
|   Receivable     |       |     Payable       |       |    Invoice       |
+------------------+       +-------------------+       +------------------+
| PK receivable_id |       | PK payable_id     |       | PK invoice_id    |
| FK customer_id   |       | FK supplier_id    |       | invoice_number   |
| FK invoice_id    |       | FK invoice_id     |       | FK customer_id   |
| invoice_date     |       | invoice_date      |       | invoice_date     |
| due_date         |       | due_date          |       | FK supplier_id   |
| original_amount  |       | original_amount   |       | subtotal         |
| paid_amount      |       | paid_amount       |       | vat_amount       |
| outstanding      |       | outstanding       |       | grand_total      |
| status           |       | status            |       | status           |
| aging_bucket     |       | aging_bucket      |       | e_invoice_code   |
+------------------+       +-------------------+       | qr_code          |
                                                       +------------------+

+---------------------+       +--------------------+       +------------------+
| TaxDeclaration      |       |    FinancialReport |       |    Budget        |
+---------------------+       +--------------------+       +------------------+
| PK tax_id           |       | PK report_id       |       | PK budget_id     |
| tax_type            |       | report_type        |       | FK account_code  |
| tax_period          |       | period             |       | FK department_id |
| input_vat           |       | FK created_by      |       | budget_year      |
| output_vat          |       | report_data        |       | month            |
| net_vat             |       | status             |       | budget_amount    |
| submission_date     |       | generated_date     |       | actual_amount    |
| status              |       +--------------------+       | variance         |
+---------------------+                                    | status           |
                                                           +------------------+
```

### 4.2 Table Schemas

#### ChartOfAccounts

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| account_code | VARCHAR(10) | PK, NOT NULL | Unique account identifier (e.g., 111, 112, 131) |
| account_name | NVARCHAR(200) | NOT NULL | Vietnamese account name |
| account_type | VARCHAR(20) | NOT NULL, CHECK | Asset, Liability, Equity, Revenue, Expense |
| parent_code | VARCHAR(10) | FK REFERENCES ChartOfAccounts | Parent account for hierarchy |
| account_level | INT | NOT NULL | Hierarchy level (1-5) |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Whether account is active |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Default currency |
| description | NVARCHAR(500) | NULL | Account description |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### JournalEntry

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| entry_id | BIGINT | PK, SERIAL | Unique entry identifier |
| entry_number | VARCHAR(20) | NOT NULL, UNIQUE | Sequential entry number (e.g., JE-2026-0001) |
| entry_date | DATE | NOT NULL | Date of the transaction |
| description | NVARCHAR(500) | NOT NULL | Entry description |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, pending, posted, reversed |
| source_module | VARCHAR(30) | NOT NULL | Source: Sales, Purchasing, Manual, Payroll |
| source_reference | VARCHAR(50) | NULL | Reference to source document |
| total_debit | DECIMAL(18,2) | NOT NULL | Sum of all debit lines |
| total_credit | DECIMAL(18,2) | NOT NULL | Sum of all credit lines |
| created_by | BIGINT | FK REFERENCES User | User who created entry |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| posted_by | BIGINT | FK REFERENCES User | User who posted entry |
| posted_date | TIMESTAMP | NULL | Posting timestamp |
| fiscal_year | INT | NOT NULL | Fiscal year |
| fiscal_period | INT | NOT NULL | Fiscal period (month) |

#### JournalEntryLine

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| line_id | BIGINT | PK, SERIAL | Unique line identifier |
| entry_id | BIGINT | NOT NULL, FK REFERENCES JournalEntry | Parent journal entry |
| account_code | VARCHAR(10) | NOT NULL, FK REFERENCES ChartOfAccounts | Account for this line |
| debit_amount | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Debit amount |
| credit_amount | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Credit amount |
| description | NVARCHAR(300) | NULL | Line description |
| cost_center | VARCHAR(20) | NULL | Cost center code |
| department_id | BIGINT | FK REFERENCES Department | Department |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Line currency |
| exchange_rate | DECIMAL(12,6) | NOT NULL, DEFAULT 1 | Exchange rate to base currency |

#### GeneralLedger

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| gl_id | BIGINT | PK, SERIAL | Unique GL identifier |
| entry_id | BIGINT | NOT NULL, FK REFERENCES JournalEntry | Source journal entry |
| account_code | VARCHAR(10) | NOT NULL, FK REFERENCES ChartOfAccounts | Account code |
| period_id | INT | NOT NULL | Fiscal period ID |
| debit_amount | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Debit for this period |
| credit_amount | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Credit for this period |
| balance | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Running balance |
| transaction_date | DATE | NOT NULL | Transaction date |

#### CashBook

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| cash_id | BIGINT | PK, SERIAL | Unique cash transaction ID |
| voucher_number | VARCHAR(20) | NOT NULL, UNIQUE | Cash voucher number |
| transaction_date | DATE | NOT NULL | Transaction date |
| FK account_code | VARCHAR(10) | NOT NULL, FK REFERENCES ChartOfAccounts | Cash account |
| description | NVARCHAR(300) | NOT NULL | Transaction description |
| debit_amount | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Cash received |
| credit_amount | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Cash paid |
| balance | DECIMAL(18,2) | NOT NULL | Running balance |
| reference | VARCHAR(100) | NULL | Reference document |
| FK customer_id | BIGINT | NULL, FK REFERENCES Customer | Related customer |
| FK supplier_id | BIGINT | NULL, FK REFERENCES Supplier | Related supplier |
| created_by | BIGINT | FK REFERENCES User | User who recorded |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, posted, voided |

#### DepositBook

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| deposit_id | BIGINT | PK, SERIAL | Unique deposit ID |
| deposit_number | VARCHAR(20) | NOT NULL, UNIQUE | Deposit number |
| FK bank_id | BIGINT | NOT NULL, FK REFERENCES BankAccount | Bank account |
| deposit_date | DATE | NOT NULL | Deposit date |
| amount | DECIMAL(18,2) | NOT NULL | Deposit amount |
| interest_rate | DECIMAL(6,4) | NOT NULL | Annual interest rate |
| maturity_date | DATE | NOT NULL | Maturity date |
| interest_earned | DECIMAL(18,2) | DEFAULT 0 | Accrued interest |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active, matured, withdrawn |
| created_by | BIGINT | FK REFERENCES User | User who recorded |

#### Receivable

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| receivable_id | BIGINT | PK, SERIAL | Unique receivable ID |
| FK customer_id | BIGINT | NOT NULL, FK REFERENCES Customer | Customer |
| FK invoice_id | BIGINT | FK REFERENCES Invoice | Source invoice |
| invoice_date | DATE | NOT NULL | Invoice date |
| due_date | DATE | NOT NULL | Payment due date |
| original_amount | DECIMAL(18,2) | NOT NULL | Original invoice amount |
| paid_amount | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Amount paid to date |
| outstanding | DECIMAL(18,2) | NOT NULL | Remaining balance |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'unpaid' | unpaid, partial, paid, overdue, written_off |
| aging_bucket | VARCHAR(20) | CALCULATED | current, 1-30, 31-60, 61-90, 90+ |

#### Payable

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| payable_id | BIGINT | PK, SERIAL | Unique payable ID |
| FK supplier_id | BIGINT | NOT NULL, FK REFERENCES Supplier | Supplier |
| FK invoice_id | BIGINT | FK REFERENCES Invoice | Source invoice |
| invoice_date | DATE | NOT NULL | Invoice date |
| due_date | DATE | NOT NULL | Payment due date |
| original_amount | DECIMAL(18,2) | NOT NULL | Original invoice amount |
| paid_amount | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Amount paid to date |
| outstanding | DECIMAL(18,2) | NOT NULL | Remaining balance |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'unpaid' | unpaid, partial, paid, overdue |

#### Invoice

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| invoice_id | BIGINT | PK, SERIAL | Unique invoice ID |
| invoice_number | VARCHAR(30) | NOT NULL, UNIQUE | Sequential invoice number |
| invoice_date | DATE | NOT NULL | Invoice date |
| FK customer_id | BIGINT | NULL, FK REFERENCES Customer | Customer (for sales) |
| FK supplier_id | BIGINT | NULL, FK REFERENCES Supplier | Supplier (for purchases) |
| invoice_type | VARCHAR(20) | NOT NULL | sales, purchase, credit_note, debit_note |
| subtotal | DECIMAL(18,2) | NOT NULL | Before tax |
| vat_rate | DECIMAL(5,2) | NOT NULL | VAT percentage |
| vat_amount | DECIMAL(18,2) | NOT NULL | VAT amount |
| grand_total | DECIMAL(18,2) | NOT NULL | Total including VAT |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, issued, paid, cancelled |
| e_invoice_code | VARCHAR(50) | NULL | E-invoice code from tax authority |
| qr_code | VARCHAR(200) | NULL | QR code data string |
| payment_terms | VARCHAR(100) | NULL | Payment terms description |
| notes | TEXT | NULL | Additional notes |

#### TaxDeclaration

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| tax_id | BIGINT | PK, SERIAL | Unique tax declaration ID |
| tax_type | VARCHAR(20) | NOT NULL | VAT, CIT, PIT |
| tax_period | VARCHAR(10) | NOT NULL | Period (e.g., 2026-Q1) |
| input_vat | DECIMAL(18,2) | DEFAULT 0 | Input VAT amount |
| output_vat | DECIMAL(18,2) | DEFAULT 0 | Output VAT amount |
| net_vat | DECIMAL(18,2) | DEFAULT 0 | Net VAT payable/receivable |
| submission_date | DATE | NULL | Date submitted |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, submitted, accepted, amended |
| xml_data | TEXT | NULL | XML declaration data |

#### FinancialReport

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| report_id | BIGINT | PK, SERIAL | Unique report ID |
| report_type | VARCHAR(30) | NOT NULL | balance_sheet, income_statement, cash_flow |
| period | VARCHAR(20) | NOT NULL | Reporting period |
| report_data | JSONB | NOT NULL | Report content as JSON |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, generated, approved |
| FK created_by | BIGINT | FK REFERENCES User | User who generated |
| generated_date | TIMESTAMP | NOT NULL, DEFAULT NOW() | Generation timestamp |

#### Budget

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| budget_id | BIGINT | PK, SERIAL | Unique budget ID |
| FK account_code | VARCHAR(10) | NOT NULL, FK REFERENCES ChartOfAccounts | Account |
| FK department_id | BIGINT | FK REFERENCES Department | Department |
| budget_year | INT | NOT NULL | Budget year |
| month | INT | NOT NULL, CHECK 1-12 | Budget month |
| budget_amount | DECIMAL(18,2) | NOT NULL | Budgeted amount |
| actual_amount | DECIMAL(18,2) | DEFAULT 0 | Actual spending |
| variance | DECIMAL(18,2) | CALCULATED | Variance amount |
| variance_pct | DECIMAL(5,2) | CALCULATED | Variance percentage |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, pending, approved, locked |

#### BankAccount

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| bank_id | BIGINT | PK, SERIAL | Unique bank account ID |
| bank_name | NVARCHAR(100) | NOT NULL | Bank name |
| account_number | VARCHAR(30) | NOT NULL, UNIQUE | Bank account number |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Account currency |
| current_balance | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Current balance |
| FK bank_code | VARCHAR(10) | NULL | Bank routing code |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

### 4.3 Indexes

```sql
-- ChartOfAccounts indexes
CREATE INDEX idx_coa_parent ON ChartOfAccounts(parent_code);
CREATE INDEX idx_coa_type ON ChartOfAccounts(account_type);
CREATE INDEX idx_coa_active ON ChartOfAccounts(is_active) WHERE is_active = TRUE;

-- JournalEntry indexes
CREATE INDEX idx_je_number ON JournalEntry(entry_number);
CREATE INDEX idx_je_date ON JournalEntry(entry_date);
CREATE INDEX idx_je_status ON JournalEntry(status);
CREATE INDEX idx_je_fiscal ON JournalEntry(fiscal_year, fiscal_period);
CREATE INDEX idx_je_source ON JournalEntry(source_module, source_reference);

-- JournalEntryLine indexes
CREATE INDEX idx_je_line_entry ON JournalEntryLine(entry_id);
CREATE INDEX idx_je_line_account ON JournalEntryLine(account_code);
CREATE INDEX idx_je_line_cost_center ON JournalEntryLine(cost_center);

-- GeneralLedger indexes
CREATE INDEX idx_gl_account_period ON GeneralLedger(account_code, period_id);
CREATE INDEX idx_gl_entry ON GeneralLedger(entry_id);
CREATE INDEX idx_gl_date ON GeneralLedger(transaction_date);

-- CashBook indexes
CREATE INDEX idx_cb_voucher ON CashBook(voucher_number);
CREATE INDEX idx_cb_date ON CashBook(transaction_date);
CREATE INDEX idx_cb_account ON CashBook(account_code);
CREATE INDEX idx_cb_status ON CashBook(status);

-- Receivable indexes
CREATE INDEX idx_ar_customer ON Receivable(customer_id);
CREATE INDEX idx_ar_status ON Receivable(status);
CREATE INDEX idx_ar_due_date ON Receivable(due_date);
CREATE INDEX idx_ar_aging ON Receivable(aging_bucket);

-- Payable indexes
CREATE INDEX idx_ap_supplier ON Payable(supplier_id);
CREATE INDEX idx_ap_status ON Payable(status);
CREATE INDEX idx_ap_due_date ON Payable(due_date);

-- Invoice indexes
CREATE INDEX idx_inv_number ON Invoice(invoice_number);
CREATE INDEX idx_inv_date ON Invoice(invoice_date);
CREATE INDEX idx_inv_customer ON Invoice(customer_id);
CREATE INDEX idx_inv_supplier ON Invoice(supplier_id);
CREATE INDEX idx_inv_type ON Invoice(invoice_type);
CREATE INDEX idx_inv_status ON Invoice(status);

-- Budget indexes
CREATE INDEX idx_budget_account_year ON Budget(account_code, budget_year);
CREATE INDEX idx_budget_department ON Budget(department_id);
CREATE INDEX idx_budget_status ON Budget(status);

-- TaxDeclaration indexes
CREATE INDEX idx_tax_type_period ON TaxDeclaration(tax_type, tax_period);
CREATE INDEX idx_tax_status ON TaxDeclaration(status);
```

### 4.4 Summary of Tables

| Table | Primary Key | Foreign Keys | Purpose | Estimated Rows |
|-------|-------------|--------------|---------|----------------|
| ChartOfAccounts | account_code | parent_code (self) | Account structure definition | 500 |
| JournalEntry | entry_id | User (created_by, posted_by) | Journal entry header | 50,000/year |
| JournalEntryLine | line_id | JournalEntry, ChartOfAccounts, Department | Journal entry detail lines | 200,000/year |
| GeneralLedger | gl_id | JournalEntry, ChartOfAccounts | Aggregated ledger balances | 500,000/year |
| CashBook | cash_id | ChartOfAccounts, User | Cash transactions | 20,000/year |
| DepositBook | deposit_id | BankAccount, User | Bank deposit records | 2,000/year |
| Receivable | receivable_id | Customer, Invoice | Customer outstanding balances | 10,000/year |
| Payable | payable_id | Supplier, Invoice | Supplier outstanding balances | 8,000/year |
| Invoice | invoice_id | Customer, Supplier | All invoices (sales/purchase) | 30,000/year |
| TaxDeclaration | tax_id | - | Tax declarations | 48/year |
| FinancialReport | report_id | User | Financial statement reports | 200/year |
| Budget | budget_id | ChartOfAccounts, Department | Budget planning and tracking | 6,000/year |
| BankAccount | bank_id | - | Bank account master data | 50 |

---

## 5. API Specifications

### 5.1 Journal Entry API

#### POST /api/v1/accounting/journal-entries

Create a new journal entry.

**Request Body**:
```json
{
  "entry_date": "2026-04-14",
  "description": "Monthly depreciation posting",
  "source_module": "Manual",
  "source_reference": null,
  "fiscal_year": 2026,
  "fiscal_period": 4,
  "lines": [
    {
      "account_code": "6424",
      "debit_amount": 5000000,
      "credit_amount": 0,
      "description": "Depreciation expense - Buildings",
      "cost_center": "CC001",
      "department_id": 10
    },
    {
      "account_code": "2141",
      "debit_amount": 0,
      "credit_amount": 5000000,
      "description": "Accumulated depreciation - Buildings",
      "cost_center": null,
      "department_id": null
    }
  ]
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "entry_id": 10542,
    "entry_number": "JE-2026-00421",
    "entry_date": "2026-04-14",
    "description": "Monthly depreciation posting",
    "status": "draft",
    "source_module": "Manual",
    "total_debit": 5000000,
    "total_credit": 5000000,
    "created_by": 1001,
    "created_at": "2026-04-14T10:30:00Z",
    "lines": [
      {
        "line_id": 21084,
        "account_code": "6424",
        "debit_amount": 5000000,
        "credit_amount": 0,
        "description": "Depreciation expense - Buildings"
      },
      {
        "line_id": 21085,
        "account_code": "2141",
        "debit_amount": 0,
        "credit_amount": 5000000,
        "description": "Accumulated depreciation - Buildings"
      }
    ]
  }
}
```

#### GET /api/v1/accounting/journal-entries

List journal entries with filtering.

**Query Parameters**:
- `fiscal_year` (int): Filter by fiscal year
- `fiscal_period` (int): Filter by fiscal period
- `status` (string): Filter by status (draft, posted, reversed)
- `source_module` (string): Filter by source module
- `date_from` (date): Start date filter
- `date_to` (date): End date filter
- `page` (int): Page number (default 1)
- `per_page` (int): Items per page (default 20, max 100)

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "entries": [
      {
        "entry_id": 10542,
        "entry_number": "JE-2026-00421",
        "entry_date": "2026-04-14",
        "description": "Monthly depreciation posting",
        "status": "draft",
        "source_module": "Manual",
        "total_debit": 5000000,
        "total_credit": 5000000,
        "created_at": "2026-04-14T10:30:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 20,
      "total_pages": 250,
      "total_count": 5000
    }
  }
}
```

#### GET /api/v1/accounting/journal-entries/{entry_id}

Get a specific journal entry with lines.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "entry_id": 10542,
    "entry_number": "JE-2026-00421",
    "entry_date": "2026-04-14",
    "description": "Monthly depreciation posting",
    "status": "draft",
    "source_module": "Manual",
    "total_debit": 5000000,
    "total_credit": 5000000,
    "created_by": 1001,
    "created_at": "2026-04-14T10:30:00Z",
    "posted_by": null,
    "posted_date": null,
    "lines": [
      {
        "line_id": 21084,
        "account_code": "6424",
        "account_name": "Chi phí khấu hao TSCĐ",
        "debit_amount": 5000000,
        "credit_amount": 0,
        "description": "Depreciation expense - Buildings",
        "cost_center": "CC001"
      },
      {
        "line_id": 21085,
        "account_code": "2141",
        "account_name": "Hao mòn TSCĐ hữu hình",
        "debit_amount": 0,
        "credit_amount": 5000000,
        "description": "Accumulated depreciation - Buildings",
        "cost_center": null
      }
    ]
  }
}
```

#### PUT /api/v1/accounting/journal-entries/{entry_id}

Update a draft journal entry.

**Request Body**:
```json
{
  "description": "Monthly depreciation posting - revised",
  "lines": [
    {
      "line_id": 21084,
      "account_code": "6424",
      "debit_amount": 5500000,
      "credit_amount": 0,
      "description": "Depreciation expense - Buildings (adjusted)"
    },
    {
      "line_id": 21085,
      "account_code": "2141",
      "debit_amount": 0,
      "credit_amount": 5500000,
      "description": "Accumulated depreciation - Buildings (adjusted)"
    }
  ]
}
```

**Response** (200 OK): Same as GET single entry response.

#### POST /api/v1/accounting/journal-entries/{entry_id}/post

Post a journal entry (requires approval).

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "entry_id": 10542,
    "entry_number": "JE-2026-00421",
    "status": "posted",
    "posted_by": 1005,
    "posted_date": "2026-04-14T14:00:00Z"
  }
}
```

#### DELETE /api/v1/accounting/journal-entries/{entry_id}

Delete a draft journal entry (cannot delete posted entries).

**Response** (200 OK):
```json
{
  "success": true,
  "message": "Journal entry JE-2026-00421 has been deleted"
}
```

### 5.2 Cash Book API

#### POST /api/v1/accounting/cash-book

Record a cash transaction.

**Request Body**:
```json
{
  "transaction_date": "2026-04-14",
  "account_code": "1111",
  "description": "Payment received from customer Nguyen Van A",
  "debit_amount": 15000000,
  "credit_amount": 0,
  "reference": "HD-2026-00125",
  "customer_id": 5021
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "cash_id": 8542,
    "voucher_number": "PC-2026-00342",
    "transaction_date": "2026-04-14",
    "account_code": "1111",
    "description": "Payment received from customer Nguyen Van A",
    "debit_amount": 15000000,
    "credit_amount": 0,
    "balance": 7515000000,
    "reference": "HD-2026-00125",
    "customer_id": 5021,
    "status": "draft"
  }
}
```

#### GET /api/v1/accounting/cash-book

List cash transactions.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "transactions": [
      {
        "cash_id": 8542,
        "voucher_number": "PC-2026-00342",
        "transaction_date": "2026-04-14",
        "description": "Payment received from customer Nguyen Van A",
        "debit_amount": 15000000,
        "credit_amount": 0,
        "balance": 7515000000,
        "status": "draft"
      }
    ],
    "summary": {
      "opening_balance": 7500000000,
      "total_receipts": 45000000,
      "total_payments": 30000000,
      "closing_balance": 7515000000
    },
    "pagination": {
      "page": 1,
      "per_page": 20,
      "total_count": 842
    }
  }
}
```

### 5.3 Receivable/Payable API

#### GET /api/v1/accounting/receivables

List receivables with aging.

**Query Parameters**:
- `customer_id` (int): Filter by customer
- `status` (string): Filter by status
- `aging_bucket` (string): Filter by aging bucket
- `date_from` (date): Invoice date from
- `date_to` (date): Invoice date to

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "receivables": [
      {
        "receivable_id": 3001,
        "customer_id": 5021,
        "customer_name": "Nguyen Van A",
        "invoice_number": "HD-2026-00125",
        "invoice_date": "2026-03-15",
        "due_date": "2026-04-15",
        "original_amount": 22000000,
        "paid_amount": 7000000,
        "outstanding": 15000000,
        "status": "partial",
        "aging_bucket": "current"
      }
    ],
    "aging_summary": {
      "current": 3500000000,
      "1-30": 1200000000,
      "31-60": 280000000,
      "61-90": 82000000,
      "90+": 20000000,
      "total": 5082000000
    }
  }
}
```

#### POST /api/v1/accounting/receivables/{receivable_id}/payment

Apply payment to a receivable.

**Request Body**:
```json
{
  "payment_amount": 15000000,
  "payment_date": "2026-04-14",
  "payment_method": "cash",
  "reference": "PC-2026-00342",
  "notes": "Full payment received"
}
```

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "receivable_id": 3001,
    "previous_outstanding": 15000000,
    "payment_amount": 15000000,
    "new_outstanding": 0,
    "status": "paid",
    "cash_book_entry_id": 8542
  }
}
```

### 5.4 Budget API

#### GET /api/v1/accounting/budgets/{budget_year}/variance

Get budget variance report.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "budget_year": 2026,
    "as_of_date": "2026-04-14",
    "variances": [
      {
        "account_code": "6421",
        "account_name": "Chi phí nhân viên",
        "department_id": 10,
        "department_name": "Phòng kế toán",
        "annual_budget": 2400000000,
        "ytd_budget": 800000000,
        "ytd_actual": 720000000,
        "variance_amount": 80000000,
        "variance_pct": 10.0,
        "status": "within_budget"
      }
    ],
    "summary": {
      "total_budget": 52000000000,
      "total_actual": 51212000000,
      "total_variance": 788000000,
      "total_variance_pct": 1.5
    }
  }
}
```

### 5.5 Financial Report API

#### GET /api/v1/accounting/reports/{report_type}

Generate financial report.

**Path Parameters**:
- `report_type`: balance_sheet, income_statement, cash_flow

**Query Parameters**:
- `period` (string): Report period (e.g., 2026-Q1)
- `comparison_period` (string): Optional comparison period
- `department_id` (int): Optional department filter

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "report_type": "income_statement",
    "period": "2026-Q1",
    "comparison_period": "2025-Q1",
    "generated_at": "2026-04-14T10:30:00Z",
    "sections": [
      {
        "section": "Revenue",
        "lines": [
          {
            "account_code": "5111",
            "account_name": "Doanh thu bán hàng",
            "current_amount": 65338000000,
            "comparison_amount": 56816000000,
            "variance_pct": 15.0
          }
        ],
        "section_total": {
          "current": 65338000000,
          "comparison": 56816000000,
          "variance_pct": 15.0
        }
      },
      {
        "section": "Expenses",
        "lines": [
          {
            "account_code": "6421",
            "account_name": "Chi phí nhân viên",
            "current_amount": 25000000000,
            "comparison_amount": 24500000000,
            "variance_pct": 2.0
          },
          {
            "account_code": "6422",
            "account_name": "Chi phí quản lý",
            "current_amount": 26212000000,
            "comparison_amount": 29980000000,
            "variance_pct": -12.6
          }
        ],
        "section_total": {
          "current": 51212000000,
          "comparison": 54480000000,
          "variance_pct": -6.0
        }
      },
      {
        "section": "Profit",
        "section_total": {
          "current": 5126000000,
          "comparison": 4661000000,
          "variance_pct": 10.0
        }
      }
    ]
  }
}
```

### 5.6 Tax Declaration API

#### GET /api/v1/accounting/tax-declarations/{tax_type}/{period}

Get tax declaration data.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "tax_id": 1205,
    "tax_type": "VAT",
    "tax_period": "2026-Q1",
    "input_vat": 5120000000,
    "output_vat": 6534000000,
    "net_vat": 1414000000,
    "status": "draft",
    "line_items": [
      {
        "line_type": "output",
        "invoice_count": 1250,
        "total_base": 65338000000,
        "vat_amount": 6534000000,
        "vat_rate": 10
      },
      {
        "line_type": "input",
        "invoice_count": 890,
        "total_base": 51200000000,
        "vat_amount": 5120000000,
        "vat_rate": 10
      }
    ]
  }
}
```

### 5.7 Error Response Format

All API errors follow this format:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Journal entry is not balanced",
    "details": [
      {
        "field": "lines",
        "message": "Total debits (5,000,000) do not equal total credits (4,500,000)"
      }
    ],
    "request_id": "req-abc123"
  }
}
```

Common error codes:
- `VALIDATION_ERROR`: Input validation failed (400)
- `NOT_FOUND`: Resource not found (404)
- `UNAUTHORIZED`: Authentication required (401)
- `FORBIDDEN`: Insufficient permissions (403)
- `PERIOD_CLOSED`: Cannot modify closed period (409)
- `ENTRY_POSTED`: Cannot modify posted entry (409)
- `DUPLICATE_ENTRY`: Duplicate entry detected (409)
- `INTERNAL_ERROR`: Server error (500)

### 5.8 API Summary Table

| Endpoint | Method | Auth Required | Rate Limit | Description |
|----------|--------|---------------|------------|-------------|
| /api/v1/accounting/journal-entries | POST | Yes | 100/min | Create journal entry |
| /api/v1/accounting/journal-entries | GET | Yes | 200/min | List journal entries |
| /api/v1/accounting/journal-entries/{id} | GET | Yes | 200/min | Get journal entry |
| /api/v1/accounting/journal-entries/{id} | PUT | Yes | 100/min | Update journal entry |
| /api/v1/accounting/journal-entries/{id}/post | POST | Yes | 50/min | Post journal entry |
| /api/v1/accounting/journal-entries/{id} | DELETE | Yes | 50/min | Delete journal entry |
| /api/v1/accounting/cash-book | POST | Yes | 100/min | Record cash transaction |
| /api/v1/accounting/cash-book | GET | Yes | 200/min | List cash transactions |
| /api/v1/accounting/receivables | GET | Yes | 200/min | List receivables |
| /api/v1/accounting/receivables/{id}/payment | POST | Yes | 100/min | Apply payment |
| /api/v1/accounting/payables | GET | Yes | 200/min | List payables |
| /api/v1/accounting/payables/{id}/payment | POST | Yes | 100/min | Record payment |
| /api/v1/accounting/invoices | POST | Yes | 100/min | Create invoice |
| /api/v1/accounting/invoices | GET | Yes | 200/min | List invoices |
| /api/v1/accounting/tax-declarations/{type}/{period} | GET | Yes | 50/min | Get tax declaration |
| /api/v1/accounting/tax-declarations | POST | Yes | 10/min | Submit tax declaration |
| /api/v1/accounting/budgets/{year}/variance | GET | Yes | 50/min | Budget variance report |
| /api/v1/accounting/reports/{type} | GET | Yes | 30/min | Generate financial report |
| /api/v1/accounting/summary | GET | Yes | 100/min | Dashboard summary |
| /api/v1/accounting/chart-of-accounts | GET | Yes | 200/min | List chart of accounts |

---

## 6. Permissions Matrix

### 6.1 Role Definitions

| Role | Code | Description | Department |
|------|------|-------------|------------|
| Chief Accountant | CHIEF_ACCT | Full accounting module access | Finance |
| Cashier | CASHIER | Cash and deposit operations | Finance |
| AP Clerk | AP_CLERK | Payable management | Finance |
| AR Clerk | AR_CLERK | Receivable management | Finance |
| Tax Accountant | TAX_ACCT | Tax declarations and e-invoices | Finance |
| Cost Accountant | COST_ACCT | Costing and inventory valuation | Finance |
| Financial Analyst | FIN_ANALYST | Read-only analysis and reporting | Finance |
| Finance Manager | FIN_MGR | Budget approval, policy setting | Finance |

### 6.2 CRUD Permissions

| Resource | CHIEF_ACCT | CASHIER | AP_CLERK | AR_CLERK | TAX_ACCT | COST_ACCT | FIN_ANALYST | FIN_MGR |
|----------|------------|---------|----------|----------|----------|-----------|-------------|---------|
| JournalEntry | CRUD | R | R | R | R | R | R | CRUD |
| JournalEntryLine | CRUD | R | R | R | R | R | R | CRUD |
| CashBook | CRUD | CRUD | R | R | R | R | R | CRUD |
| DepositBook | CRUD | CRUD | - | - | R | R | R | CRUD |
| Receivable | CRUD | R | R | CRUD | R | R | R | CRUD |
| Payable | CRUD | R | CRUD | R | R | R | R | CRUD |
| Invoice | CRUD | R | R | CRUD | CRUD | R | R | CRUD |
| TaxDeclaration | CRUD | - | - | - | CRUD | R | R | CRUD |
| FinancialReport | CRUD | - | - | - | R | R | R | CRUD |
| Budget | CRUD | - | R | R | R | R | R | CRUD |
| ChartOfAccounts | CRUD | R | R | R | R | R | R | CRUD |
| GeneralLedger | CRUD | R | R | R | R | R | R | CRUD |
| BankAccount | CRUD | CRUD | - | - | - | - | R | CRUD |
| FinancialAnalysis | R | R | R | R | R | R | CRUD | CRUD |

Legend: C=Create, R=Read, U=Update, D=Delete, -=No access

### 6.3 Row-Level Security

| Resource | Rule | Description |
|----------|------|-------------|
| CashBook | Cashier sees only their assigned cash accounts | `account_code IN (user.cash_accounts)` |
| Receivable | AR Clerk sees only assigned customers | `customer_id IN (user.assigned_customers) OR user.role = 'CHIEF_ACCT'` |
| Payable | AP Clerk sees only assigned suppliers | `supplier_id IN (user.assigned_suppliers) OR user.role = 'CHIEF_ACCT'` |
| Budget | Department managers see only their department budgets | `department_id = user.department_id OR user.role IN ('CHIEF_ACCT', 'FIN_MGR')` |
| JournalEntry | All users see all entries (full visibility required for audit) | No restriction |
| Invoice | Based on type: sales invoices visible to AR, purchase to AP | `invoice_type = 'sales' OR user.role IN ('AP_CLERK', 'CHIEF_ACCT')` |

### 6.4 Approval Workflows

| Action | Required Approver | Threshold | Escalation |
|--------|-------------------|-----------|------------|
| Post Journal Entry | Chief Accountant | All entries | Finance Manager if > 100M |
| Cash Payment | Chief Accountant | > 10M | Finance Manager if > 100M |
| Budget Approval | Finance Manager | All budgets | Director if > 1B |
| Bad Debt Write-off | Finance Manager | > 5M | Director if > 50M |
| Tax Submission | Chief Accountant | All declarations | - |
| Budget Transfer | Finance Manager | > 10% of line item | Director if > 500M |

---

## 7. State Machine

### 7.1 Journal Entry State Diagram

```
                    +----------+
                    |  DRAFT   |
                    +----+-----+
                         |
              +----------+----------+
              |                     |
              v                     v
        +-----------+        +------------+
        | PENDING   |        | DELETED    |
        | (Review)  |        +------------+
        +-----+-----+
              |
       +------+------+
       |             |
       v             v
+------------+  +-----------+
|  POSTED    |  | REJECTED  |
+------+-----+  +-----------+
       |
       v
+------------+
| REVERSED   |
+------------+
```

### 7.2 Invoice State Diagram

```
                    +----------+
                    |  DRAFT   |
                    +----+-----+
                         |
                         v
                    +----------+
              +---->|  ISSUED  |<----+
              |     +-----+----+     |
              |           |          |
              |     +-----+-----+    |
              |     |           |    |
              v     v           |    |
        +----------+     +-----------+
        |   PAID   |     | CANCELLED |
        +----------+     +-----------+
              |
              v
        +----------+
        | CLOSED   |
        +----------+
```

### 7.3 State Transition Tables

#### Journal Entry Transitions

| From State | To State | Trigger | Required Role | Auto Conditions |
|-----------|----------|---------|---------------|-----------------|
| DRAFT | PENDING | Submit for review | Any accountant | Balances must match |
| DRAFT | DELETED | Delete | Creator only | Must be draft |
| PENDING | POSTED | Approve and post | Chief Accountant | Balances must match |
| PENDING | REJECTED | Reject | Chief Accountant | Reason required |
| REJECTED | DRAFT | Resubmit | Creator | Fixes applied |
| POSTED | REVERSED | Create reversal | Chief Accountant | Reversal entry created |

#### Invoice Transitions

| From State | To State | Trigger | Required Role | Auto Conditions |
|-----------|----------|---------|---------------|-----------------|
| DRAFT | ISSUED | Issue invoice | AR Clerk / Tax Accountant | All fields valid |
| ISSUED | PAID | Record payment | AR Clerk | Payment = outstanding |
| ISSUED | ISSUED | Partial payment | AR Clerk | Payment < outstanding |
| ISSUED | CANCELLED | Cancel | Chief Accountant | Reason required |
| PAID | CLOSED | Close period | System | Fiscal period close |
| CANCELLED | DRAFT | Restore | Chief Accountant | Reason required |

#### Receivable Transitions

| From State | To State | Trigger | Required Role | Auto Conditions |
|-----------|----------|---------|---------------|-----------------|
| UNPAID | PARTIAL | Partial payment | AR Clerk | Payment < outstanding |
| UNPAID | PAID | Full payment | AR Clerk | Payment = outstanding |
| UNPAID | OVERDUE | Past due date | System | due_date < today |
| PARTIAL | PAID | Final payment | AR Clerk | Payment = outstanding |
| PARTIAL | OVERDUE | Past due date | System | due_date < today |
| OVERDUE | WRITTEN_OFF | Write-off approval | Finance Manager | Approval workflow |
| ANY | CURRENT | Payment received | AR Clerk | Brings current |

#### Budget Transitions

| From State | To State | Trigger | Required Role | Auto Conditions |
|-----------|----------|---------|---------------|-----------------|
| DRAFT | PENDING | Submit for approval | Finance Manager | All lines filled |
| PENDING | APPROVED | Approve | Director | Review complete |
| PENDING | DRAFT | Reject | Director | Changes needed |
| APPROVED | LOCKED | Lock budget | Director | Final approval |
| LOCKED | LOCKED | Auto-update actuals | System | GL posting |

### 7.4 Approval Workflow

#### Journal Entry Approval Flow

```
Creator submits JE -> Pending Review
                      |
                      v
              Chief Accountant Review
                 /           \
          Approve             Reject
             |                  |
             v                  v
         Posted            Returned to Draft
             |                  |
             v                  v
         GL Updated         Maker makes fixes
                                   |
                                   v
                             Resubmit -> Loop
```

#### Invoice Approval Flow (for high-value invoices > 100M)

```
AR Clerk creates invoice -> Draft
     |
     v
Auto-validate (amounts, tax, customer)
     |
     v
Chief Accountant Review
     /           \
  Approve        Reject
     |             |
     v             v
Issued +        Returned
AR Created        |
     |             v
     v          AR Clerk revises
Payment            |
Tracking           v
     |         Resubmit
     v
Paid -> Closed
```

---

## 8. Reports

### 8.1 Report Inventory

| Report | Description | Dimensions | Metrics | Filters | Chart Type |
|--------|-------------|------------|---------|---------|------------|
| Balance Sheet | Financial position at point in time | Account hierarchy | Assets, Liabilities, Equity | Date, department | Horizontal bar |
| Income Statement | Revenue and expenses over period | Account hierarchy | Revenue, Expenses, Profit | Period, department | Column chart |
| Cash Flow Statement | Cash inflows and outflows | Operating/Investing/Financing | Cash in, Cash out, Net | Period | Waterfall chart |
| AR Aging | Outstanding receivables by age | Customer, aging bucket | Outstanding amount, count | Date, customer | Stacked bar |
| AP Aging | Outstanding payables by age | Supplier, aging bucket | Outstanding amount, count | Date, supplier | Stacked bar |
| Trial Balance | All account balances for period | Account code | Debit total, Credit total | Period | Table |
| Budget Variance | Budget vs actual comparison | Account, department, month | Budget, Actual, Variance, % | Year, department | Bar + line |
| Revenue Trend | Revenue over time with trends | Month, quarter, year | Revenue amount, growth % | Period range | Line chart |
| Expense Analysis | Expense breakdown | Category, department | Expense amount, % of total | Period, category | Pie chart |
| Tax Summary | Tax obligations summary | Tax type, period | Input VAT, Output VAT, Net | Period | Table |
| Cash Position | Current cash status | Bank account, cash account | Balance, daily change | Date | Gauge + sparkline |
| Profitability | Profit margins by segment | Product, customer, region | Revenue, COGS, Gross margin, Net profit | Period, segment | Multi-line |

### 8.2 Report Specifications

#### Balance Sheet Report

- **Dimensions**: Account hierarchy (Level 1-5), department
- **Metrics**: Total assets, total liabilities, total equity, working capital
- **Filters**: As-of date, department, comparison date
- **Chart Type**: Horizontal bar chart comparing current vs prior period
- **Export Formats**: PDF, Excel, XML

#### AR Aging Report

- **Dimensions**: Customer, aging bucket (current, 1-30, 31-60, 61-90, 90+)
- **Metrics**: Outstanding amount, number of invoices, average days outstanding
- **Filters**: Customer group, date range, status
- **Chart Type**: Stacked bar chart showing aging distribution
- **Export Formats**: PDF, Excel, CSV

#### Budget Variance Report

- **Dimensions**: Account code, department, month
- **Metrics**: Budget amount, actual amount, variance amount, variance percentage
- **Filters**: Budget year, department, account category
- **Chart Type**: Bar chart (budget vs actual) with line (variance %)
- **Export Formats**: PDF, Excel

#### Revenue Trend Report

- **Dimensions**: Month, quarter, year, product category
- **Metrics**: Revenue amount, growth rate (YoY, MoM), cumulative revenue
- **Filters**: Date range, product category, customer group
- **Chart Type**: Line chart with trend line and YoY comparison
- **Export Formats**: PDF, Excel, PowerPoint

### 8.3 Report Summary Table

| Report ID | Report Name | Schedule | Auto-Generate | Distribution | Retention |
|-----------|-------------|----------|---------------|--------------|-----------|
| RPT-001 | Balance Sheet | Monthly | Yes | Finance Manager, Director | 10 years |
| RPT-002 | Income Statement | Monthly | Yes | Finance Manager, Director | 10 years |
| RPT-003 | Cash Flow Statement | Monthly | Yes | Finance Manager | 10 years |
| RPT-004 | AR Aging | Weekly | Yes | AR Clerk, Chief Accountant | 5 years |
| RPT-005 | AP Aging | Weekly | Yes | AP Clerk, Chief Accountant | 5 years |
| RPT-006 | Trial Balance | Monthly | Yes | Chief Accountant | 10 years |
| RPT-007 | Budget Variance | Monthly | Yes | Finance Manager, Dept Managers | 5 years |
| RPT-008 | Revenue Trend | Monthly | Yes | Finance Manager, Director | 5 years |
| RPT-009 | Expense Analysis | Monthly | Yes | Chief Accountant, Dept Managers | 5 years |
| RPT-010 | Tax Summary | Monthly/Quarterly | Yes | Tax Accountant | 10 years |
| RPT-011 | Cash Position | Daily | Yes | Cashier, Finance Manager | 1 year |
| RPT-012 | Profitability | Quarterly | Yes | Director, Finance Manager | 5 years |

---

## 9. Integration Points

### 9.1 External API Integrations

| Integration | Provider | Protocol | Direction | Frequency | Data Exchanged |
|------------|----------|----------|-----------|-----------|----------------|
| Bank Statement Import | Vietcombank, BIDV, etc. | SFTP/API | Inbound | Daily | Statement lines, balances |
| Payment Processing | Vietcombank, MoMo | API | Outbound | Real-time | Payment instructions, confirmations |
| E-Invoice Submission | General Dept of Taxation | HTTPS/XML | Outbound | Real-time | Invoice data, QR code |
| Tax Declaration Filing | General Dept of Taxation | HTTPS/XML | Outbound | Monthly/Quarterly | Tax forms, supporting data |
| Exchange Rate | State Bank of Vietnam | HTTPS/JSON | Inbound | Daily | VND exchange rates |
| Customer Tax Code Validation | Tax Authority | HTTPS/API | Outbound | Real-time | Tax code verification |

### 9.2 Internal Module Integrations

| Source Module | Target | Data Flow | Trigger | Frequency |
|---------------|--------|-----------|---------|-----------|
| M07 Purchasing | M04 Accounting | Purchase invoice -> AP entry | Invoice receipt | Real-time |
| M08 Sales | M04 Accounting | Sales invoice -> AR entry + Revenue | Invoice issuance | Real-time |
| M09 Inventory | M04 Accounting | Inventory movements -> COGS/Stock entries | Stock transaction | Real-time |
| M05 Employee Mgmt | M04 Accounting | Payroll -> Expense entries | Payroll run | Monthly |
| M10 Asset Mgmt | M04 Accounting | Depreciation -> Expense entries | Monthly close | Monthly |
| M11 Contract Mgmt | M04 Accounting | Payment schedules -> Cash flow plan | Contract update | Real-time |

### 9.3 Webhook Events

| Event | Trigger | Payload | Subscribers |
|-------|---------|---------|-------------|
| journal_entry.posted | Journal entry posted | entry_id, entry_number, amounts, accounts | GL, Budget, Reports |
| cash_book.recorded | Cash transaction recorded | cash_id, amount, account, balance | Cash dashboard, Reports |
| receivable.updated | Receivable status changed | receivable_id, customer_id, status, outstanding | AR dashboard, Collections |
| payable.updated | Payable status changed | payable_id, supplier_id, status, outstanding | AP dashboard, Payments |
| invoice.issued | Invoice issued | invoice_id, customer, total, vat, qr_code | Tax system, AR, Reports |
| tax_declaration.submitted | Tax declaration submitted | tax_id, type, period, net_amount | Tax authority, Audit |
| budget.exceeded | Budget threshold exceeded | budget_id, account, actual, budget, variance | Budget alerts, Managers |
| fiscal_period.closed | Fiscal period closed | period_id, year, month | All modules, Audit |

### 9.4 Event Schema Example

```json
{
  "event_type": "journal_entry.posted",
  "event_id": "evt-20260414-001",
  "timestamp": "2026-04-14T10:30:00Z",
  "source": "m04-accounting",
  "data": {
    "entry_id": 10542,
    "entry_number": "JE-2026-00421",
    "entry_date": "2026-04-14",
    "total_debit": 5000000,
    "total_credit": 5000000,
    "source_module": "Manual",
    "posted_by": 1005,
    "posted_at": "2026-04-14T10:30:00Z",
    "lines": [
      {
        "account_code": "6424",
        "debit_amount": 5000000,
        "credit_amount": 0
      },
      {
        "account_code": "2141",
        "debit_amount": 0,
        "credit_amount": 5000000
      }
    ]
  }
}
```

---

## 10. Technical Notes

### 10.1 Performance Requirements

| Metric | Target | Measurement | Notes |
|--------|--------|-------------|-------|
| Journal Entry Save | < 500ms | P95 latency | Includes validation and line creation |
| GL Query (1M rows) | < 2s | P95 latency | With date and account filters |
| Dashboard Load | < 1.5s | P95 latency | All KPIs and charts |
| Report Generation | < 5s | P95 latency | Standard reports |
| Batch Invoice Import | 1000 in < 30s | Throughput | With validation |
| Cash Book Search | < 1s | P95 latency | With multiple filters |
| Concurrent Users | 100+ | Simultaneous | Read and write operations |

### 10.2 Caching Strategy

| Cache Type | Data | TTL | Invalidation |
|------------|------|-----|--------------|
| Redis | Chart of Accounts | 1 hour | On account update |
| Redis | Dashboard KPIs | 5 minutes | On any transaction posting |
| Redis | Exchange Rates | 24 hours | On daily rate import |
| Redis | Budget data | 30 minutes | On budget update |
| Redis | AR/AP aging summary | 15 minutes | On payment/transaction |
| Application | Account hierarchy tree | 1 hour | On structure change |
| Browser | Report filters (last used) | Session | On session end |

### 10.3 Key Files and Components

| File/Component | Path | Purpose |
|----------------|------|---------|
| Journal Entry Service | `src/modules/accounting/services/journal-entry.service.ts` | JE CRUD, validation, posting |
| Cash Book Service | `src/modules/accounting/services/cash-book.service.ts` | Cash transaction management |
| GL Posting Engine | `src/modules/accounting/services/gl-posting.service.ts` | Post entries to General Ledger |
| Tax Calculator | `src/modules/accounting/services/tax-calculator.service.ts` | VAT, CIT, PIT calculations |
| Budget Engine | `src/modules/accounting/services/budget-engine.service.ts` | Budget tracking and variance |
| E-Invoice Generator | `src/modules/accounting/services/e-invoice.service.ts` | E-invoice with QR generation |
| Financial Report Generator | `src/modules/accounting/services/report-generator.service.ts` | Balance sheet, income statement |
| Aging Calculator | `src/modules/accounting/services/aging-calculator.service.ts` | AR/AP aging analysis |
| Chart of Accounts Manager | `src/modules/accounting/services/coa.service.ts` | Account hierarchy management |
| Reconciliation Service | `src/modules/accounting/services/reconciliation.service.ts` | Bank and account reconciliation |

### 10.4 Localization

| Aspect | Configuration | Notes |
|--------|---------------|-------|
| Language | Vietnamese (primary), English (secondary) | All labels, messages, reports |
| Currency | VND (base), multi-currency support | Exchange rate from SBV |
| Date Format | DD/MM/YYYY | Vietnamese standard |
| Number Format | 1.234.567.890 (dot separator) | Vietnamese standard |
| Account Names | Vietnamese | Per VAS chart of accounts |
| Tax Compliance | Vietnamese tax regulations | Circular 200/2014/TT-BTC |
| Fiscal Year | Calendar year (Jan-Dec) | Standard Vietnamese fiscal year |
| Report Templates | VAS-compliant formats | Per Vietnamese accounting standards |

### 10.5 Mobile Support

| Feature | Mobile Support | Notes |
|---------|----------------|-------|
| Dashboard | Responsive | KPI cards, trend charts |
| Cash Entry | Responsive | Full entry form optimized for mobile |
| Invoice Creation | Responsive | Simplified invoice form |
| AR/AP View | Responsive | List view with filtering |
| Report Viewing | Responsive | View-only, export to PDF |
| Approval | Responsive | Approve/reject workflow |
| Notifications | Push | Payment received, budget alerts |
| Barcode/QR Scan | Camera integration | Scan e-invoice QR codes |

### 10.6 Security Considerations

| Aspect | Implementation |
|--------|----------------|
| Data Encryption | AES-256 for sensitive financial data at rest |
| Transport Security | TLS 1.3 for all API communications |
| Audit Logging | Complete audit trail for all financial operations |
| Role-Based Access | RBAC with row-level security |
| Session Management | JWT tokens with 15-minute expiry, refresh tokens |
| Data Masking | Mask sensitive amounts in logs and error messages |
| Backup | Daily encrypted backups with 30-day retention |
| Segregation of Duties | Maker-checker for journal entry posting |
| Anti-Tampering | Hash chain for journal entries to detect modifications |
| Compliance | SOX-style controls for financial data integrity |

### 10.7 Database Considerations

- All monetary values stored as DECIMAL(18,2) to avoid floating-point issues
- Transaction isolation level: READ COMMITTED for queries, SERIALIZABLE for postings
- Soft deletes for audit compliance (is_deleted flag)
- Partition GeneralLedger table by fiscal period for performance
- Archive closed fiscal year data to cold storage
- Materialized views for dashboard KPIs (refreshed on transaction posting)
- Full-text search on description fields for transaction lookup

---

*Document Version: 1.0*
*Last Updated: 2026-04-14*
*Author: ERP Design Team*
*Review Status: Draft*
