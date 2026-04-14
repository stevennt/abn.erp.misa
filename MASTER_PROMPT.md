# MASTER PROMPT: Generate All 41 ERP Module Specifications

## Instructions for AI Assistant

You are a **Senior Software Architect** tasked with documenting a complete ERP system (MISA AMIS clone) based on 73 reference screenshots.

### Your Mission

Generate **complete technical specification documents** for all **41 ERP modules** listed below. Generate them **one at a time in priority order**. Do NOT ask for review between modules. Do NOT stop until all 41 are complete.

### Output Format

For EACH module, generate a **separate, complete Markdown file** with this exact filename:
```
docs/M{number}_{module_name_snake_case}.md
```

Each file must contain ALL 10 sections described in the template below.

---

## Module Specification Template

Every module document MUST follow this exact structure:

```markdown
# M{XX} - {Module Name} ({Vietnamese Name})

**Category:** {Finance|Business|HR|Digital Office|Supply Chain|Platform}  
**Priority:** P{0-6}  
**Reference Images:** assets/image_{XX}.png  
**Dependencies:** M{XX}, M{YY}  
**Generated:** {date}

---

## 1. Module Overview

### Purpose
{1-2 paragraphs describing what this module does and why it exists}

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| {Role} | {Description} | {Full/Partial/Read-only} |

### Module Dependencies
- **Requires:** M{XX} ({Name}), M{YY} ({Name})
- **Used by:** M{XX} ({Name}), M{YY} ({Name})

### Key Business Rules
1. {Rule 1}
2. {Rule 2}
3. {Rule 3}

---

## 2. Screen Inventory

### 2.1 Screen: {Screen Name}
**Route:** `/{route-path}`  
**Purpose:** {1 sentence}

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| {Component} | {Card/Table/Form/Chart/Modal} | {Description} |

**User Interactions:**
- {Action 1}
- {Action 2}

**Data Displayed:**
- {Data element 1}
- {Data element 2}

### 2.2 Screen: {Screen Name}
...

---

## 3. Feature Specifications

### 3.1 Feature: {Feature Name}
**Description:** {1-2 sentences}

**User Stories:**
- As a {role}, I want to {action}, so that {benefit}

**Acceptance Criteria:**
- Given {precondition}
- When {action}
- Then {result}

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| {Field} | {Rule} | {Message} |

**Edge Cases:**
- {Edge case 1}
- {Edge case 2}

### 3.2 Feature: {Feature Name}
...

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|    Entity1     |       |    Entity2     |       |    Entity3     |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |<----->| id (PK)        |
| field1         |       | entity1_id(FK) |       | entity2_id(FK) |
| field2         |       | field1         |       | field1         |
| created_at     |       | created_at     |       | created_at     |
| updated_at     |       | updated_at     |       | updated_at     |
+----------------+       +----------------+       +----------------+
```

### 4.2 Table Schemas

#### Table: {table_name}
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| {column} | {type} | {constraints} | {description} |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_{table}_{column} ON {table}({column});
CREATE INDEX idx_{table}_{column1}_{column2} ON {table}({column1}, {column2});
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| {table} | {Purpose} | {Estimate} |

---

## 5. API Specifications

### 5.1 {Resource} Endpoints

#### GET /api/v1/{resource}
**Description:** List all {resource} with pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| limit | integer | No | Items per page (default: 20, max: 100) |
| sort | string | No | Sort field |
| order | string | No | asc/desc |
| {filter} | {type} | No | {Description} |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "field1": "value",
      "field2": "value",
      "created_at": "2024-01-01T00:00:00Z",
      "updated_at": "2024-01-01T00:00:00Z"
    }
  ],
  "meta": {
    "total": 100,
    "page": 1,
    "limit": 20,
    "last_page": 5
  }
}
```

**Permission:** `{permission_code}`

#### POST /api/v1/{resource}
**Description:** Create a new {resource}

**Request Body:**
```json
{
  "field1": "value",
  "field2": "value"
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "field1": "value",
  "created_at": "2024-01-01T00:00:00Z"
}
```

**Permission:** `{permission_code}`

#### GET /api/v1/{resource}/{id}
**Description:** Get single {resource} by ID

**Response (200 OK):**
```json
{
  "id": 1,
  "field1": "value"
}
```

**Response (404 Not Found):**
```json
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "{Resource} with id {id} not found"
  }
}
```

#### PUT /api/v1/{resource}/{id}
**Description:** Update a {resource}

**Request Body:**
```json
{
  "field1": "updated_value"
}
```

**Permission:** `{permission_code}`

#### DELETE /api/v1/{resource}/{id}
**Description:** Soft delete a {resource}

**Response (204 No Content)**

**Permission:** `{permission_code}`

### 5.2 Error Response Format
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": {}
  }
}
```

### 5.3 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/{resource} | List | {perm} |
| POST | /api/v1/{resource} | Create | {perm} |
| GET | /api/v1/{resource}/{id} | Get | {perm} |
| PUT | /api/v1/{resource}/{id} | Update | {perm} |
| DELETE | /api/v1/{resource}/{id} | Delete | {perm} |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| {Role} | {code} | {Description} |

### Permission Matrix
| Role | Create | Read | Update | Delete | Export | Approve |
|------|--------|------|--------|--------|--------|---------|
| Admin | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Manager | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ |
| User | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Viewer | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |

### Row-Level Security
- Users can only {rule}
- Managers can {rule}
- Admins can {rule}

---

## 7. State Machine / Workflows

### 7.1 {Entity} Status Transitions

```
[DRAFT] ──submit──→ [PENDING] ──approve──→ [ACTIVE] ──complete──→ [COMPLETED]
   │                    │                    │
   └──delete───────────┘                    └──cancel──→ [CANCELLED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | PENDING | submit | Creator | Send notification |
| PENDING | ACTIVE | approve | Manager | Lock fields |
| PENDING | DRAFT | reject | Manager | Return to draft |
| ACTIVE | COMPLETED | complete | System | Archive |
| ACTIVE | CANCELLED | cancel | Creator | Void |

### 7.2 Approval Workflow
```
Submitter → Department Manager → Finance Manager → Director
     ↓              ↓                ↓              ↓
  [Pending]    [Dept Approved]  [Finance OK]   [Final Approved]
```

---

## 8. Reports & Analytics

### 8.1 Report: {Report Name}
**Type:** {Real-time/Scheduled/Ad-hoc}  
**Purpose:** {Description}

**Dimensions:**
- {Dimension 1}
- {Dimension 2}

**Metrics:**
- {Metric 1}
- {Metric 2}

**Filters:**
- {Filter 1}
- {Filter 2}

**Chart Type:** {Bar/Line/Pie/Table}

### 8.2 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| {Name} | {Type} | {Frequency} | {Format} |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| {API} | {Purpose} | {Method} | {Auth} |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| {event.name} | {schema} | {subscribers} |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| {event.name} | {Source} | {Action} |

---

## 10. Technical Notes

### Performance
- Expected data volume: {estimate}
- Query complexity: {Low/Medium/High}
- Expected response time: {ms}

### Caching Strategy
- {Cache layer description}
- TTL: {duration}
- Cache invalidation: {strategy}

### File Attachments
- Supported types: {types}
- Max size: {size}
- Storage: {S3/Local/CDN}

### Localization
- Supported languages: {languages}
- Date format: {format}
- Currency: {currency}

### Mobile Considerations
- {Mobile-specific notes}
```

---

## Module Generation Context

Below is the complete context for ALL 41 modules. Use this context when generating each module's specification.

### P0 - Foundation Modules

#### M01 - User & Authentication
**Images:** 02, 35, 37
**Context:**
- Image 02: Welcome email for new employee Trương Thanh Sơn. Lists workflows available: advance payment, payment, purchase workflow, contract approval, leave approval. Mentions AMIS Chat and OneAI (GPT, Gemini, Grok, Claude). Activation valid 24 hours.
- Image 35: Employee detail page "Quan hệ lao động" showing: Employee list sidebar (100 employees), employee profile with tabs (Basic Info, Contact, Work Info, Account, Family, Other). Fields: Employee ID (D02-0067), Middle name, Last name, Gender, DOB, Place of birth, Marital status, Personal tax ID, Family type, Ethnicity, Religion. Department: Sales staff - Hanoi Office.
- Image 37: HR Overview dashboard: Total employees 1,253, New employees 15 (+50%), Successful probation 11 (+22%), Resignations 10 (-33%). Charts: HR movement 2016-2020, Employee count by year (250→875), Department structure (Hanoi 350/40%, Da Nang 175/20%, HCM 300/40%, Can Tho 50/5%), Contract types (Probation 5%, Indefinite 40%, Fixed-term 34%, Seasonal 20%).
**Entities:** User, Employee, UserProfile, LoginSession, PasswordReset, EmailVerification, Department
**Features:** Registration, login, profile CRUD, employee directory, onboarding workflow, department assignment

#### M02 - Roles, Permissions & Access Control
**Images:** 02, 35, 60
**Context:**
- Image 02: Shows approval workflows requiring specific roles (department manager, finance manager, director).
- Image 35: Department-based access control (employee belongs to department, manager oversees department).
- Image 60: System admin dashboard showing user counts per app, access tracking, new user creation events.
**Entities:** Role, Permission, UserRole, RolePermission, AccessLog, Department
**Features:** RBAC, department-level permissions, access logging, permission inheritance, approval delegation

#### M03 - Company & Organization Structure
**Images:** 29, 30, 37, 39, 60
**Context:**
- Image 29: Agricultural cooperative structure with supplier lists, debt tracking.
- Image 30: Group management with parent company (Phuong Nam Food Corporation) and 6 subsidiaries. Shows data synchronization status, org hierarchy tree. "Chưa có dữ liệu" (No data yet) for unsynced subsidiaries.
- Image 37: Department breakdown: Hanoi 40%, Da Nang 20%, HCM 40%, Can Tho 5%.
- Image 39: Org tree for objectives: MISA JSC → Production Block, Head Office, Hanoi Office, Da Nang Office, Buon Ma Thuot Office, HCM Office, Can Tho Office.
- Image 60: System admin showing 2,800 total users, per-app usage stats, new entity creation events.
**Entities:** Company, Department, Subsidiary, OrgChart, Location, Branch, DataSyncConfig
**Features:** Multi-company support, org hierarchy management, branch CRUD, data consolidation, inter-company data sync

### P1 - Core Modules

#### M04 - Accounting (Kế toán)
**Images:** 15, 25, 28, 29, 62
**Context:**
- Image 15: Financial dashboard: Cash 7,500, Deposit 1,000, Receivable 5,082, Payable 903, Revenue 2,500, Expense 1,009, Profit 1,491, Inventory 640. Receivables: 5,082 total (825 overdue, 4,257 within term). Payables: 4,066 total (2,066 overdue, 2,000 within term). Revenue/expense/profit monthly bar chart. Cash flow chart. Left nav: Cash, Deposit, Purchasing, Sales, Invoice Management, Warehouse, Tools, Fixed Assets, Tax, Costing, Summary, Budget, Reports, Financial Analysis.
- Image 25: Sales workflow (Sales → Invoice → Collect Payment, with returns/discount options). Purchasing workflow (Purchase → Receive Invoice → Pay, with returns/discount options).
- Image 28: E-invoice dashboard with QR codes, invoice statistics, tax period declaration table, mobile app preview.
- Image 29: Cooperative purchasing: Order value 7,862M, Executed 1,100M, Paid 579M, Remaining 256M. Contract value 7,862M. Total purchase 7,862M, Paid 579M, Remaining payable 1,100M. Supplier debt table (310M total). Supplier purchase value table (498M total).
- Image 62: Operations financial dashboard: Revenue 65,338M (+15%), Expenses 51,212M (-6%), Profit 5,126M (+10%). Monthly charts. Cash balances by branch. Cash flow chart.
**Entities:** Account, JournalEntry, GeneralLedger, CashBook, DepositBook, Receivable, Payable, Invoice, TaxDeclaration, FinancialReport, Budget, BankAccount, ChartOfAccounts
**Features:** Double-entry bookkeeping, chart of accounts, financial statements (balance sheet, income statement), cash flow management, tax reporting, budget tracking, receivable/payable aging, invoice management

#### M05 - Employee Management (Nhân viên)
**Images:** 21, 35, 37, 38
**Context:**
- Image 21/38: Timesheet calendar showing monthly view with shift blocks (CA SÁNG 08:00-12:00, CA CHIỀU 13:30-17:30). Summary: Paid days 27.5, Overtime 20h, Late/early 2 times, Leave 1 day. Special markers: leave, unpaid leave, business trip, late arrival. Deadline for confirmation shown.
- Image 35: Employee database with 100 employees. Detail view with 6 tabs. Search by employee ID, name, phone, email. Filter by company.
- Image 37: HR overview with 1,253 total employees, hiring/resignation trends, department structure, contract type distribution.
**Entities:** Employee, EmployeeProfile, ContactInfo, WorkHistory, FamilyMember, Contract, Document, Timesheet, AttendanceRecord
**Features:** Employee CRUD, profile management with multi-tab form, document attachment, org assignment, contract tracking, timesheet view, search/filter

#### M06 - Task Management (Công việc)
**Images:** 17, 22, 55, 60
**Context:**
- Image 17/55: Task dashboard with greeting, time display, motivational quote. Summary cards: Today's tasks (1 overdue, 2 due, 0 soon, 0 awaiting approval, 1 to assign). Weekly donut: 16 tasks (1 on-time, 3 late, 4 incomplete, 4 pending, 4 overdue). Task list with deadlines, priority tags (Urgent, AMIS Mobile), tags (Recruitment). Left nav: Overview, My Tasks, Reports, Search Projects, Personal Projects, Operations Block (HR Dept, Admin Dept).
- Image 22: Marketing graphic showing task creation within AMIS Chat. Chat group "AMIS Chat Design Analysis Group" (8 members). Messages with Figma links, @mentions, auto-notifications about room availability, checklist creation, file sharing. Task creation popup with assignee, due date, priority, subtasks.
- Image 60: Operations dashboard showing 3 new tasks, 5 new workflow runs, 5 new posts.
**Entities:** Task, TaskProject, TaskAssignment, TaskComment, TaskAttachment, TaskChecklist, TaskPriority, TaskStatus, TaskTag
**Features:** Task CRUD, assignment, priority levels, deadline management, status tracking, checklists, comments, file attachments, chat integration, department views, personal projects, search, dashboard analytics

#### P2 - Operations Modules

#### M07 - Purchasing (Mua hàng)
**Images:** 29, 68, 69, 70
**Context:**
- Image 29: Purchasing dashboard: Order value 7,862M, Executed 1,100M, Paid 579M, Remaining 256M. Contract value same. Supplier debt (310M top: NCC0012 180, NCC0002 65). Supplier purchase values (498M top: NCC0012 251, NCC0002 95).
- Image 68: Quotation approval workflow. Tabs: All (24), Created (8), Pending (12), Approved (24), Rejected (4). Table with request number, date, quotation status, requester, process status (visual dots), delivery deadline, requesting unit, request type (domestic/foreign purchase).
- Image 69: Purchase request list. Columns: Request number, date, purpose (office computers, scheduled equipment), status (Rejected, Approved, Not yet proposed, Proposed, Pending, Completed), delivery deadline. 45 total requests.
- Image 70: Quotation detail for QTH0012. 3 supplier tabs (Green Talen, Hoa Son, Tan Hoang Minh). Table: Item code, name, quantity (8), unit price (200,000), total (1,600,000), discount (0), VAT 10%, tax (160,000), grand total (1,760,000). Delivery: 29/05/2023, payment terms, address (Ngoai Giao Doan, Tay Ho, Hanoi). Right panel: Approval timeline with timestamps (10:30 25/03, 10:30 26/03), actors (Nguyen Thanh Dat - Accounting Manager, Vo Dinh Hoang - Purchasing Staff, Luong Thuy Kieu - Accounting Manager), actions (Rejected, Sent for approval, Created request, Approved).
**Entities:** PurchaseRequest, PurchaseOrder, Quotation, Supplier, SupplierQuotation, PurchaseContract, PurchaseApproval, PurchaseItem, PurchaseCategory, DeliverySchedule
**Features:** Purchase requisition, multi-supplier quotation comparison, approval workflow with timeline, PO creation, supplier management, delivery tracking, domestic/foreign purchase types, request categorization

#### M08 - Sales (Bán hàng)
**Images:** 34, 62
**Context:**
- Image 34: POS interface. Left: Shopping cart with items (men's shirts, women's shirts, combos), quantities, unit prices, totals. Customer phone number lookup. Total 1,700,000. Right: Product grid with images, names, prices (280k-699k). Category: Women's fashion, New arrivals. Bottom: Cancel, Save (F10), Collect Payment (F12) buttons. Top: Price list selector, sales staff selector, sales channel, search (F3).
- Image 62: Sales revenue shown in operations financial dashboard: 65,338M monthly revenue.
**Entities:** SaleOrder, SaleItem, Customer, PriceList, Discount, SaleInvoice, SaleReturn, POS_Session, POS_Transaction, Product, ProductCategory, ProductImage
**Features:** POS operations, product catalog with images, customer lookup by phone, shopping cart, pricing, discounts, invoicing, returns, keyboard shortcuts, price list management, sales channel tracking

#### M09 - Inventory & Warehouse (Kho hàng)
**Images:** 01, 29, 68
**Context:**
- Image 01: Warehouse listed as core module "Kho, thủ kho" (Warehouse, warehouse keeper).
- Image 29: Supplier data shows inventory-related purchases. Warehouse department mentioned in purchase requests.
- Image 68: Warehouse department shown as requesting unit for purchases.
**Entities:** Warehouse, Location, StockItem, StockMovement, StockAdjustment, StockTransfer, StockCount, StockAlert, WarehouseKeeper, StockBatch
**Features:** Multi-warehouse support, stock level tracking, movement logging (in/out/transfer), stock adjustments, periodic counts, low stock alerts, batch/lot tracking, warehouse keeper assignment

#### M10 - Asset Management (Tài sản)
**Images:** 08, 11, 12, 59
**Context:**
- Image 08: Product detail page. Description: "AMIS Tài sản is software for managing tools and equipment, supporting management, operations, and asset administration across all departments. Meets all operations with smart features. Multi-device support, easy integration with other management software." Video preview 1:45 showing mobile app. Related apps: Tasks, Social Network, Notes, Meeting Room, Video Library, Photo Library.
- Image 11/12/59: Dashboard with 4 widgets. Asset status donut: 165 total (60 in use, 30 not used, 24 under maintenance, 10 being repaired, 10 damaged report, 5 broken, 5 lost, 5 insurance loss, 5 destroyed, 5 proposed disposal, 3 in transfer). Asset movement bar/line chart T1-T12. Repair status chart with quantity/value/cost. Reminders: Today 5, Yesterday 3, Last 7 days 2, This month 4, Older than 6 months 2. Left nav: Overview, Assets, Allocation-Recovery, Transfer, Repair-Maintenance, Loss-Destroy-Disposal, Inventory, Reports, Settings.
**Entities:** Asset, AssetCategory, AssetStatus, AssetMovement, AssetAllocation, AssetRecovery, AssetTransfer, AssetRepair, AssetMaintenance, AssetDisposal, AssetInventory, AssetReminder, AssetDepreciation
**Features:** Asset lifecycle management, status tracking (11 statuses), allocation/recovery workflow, inter-department transfer, repair/maintenance scheduling, loss/destruction/disposal processing, periodic inventory, dashboard analytics, reminder system, depreciation calculation

#### M11 - Contract Management (Hợp đồng)
**Images:** 01, 62
**Context:**
- Image 01: Contracts "Hợp đồng" listed as core module.
- Image 62: Financial contracts tracked in operations dashboard.
**Entities:** Contract, ContractTemplate, ContractParty, ContractTerm, ContractPayment, ContractRenewal, ContractAlert, ContractType, ContractStatus
**Features:** Contract creation from templates, party management (multi-party), term configuration, payment schedule tracking, auto-renewal alerts, status management (draft/active/expired/terminated)

#### P3 - HR Suite Modules

#### M12 - Payroll (Tiền lương)
**Images:** 20, 50, 51, 63
**Context:**
- Image 20/50: Payroll dashboard for Vu Ngoc Sim, Pham Gia Golden JSC. Total salary: 1,250 Billion VND. PIT: 50 Billion. Insurance: 50 Billion. Salary level analysis: Above 30M (50 employees), 20-30M (~150), 10-20M (~200), Below 10M (~150). Income structure donut: Base salary 54.6%, Sales bonus 28.6%, Kicker bonus 14.3%, Excellent employee bonus 1.8%, Sympathy/Celebration/Birthday 0.7%. Average income over time (T1-T6, Aug: 15,000,000). Average income by office: HN ~18M, HCM ~14M, CT 15M, BMT ~11M, DN ~10M. Payslip feedback with employee avatars. Reminders: Unsent payslips (2), Employees not enrolled in insurance (2), Irregular insurance salary.
- Image 51: Income summary by position. Bar chart and table. Positions: CEO (4 people, 4,853,000), BA (4, 7,833,000), Deputy CEO (18, 4,443,000), Receptionist (22, 5,933,000), Chief Accountant (66, 1,313,000), UX Designer (4, 995,000), Sales Manager (4, 838,000), ISO Staff (18, 2,448,000), Market Analyst (22, 9,758,000). Salary components A, B, C columns.
- Image 63: HR dashboard showing salary & benefits tab.
**Entities:** PayrollPeriod, EmployeePayroll, SalaryComponent, SalaryPolicy, Payslip, PayrollAdjustment, TaxDeclaration, InsuranceDeclaration, PayrollReport, BonusScheme, DeductionRule
**Features:** Salary calculation engine, component management (base/bonus/deductions), policy configuration, payslip generation and distribution, PIT withholding, social insurance deduction, multi-office salary comparison, position-based salary analysis, reminder system

#### M13 - Time Tracking (Chấm công)
**Images:** 21, 38, 47, 48, 49
**Context:**
- Image 21/38: Monthly timesheet with shift blocks. Summary: Paid days 27.5, Overtime 20h (regular 20, rest day 0, holiday 0), Late/early 2, Leave 1. Calendar shows CA SÁNG (08:00-12:00) and CA CHIỀU (13:30-17:30) for each workday. Special: Nov 22 leave, Nov 29 unpaid leave, Nov 16 business trip, Nov 3 late 60 min. Confirmation deadline shown.
- Image 47: Overview dashboard. Late/early today: 40 (+23). Absent today: 16 (-16). Absent this week: 23 (-2). Leave trend chart (May peak: 38). Most late employees: Nguyen Danh Tuyen 36, Tran Bang Kieu 29, Nguyen Thi Ly 25. Leave type donut: Leave 60, Unpaid 60, Sick 40, Marriage 80, Other 120 (total 360). Late frequency: 1 time (36 people), 2 times (29), 3 times (25), 4 times (22), 5 times (20).
- Image 48: Shift setup form. Name: "Administrative shift", Shift: "Morning admin", Period: 06/08-31/08/2020, Repeat: weekly every 2 weeks, Days: Mon-Fri checked. Apply to employee list. Calendar preview showing HC_SA markers.
- Image 49: Comprehensive shift table. 120 records. Shift codes: HC_SA (morning admin), HC_CH (afternoon admin), PT_SA (part-time morning), CA_1/2/3. Modal for selecting shift for specific employee on specific date.
**Entities:** Timesheet, Shift, ShiftSchedule, AttendanceRecord, OvertimeRecord, LeaveRequest, LeaveType, LateEarlyRecord, BusinessTrip, ShiftAssignment, LeaveBalance
**Features:** Shift scheduling with recurrence, attendance tracking via timesheet, overtime calculation (regular/rest day/holiday rates), leave request workflow, late/early monitoring, business trip tracking, shift assignment modal, confirmation workflow, analytics dashboard

#### M14 - Recruitment (Tuyển dụng)
**Images:** 36, 63
**Context:**
- Image 36: Recruitment pipeline for "Android Developer" position, Production block, 45 headcount. Kanban stages: Applied (2 - Nguyen Van Thanh, Ha Quynh Nga - both marked unsuitable), Skills Test (10 - various candidates), IQ Test (3 - Nguyen Hai Nam marked insufficient score), Interview (5), Hired (0), Recruited (1 - Tran Van Hai). Left nav: Overview, Job Postings, Candidates, Calendar, Talent Pools, Tasks, Inbox, Reports, Settings. Actions: Auto-filter candidates, Public status, Export, Add candidate.
- Image 63: Resignation reasons chart: Benefits (60), Work environment (35), Adaptation difficulty (30), Career change (25), Performance (20).
**Entities:** JobPosting, Candidate, Application, Interview, TestResult, OfferLetter, RecruitmentStage, TalentPool, JobRequirement, InterviewFeedback
**Features:** Job posting creation, candidate sourcing, Kanban pipeline management (6 stages), skills testing, IQ testing, interview scheduling with feedback, offer management, auto-filtering, talent pool, recruitment reports

#### M15 - Performance Evaluation (Đánh giá)
**Images:** 43, 44, 45, 46
**Context:**
- Image 43: Create evaluation period form. 3 steps: General Info, Evaluation Process, Evaluation Form. Fields: Period name (Feb Objective Survey), Applied unit (Technical Economics Dept), Positions (Accounting, BO Assistant, BA), Deadline (10/03/2023), Methods (Objectives, Competency, Work Report), Objective collection time (Feb 2023), Report template (Work report template), Auto-reminder toggle, Recurring setup link. Employee table: 7 employees with positions, departments, direct managers, criteria list links. Total 5 records shown.
- Image 44: Evaluation Gantt chart. Left nav: Methods (Personal Objectives, Competency, Work Report), Evaluation Period, Settings (Employees, Customize, Roles, Users, System). Employee list sidebar. Timeline view showing objectives with green bars across months: Product group N1 sales (Jan-Jul), Detergent new sales (Jan-Apr), Regional breakdowns (Northeast, Southwest, South Central), Insurance sales.
- Image 45: Evaluation results for Doan Thi Hien (B05-0450, Sales staff, Technical Economics Dept). Completion rate 66.67%. Self-evaluation ✓, Manager evaluation ✓, Approval --. Radar chart with 8 axes: Attitude, Product knowledge, Business knowledge, Product management, Analysis, Project management, Problem solving (200 points), Responsibility. Green = evaluation score, Orange = max score.
- Image 46: Template library modal. Tabs: Objective list, Competency criteria, Survey questions. 5 template cards: HR template, Marketing template, Sales template, Customer Care template, Production template. Each with illustration.
**Entities:** EvaluationPeriod, EvaluationMethod, EvaluationForm, EvaluationCriteria, EmployeeEvaluation, SelfEvaluation, ManagerEvaluation, EvaluationResult, EvaluationTemplate, CompetencyModel, RatingScale, EvaluationComment
**Features:** Evaluation period creation with multi-method support, employee selection, Gantt timeline view, radar chart visualization, self and manager evaluation, 360-degree feedback, template library by department, competency-based assessment, recurring evaluations

#### M16 - Objectives & KPI (Mục tiêu)
**Images:** 39, 40, 41, 42
**Context:**
- Image 39: Objectives table view. Left org tree: MISA JSC → Production Block, Head Office, Hanoi, Da Nang, Buon Ma Thuot, HCM, Can Tho. Table: Objective name, Period (2023), Target, Type (MBO/OKR/KPI), Value type (Minimum), Value (1000B, 200B, 100B), Result (500, 100, 50), Unit (Billion VND/Percent), Progress (50% green bar), Status (Incomplete). Company revenue target 1000B (50%), Office targets 200B each (50%), Center targets 100B each (50%), Recruitment plan OKR (not measured), Recruitment KPIs (50%).
- Image 40: Mind map view. HR Dept → HR Stability Rate 2023 (25/50 50%). Quarterly breakdown Q1-Q4, each 25/50. Individual assignments: Pham Thi Phuong (Q1-Q4), Do Thuy Nga (Q2, completed), Ho Kim Dung (Q2, completed), Le Quynh Trang (Q2, KPI: 0).
- Image 41: BSC Strategy map. 4 perspectives: Financial (4 objectives: increase profit margin, increase revenue, reduce purchasing cost, reduce inventory cost), Customer (6: improve quality, competitive pricing, improve after-sales, dealer trust, strong brand), Internal Process (5: supply chain, warehouse management, production efficiency, channel development, brand development), Learning & Growth (5: management capability, worker skills, R&D/QC capability, sales/marketing capability, management info capability).
- Image 42: Add objective form. Type: MBO/KPI/OKR/BSC radio. Name, Max value (97.0), Unit (Percent), Period (2023), Target (Department/Individual), Department (HR Management), Representative (Pham Thi Phuong), Calculation method (Manual entry), Parent objective, Aggregate checkbox, Attachments, Description.
**Entities:** Objective, ObjectiveType, ObjectiveValue, ObjectiveAssignment, ObjectiveProgress, BSC_Perspective, StrategicMap, KeyResult, ObjectiveComment, ObjectiveTemplate
**Features:** MBO/OKR/KPI/BSC support, hierarchical objectives, progress tracking with visual bars, mind map visualization, strategy mapping with 4 BSC perspectives, quarterly breakdowns, department/individual assignment, parent-child relationships, template library

#### M17 - Social Insurance (Bảo hiểm xã hội)
**Images:** 52, 53, 54
**Context:**
- Image 52: Electronic records list. Left nav: Electronic Records, Paper Records, Payment Tracking, Contribution Tracking, Labor Management, Settings. Table: Record name, Number, Submission count, Created date, Submission date, Type, Status. Statuses: Not sent, Sent, Missing info, Rejected, Approved. Records: Labor increase report, Labor decrease, Contribution adjustment, Retroactive collection (violations), Retroactive (salary adjustment), Card reissue.
- Image 53: Create electronic record modal. Left categories: All, SI Collection/Number/Card, Reissue SI number, Reissue HI card, SI Policy, HI Policy, HI only, Voluntary SI, COVID-19 support. Center: Procedure list with codes (600, 600c, 600d, 600o, 601, 605, 607, 608, 610, 612). Right: Procedure 600 details with 9 cases (new employee, returning after benefit, returning after unpaid leave, end of suspension, reduction, benefit reduction, unpaid leave reduction, salary adjustment, position change).
- Image 54: Employee list for form 600a (labor increase). Period 06/2020, Submission 01. Table: SI number, Name, ID number, DOB, Gender, Position, Department, Benefit group, Option. 20 of 123 employees shown. Summary: 20 declared, 20 SI books issued, 15 HI cards issued, 120M VND total salary fund.
**Entities:** SIRecord, SIProcedure, SIType, EmployeeSI, SIContribution, SIDeclaration, SIReport, HIRecord, UIRecord, SIHistory
**Features:** Electronic SI/HI/UI declarations, procedure code management (600-series), employee SI tracking, contribution calculation, submission status workflow, multi-form support, salary fund reporting

#### M18 - Labor Relations (Quan hệ lao động)
**Images:** 35, 37, 63
**Context:**
- Image 35: Employee database with labor relations context (contracts, positions, departments).
- Image 37: HR overview with navigation: Profiles, Contracts, Appointments, Dismissals, Transfers, Resignations, Commendations, Incidents, Planning, Reports, Settings. Contract type stats: Probation 50 (5%), Indefinite 350 (40%), Fixed-term 300 (34%), Seasonal 175 (20%).
- Image 63: Resignation analysis with reasons breakdown.
**Entities:** LaborContract, ContractType, Appointment, Dismissal, Transfer, Resignation, ResignationReason, Commendation, Incident, LaborDispute, PlanningRecord
**Features:** Labor contract lifecycle (create/renew/terminate), personnel actions (appointment/dismissal/transfer), resignation tracking with reason analysis, commendation recording, incident management, workforce planning

#### P4 - Digital Office Modules

#### M19 - Workflow (Quy trình)
**Images:** 13, 56, 61
**Context:**
- Image 13: Workflow run history. Tabs: Overview, Runs, Design, Reports, Settings. Button: Run Workflow. Left filters: All, To Do (5), Created, Participated, Draft. Table: ID, Title, Workflow, Created date, Creator, Executor, Deadline, Status. Statuses: In progress, Completed, Rejected, Draft, Cancelled. Sample: IT resource allocation requests with various statuses. 100 total records.
- Image 56: Similar run history with different sidebar: To Do (5), Done, Draft, All Runs (active), Custom Filters (Mine, IT). Same IT resource request data.
- Image 61: Operations workflow stats. Total runs: 750, In progress: 120, Completed: 600, Cancelled: 30, Draft: 30. Bar chart by department: Marketing 96 (highest), HCTH 81, QTKD 46, Doi tac 41, CSKH 27, HR 26, Accounting 26, TCDN 21, NSBH 17, CNTT 15, VP TCT 31, Other 57.
**Entities:** Workflow, WorkflowStep, WorkflowRun, WorkflowAction, WorkflowApproval, WorkflowTemplate, WorkflowFilter, WorkflowCondition, WorkflowNotification
**Features:** Visual workflow designer, run execution engine, multi-step approval chains, custom filters, department statistics, status lifecycle, deadline management, notification triggers

#### M20 - Internal Chat (Chat)
**Images:** 19, 22, 58
**Context:**
- Image 19/58: Social network feed with chat-like features. Post composer, sharing options, likes, comments.
- Image 22: Chat group "AMIS Chat Design Analysis Group" (8 members). Messages: Figma design link sharing, @mentions, auto-notification "Berlin meeting room available", checklist creation, Excel file sharing. Task creation overlay integrated into chat.
**Entities:** ChatRoom, ChatMessage, ChatMember, ChatMention, ChatFile, ChatReaction, ChatPin, ChatSearch
**Features:** Group and direct messaging, file sharing (images, docs, spreadsheets), @mentions, reactions, message pinning, search, task creation from messages, auto-notifications from other modules, typing indicators

#### M21 - Document Management (Ghi chép/Tài liệu)
**Images:** 18, 57
**Context:**
- Image 18: Document library with hierarchical categories. Left tree: Favorites, All, Management docs, Process docs/regulations (expanded: Standard docs, Archive docs, QA docs), Sales handbook, Business model docs, Business policies, HR handbook, Employee handbook, Software dealer list, Partner contacts, Market development handbook, IT handbook, PR handbook, Admin handbook. Document list: Standard docs (company management info), Archive docs, QA docs, Quality handbook, ISO draft, Customer complaint scenarios. Total 10 docs.
- Image 57: WeSign upload area mentioning pdf, docx, doc, jpg, png support.
**Entities:** Document, DocumentCategory, DocumentVersion, DocumentPermission, DocumentComment, DocumentFavorite, DocumentTag, DocumentLock
**Features:** Hierarchical folder structure, document CRUD, version control, file type support (pdf/docx/doc/jpg/png), search, permissions, favorites, comments, locking, ISO document management

#### M22 - Correspondence (Văn thư)
**Images:** 18, 60
**Context:**
- Image 18: Document management context with "Văn thư" (Correspondence) mentioned in app marketplace.
- Image 60: Operations dashboard showing 3 new documents, 5 new workflow runs, correspondence usage stats.
**Entities:** Correspondence, IncomingDocument, OutgoingDocument, DocumentType, DocumentFlow, TrackingNumber, DocumentArchive, SignatoryAuthority
**Features:** Incoming/outgoing document registration, tracking number assignment, document flow management, signatory authority, archive management, correspondence reports

#### M23 - Internal Social Network (Mạng xã hội)
**Images:** 19, 58
**Context:**
- Image 19/58: Social feed with post composer ("What do you want to share?"), quick actions (Share/Ideas/News). Post by Dang Thuy Anh about teambuilding with beach photos. Likes (17), Comments (34), Shares (5). Right sidebar: Birthdays (3 upcoming), Notifications (new hires, commendations, promotions, marriages, resignations, transfers). Left nav: News Feed, News, Opportunity Pool, Ideas.
**Entities:** SocialPost, SocialComment, SocialLike, SocialShare, SocialAlbum, SocialEvent, Birthday, Announcement, Idea, OpportunityPool
**Features:** Social feed with multiple post types, image albums, engagement tracking, birthday reminders, real-time notifications (hires, promotions, commendations, marriages, resignations, transfers), idea submission, opportunity pool

#### M24 - Meeting Room Booking (Phòng họp)
**Images:** 16, 60
**Context:**
- Image 16: Room booking calendar. Left: Building N03 T1, navigation by floor (Mezzanine, Floor 2 expanded, Floor 4). Floor 2 rooms: Berlin, London, Madrid, Paris (Video Conference), Praha (Mobile), Rome, Sofia, Sydney. Calendar grid: Aug 22, 2019, time 08:00-17:00. Bookings: Berlin 09:30-10:30 (PQA Seminar CMMI 2.0), Paris 10:00-11:30 (BA training), Sofia 08:30-10:30 (locked), Madrid 14:00-16:30 (locked), Sydney 15:00-17:30 (ISO training), Berlin 16:30-17:30 (Employee evaluation). Right: Toggle all rooms/my rooms, Reload, Book room button.
- Image 60: Operations dashboard showing 3 new meetings booked.
**Entities:** MeetingRoom, RoomBooking, RoomSchedule, Building, Floor, RoomEquipment, BookingConflict, RecurringBooking
**Features:** Building/floor/room hierarchy, calendar grid view, time slot booking, private/locked meetings, video conference room support, equipment tracking, recurring bookings, conflict detection

#### M25 - Contact Directory (Danh bạ)
**Images:** 03, 04, 05
**Context:**
- Image 03/04: Onboarding guide mentioning "Danh bạ - Xem, tìm kiếm liên hệ và trao đổi với đồng nghiệp" (Contacts - View, search contacts and communicate with colleagues).
- Image 05: Contact app icon on dashboard (purple with phone symbol).
**Entities:** Contact, ContactGroup, ContactTag, ContactNote, ContactFavorite, ContactHistory
**Features:** Contact CRUD, group organization, tagging, search, notes, favorites, communication history

#### M26 - Video Library (Kho phim)
**Images:** 23
**Context:**
- Image 23: Video management interface. Title: "Video Library Management". Sort by: Title, Upload date, Views. Add video modal: Video link (with hint "Enter link then press Enter to fetch info"), Title, Thumbnail (upload/delete), Description. Video list: SME service video, AMIS launch video, MISAGottalent 2017 folk dance (150 views), MISAGottalent 2017 Tam Cam play.
**Entities:** Video, VideoCategory, VideoComment, VideoLike, VideoView, VideoPlaylist, VideoSource
**Features:** Video management via link (Vimeo/YouTube), metadata fetching, thumbnail management, categorization, view tracking, comments, likes, playlists

#### M27 - Photo Library (Kho ảnh)
**Images:** 24, 72
**Context:**
- Image 24: Photo viewer. Full-screen aerial beach photo. Navigation arrows. Right panel: Uploader Dang Thu Trang, date 04/07/2017. Engagement: 17 Likes, 34 Comments, 5 Shares. Comments with timestamps. Comment input. Social interaction model.
- Image 72: Mobile app showing photo library icon.
**Entities:** Photo, PhotoAlbum, PhotoComment, PhotoLike, PhotoShare, PhotoTag, PhotoMetadata, PhotoPrivacy
**Features:** Photo upload/storage, album organization, full-screen viewer with navigation, social engagement (like/comment/share), tagging, metadata (EXIF), privacy settings, mobile access

#### M28 - Notes & Knowledge (Kiến thức)
**Images:** 18, 72
**Context:**
- Image 18: Document management with knowledge base categories (handbooks, policies, guides).
- Image 72: Mobile app showing Knowledge app icon (yellow with book and sun).
**Entities:** KnowledgeArticle, KnowledgeCategory, KnowledgeTag, KnowledgeComment, KnowledgeRating, KnowledgeVersion, KnowledgeAuthor
**Features:** Article creation and editing, categorization, tagging, version control, comments, ratings, multi-author support, search, mobile access

#### M29 - Electronic Signature (WeSign)
**Images:** 57, 64
**Context:**
- Image 57/64: WeSign dashboard. Left nav: Overview, Document List, Signer List, Document Type List, User Management, Access Log. Top cards: Waiting for my signature (12), Waiting for others (12), Completed (168), Rejected (4). Upload area: Drag & drop or upload, supported formats (pdf, docx, doc, jpg, png), Start button. Bottom: Help card with FAQ link, Mobile app download card ("Sign documents anytime, anywhere").
**Entities:** SignatureDocument, SignatureRequest, Signer, SignatureField, SignatureCertificate, SignatureLog, SignatureTemplate, SignatureStatus
**Features:** Document upload with multi-format support, signature request creation, multi-signer workflow, signature field placement, certificate management, audit log, template creation, mobile signing

#### M30 - URL Shortener (MISA ShortLink)
**Images:** 65
**Context:**
- Image 65: Mobile preview showing MISA ShortLink interface. Shortened URL display, QR code, analytics graph with click tracking. "Use Now" button.
**Entities:** ShortURL, URLAnalytics, URLClick, QRCode, URLTag, URLCampaign
**Features:** URL shortening, custom aliases, QR code generation, click analytics with graphs, campaign tracking, tag management, mobile preview

#### P5 - Advanced Modules

#### M31 - aiMarketing
**Images:** 09, 10, 14, 31, 32, 60
**Context:**
- Image 09/14/31/32: Dashboard. Revenue 40,435,500,000đ (+15.2%), Cost 20,121,500,000đ (+15.2%), ROI 28.55% (+15.2%). Conversion funnel: Lead 1,852,371 → MQL 1,239,091 → SAL 1,055,108 → SQL 1,012,242. Customer care: Opportunity rate 38%, Revenue rate 25.1%, AOV 58,950,000đ. Line chart showing daily conversion trends. Left nav: Overview, Costs, Campaigns, Lead, Contacts, Contact Groups, Landing page, Email, Form, Banner, CTA, Workflow, URL tracking, Keyword Research.
- Image 10: Features table. 1) Email Marketing: templates, drag-drop design, bulk sending, spam filtering, open/click rate optimization. 2) Landing Page: quick design, drag-drop, no code needed, conversion optimization. 3) Lead Form: website/landing page forms, customer profiles. 4) Data Management: centralized data from all sources, prevent fragmentation, segment by customer journey. 5) Reports: 20+ multi-dimensional reports, real-time, auto-generated. 6) Integration: CRM data sync, SMS/Zalo/Facebook channels.
- Image 60: Usage stats: 1,130 daily users for aiMarketing.
**Entities:** Campaign, EmailTemplate, LandingPage, Lead, LeadForm, Contact, ContactGroup, MarketingReport, AdChannel, ConversionFunnel, EmailCampaign, SMSCampaign, FacebookCampaign, ABTest
**Features:** Email marketing with drag-drop builder, landing page builder (no-code), lead collection forms, centralized data management, 20+ report types, multi-channel campaigns (Email/SMS/Zalo/Facebook), conversion tracking, A/B testing, keyword research, URL tracking, CTA management

#### M32 - CRM
**Images:** 60, 67
**Context:**
- Image 60: 1,130 daily users. Usage stats in operations dashboard.
- Image 67: Workflow integrations: CRM-Warehouse connection (36 total runs, 30 success, 6 failed), CRM-Accounting connection (same stats).
**Entities:** Lead, Opportunity, Account, Contact, Deal, Activity, Pipeline, Stage, ActivityType, Competitor, Product, Quote
**Features:** Lead management, opportunity tracking with pipeline visualization, account management, contact database, deal tracking, activity logging, competitor tracking, product catalog integration, quote generation, workflow integrations with warehouse and accounting

#### M33 - E-Invoice (Hóa đơn điện tử)
**Images:** 28
**Context:**
- Image 28: E-invoice dashboard. Sample invoice with QR code. Stats cards with invoice counts and values. Timeline chart of invoice submissions. Pie chart by invoice type. Mobile app showing usage: 1,091 times used, 27 this month, usage categories. Tax period declaration table with submission statuses.
**Entities:** EInvoice, InvoiceItem, TaxPeriod, TaxDeclaration, InvoiceTemplate, InvoiceStatus, InvoiceType, InvoiceBatch, QRCode
**Features:** E-invoice creation with QR code, invoice templates, batch processing, tax period management, declaration submission to tax authority, status tracking (submitted/not submitted), mobile access, usage analytics

#### M34 - Payment Gateway (Cổng thanh toán)
**Images:** 27
**Context:**
- Image 27: JetPay dashboard for Hong Thai Construction JSC. Top cards: Payments 292.2B (2,822B total, +12.5%), 210 transactions (+12.5%), Fee 3.332B. Refunds 32B (33B total, -2.5%), 19 transactions (+0.5%), Fee 0.2B. Net received: 240B transferred, 50.13B remaining. Payment value trend chart (monthly). Payment method donut: Domestic card, International card, QR Code, MOMO e-wallet, Viettel Money, Transfer, ZaloPay. Transaction count: 47,074 total, monthly trend chart.
**Entities:** PaymentTransaction, RefundTransaction, PaymentMethod, PaymentGateway, TransactionFee, SettlementReport, PaymentLink, MerchantAccount, TransactionStatus
**Features:** Multi-payment method support (7 methods), real-time transaction tracking, refund management, fee calculation, settlement reports, payment link generation, merchant account management, trend analytics, monthly reconciliation

#### M35 - Digital Signature Service (Chữ ký số)
**Images:** 26
**Context:**
- Image 26: MISA ESign interface. Left nav: Configuration, Digital Certificate (active), Certificate renewal, Info change, Certificate document, Customer info, Update, Introduction. Main: Certificate management tree with owner name and certificate ID (9651EE75-C426-4829-A232-2D3C8C6BC41A). View and Exit buttons. Support phone: 090 488 5833.
**Entities:** DigitalCertificate, CertificateOwner, CertificateValidity, CertificateRevocation, CertificateUsage, CertificateProvider
**Features:** Certificate management, validity period tracking, owner information management, certificate renewal, info change requests, revocation handling, usage logging, provider integration

#### M36 - Promotion Management (Khuyến mại)
**Images:** 33
**Context:**
- Image 33: Promotion analysis. Filters: Time (this month), Date range (01/01-31/01/2021), Program (Covid-Business Support), Product type (AMIS Time Tracking). Pie chart: HH1834, HH1835, HH1836, HH1837. Table: Product code, name, purchase quantity, order count, discount amount. HH1834: First year Standard subscription (93 units, 30 orders, 16M discount). HH1835: Second year+ Standard (61 units, 70 orders, 25M). HH1836: First year Professional (61 units, 53 orders, 54M). HH1837: Second year+ Professional (36 units, 42 orders, 26M). Total: 251 units, 195 orders, 121M discount.
**Entities:** PromotionProgram, PromotionItem, PromotionDiscount, PromotionReport, PromotionCondition, PromotionSchedule, PromotionBudget, PromotionType
**Features:** Promotion program creation, condition setting (time/date/product), discount calculation (fixed/percentage), effectiveness analysis with pie charts, product-level breakdown, order vs quantity tracking, budget management, export

#### M37 - Store/Retail POS (Cửa hàng)
**Images:** 34
**Context:**
- Image 34: POS interface. Customer: phone 12345678901. Left: Shopping cart with items (men's shirts HQ 280k, women's shirts various 250k-450k, pink dress 680k). Quantities, unit prices, line totals. Total 1,700,000. Right: Product grid with images, category filters (Women's fashion, New arrivals), price tags. Bottom: Cancel, Save (F10), Collect Payment (F12). Top: Price list, sales staff, sales channel selectors, search (F3).
**Entities:** POS_Session, POS_Transaction, POS_Item, POS_Customer, POS_PriceList, POS_Discount, POS_Return, POS_Payment, Product, ProductVariant, ProductImage, Barcode
**Features:** POS session management, product catalog with images and variants, customer lookup by phone, shopping cart, price list management, discount application, multiple payment methods, return processing, keyboard shortcuts, barcode scanning, sales channel tracking, receipt printing

#### P6 - Platform Modules

#### M38 - OneAI Platform
**Images:** 03, 66, 72
**Context:**
- Image 03: Onboarding guide showing OneAI assistant. "Chat with leading AI: GPT, Gemini, Grok,..."
- Image 66: OneAI product page. "AMIS OneAI is a platform integrating the most advanced AI models on a single product, helping organizations equip all employees with AI resources at reasonable cost - maximum efficiency - safe and secure." Integrated models: ChatGPT, Gemini, Grok, DeepSeek, Claude, +10. Benefits for organization: Buy once for entire team, no additional purchase for new employees, simple procedures, cost-effective vs multiple providers. Centralized management: allocate/recover/adjust quotas, ensure right person/right access, no info leaks, no resource waste on departure. Detailed reports: leadership visibility, adjustment basis, resource tracking, smart investment decisions. Benefits for members: Quick info search, smart AI Q&A, data research/analysis, document processing, summarization, content creation, planning, daily task automation. Saves 80% time/effort. Optimized experience: single account for multiple models, user-friendly design, advanced utilities, multi-platform (web/mobile), convenient for everyone.
- Image 72: Mobile app showing OneAI integration.
**Entities:** AIModel, AIConversation, AIPrompt, AITemplate, AIUsage, AIQuota, AICost, AIBot, AIConversationHistory, AIToken, AISettings
**Features:** Multi-model AI integration (6+ models), conversation management, prompt library, template system, usage tracking per user/model, quota management, cost optimization, custom bot builder, browser extension, mobile support, centralized admin panel, detailed usage reports, team-wide licensing

#### M39 - Operations Dashboard (Điều hành)
**Images:** 60, 61, 62, 63
**Context:**
- Image 60: System admin dashboard. Total users: 2,800. Access trend (7 days): 5→80. Usage table: Tasks 512, Workflow 401, Correspondence 133, Time Tracking 15, Notes 120, WeSign 110, aiMarketing 1,130, CRM 1,130, Social Network 112. Product usage chart with bars (total users, active, inactive) and line (usage rate). Generated data: 3 new tasks, 5 new runs, 5 new posts, 3 new documents, 5 new files, 5 new notes, 3 new meetings, 5 new employees.
- Image 61: Workflow stats: 750 total runs, 120 in progress, 600 completed, 30 cancelled, 30 draft. Department bar chart: Marketing 96, HCTH 81, QTKD 46, Partners 41, CSKH 27, HR 26, Accounting 26, TCDN 21, NSBH 17, CNTT 15, VP 31, Other 57.
- Image 62: Financial dashboard: Revenue 65,338M (+15%), Expenses 51,212M (-6%), Profit 5,126M (+10%). Monthly charts. Cash balances by branch.
- Image 63: HR dashboard: 130 employees, 2 new, 2 probation success, 2 resignations. Age distribution donut. HR movement trends. Resignation reasons.
**Entities:** DashboardWidget, DashboardConfig, UsageMetric, SystemLog, AggregatedReport, WidgetType, DataAggregation, CrossModuleMetric
**Features:** Cross-module dashboards, real-time metrics, user access analytics, per-app usage tracking, financial overview, HR overview, workflow statistics, custom widget configuration, data aggregation engine, trend analysis, generated data summary

#### M40 - Group Management (Quản lý tập đoàn)
**Images:** 30, 60
**Context:**
- Image 30: Group data management. Parent: Phuong Nam Food Corporation. Subsidiaries: Head office, Quan Binh flour mill, Quan Binh 2 rice mill, An Nam transport, Phuong Nam North (no data yet), Phuong Nam South, Phuong Nam Central. Org hierarchy tree showing database sync status. "No synced data from Long An Food Company."
- Image 60: System admin showing group-level metrics.
**Entities:** GroupCompany, Subsidiary, ConsolidatedData, InterCompanyTransaction, GroupReport, DataSyncLog, SubsidiaryConfig, GroupHierarchy
**Features:** Multi-entity management, parent-subsidiary hierarchy, data synchronization status tracking, consolidated reporting, inter-company transactions, org hierarchy visualization, per-subsidiary data management

#### M41 - E-Banking (Ngân hàng điện tử)
**Images:** 15, 27
**Context:**
- Image 15: Banking integration in accounting. Deposit tracking (1,000), bank balance display.
- Image 27: JetPay payment gateway showing bank transfer as payment method. E-banking mentioned in marketplace.
**Entities:** BankAccount, BankTransaction, BankStatement, BankReconciliation, BankConnection, BankProvider, TransactionCategory, BankBalance
**Features:** Bank account management, transaction synchronization, statement import (MT940/BAI2), auto-reconciliation, multi-bank support, balance tracking, transaction categorization, connection management

---

## Execution Instructions

1. Start with **M01** (User & Authentication)
2. Generate the complete spec following the template
3. Output the markdown content
4. Move to **M02**, repeat
5. Continue through **M41**
6. Do NOT stop or ask for review between modules
7. If context window gets full, summarize and continue with remaining modules
8. Each module file should be saved as: `docs/M{XX}_{snake_case_name}.md`

Begin with M01 now.
