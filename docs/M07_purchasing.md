# M07 Purchasing Module Specification

## 1. Module Overview

### 1.1 Purpose

The Purchasing Module (M07) is the comprehensive procurement management system within the ABN ERP platform. It handles the complete procurement lifecycle from purchase request creation through quotation comparison, purchase order generation, approval workflows, goods receipt, supplier invoice matching, and payment tracking. The module provides real-time visibility into purchasing activity, supplier performance, and budget compliance.

Key financial metrics: Total purchase orders 7,862M, executed 1,100M, paid 579M, remaining 256M. Supplier debt stands at 310M. The module manages 45 purchase requests across status categories: All, Created (8), Pending (12), Approved (24), Rejected (4). Quotation QTH0012 demonstrates competitive bidding with 3 suppliers, MSI mouse quantity 8, price 200K, total 1.6M, VAT 10%, grand total 1.76M. The module tracks complete approval timelines for audit and compliance.

### 1.2 Personas

| Persona | Role | Key Responsibilities | Primary Screens | Access Level |
|---------|------|---------------------|-----------------|--------------|
| Purchasing Manager | Department leadership | Approve POs, manage suppliers, negotiate contracts | Dashboard, Approvals, Reports | Full |
| Purchasing Officer | Procurement execution | Create requests, manage quotations, track orders | Purchase Request, Quotation, PO | Read/Write |
| Requester | Internal customer | Submit purchase requests, track status | Request Creation, My Requests | Read/Write |
| Approver | Authorization | Review and approve/reject requests and POs | Approval Queue | Approve/Reject |
| Receiver | Goods receipt | Receive goods, inspect quality, update delivery status | Goods Receipt, Inspection | Read/Write |
| Finance Officer | Payment processing | Match invoices, process payments, track supplier debt | Invoice Matching, Payments | Read/Write |
| Supplier | External vendor | View RFQs, submit quotations, track orders | Supplier Portal | Limited |
| Director | Strategic oversight | Review purchasing spend, supplier analysis, budget impact | Reports, Dashboard | Read-only |

### 1.3 Dependencies

| Dependency | Type | Direction | Description |
|-----------|------|-----------|-------------|
| M04 Accounting | Downstream | Outbound | Purchase invoices -> AP entries, payments |
| M09 Inventory/Warehouse | Downstream | Outbound | Goods receipt -> Stock updates |
| M10 Asset Management | Downstream | Outbound | Asset purchases -> Asset creation |
| M06 Task Management | Peer | Inbound | Purchase approval tasks |
| M11 Contract Management | Peer | Inbound | Supplier contract terms |
| Supplier Portal | External | Bidirectional | RFQ distribution, quotation submission |
| Budget System | Upstream | Inbound | Budget availability check |

### 1.4 Business Rules

1. **Sequential Numbering**: Purchase requests and orders must be numbered sequentially with unique identifiers (e.g., PR-2026-0001, PO-2026-0001).
2. **Quotation Requirement**: Purchases exceeding 5M require at least 2 supplier quotations; purchases exceeding 50M require at least 3 quotations.
3. **Approval Thresholds**: Purchase requests under 10M auto-approved by department manager; 10M-100M require purchasing manager; over 100M require director approval.
4. **Budget Check**: All purchase requests must pass budget availability check; requests exceeding budget require additional approval.
5. **Three-Way Match**: Supplier invoices must match purchase order and goods receipt within tolerance (2% or 500K VND).
6. **Supplier Debt Limit**: Total outstanding debt per supplier cannot exceed predefined credit limit.
7. **Delivery Tracking**: Each purchase order tracks delivery schedule with expected and actual delivery dates.
8. **Goods Inspection**: Received goods must pass quality inspection before stock update.
9. **Approval Timeline**: All approval actions are logged with timestamp, approver, and reason for audit.
10. **Purchase Category Classification**: All items must be classified into purchase categories for reporting and budget tracking.
11. **Quotation Validity**: Quotations are valid for 30 days from submission date unless otherwise specified.
12. **PO Amendment**: Approved purchase orders can only be amended with re-approval if total value changes by > 5%.

---

## 2. Screen Inventory

### 2.1 Screen Routes and Purposes

| Route | Purpose | Personas | Priority |
|-------|---------|----------|----------|
| `/purchasing/dashboard` | Purchasing dashboard with KPIs and status summary | All personas | Critical |
| `/purchasing/requests` | Purchase request list and creation | Requester, Purchasing Officer | Critical |
| `/purchasing/requests/{id}` | Purchase request detail with approval timeline | All personas | High |
| `/purchasing/quotations` | Quotation management and comparison | Purchasing Officer | High |
| `/purchasing/quotations/{id}` | Quotation detail (QTH0012 example) | Purchasing Officer, Supplier | High |
| `/purchasing/orders` | Purchase order list and tracking | Purchasing Officer, Finance Officer | Critical |
| `/purchasing/orders/{id}` | Purchase order detail with delivery schedule | All personas | High |
| `/purchasing/approvals` | Approval queue with pending items | Approver, Purchasing Manager | Critical |
| `/purchasing/suppliers` | Supplier management and performance | Purchasing Manager, Purchasing Officer | High |
| `/purchasing/goods-receipt` | Goods receipt recording and inspection | Receiver | High |
| `/purchasing/invoice-matching` | Three-way match: PO vs GR vs Invoice | Finance Officer | High |
| `/purchasing/payments` | Payment tracking and supplier debt | Finance Officer | High |
| `/purchasing/reports` | Purchasing reports: spend analysis, supplier performance | Purchasing Manager, Director | High |
| `/purchasing/categories` | Purchase category management | Purchasing Manager, Admin | Medium |

### 2.2 UI Components by Screen

#### /purchasing/dashboard

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Total Orders KPI | Metric Card | Total POs 7,862M | PurchaseOrder sum |
| Executed KPI | Metric Card | Executed 1,100M | GoodsReceipt linked POs |
| Paid KPI | Metric Card | Paid 579M | Payment records |
| Remaining KPI | Metric Card | Remaining 256M | Executed - Paid |
| Supplier Debt KPI | Metric Card | Supplier debt 310M | Payable summary |
| Status Breakdown | Badge Group | Created 8, Pending 12, Approved 24, Rejected 4 | Request status query |
| Pending Approvals | List | Requests awaiting approval | Approval queue |
| Upcoming Deliveries | List | Expected deliveries this week | DeliverySchedule |
| Spend Trend Chart | Line Chart | Monthly purchasing spend | PurchaseOrder aggregation |
| Top Suppliers Chart | Bar Chart | Top 10 suppliers by spend | Supplier spend analysis |
| Quick Actions Bar | Action Bar | New request, new quotation, approval | N/A |

#### /purchasing/quotations/{id} (Quotation Detail - QTH0012)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Quotation Header | Info Panel | Quotation number QTH0012, status, date | Quotation master |
| Supplier Comparison | Table | 3 suppliers side by side | SupplierQuotation |
| Item Grid | Data Table | MSI mouse, qty 8, price 200K, total 1.6M | QuotationItem |
| VAT Calculator | Info Panel | VAT 10% on 1.6M = 160K | Tax calculation |
| Grand Total | Summary | Grand total 1.76M | Total + VAT |
| Comparison Matrix | Table | Price, quality, delivery time, warranty per supplier | SupplierQuotation |
| Recommended Supplier | Badge | Best value recommendation | Comparison algorithm |
| Approval Timeline | Timeline | Approval history with timestamps | QuotationApproval |
| Convert to PO Button | Action Button | Convert selected quotation to PO | Quotation -> PO |

### 2.3 User Interactions

1. **Purchase Request Creation**: Requester clicks "New Request" -> fills form (items, quantity, category, justification, budget code) -> submits for approval -> request enters pending status -> approver reviews and approves/rejects.
2. **Quotation Comparison**: Purchasing Officer creates quotation request -> sends to 3 suppliers -> suppliers submit quotations -> system displays comparison (QTH0012: MSI mouse qty 8, price 200K, total 1.6M, VAT 10%, grand 1.76M) -> selects best quotation -> converts to purchase order.
3. **Three-Way Match**: Finance Officer receives supplier invoice -> matches against PO and goods receipt -> system validates within tolerance -> approves for payment -> creates AP entry in M04 Accounting.
4. **Approval Workflow**: Approver receives notification -> reviews request details -> approves with comment or rejects with reason -> timeline updated with action -> requester notified of outcome.

### 2.4 Data Displayed

- Purchase Request: ID, title, requester, department, category, total amount, status, submission date, approval status
- Quotation QTH0012: 3 suppliers, item MSI mouse qty 8, price 200K, total 1.6M, VAT 10%, grand total 1.76M
- Purchase Order: ID, supplier, total value, delivery schedule, received quantity, paid amount, remaining balance, status
- Approval Timeline: Step name, approver, action (approved/rejected), timestamp, comment

---

## 3. Feature Specifications

### 3.1 Purchase Request Management

**Description**: Create, submit, and track purchase requests with approval workflows, budget checks, and category classification.

**User Stories**:
- As a Requester, I want to create purchase requests with item details and justification so that procurement can proceed.
- As a Purchasing Officer, I want to review and consolidate purchase requests so that I can source efficiently.
- As an Approver, I want to review purchase requests with budget information so that I can make informed approval decisions.

**Acceptance Criteria**:
1. Purchase request form with: title, description, items (product/service, quantity, estimated price), category, budget code, justification, urgency.
2. Auto-generated request number (PR-2026-0001).
3. Budget availability check at submission.
4. Approval workflow based on amount thresholds.
5. Status tracking: Draft, Pending, Approved, Rejected, Cancelled.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-001 | Title | Required, 3-200 chars | "Title is required (3-200 characters)" |
| VR-002 | Items | At least one item required | "Add at least one item" |
| VR-003 | Category | Required | "Purchase category is required" |
| VR-004 | Budget Code | Required, must be valid | "Invalid budget code" |
| VR-005 | Total Amount | Must be > 0 | "Total amount must be positive" |
| VR-006 | Budget | Must not exceed available budget | "Insufficient budget" |
| VR-007 | Urgency | Required, valid level | "Valid urgency level is required" |

**Edge Cases**:
- Emergency request: Bypasses normal approval with director override.
- Request splitting: Splitting large requests to avoid approval thresholds detected and flagged.
- Request cancellation: Only allowed before PO creation.
- Bulk request: Import multiple requests from spreadsheet.

### 3.2 Quotation Management

**Description**: Manage RFQs, supplier quotations, and quotation comparison for competitive bidding.

**User Stories**:
- As a Purchasing Officer, I want to send RFQs to multiple suppliers so that I can compare prices and terms.
- As a Purchasing Officer, I want to compare quotations side by side so that I can select the best value.
- As a Supplier, I want to submit quotations through the portal so that I can compete for business.

**Acceptance Criteria**:
1. RFQ creation with items, specifications, delivery requirements, deadline.
2. Send RFQ to selected suppliers (minimum 2 for > 5M, 3 for > 50M).
3. Supplier quotation submission with price, delivery time, warranty, validity.
4. Quotation comparison matrix with scoring (price 40%, quality 30%, delivery 20%, warranty 10%).
5. Selected quotation converts to purchase order.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-010 | Supplier Count | Min 2 for > 5M, min 3 for > 50M | "Insufficient quotations for this amount" |
| VR-011 | Quotation Validity | Must be within validity period | "Quotation has expired" |
| VR-012 | Price | Must be > 0 | "Price must be positive" |
| VR-013 | Delivery Date | Must be after order date | "Delivery date must be after order date" |
| VR-014 | Supplier | Must be active, approved supplier | "Supplier is not approved" |

**Edge Cases**:
- Single-source procurement: Allowed with justification and special approval.
- Quotation revision: Supplier can revise quotation before deadline.
- All quotations exceed budget: Escalate for budget increase or re-negotiation.

### 3.3 Purchase Order Management

**Description**: Create, track, and manage purchase orders with delivery schedules, goods receipt, and payment tracking.

**User Stories**:
- As a Purchasing Officer, I want to create purchase orders from approved quotations so that orders are accurate.
- As a Receiver, I want to record goods receipt against purchase orders so that inventory is updated.
- As a Finance Officer, I want to track payments against purchase orders so that supplier debt is accurate.

**Acceptance Criteria**:
1. PO creation from selected quotation or manual entry.
2. PO number auto-generated (PO-2026-0001).
3. Delivery schedule with multiple delivery dates and quantities.
4. Goods receipt against PO with quantity verification and quality inspection.
5. Payment tracking: total ordered, received, paid, remaining.
6. PO amendment with re-approval for significant changes.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-020 | Supplier | Required, active supplier | "Valid supplier is required" |
| VR-021 | Items | At least one item | "Add at least one item" |
| VR-022 | Delivery Date | At least one delivery date | "Delivery schedule required" |
| VR-023 | Total | Must match quotation within tolerance | "Total differs from quotation" |
| VR-024 | Receive Qty | Cannot exceed ordered quantity | "Received quantity exceeds ordered" |
| VR-025 | Payment | Cannot exceed received value | "Payment exceeds received value" |

**Edge Cases**:
- Partial delivery: Track received vs ordered quantity per delivery.
- Over-delivery: Flag when received exceeds ordered by > 5%.
- PO cancellation: Only allowed before any goods receipt.
- PO closure: Auto-close when fully received and paid.

### 3.4 Approval Workflow

**Description**: Multi-level approval workflow for purchase requests and purchase orders with threshold-based routing.

**User Stories**:
- As an Approver, I want to see all pending approvals in one queue so that I can process them efficiently.
- As a Requester, I want to see the approval timeline so that I know where my request stands.
- As a Purchasing Manager, I want to set approval thresholds so that the right people approve the right requests.

**Acceptance Criteria**:
1. Approval levels based on amount: < 10M (dept manager), 10M-100M (purchasing manager), > 100M (director).
2. Approval actions: Approve, Reject, Request More Info, Delegate.
3. Approval timeline with timestamps, approver names, actions, and comments.
4. Auto-escalation after 48 hours without action.
5. Approval delegation to alternate approver.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-030 | Approver | Must have approval authority | "No approval authority for this amount" |
| VR-031 | Reason | Required for rejection | "Reason required for rejection" |
| VR-032 | Delegation | Must designate valid alternate | "Invalid delegate" |
| VR-033 | Timeout | Auto-escalate after 48h | "Approval request expired" |

**Edge Cases**:
- Approver unavailable: Auto-delegate to alternate or escalate.
- Approval loop: Maximum 3 revision cycles before escalation.
- Emergency approval: Director can approve out-of-band with audit trail.

### 3.5 Three-Way Invoice Matching

**Description**: Match supplier invoices against purchase orders and goods receipts for payment authorization.

**User Stories**:
- As a Finance Officer, I want to match supplier invoices to POs and goods receipts so that payments are accurate.
- As a Finance Officer, I want tolerance-based matching so that minor discrepancies don't block payments.
- As a Purchasing Manager, I want to review unmatched invoices so that discrepancies are resolved.

**Acceptance Criteria**:
1. Invoice entry with: invoice number, date, supplier, items, amounts, VAT.
2. Auto-match to PO and goods receipt.
3. Tolerance-based matching: 2% or 500K VND (whichever is greater).
4. Discrepancy flagging: price variance, quantity variance, missing receipt.
5. Matched invoices auto-approved for payment; unmatched require manual review.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-040 | Invoice Number | Required, unique per supplier | "Invoice number already recorded" |
| VR-040 | PO Match | Must match a PO | "No matching purchase order" |
| VR-041 | Variance | Within tolerance for auto-approve | "Variance exceeds tolerance" |
| VR-042 | Supplier | Must match PO supplier | "Supplier mismatch" |
| VR-043 | Amount | Must not exceed PO value | "Invoice exceeds PO value" |

**Edge Cases**:
- Invoice without PO: Route for special approval.
- Partial invoice: Match to partial delivery.
- Credit note: Reduce payable balance.
- Currency difference: Exchange rate at invoice date vs PO date.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+---------------------+       +-------------------+       +-------------------+
|   PurchaseCategory  |       |  PurchaseRequest  |       | PurchaseApproval  |
+---------------------+       +-------------------+       +-------------------+
| PK category_id      |<----->| PK request_id     |       | PK approval_id    |
| category_name       |       | request_number    |       | FK request_id     |
| parent_id           |       | title             |       | FK approver_id    |
| budget_code_prefix  |       | FK category_id    |       | approval_level    |
| is_active           |       | FK requester_id   |       | action            |
+---------------------+       | FK budget_id      |       | action_date       |
                              | total_amount      |       | comments          |
                              | status            |       | status            |
                              | urgency           |       +-------------------+
                              | created_at        |
                              +-------------------+       +-------------------+
                                    |                    |    Quotation      |
                    +-------------------------------+    +-------------------+
                    |     PurchaseRequestItem     |    | PK quotation_id   |
                    +-------------------------------+    | quotation_number  |
                    | PK item_id                  |    | FK request_id     |
                    | FK request_id               |    | FK supplier_id    |
                    | product_name                |    | quotation_date    |
                    | quantity                    |    | validity_date     |
                    | estimated_price             |    | status            |
                    | FK category_id              |    | total_amount      |
                    +-------------------------------+    +-------------------+

+---------------------+       +---------------------+       +---------------------+
|  PurchaseOrder      |       |   PurchaseItem      |       | DeliverySchedule    |
+---------------------+       +---------------------+       +---------------------+
| PK order_id         |       | PK item_id          |       | PK schedule_id      |
| order_number        |       | FK order_id         |       | FK order_id         |
| FK supplier_id      |       | FK quotation_item_id|       | delivery_date       |
| FK quotation_id     |       | product_name        |       | quantity            |
| order_date          |       | quantity            |       | received_quantity   |
| delivery_terms      |       | unit_price          |       | received_date       |
| total_amount        |       | vat_rate            |       | status              |
| received_amount     |       | line_total          |       | notes               |
| paid_amount         |       +---------------------+       +---------------------+
| remaining_amount    |
| status              |       +---------------------+
+---------------------+       |  SupplierQuotation  |
                              +---------------------+
                              | PK sq_id            |
                              | FK quotation_id     |
                              | FK supplier_id      |
                              | unit_price          |
                              | delivery_days       |
                              | warranty_months     |
                              | validity_days       |
                              | score               |
                              | notes               |
                              +---------------------+

+---------------------+       +---------------------+       +---------------------+
|   GoodsReceipt      |       |   Supplier          |       | PurchaseContract    |
+---------------------+       +---------------------+       +---------------------+
| PK receipt_id       |       | PK supplier_id      |       | PK contract_id      |
| receipt_number      |       | supplier_code       |       | contract_number     |
| FK order_id         |       | supplier_name       |       | FK supplier_id      |
| receipt_date        |       | tax_code            |       | start_date          |
| FK receiver_id      |       | contact_person      |       | end_date            |
| total_received      |       | phone               |       | total_value         |
| quality_status      |       | email               |       | status              |
| status              |       | credit_limit        |       | terms               |
+---------------------+       | is_active           |       +---------------------+
                              +---------------------+
```

### 4.2 Table Schemas

#### PurchaseRequest

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| request_id | BIGINT | PK, SERIAL | Unique request ID |
| request_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated (PR-2026-0001) |
| title | NVARCHAR(200) | NOT NULL | Request title |
| description | TEXT | NULL | Request description |
| FK category_id | BIGINT | NOT NULL, FK REFERENCES PurchaseCategory | Purchase category |
| FK requester_id | BIGINT | NOT NULL, FK REFERENCES User | Request creator |
| FK department_id | BIGINT | NOT NULL, FK REFERENCES Department | Requesting department |
| FK budget_id | BIGINT | NOT NULL, FK REFERENCES Budget | Budget allocation |
| total_amount | DECIMAL(18,2) | NOT NULL | Total estimated amount |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, pending, approved, rejected, cancelled, converted |
| urgency | VARCHAR(20) | NOT NULL, DEFAULT 'normal' | low, normal, high, emergency |
| justification | TEXT | NULL | Business justification |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| approved_at | TIMESTAMP | NULL | Final approval timestamp |

#### PurchaseRequestItem

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| item_id | BIGINT | PK, SERIAL | Unique item ID |
| FK request_id | BIGINT | NOT NULL, FK REFERENCES PurchaseRequest | Parent request |
| product_name | NVARCHAR(200) | NOT NULL | Product/service name |
| specification | TEXT | NULL | Detailed specifications |
| quantity | DECIMAL(12,2) | NOT NULL | Requested quantity |
| unit | VARCHAR(20) | NOT NULL | Unit of measure |
| estimated_price | DECIMAL(18,2) | NOT NULL | Estimated unit price |
| line_total | DECIMAL(18,2) | NOT NULL | Quantity * price |
| FK category_id | BIGINT | FK REFERENCES PurchaseCategory | Item category |
| notes | NVARCHAR(300) | NULL | Additional notes |

#### Quotation

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| quotation_id | BIGINT | PK, SERIAL | Unique quotation ID |
| quotation_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated (QTH-0012) |
| FK request_id | BIGINT | FK REFERENCES PurchaseRequest | Source request |
| quotation_date | DATE | NOT NULL | Quotation creation date |
| validity_date | DATE | NOT NULL | Quotation validity end date |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'open' | open, submitted, selected, expired, cancelled |
| total_amount | DECIMAL(18,2) | NOT NULL | Total quotation amount |
| vat_amount | DECIMAL(18,2) | DEFAULT 0 | VAT amount |
| grand_total | DECIMAL(18,2) | NOT NULL | Total including VAT |
| FK created_by | BIGINT | FK REFERENCES User | Creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### SupplierQuotation

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| sq_id | BIGINT | PK, SERIAL | Unique supplier quotation ID |
| FK quotation_id | BIGINT | NOT NULL, FK REFERENCES Quotation | Parent quotation |
| FK supplier_id | BIGINT | NOT NULL, FK REFERENCES Supplier | Submitting supplier |
| unit_price | DECIMAL(18,2) | NOT NULL | Quoted unit price |
| quantity | DECIMAL(12,2) | NOT NULL | Quoted quantity |
| line_total | DECIMAL(18,2) | NOT NULL | Line total |
| vat_rate | DECIMAL(5,2) | DEFAULT 10 | VAT percentage |
| vat_amount | DECIMAL(18,2) | DEFAULT 0 | VAT amount |
| grand_total | DECIMAL(18,2) | NOT NULL | Line grand total |
| delivery_days | INT | NOT NULL | Delivery lead time in days |
| warranty_months | INT | DEFAULT 0 | Warranty period |
| validity_days | INT | DEFAULT 30 | Quotation validity days |
| score | DECIMAL(5,2) | NULL | Comparison score |
| is_selected | BOOLEAN | DEFAULT FALSE | Selected for PO |
| notes | TEXT | NULL | Supplier notes |
| submitted_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Submission timestamp |

#### PurchaseOrder

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| order_id | BIGINT | PK, SERIAL | Unique order ID |
| order_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated (PO-2026-0001) |
| FK supplier_id | BIGINT | NOT NULL, FK REFERENCES Supplier | Supplier |
| FK quotation_id | BIGINT | FK REFERENCES Quotation | Source quotation |
| order_date | DATE | NOT NULL | Order creation date |
| delivery_terms | NVARCHAR(200) | NULL | Delivery terms |
| payment_terms | NVARCHAR(200) | NULL | Payment terms |
| total_amount | DECIMAL(18,2) | NOT NULL | Total order value |
| vat_amount | DECIMAL(18,2) | DEFAULT 0 | VAT amount |
| grand_total | DECIMAL(18,2) | NOT NULL | Total including VAT |
| received_amount | DECIMAL(18,2) | DEFAULT 0 | Amount received |
| paid_amount | DECIMAL(18,2) | DEFAULT 0 | Amount paid |
| remaining_amount | DECIMAL(18,2) | CALCULATED | Grand total - paid |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, approved, ordered, receiving, completed, cancelled |
| FK created_by | BIGINT | FK REFERENCES User | Creator |
| FK approved_by | BIGINT | NULL, FK REFERENCES User | Approver |
| approved_at | TIMESTAMP | NULL | Approval timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### PurchaseItem

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| item_id | BIGINT | PK, SERIAL | Unique item ID |
| FK order_id | BIGINT | NOT NULL, FK REFERENCES PurchaseOrder | Parent order |
| FK quotation_item_id | BIGINT | FK REFERENCES SupplierQuotation | Source quotation item |
| product_name | NVARCHAR(200) | NOT NULL | Product name |
| specification | TEXT | NULL | Product specifications |
| quantity | DECIMAL(12,2) | NOT NULL | Ordered quantity |
| received_quantity | DECIMAL(12,2) | DEFAULT 0 | Received quantity |
| unit | VARCHAR(20) | NOT NULL | Unit of measure |
| unit_price | DECIMAL(18,2) | NOT NULL | Agreed unit price |
| vat_rate | DECIMAL(5,2) | DEFAULT 10 | VAT percentage |
| line_total | DECIMAL(18,2) | NOT NULL | Line total before VAT |
| vat_amount | DECIMAL(18,2) | DEFAULT 0 | VAT amount |
| line_grand_total | DECIMAL(18,2) | NOT NULL | Line total including VAT |

#### DeliverySchedule

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| schedule_id | BIGINT | PK, SERIAL | Unique schedule ID |
| FK order_id | BIGINT | NOT NULL, FK REFERENCES PurchaseOrder | Parent order |
| delivery_date | DATE | NOT NULL | Expected delivery date |
| quantity | DECIMAL(12,2) | NOT NULL | Expected quantity |
| received_quantity | DECIMAL(12,2) | DEFAULT 0 | Actually received |
| received_date | DATE | NULL | Actual receipt date |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, received, partial, overdue, cancelled |
| notes | NVARCHAR(300) | NULL | Delivery notes |

#### Supplier

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| supplier_id | BIGINT | PK, SERIAL | Unique supplier ID |
| supplier_code | VARCHAR(20) | NOT NULL, UNIQUE | Supplier code |
| supplier_name | NVARCHAR(200) | NOT NULL | Supplier name |
| tax_code | VARCHAR(20) | NOT NULL | Tax registration code |
| contact_person | NVARCHAR(100) | NOT NULL | Primary contact |
| phone | VARCHAR(20) | NOT NULL | Contact phone |
| email | VARCHAR(100) | NULL | Contact email |
| address | NVARCHAR(300) | NOT NULL | Business address |
| credit_limit | DECIMAL(18,2) | DEFAULT 0 | Maximum credit limit |
| outstanding_debt | DECIMAL(18,2) | DEFAULT 0 | Current outstanding |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| rating | INT | DEFAULT 0 | Supplier rating (1-5) |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### GoodsReceipt

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| receipt_id | BIGINT | PK, SERIAL | Unique receipt ID |
| receipt_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated |
| FK order_id | BIGINT | NOT NULL, FK REFERENCES PurchaseOrder | Source order |
| FK schedule_id | BIGINT | FK REFERENCES DeliverySchedule | Delivery schedule |
| receipt_date | DATE | NOT NULL | Receipt date |
| FK receiver_id | BIGINT | FK REFERENCES User | Receiver |
| total_received | DECIMAL(18,2) | NOT NULL | Total received value |
| quality_status | VARCHAR(20) | DEFAULT 'pending' | pending, passed, failed, conditional |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'received' | received, inspected, accepted, rejected |
| inspection_notes | TEXT | NULL | Quality inspection notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### PurchaseApproval

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| approval_id | BIGINT | PK, SERIAL | Unique approval ID |
| FK request_id | BIGINT | NOT NULL, FK REFERENCES PurchaseRequest | Request being approved |
| FK approver_id | BIGINT | NOT NULL, FK REFERENCES User | Approver |
| approval_level | INT | NOT NULL | Approval level (1, 2, 3) |
| action | VARCHAR(20) | NOT NULL | approved, rejected, delegated, info_requested |
| action_date | TIMESTAMP | NOT NULL | Action timestamp |
| comments | TEXT | NULL | Approval comments/reason |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, actioned, escalated |

#### PurchaseCategory

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| category_id | BIGINT | PK, SERIAL | Unique category ID |
| category_name | NVARCHAR(100) | NOT NULL, UNIQUE | Category name |
| parent_id | BIGINT | NULL, FK REFERENCES PurchaseCategory | Parent category |
| budget_code_prefix | VARCHAR(10) | NOT NULL | Budget code prefix |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| description | NVARCHAR(300) | NULL | Category description |

### 4.3 Indexes

```sql
-- PurchaseRequest indexes
CREATE INDEX idx_pr_number ON PurchaseRequest(request_number);
CREATE INDEX idx_pr_requester ON PurchaseRequest(requester_id);
CREATE INDEX idx_pr_status ON PurchaseRequest(status);
CREATE INDEX idx_pr_category ON PurchaseRequest(category_id);
CREATE INDEX idx_pr_department ON PurchaseRequest(department_id);
CREATE INDEX idx_pr_created ON PurchaseRequest(created_at);
CREATE INDEX idx_pr_amount ON PurchaseRequest(total_amount);

-- PurchaseRequestItem indexes
CREATE INDEX idx_pri_request ON PurchaseRequestItem(request_id);
CREATE INDEX idx_pri_category ON PurchaseRequestItem(category_id);

-- Quotation indexes
CREATE INDEX idx_quotation_number ON Quotation(quotation_number);
CREATE INDEX idx_quotation_request ON Quotation(request_id);
CREATE INDEX idx_quotation_status ON Quotation(status);
CREATE INDEX idx_quotation_validity ON Quotation(validity_date);

-- SupplierQuotation indexes
CREATE INDEX idx_sq_quotation ON SupplierQuotation(quotation_id);
CREATE INDEX idx_sq_supplier ON SupplierQuotation(supplier_id);
CREATE INDEX idx_sq_selected ON SupplierQuotation(quotation_id, is_selected) WHERE is_selected = TRUE;

-- PurchaseOrder indexes
CREATE INDEX idx_po_number ON PurchaseOrder(order_number);
CREATE INDEX idx_po_supplier ON PurchaseOrder(supplier_id);
CREATE INDEX idx_po_status ON PurchaseOrder(status);
CREATE INDEX idx_po_quotation ON PurchaseOrder(quotation_id);
CREATE INDEX idx_po_dates ON PurchaseOrder(order_date);
CREATE INDEX idx_po_remaining ON PurchaseOrder(remaining_amount) WHERE remaining_amount > 0;

-- PurchaseItem indexes
CREATE INDEX idx_pi_order ON PurchaseItem(order_id);

-- DeliverySchedule indexes
CREATE INDEX idx_ds_order ON DeliverySchedule(order_id);
CREATE INDEX idx_ds_date ON DeliverySchedule(delivery_date);
CREATE INDEX idx_ds_status ON DeliverySchedule(status);
CREATE INDEX idx_ds_overdue ON DeliverySchedule(delivery_date, status) WHERE status = 'pending' AND delivery_date < CURRENT_DATE;

-- Supplier indexes
CREATE INDEX idx_supplier_code ON Supplier(supplier_code);
CREATE INDEX idx_supplier_active ON Supplier(is_active) WHERE is_active = TRUE;
CREATE INDEX idx_supplier_debt ON Supplier(outstanding_debt) WHERE outstanding_debt > 0;

-- GoodsReceipt indexes
CREATE INDEX idx_gr_number ON GoodsReceipt(receipt_number);
CREATE INDEX idx_gr_order ON GoodsReceipt(order_id);
CREATE INDEX idx_gr_date ON GoodsReceipt(receipt_date);

-- PurchaseApproval indexes
CREATE INDEX idx_pa_request ON PurchaseApproval(request_id);
CREATE INDEX idx_pa_approver ON PurchaseApproval(approver_id);
CREATE INDEX idx_pa_status ON PurchaseApproval(status);
CREATE INDEX idx_pa_level ON PurchaseApproval(request_id, approval_level);

-- PurchaseCategory indexes
CREATE INDEX idx_pc_parent ON PurchaseCategory(parent_id);
CREATE INDEX idx_pc_active ON PurchaseCategory(is_active) WHERE is_active = TRUE;
```

### 4.4 Summary of Tables

| Table | Primary Key | Foreign Keys | Purpose | Estimated Rows |
|-------|-------------|--------------|---------|----------------|
| PurchaseRequest | request_id | User, Department, Budget, Category | Purchase requests | 5,000/year |
| PurchaseRequestItem | item_id | PurchaseRequest, Category | Request line items | 15,000/year |
| Quotation | quotation_id | PurchaseRequest, User | Quotation headers | 3,000/year |
| SupplierQuotation | sq_id | Quotation, Supplier | Supplier responses | 9,000/year |
| PurchaseOrder | order_id | Supplier, Quotation, User | Purchase orders | 4,000/year |
| PurchaseItem | item_id | PurchaseOrder, SupplierQuotation | Order line items | 12,000/year |
| DeliverySchedule | schedule_id | PurchaseOrder | Delivery tracking | 8,000/year |
| Supplier | supplier_id | - | Supplier master data | 500 |
| GoodsReceipt | receipt_id | PurchaseOrder, DeliverySchedule, User | Goods receipt records | 6,000/year |
| PurchaseApproval | approval_id | PurchaseRequest, User | Approval workflow | 10,000/year |
| PurchaseCategory | category_id | PurchaseCategory (self) | Category hierarchy | 100 |
| PurchaseContract | contract_id | Supplier | Supplier contracts | 200 |

---

## 5. API Specifications

### 5.1 Purchase Request API

#### POST /api/v1/purchasing/requests

Create a purchase request.

**Request Body**:
```json
{
  "title": "Purchase office supplies - Q2 2026",
  "description": "Regular quarterly office supply replenishment",
  "category_id": 5,
  "budget_id": 102,
  "urgency": "normal",
  "justification": "Stock levels below minimum threshold",
  "items": [
    {
      "product_name": "MSI Gaming Mouse",
      "specification": "MSI Clutch GM41, wired, RGB",
      "quantity": 8,
      "unit": "piece",
      "estimated_price": 200000,
      "category_id": 5,
      "notes": "Preferred brand for IT department"
    },
    {
      "product_name": "A4 Paper Ream",
      "specification": "80gsm, white, 500 sheets",
      "quantity": 50,
      "unit": "ream",
      "estimated_price": 65000,
      "category_id": 3,
      "notes": ""
    }
  ]
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "request_id": 4501,
    "request_number": "PR-2026-0045",
    "title": "Purchase office supplies - Q2 2026",
    "category_id": 5,
    "category_name": "Office Supplies",
    "requester_id": 1001,
    "requester_name": "Nguyen Van A",
    "department_id": 3,
    "department_name": "Phong Hanh chinh",
    "total_amount": 4850000,
    "status": "pending",
    "urgency": "normal",
    "items": [
      {
        "item_id": 13501,
        "product_name": "MSI Gaming Mouse",
        "quantity": 8,
        "unit": "piece",
        "estimated_price": 200000,
        "line_total": 1600000
      },
      {
        "item_id": 13502,
        "product_name": "A4 Paper Ream",
        "quantity": 50,
        "unit": "ream",
        "estimated_price": 65000,
        "line_total": 3250000
      }
    ],
    "approval_required": true,
    "approval_level": 1,
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

### 5.2 Quotation API

#### GET /api/v1/purchasing/quotations/{quotation_id}

Get quotation detail with supplier comparison.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "quotation_id": 1205,
    "quotation_number": "QTH0012",
    "request_number": "PR-2026-0045",
    "quotation_date": "2026-04-10",
    "validity_date": "2026-05-10",
    "status": "submitted",
    "suppliers_count": 3,
    "supplier_quotations": [
      {
        "sq_id": 3601,
        "supplier_id": 201,
        "supplier_name": "Cong ty TNHH Thiet bi IT Vina",
        "product_name": "MSI Gaming Mouse",
        "quantity": 8,
        "unit_price": 200000,
        "line_total": 1600000,
        "vat_rate": 10,
        "vat_amount": 160000,
        "grand_total": 1760000,
        "delivery_days": 3,
        "warranty_months": 12,
        "score": 92.5,
        "is_selected": false
      },
      {
        "sq_id": 3602,
        "supplier_id": 202,
        "supplier_name": "Cong ty CP Phan cung Minh Anh",
        "product_name": "MSI Gaming Mouse",
        "quantity": 8,
        "unit_price": 210000,
        "line_total": 1680000,
        "vat_rate": 10,
        "vat_amount": 168000,
        "grand_total": 1848000,
        "delivery_days": 2,
        "warranty_months": 12,
        "score": 88.0,
        "is_selected": false
      },
      {
        "sq_id": 3603,
        "supplier_id": 203,
        "supplier_name": "Cong ty TNHH Thuong mai Phong Vu",
        "product_name": "MSI Gaming Mouse",
        "quantity": 8,
        "unit_price": 215000,
        "line_total": 1720000,
        "vat_rate": 10,
        "vat_amount": 172000,
        "grand_total": 1892000,
        "delivery_days": 5,
        "warranty_months": 6,
        "score": 78.5,
        "is_selected": false
      }
    ],
    "recommended_supplier_id": 201,
    "recommended_reason": "Best price with acceptable delivery time and warranty"
  }
}
```

### 5.3 Purchase Order API

#### POST /api/v1/purchasing/orders

Create purchase order from selected quotation.

**Request Body**:
```json
{
  "quotation_id": 1205,
  "selected_supplier_quotation_ids": [3601],
  "delivery_terms": "DAP - Delivered at Place",
  "payment_terms": "Net 30 days from receipt",
  "delivery_schedule": [
    {
      "delivery_date": "2026-04-20",
      "quantity": 8
    }
  ]
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "order_id": 2201,
    "order_number": "PO-2026-0022",
    "supplier_id": 201,
    "supplier_name": "Cong ty TNHH Thiet bi IT Vina",
    "order_date": "2026-04-14",
    "delivery_terms": "DAP - Delivered at Place",
    "payment_terms": "Net 30 days from receipt",
    "total_amount": 1600000,
    "vat_amount": 160000,
    "grand_total": 1760000,
    "received_amount": 0,
    "paid_amount": 0,
    "remaining_amount": 1760000,
    "status": "approved",
    "items": [
      {
        "item_id": 6601,
        "product_name": "MSI Gaming Mouse",
        "quantity": 8,
        "unit": "piece",
        "unit_price": 200000,
        "line_total": 1600000
      }
    ],
    "delivery_schedule": [
      {
        "schedule_id": 4401,
        "delivery_date": "2026-04-20",
        "quantity": 8,
        "status": "pending"
      }
    ]
  }
}
```

### 5.4 Dashboard API

#### GET /api/v1/purchasing/dashboard

Get purchasing dashboard summary.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "financial_summary": {
      "total_orders": 7862000000,
      "executed": 1100000000,
      "paid": 579000000,
      "remaining": 256000000,
      "supplier_debt": 310000000
    },
    "status_breakdown": {
      "all": 45,
      "created": 8,
      "pending": 12,
      "approved": 24,
      "rejected": 4
    },
    "pending_approvals": [
      {
        "request_id": 4501,
        "request_number": "PR-2026-0045",
        "title": "Purchase office supplies - Q2 2026",
        "total_amount": 4850000,
        "requester": "Nguyen Van A",
        "submitted_at": "2026-04-14T09:00:00Z"
      }
    ],
    "upcoming_deliveries": [
      {
        "order_number": "PO-2026-0022",
        "supplier": "Cong ty TNHH Thiet bi IT Vina",
        "delivery_date": "2026-04-20",
        "items": "MSI Gaming Mouse x8"
      }
    ],
    "spend_trend": [
      {"month": "2026-01", "amount": 450000000},
      {"month": "2026-02", "amount": 380000000},
      {"month": "2026-03", "amount": 520000000},
      {"month": "2026-04", "amount": 275000000}
    ]
  }
}
```

### 5.5 Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Purchase request validation failed",
    "details": [
      {
        "field": "budget_id",
        "message": "Insufficient budget for this request. Available: 3,000,000; Requested: 4,850,000"
      }
    ],
    "request_id": "req-pr123"
  }
}
```

### 5.6 API Summary Table

| Endpoint | Method | Auth Required | Rate Limit | Description |
|----------|--------|---------------|------------|-------------|
| /api/v1/purchasing/requests | POST | Yes | 50/min | Create request |
| /api/v1/purchasing/requests | GET | Yes | 200/min | List requests |
| /api/v1/purchasing/requests/{id} | GET | Yes | 200/min | Get request detail |
| /api/v1/purchasing/requests/{id}/approve | POST | Yes | 30/min | Approve request |
| /api/v1/purchasing/quotations | POST | Yes | 30/min | Create quotation |
| /api/v1/purchasing/quotations/{id} | GET | Yes | 200/min | Get quotation detail |
| /api/v1/purchasing/quotations/{id}/select | POST | Yes | 20/min | Select supplier |
| /api/v1/purchasing/orders | POST | Yes | 30/min | Create purchase order |
| /api/v1/purchasing/orders | GET | Yes | 200/min | List purchase orders |
| /api/v1/purchasing/orders/{id} | GET | Yes | 200/min | Get order detail |
| /api/v1/purchasing/orders/{id}/receive | POST | Yes | 50/min | Record goods receipt |
| /api/v1/purchasing/suppliers | POST | Yes | 20/min | Create supplier |
| /api/v1/purchasing/suppliers | GET | Yes | 200/min | List suppliers |
| /api/v1/purchasing/goods-receipt | POST | Yes | 50/min | Record receipt |
| /api/v1/purchasing/invoice-match | POST | Yes | 30/min | Three-way match |
| /api/v1/purchasing/dashboard | GET | Yes | 100/min | Dashboard summary |
| /api/v1/purchasing/reports/spend | GET | Yes | 30/min | Spend analysis |
| /api/v1/purchasing/approvals/pending | GET | Yes | 100/min | Pending approvals |
| /api/v1/purchasing/categories | GET | Yes | 200/min | List categories |

---

## 6. Permissions Matrix

### 6.1 Role Definitions

| Role | Code | Description | Department |
|------|------|-------------|------------|
| Purchasing Manager | PURCH_MGR | Full purchasing control | Purchasing |
| Purchasing Officer | PURCH_OFFICER | Procurement execution | Purchasing |
| Requester | REQUESTER | Submit requests | All departments |
| Approver | APPROVER | Authorization | Management |
| Receiver | RECEIVER | Goods receipt | Warehouse |
| Finance Officer | FIN_OFFICER | Payment processing | Finance |
| Supplier | SUPPLIER | External vendor | External |
| Director | DIRECTOR | Strategic oversight | Executive |

### 6.2 CRUD Permissions

| Resource | PURCH_MGR | PURCH_OFFICER | REQUESTER | APPROVER | RECEIVER | FIN_OFFICER | SUPPLIER | DIRECTOR |
|----------|-----------|---------------|-----------|----------|----------|-------------|----------|----------|
| PurchaseRequest | CRUD | CRUD | CRUD (own) | R + Approve | R | R | - | R |
| Quotation | CRUD | CRUD | R | R | - | R | R/U (own) | R |
| PurchaseOrder | CRUD | CRUD | R | R + Approve | R | R | R (own) | R |
| SupplierQuotation | CRUD | CRUD | R | R | - | R | CRUD (own) | R |
| GoodsReceipt | CRUD | R | R | R | CRUD | R | R | R |
| Supplier | CRUD | CRUD | R | R | R | R | R (own profile) | R |

### 6.3 Row-Level Security

| Resource | Rule | Description |
|----------|------|-------------|
| PurchaseRequest | Requesters see only own requests | `requester_id = user.id OR user.role IN ('PURCH_MGR', 'APPROVER', 'DIRECTOR')` |
| SupplierQuotation | Suppliers see only own quotations | `supplier_id = user.supplier_id OR user.role NOT IN ('SUPPLIER')` |
| PurchaseOrder | Suppliers see only own orders | `supplier_id = user.supplier_id OR user.role NOT IN ('SUPPLIER')` |
| GoodsReceipt | Receivers see all receipts | No restriction for warehouse staff |

### 6.4 Approval Thresholds

| Amount Range | Required Approver | Escalation |
|--------------|-------------------|------------|
| < 10M VND | Department Manager | 24h -> Purchasing Manager |
| 10M - 100M VND | Purchasing Manager | 48h -> Director |
| > 100M VND | Director | No escalation |
| Emergency | Director (immediate) | Audit trail required |

---

## 7. State Machine

### 7.1 Purchase Request State Diagram

```
                    +---------+
                    |  DRAFT  |
                    +----+----+
                         |
                   (Submit)
                         v
                    +---------+
             +----->| PENDING |<----+
             |      +----+----+     |
             |           |          |
        +----+----+ +----+----+     |
        |         | |         |     |
        v         v v         v     |
    +--------+ +-----------+ +----------+
    |APPROVED| | REJECTED  | |CANCELLED |
    +----+---+ +-----------+ +----------+
         |           ^
         v           |
    +---------+      |
    |CONVERTED|------+
    +---------+
```

### 7.2 Purchase Order State Diagram

```
                    +---------+
                    |  DRAFT  |
                    +----+----+
                         |
                   (Submit)
                         v
                    +---------+
                    | APPROVED|
                    +----+----+
                         |
                   (Send to Supplier)
                         v
                    +---------+
                    | ORDERED |
                    +----+----+
                         |
                   (Goods Received)
                         v
                    +---------+
                    |RECEIVING|
                    +----+----+
                         |
              +----------+----------+
              |                     |
              v                     v
        +----------+         +-----------+
        | COMPLETED|         | CANCELLED |
        +----------+         +-----------+
```

### 7.3 State Transition Tables

#### Purchase Request Transitions

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| DRAFT | PENDING | Submit | Requester | All fields complete |
| PENDING | APPROVED | Approve | Approver | Within authority |
| PENDING | REJECTED | Reject | Approver | Reason required |
| PENDING | PENDING | Request Info | Approver | Additional info needed |
| PENDING | DRAFT | Return | Approver | Changes needed |
| APPROVED | CONVERTED | Convert to PO | Purchasing Officer | Quotation complete |
| APPROVED | CANCELLED | Cancel | Requester/Purchasing Manager | Before PO creation |
| REJECTED | DRAFT | Revise | Requester | Address rejection reason |

#### Purchase Order Transitions

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| DRAFT | APPROVED | Approve | Approver | Within authority |
| APPROVED | ORDERED | Send to supplier | Purchasing Officer | Supplier confirmed |
| ORDERED | RECEIVING | Goods received | Receiver | Partial or full receipt |
| RECEIVING | RECEIVING | Additional receipt | Receiver | Partial delivery |
| RECEIVING | COMPLETED | Fully received and paid | System | All deliveries complete |
| ORDERED | CANCELLED | Cancel | Purchasing Manager | Before any receipt |
| RECEIVING | CANCELLED | Cancel with return | Purchasing Manager | Return goods first |

### 7.4 Approval Timeline Example

```
PR-2026-0045: Purchase office supplies - Q2 2026 (4,850,000 VND)

[2026-04-14 09:00] Nguyen Van A submitted request
[2026-04-14 09:00] Auto-routed to Level 1 (Department Manager) - amount < 10M
[2026-04-14 10:30] Le Thi C (Dept Manager) approved
                    Comment: "Approved, within quarterly budget"
[2026-04-14 10:30] Status changed to APPROVED
[2026-04-14 14:00] Purchasing Officer converted to quotation QTH0012
```

---

## 8. Reports

### 8.1 Report Inventory

| Report | Description | Dimensions | Metrics | Filters | Chart Type |
|--------|-------------|------------|---------|---------|------------|
| Spend Analysis | Purchasing spend over time | Category, supplier, month | Total spend, count, avg order value | Period, category | Line chart |
| Supplier Performance | Supplier evaluation | Supplier, quarter | On-time delivery %, quality rate, cost variance | Period, supplier | Radar chart |
| Approval Timeline | Approval efficiency | Approver, level, department | Avg approval time, rejection rate | Period, department | Bar chart |
| Budget Compliance | Budget vs actual spending | Budget, category, department | Budget, actual, variance, compliance % | Period, department | Stacked bar |
| Order Status | Order pipeline analysis | Status, supplier, month | Count by status, total value by status | Period | Funnel chart |
| Delivery Performance | Delivery tracking | Supplier, month | On-time rate, avg delay, overdue count | Period, supplier | Line + bar |
| Price Comparison | Quotation price analysis | Item, supplier | Price range, avg price, savings | Period, item | Box plot |
| Supplier Debt | Outstanding debt analysis | Supplier, aging | Total debt, aging breakdown, credit utilization | Current | Table |

### 8.2 Report Summary Table

| Report ID | Report Name | Schedule | Auto-Generate | Distribution | Retention |
|-----------|-------------|----------|---------------|--------------|-----------|
| RPT-PR-001 | Spend Analysis | Monthly | Yes | Purchasing Manager, Director | 5 years |
| RPT-PR-002 | Supplier Performance | Quarterly | Yes | Purchasing Manager | 5 years |
| RPT-PR-003 | Approval Timeline | Monthly | Yes | Purchasing Manager | 2 years |
| RPT-PR-004 | Budget Compliance | Monthly | Yes | Finance Manager, Dept Managers | 5 years |
| RPT-PR-005 | Order Status | Weekly | Yes | Purchasing Officer, Manager | 1 year |
| RPT-PR-006 | Delivery Performance | Monthly | Yes | Purchasing Manager | 2 years |
| RPT-PR-007 | Price Comparison | Per Quotation | Yes | Purchasing Officer | 2 years |
| RPT-PR-008 | Supplier Debt | Weekly | Yes | Finance Officer, Manager | 1 year |

---

## 9. Integration Points

### 9.1 External API Integrations

| Integration | Provider | Protocol | Direction | Frequency | Data Exchanged |
|------------|----------|----------|-----------|-----------|----------------|
| Supplier Portal | External suppliers | HTTPS/REST | Bidirectional | Real-time | RFQs, quotations, order status |
| Email Gateway | SMTP/SendGrid | SMTP/API | Outbound | Real-time | RFQ notifications, approval alerts |
| E-Procurement | Government portal | HTTPS/API | Outbound | As needed | Tender submissions |
| Logistics Tracking | Shipping providers | HTTPS/API | Inbound | Real-time | Delivery tracking |

### 9.2 Internal Module Integrations

| Source Module | Target | Data Flow | Trigger | Frequency |
|---------------|--------|-----------|---------|-----------|
| M07 Purchasing | M04 Accounting | Supplier invoice -> AP | Invoice matched | Real-time |
| M07 Purchasing | M09 Inventory | Goods receipt -> Stock increase | Receipt recorded | Real-time |
| M07 Purchasing | M10 Asset Mgmt | Asset purchase -> Asset creation | Asset item received | Real-time |
| M09 Inventory | M07 Purchasing | Low stock alert -> PR auto-create | Stock below minimum | Real-time |
| M04 Accounting | M07 Purchasing | Payment confirmation -> PO update | Payment posted | Real-time |
| M06 Task Mgmt | M07 Purchasing | Approval task -> Request status | Task completed | Real-time |

### 9.3 Webhook Events

| Event | Trigger | Payload | Subscribers |
|-------|---------|---------|-------------|
| purchase_request.submitted | New request submitted | request_id, number, amount, requester | Approver, Purchasing |
| purchase_request.approved | Request approved | request_id, approver, approval_time | Requester, Purchasing |
| purchase_request.rejected | Request rejected | request_id, approver, reason | Requester |
| purchase_order.created | PO created | order_id, number, supplier, total | Supplier, Finance |
| purchase_order.received | Goods received | order_id, receipt_id, quantity | Requester, Finance, Inventory |
| quotation.submitted | Supplier quotation submitted | quotation_id, supplier_id, total | Purchasing Officer |
| approval.escalated | Approval timeout escalated | request_id, approver, elapsed_hours | Next level approver |
| budget.exceeded | Budget threshold exceeded | request_id, budget_id, amount, available | Finance, Manager |

---

## 10. Technical Notes

### 10.1 Performance Requirements

| Metric | Target | Measurement | Notes |
|--------|--------|-------------|-------|
| Request Save | < 500ms | P95 latency | With item validation |
| Quotation Load | < 800ms | P95 latency | With 3 supplier comparisons |
| PO Search | < 500ms | P95 latency | With multiple filters |
| Approval Queue Load | < 300ms | P95 latency | Pending approvals |
| Dashboard Load | < 1.5s | P95 latency | All KPIs and charts |
| Report Generation | < 5s | P95 latency | Standard reports |
| Concurrent Users | 100+ | Simultaneous | Including supplier portal |

### 10.2 Caching Strategy

| Cache Type | Data | TTL | Invalidation |
|------------|------|-----|--------------|
| Redis | Supplier catalog | 1 hour | On supplier update |
| Redis | Category hierarchy | 1 hour | On category change |
| Redis | Dashboard KPIs | 5 minutes | On any PO/request change |
| Redis | Pending approval count | 1 minute | On approval action |
| Redis | Budget availability | 15 minutes | On request approval |
| Application | Approval threshold config | 1 day | On config change |
| Browser | Supplier list | 30 minutes | On supplier update |

### 10.3 Key Files and Components

| File/Component | Path | Purpose |
|----------------|------|---------|
| Purchase Request Service | `src/modules/purchasing/services/request.service.ts` | Request CRUD, validation |
| Quotation Service | `src/modules/purchasing/services/quotation.service.ts` | Quotation management, comparison |
| Purchase Order Service | `src/modules/purchasing/services/order.service.ts` | PO lifecycle management |
| Approval Service | `src/modules/purchasing/services/approval.service.ts` | Approval workflow, routing |
| Supplier Service | `src/modules/purchasing/services/supplier.service.ts` | Supplier management |
| Goods Receipt Service | `src/modules/purchasing/services/receipt.service.ts` | Goods receipt processing |
| Invoice Matching Service | `src/modules/purchasing/services/invoice-match.service.ts` | Three-way matching |
| Budget Check Service | `src/modules/purchasing/services/budget-check.service.ts` | Budget availability validation |
| Report Generator | `src/modules/purchasing/services/report-generator.service.ts` | Purchasing reports |
| Notification Service | `src/modules/purchasing/services/notification.service.ts` | Event notifications |

### 10.4 Localization

| Aspect | Configuration | Notes |
|--------|---------------|-------|
| Language | Vietnamese (primary), English | All labels, messages |
| Currency | VND | All monetary amounts |
| Date Format | DD/MM/YYYY | Vietnamese standard |
| Number Format | 1.234.567.890 (dot separator) | Vietnamese standard |
| VAT Rates | 0%, 5%, 8%, 10% | Per Vietnamese tax law |
| Approval Thresholds | VND amounts | Per company policy |

### 10.5 Mobile Support

| Feature | Mobile Support | Notes |
|---------|----------------|-------|
| Request Creation | Responsive | Simplified form |
| Approval | Responsive | Approve/reject on the go |
| PO Tracking | Responsive | Status and delivery tracking |
| Goods Receipt | Responsive | Barcode scan for receipt |
| Supplier Portal | Responsive | Quotation submission |
| Notifications | Push | Approval alerts, delivery updates |

### 10.6 Security Considerations

| Aspect | Implementation |
|--------|----------------|
| Approval Audit Trail | Immutable log of all approval actions |
| Supplier Data Protection | RBAC for supplier pricing visibility |
| Budget Data Integrity | Transaction-based budget deduction |
| Anti-Fraud | Detect request splitting, unusual patterns |
| Supplier Portal Security | Time-limited access tokens, IP whitelisting |
| Document Security | Encrypted storage for quotations and contracts |
| Segregation of Duties | Requester cannot approve own requests |

### 10.7 Database Considerations

- All monetary values as DECIMAL(18,2)
- Soft deletes for requests and orders (is_deleted flag)
- Partition PurchaseApproval table by quarter
- Materialized views for dashboard KPIs
- Archive closed requests and completed orders after 1 year
- Full-text search on request title and description
- Unique constraints on request_number, order_number, quotation_number

---

*Document Version: 1.0*
*Last Updated: 2026-04-14*
*Author: ERP Design Team*
*Review Status: Draft*
