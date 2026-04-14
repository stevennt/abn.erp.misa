# M08 Sales Module Specification

## 1. Module Overview

### 1.1 Purpose

The Sales Module (M08) is the comprehensive sales management and point-of-sale system within the ABN ERP platform. It handles the complete sales lifecycle from customer management, price lists, discount management, sale order creation, POS transactions, invoicing, returns, and sales reporting. The module supports both retail POS operations and wholesale/B2B sales processes.

The POS interface provides a streamlined checkout experience with product grid display, image-based product selection, customer lookup by phone number, shopping cart management, and quick action shortcuts (F10 for payment, F12 for hold). The module manages sale orders, sale items, customers, price lists, discounts, sale invoices, sale returns, POS sessions, POS transactions, products, product categories, and product images.

### 1.2 Personas

| Persona | Role | Key Responsibilities | Primary Screens | Access Level |
|---------|------|---------------------|-----------------|--------------|
| Sales Manager | Department leadership | Set prices, manage discounts, analyze sales | Dashboard, Reports, Settings | Full |
| Sales Representative | B2B sales | Create sale orders, manage customers, apply discounts | Sale Order, Customer, Price List | Read/Write |
| Cashier | POS operations | Process POS transactions, handle payments | POS Terminal, POS Session | Read/Write |
| Customer Service | Returns and support | Process returns, handle complaints | Sale Return, Customer | Read/Write |
| Finance Officer | Revenue tracking | Reconcile POS sessions, track receivables | POS Transaction, Invoice | Read/Write |
| Warehouse Staff | Fulfillment | Pick and ship sale orders | Sale Order (fulfillment view) | Read |
| Marketing Manager | Pricing strategy | Manage price lists, promotional discounts | Price List, Discount | Read/Write |
| Director | Strategic oversight | Sales performance, revenue trends | Reports, Dashboard | Read-only |

### 1.3 Dependencies

| Dependency | Type | Direction | Description |
|-----------|------|-----------|-------------|
| M04 Accounting | Downstream | Outbound | Sales invoices -> AR entries, revenue recognition |
| M09 Inventory/Warehouse | Upstream/Downstream | Bidirectional | Stock availability check, stock deduction on sale |
| M10 Asset Management | Peer | Inbound | POS hardware tracking |
| M07 Purchasing | Peer | Inbound | Product cost data for margin calculation |
| M06 Task Management | Peer | Inbound | Follow-up tasks from sales |
| M11 Contract Management | Peer | Inbound | B2B customer contract terms |
| Payment Gateway | External | Bidirectional | Card/mobile payment processing |
| E-Invoice System | External | Outbound | E-invoice generation for sales |

### 1.4 Business Rules

1. **Price Priority**: Sale price determined by: (1) Contract price > (2) Promotional price > (3) Price list > (4) Standard price.
2. **Stock Availability**: Sale orders cannot be confirmed for items with insufficient stock unless backorder is allowed.
3. **Discount Limits**: Cashier can apply discounts up to 10% without approval; 10-25% requires supervisor; > 25% requires manager approval.
4. **Customer Identification**: POS transactions can be anonymous or linked to a customer (phone number lookup).
5. **POS Session Control**: Each POS session starts with a float, records all transactions, and ends with cash reconciliation.
6. **Tax Calculation**: VAT calculated based on product tax category; default 10% unless exempt.
7. **Return Policy**: Returns accepted within 30 days with original receipt; partial returns allowed.
8. **Credit Sales**: B2B customers with approved credit limits can purchase on credit.
9. **Price Change Audit**: All price changes logged with timestamp, user, and reason.
10. **Hold Order**: POS transactions can be held and resumed using F12 shortcut.
11. **Quick Payment**: F10 shortcut initiates payment processing from cart screen.
12. **Image Display**: Product grid shows product images for visual selection in POS mode.

---

## 2. Screen Inventory

### 2.1 Screen Routes and Purposes

| Route | Purpose | Personas | Priority |
|-------|---------|----------|----------|
| `/sales/dashboard` | Sales dashboard with KPIs and trends | All personas | Critical |
| `/sales/pos` | Point of sale terminal | Cashier | Critical |
| `/sales/orders` | Sale order list and management | Sales Representative | Critical |
| `/sales/orders/{id}` | Sale order detail | Sales Representative | High |
| `/sales/customers` | Customer management | Sales Representative, Cashier | High |
| `/sales/customers/{id}` | Customer profile with history | Sales Representative | High |
| `/sales/price-lists` | Price list management | Sales Manager, Marketing Manager | High |
| `/sales/discounts` | Discount and promotion management | Sales Manager, Marketing Manager | High |
| `/sales/invoices` | Sale invoice management | Sales Representative, Finance Officer | High |
| `/sales/returns` | Sale return processing | Customer Service | High |
| `/sales/pos/sessions` | POS session management | Cashier, Finance Officer | High |
| `/sales/reports` | Sales reports and analytics | Sales Manager, Director | Critical |

### 2.2 UI Components by Screen

#### /sales/pos (POS Terminal)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Product Grid | Card Grid | Product cards with images and prices | Product with images |
| Search Bar | Input | Search by name, barcode, SKU | Product search |
| Category Tabs | Tab Bar | Filter products by category | ProductCategory |
| Shopping Cart | List/Table | Current transaction items | Cart session |
| Customer Lookup | Input | Find customer by phone | Customer search by phone |
| Subtotal Display | Summary | Cart subtotal | Cart calculation |
| Discount Input | Input | Apply discount code/percentage | Discount rules |
| VAT Display | Summary | VAT amount | Tax calculation |
| Grand Total Display | Summary | Total including VAT | Cart + VAT |
| Quick Action Buttons | Button Bar | F10 Pay, F12 Hold, Clear, Return | N/A |
| Payment Modal | Modal | Payment method selection, amount entry | POS transaction |
| Receipt Preview | Preview | Receipt preview before printing | POS transaction |
| Session Info Bar | Info Bar | Session ID, cashier, start time | POS_Session |

#### /sales/orders (Sale Order List)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Search Bar | Input | Search by order number, customer | SaleOrder search |
| Filter Panel | Multi-select | Filter by status, customer, date | SaleOrder metadata |
| Order Grid | Data Table | Order list with key columns | SaleOrder |
| Status Badges | Badge | Order status indicator | SaleOrder.status |
| Quick Actions | Dropdown | View, edit, cancel, duplicate | Order actions |
| Create Order Button | Action Button | New sale order creation | Order form |
| Export Button | Action Button | Export to Excel | Order data |

#### /sales/dashboard

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Today Sales KPI | Metric Card | Today's total sales | POS transactions today |
| Orders Today KPI | Metric Card | Number of orders today | SaleOrder count today |
| Avg Order Value | Metric Card | Average order value | Sales / count |
| Active POS Sessions | Metric Card | Currently open POS sessions | POS_Session WHERE active |
| Sales Trend Chart | Line Chart | Daily sales trend | Historical sales data |
| Top Products Chart | Bar Chart | Top selling products | SaleItem aggregation |
| Top Customers Chart | Bar Chart | Top customers by revenue | Customer sales analysis |
| Revenue by Category | Pie Chart | Sales by product category | Category sales |

### 2.3 User Interactions

1. **POS Transaction**: Cashier scans/selects products from grid (with images) -> products added to cart -> enters customer phone number to lookup -> applies discount if any -> presses F10 for payment -> selects payment method (cash, card, transfer) -> enters amount -> completes transaction -> receipt printed.
2. **Hold and Resume**: Cashier presses F12 to hold current transaction -> serves next customer -> recalls held transaction by customer name or order reference -> continues checkout.
3. **B2B Sale Order**: Sales Rep creates sale order -> selects customer -> adds items -> system checks stock -> applies customer price list -> calculates VAT -> submits order -> warehouse picks and ships -> invoice generated.
4. **Return Processing**: Customer Service receives return request -> looks up original sale by receipt/invoice -> selects items to return -> processes refund or credit -> updates inventory -> records return reason.

### 2.4 Data Displayed

- POS Cart: Product image, name, quantity, unit price, line total, remove button
- Sale Order: Order number, customer, date, items, subtotal, VAT, grand total, status, payment status
- Customer Profile: Name, phone, email, address, credit limit, total purchases, last purchase date
- POS Session: Session ID, cashier, start time, opening float, total sales, closing balance, variance

---

## 3. Feature Specifications

### 3.1 POS Terminal

**Description**: Point-of-sale interface optimized for fast checkout with product grid, barcode scanning, customer lookup, and quick action shortcuts.

**User Stories**:
- As a Cashier, I want to quickly add products to the cart using the visual grid so that checkout is fast.
- As a Cashier, I want to look up customers by phone number so that I can apply their price list and track purchases.
- As a Cashier, I want keyboard shortcuts (F10 pay, F12 hold) so that I can work efficiently without mouse interaction.

**Acceptance Criteria**:
1. Product grid displays products with images, names, and prices in a responsive grid layout.
2. Barcode scanning automatically adds product to cart.
3. Customer lookup by phone number auto-fills customer details and applies their price list.
4. F10 shortcut opens payment modal; F12 holds current transaction.
5. Cart supports quantity adjustment, item removal, and discount application.
6. Payment modal supports multiple payment methods: cash, card, bank transfer, e-wallet.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-001 | Product | Must be active, available for sale | "Product is not available" |
| VR-002 | Quantity | Must be > 0, <= available stock | "Insufficient stock" |
| VR-003 | Customer | Valid phone number if provided | "Customer not found" |
| VR-004 | Discount | Within cashier's authority level | "Discount requires supervisor approval" |
| VR-005 | Payment Amount | Must equal or exceed grand total | "Payment amount insufficient" |
| VR-006 | Session | Must have active POS session | "No active POS session" |
| VR-007 | Cart | Cannot complete empty cart | "Cart is empty" |

**Edge Cases**:
- Price change during transaction: Use price at transaction start, not current price.
- Stock runs out during transaction: Warn but allow completion with backorder flag.
- Network failure during payment: Transaction queued for retry.
- Multiple payment methods: Split payment across cash, card, transfer.

### 3.2 Sale Order Management

**Description**: Create and manage B2B and complex sale orders with multi-item orders, pricing, delivery scheduling, and fulfillment tracking.

**User Stories**:
- As a Sales Rep, I want to create sale orders with multiple items so that I can process customer orders.
- As a Sales Rep, I want to apply customer-specific price lists so that pricing is accurate.
- As a Sales Manager, I want to track order fulfillment status so that I can ensure timely delivery.

**Acceptance Criteria**:
1. Sale order form with: customer, items (product, quantity, price), delivery address, payment terms, notes.
2. Auto-apply customer price list based on customer profile.
3. Stock availability check with backorder option.
4. Order status tracking: Draft, Confirmed, Picking, Shipped, Delivered, Invoiced, Cancelled.
5. Order duplication for repeat customers.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-010 | Customer | Required, active customer | "Valid customer is required" |
| VR-011 | Items | At least one item | "Add at least one item" |
| VR-012 | Stock | Sufficient stock or backorder allowed | "Insufficient stock for all items" |
| VR-013 | Price | Must be >= minimum price | "Price below minimum" |
| VR-014 | Credit | Within customer credit limit | "Exceeds customer credit limit" |
| VR-015 | Delivery Date | Must be after order date | "Delivery date must be in the future" |

**Edge Cases**:
- Partial fulfillment: Ship available items, backorder the rest.
- Price override: Manager override with reason logging.
- Order amendment: Before shipment, with stock re-check.

### 3.3 Price List Management

**Description**: Manage multiple price lists for different customer groups, regions, or promotional periods.

**User Stories**:
- As a Marketing Manager, I want to create price lists for different customer groups so that pricing is segmented.
- As a Sales Manager, I want to set promotional prices for limited periods so that I can run promotions.
- As a Sales Rep, I want the system to auto-apply the correct price list so that pricing is always accurate.

**Acceptance Criteria**:
1. Price list creation with name, validity period, customer group, currency.
2. Price list items: product, price, minimum quantity.
3. Price priority: Contract > Promotional > Price List > Standard.
4. Price list activation/deactivation with effective dates.
5. Bulk price update via spreadsheet import.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-020 | Price | Must be > 0 | "Price must be positive" |
| VR-021 | Validity | End date after start date | "Validity period invalid" |
| VR-022 | Product | Must be active product | "Product is not active" |
| VR-023 | Customer Group | Valid group | "Invalid customer group" |
| VR-024 | Overlap | No overlapping price lists for same group | "Overlapping price list exists" |

**Edge Cases**:
- Price list expiry: Auto-revert to standard price.
- Multiple matching price lists: Apply most specific (customer > group > general).
- Currency conversion: Price list in foreign currency with daily rate.

### 3.4 POS Session Management

**Description**: Manage POS sessions with opening float, transaction recording, and closing reconciliation.

**User Stories**:
- As a Cashier, I want to start a POS session with opening float so that I have change for transactions.
- As a Finance Officer, I want to reconcile POS sessions at end of day so that cash is accounted for.
- As a Sales Manager, I want to see session summaries so that I can track cashier performance.

**Acceptance Criteria**:
1. Session creation with: cashier, POS terminal, opening float, start time.
2. All transactions linked to active session.
3. Session close with: expected cash, actual cash, variance, explanation.
4. Session report: transaction count, total sales, payment method breakdown, variance.
5. Cannot start new session while previous session is open for same cashier/terminal.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-030 | Opening Float | Must be >= 0 | "Opening float cannot be negative" |
| VR-031 | Active Session | No existing open session | "Close previous session first" |
| VR-032 | Closing | Actual cash must be recorded | "Record actual cash amount" |
| VR-033 | Variance | Variance > threshold requires explanation | "Variance explanation required" |

**Edge Cases**:
- Session transfer: Cashier change mid-shift with float handover.
- Voided transaction: Void before session close with supervisor approval.
- Session adjustment: Post-close adjustment with manager approval.

### 3.5 Sale Returns

**Description**: Process product returns with refund or credit, inventory adjustment, and return reason tracking.

**User Stories**:
- As Customer Service, I want to process returns by looking up the original sale so that refunds are accurate.
- As Customer Service, I want to record return reasons so that we can track quality issues.
- As a Finance Officer, I want return data so that revenue adjustments are recorded.

**Acceptance Criteria**:
1. Return creation by original sale/invoice lookup.
2. Select items to return with quantity.
3. Refund method: original payment method, store credit, or exchange.
4. Inventory adjustment on return receipt.
5. Return reason categorization: defective, wrong item, customer change of mind, etc.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-040 | Original Sale | Must exist, within return window | "Sale not found or outside return window" |
| VR-041 | Return Qty | Cannot exceed original quantity | "Return quantity exceeds purchased" |
| VR-042 | Reason | Required | "Return reason is required" |
| VR-043 | Refund Amount | Cannot exceed original payment | "Refund exceeds original payment" |

**Edge Cases**:
- Partial return: Only some items or partial quantity returned.
- Exchange: Return and new sale in single transaction.
- Return without receipt: Manager approval required.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+-------------------+       +-------------------+       +-------------------+
|    Customer       |       |   SaleOrder       |       |   SaleItem        |
+-------------------+       +-------------------+       +-------------------+
| PK customer_id    |<----->| PK order_id       |<----->| PK item_id        |
| customer_code     |       | order_number      |       | FK order_id       |
| full_name         |       | FK customer_id    |       | FK product_id     |
| phone             |       | order_date        |       | quantity          |
| email             |       | delivery_date     |       | unit_price        |
| address           |       | subtotal          |       | discount          |
| credit_limit      |       | vat_amount        |       | vat_rate          |
| FK price_list_id  |       | grand_total       |       | line_total        |
| status            |       | status            |       | status            |
+-------------------+       | payment_status    |       +-------------------+
        |                   | FK created_by     |
        |                   +-------------------+       +-------------------+
        v                                                    |
+-------------------+       +-------------------+            v
|   PriceList       |       | POS_Session       |       +-------------------+
+-------------------+       +-------------------+       |    Product        |
| PK price_list_id  |       | PK session_id     |       +-------------------+
| price_list_name   |       | session_number    |       | PK product_id     |
| valid_from        |       | FK cashier_id     |       | product_code      |
| valid_to          |       | FK terminal_id    |       | product_name      |
| customer_group    |       | opening_float     |       | FK category_id    |
| currency          |       | start_time        |       | unit_price        |
| is_active         |       | end_time          |       | cost_price        |
+-------------------+       | total_sales       |       | FK image_id       |
                            | closing_balance   |       | is_active         |
                            | variance          |       | tax_category      |
                            | status            |       +-------------------+
                            +-------------------+
                                    |
                                    v
                          +-------------------+
                          | POS_Transaction   |
                          +-------------------+
                          | PK transaction_id |
                          | FK session_id     |
                          | transaction_time  |
                          | subtotal          |
                          | vat_amount        |
                          | grand_total       |
                          | payment_method    |
                          | amount_paid       |
                          | change_amount     |
                          | status            |
                          +-------------------+

+-------------------+       +-------------------+       +-------------------+
|   Discount        |       |   SaleInvoice     |       |   SaleReturn      |
+-------------------+       +-------------------+       +-------------------+
| PK discount_id    |       | PK invoice_id     |       | PK return_id      |
| discount_code     |       | invoice_number    |       | return_number     |
| discount_type     |       | FK order_id       |       | FK order_id       |
| value             |       | invoice_date      |       | return_date       |
| valid_from        |       | grand_total       |       | total_return      |
| valid_to          |       | status            |       | refund_method     |
| usage_limit       |       +-------------------+       | status            |
| usage_count       |                                   | reason            |
| is_active         |                                   +-------------------+
+-------------------+

+-------------------+       +-------------------+
| ProductCategory   |       |  ProductImage     |
+-------------------+       +-------------------+
| PK category_id    |       | PK image_id       |
| category_name     |       | product_id        |
| parent_id         |       | image_url         |
| is_active         |       | is_primary        |
+-------------------+       | display_order     |
                            +-------------------+
```

### 4.2 Table Schemas

#### SaleOrder

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| order_id | BIGINT | PK, SERIAL | Unique order ID |
| order_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated (SO-2026-0001) |
| FK customer_id | BIGINT | NOT NULL, FK REFERENCES Customer | Customer |
| order_date | DATE | NOT NULL | Order date |
| delivery_date | DATE | NULL | Expected delivery date |
| delivery_address | NVARCHAR(300) | NULL | Delivery address |
| subtotal | DECIMAL(18,2) | NOT NULL | Subtotal before tax |
| discount_amount | DECIMAL(18,2) | DEFAULT 0 | Total discount |
| vat_amount | DECIMAL(18,2) | NOT NULL | VAT amount |
| grand_total | DECIMAL(18,2) | NOT NULL | Total including VAT |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, confirmed, picking, shipped, delivered, invoiced, cancelled |
| payment_status | VARCHAR(20) | NOT NULL, DEFAULT 'unpaid' | unpaid, partial, paid, credited |
| notes | TEXT | NULL | Order notes |
| FK created_by | BIGINT | FK REFERENCES User | Order creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### SaleItem

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| item_id | BIGINT | PK, SERIAL | Unique item ID |
| FK order_id | BIGINT | NOT NULL, FK REFERENCES SaleOrder | Parent order |
| FK product_id | BIGINT | NOT NULL, FK REFERENCES Product | Product |
| quantity | DECIMAL(12,2) | NOT NULL | Ordered quantity |
| unit_price | DECIMAL(18,2) | NOT NULL | Sale unit price |
| discount | DECIMAL(18,2) | DEFAULT 0 | Line discount |
| vat_rate | DECIMAL(5,2) | DEFAULT 10 | VAT percentage |
| vat_amount | DECIMAL(18,2) | DEFAULT 0 | VAT amount |
| line_total | DECIMAL(18,2) | NOT NULL | Line total |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, fulfilled, returned, cancelled |

#### Customer

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| customer_id | BIGINT | PK, SERIAL | Unique customer ID |
| customer_code | VARCHAR(20) | NOT NULL, UNIQUE | Customer code |
| customer_type | VARCHAR(20) | NOT NULL | retail, wholesale, corporate |
| full_name | NVARCHAR(100) | NOT NULL | Customer name |
| phone | VARCHAR(20) | NOT NULL | Phone number |
| email | VARCHAR(100) | NULL | Email address |
| address | NVARCHAR(300) | NULL | Address |
| tax_code | VARCHAR(20) | NULL | Tax code (for B2B) |
| FK price_list_id | BIGINT | FK REFERENCES PriceList | Default price list |
| credit_limit | DECIMAL(18,2) | DEFAULT 0 | Credit limit (B2B) |
| outstanding_balance | DECIMAL(18,2) | DEFAULT 0 | Outstanding balance |
| total_purchases | DECIMAL(18,2) | DEFAULT 0 | Lifetime purchases |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active, inactive, blacklisted |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### PriceList

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| price_list_id | BIGINT | PK, SERIAL | Unique price list ID |
| price_list_name | NVARCHAR(100) | NOT NULL | Price list name |
| valid_from | DATE | NOT NULL | Validity start |
| valid_to | DATE | NULL | Validity end (NULL = indefinite) |
| customer_group | VARCHAR(30) | NULL | Target customer group |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Currency |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| description | NVARCHAR(300) | NULL | Description |

#### Discount

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| discount_id | BIGINT | PK, SERIAL | Unique discount ID |
| discount_code | VARCHAR(30) | NOT NULL, UNIQUE | Discount code |
| discount_type | VARCHAR(20) | NOT NULL | percentage, fixed_amount, bogo |
| value | DECIMAL(10,2) | NOT NULL | Discount value |
| valid_from | DATE | NOT NULL | Validity start |
| valid_to | DATE | NULL | Validity end |
| min_order_value | DECIMAL(18,2) | DEFAULT 0 | Minimum order value |
| max_discount | DECIMAL(18,2) | NULL | Maximum discount amount |
| usage_limit | INT | NULL | Maximum uses (NULL = unlimited) |
| usage_count | INT | DEFAULT 0 | Current usage count |
| applicable_products | JSONB | NULL | Product IDs or categories |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

#### POS_Session

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| session_id | BIGINT | PK, SERIAL | Unique session ID |
| session_number | VARCHAR(20) | NOT NULL, UNIQUE | Session number |
| FK cashier_id | BIGINT | NOT NULL, FK REFERENCES User | Cashier |
| FK terminal_id | BIGINT | FK REFERENCES POSTerminal | POS terminal |
| opening_float | DECIMAL(18,2) | NOT NULL, DEFAULT 0 | Opening float |
| start_time | TIMESTAMP | NOT NULL | Session start |
| end_time | TIMESTAMP | NULL | Session end |
| total_transactions | INT | DEFAULT 0 | Transaction count |
| total_sales | DECIMAL(18,2) | DEFAULT 0 | Total sales amount |
| closing_balance | DECIMAL(18,2) | NULL | Closing cash balance |
| variance | DECIMAL(18,2) | NULL | Expected vs actual variance |
| variance_explanation | TEXT | NULL | Variance reason |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'open' | open, closed, reconciled |
| FK closed_by | BIGINT | NULL, FK REFERENCES User | User who closed |

#### POS_Transaction

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| transaction_id | BIGINT | PK, SERIAL | Unique transaction ID |
| FK session_id | BIGINT | NOT NULL, FK REFERENCES POS_Session | POS session |
| transaction_number | VARCHAR(20) | NOT NULL, UNIQUE | Transaction number |
| transaction_time | TIMESTAMP | NOT NULL | Transaction timestamp |
| FK customer_id | BIGINT | NULL, FK REFERENCES Customer | Customer (if any) |
| subtotal | DECIMAL(18,2) | NOT NULL | Cart subtotal |
| discount_amount | DECIMAL(18,2) | DEFAULT 0 | Discount applied |
| vat_amount | DECIMAL(18,2) | NOT NULL | VAT amount |
| grand_total | DECIMAL(18,2) | NOT NULL | Total including VAT |
| payment_method | VARCHAR(20) | NOT NULL | cash, card, transfer, ewallet |
| amount_paid | DECIMAL(18,2) | NOT NULL | Amount paid |
| change_amount | DECIMAL(18,2) | DEFAULT 0 | Change returned |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'completed' | completed, voided, refunded |
| receipt_number | VARCHAR(20) | NULL | Receipt number |
| notes | NVARCHAR(300) | NULL | Transaction notes |

#### SaleInvoice

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| invoice_id | BIGINT | PK, SERIAL | Unique invoice ID |
| invoice_number | VARCHAR(30) | NOT NULL, UNIQUE | Invoice number |
| FK order_id | BIGINT | FK REFERENCES SaleOrder | Source order |
| invoice_date | DATE | NOT NULL | Invoice date |
| subtotal | DECIMAL(18,2) | NOT NULL | Before tax |
| vat_amount | DECIMAL(18,2) | NOT NULL | VAT amount |
| grand_total | DECIMAL(18,2) | NOT NULL | Total including VAT |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'issued' | issued, paid, cancelled |
| e_invoice_code | VARCHAR(50) | NULL | E-invoice code |
| qr_code | VARCHAR(200) | NULL | QR code data |

#### SaleReturn

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| return_id | BIGINT | PK, SERIAL | Unique return ID |
| return_number | VARCHAR(20) | NOT NULL, UNIQUE | Return number |
| FK order_id | BIGINT | NOT NULL, FK REFERENCES SaleOrder | Source order |
| return_date | DATE | NOT NULL | Return date |
| total_return | DECIMAL(18,2) | NOT NULL | Total return value |
| refund_method | VARCHAR(20) | NOT NULL | cash, credit, exchange |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'processed' | processed, refunded, exchanged |
| reason | VARCHAR(200) | NOT NULL | Return reason |
| notes | TEXT | NULL | Additional notes |
| FK processed_by | BIGINT | FK REFERENCES User | Processor |

#### Product

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| product_id | BIGINT | PK, SERIAL | Unique product ID |
| product_code | VARCHAR(30) | NOT NULL, UNIQUE | Product/SKU code |
| product_name | NVARCHAR(200) | NOT NULL | Product name |
| FK category_id | BIGINT | FK REFERENCES ProductCategory | Product category |
| unit_price | DECIMAL(18,2) | NOT NULL | Standard unit price |
| cost_price | DECIMAL(18,2) | NOT NULL | Cost price |
| barcode | VARCHAR(50) | NULL | Barcode |
| unit | VARCHAR(20) | NOT NULL | Unit of measure |
| tax_category | VARCHAR(20) | DEFAULT 'standard' | Tax category |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| description | TEXT | NULL | Product description |

#### ProductImage

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| image_id | BIGINT | PK, SERIAL | Unique image ID |
| FK product_id | BIGINT | NOT NULL, FK REFERENCES Product | Product |
| image_url | VARCHAR(500) | NOT NULL | Image storage URL |
| is_primary | BOOLEAN | NOT NULL, DEFAULT FALSE | Primary image flag |
| display_order | INT | DEFAULT 0 | Display order |
| alt_text | NVARCHAR(100) | NULL | Alt text for accessibility |

#### ProductCategory

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| category_id | BIGINT | PK, SERIAL | Unique category ID |
| category_name | NVARCHAR(100) | NOT NULL | Category name |
| parent_id | BIGINT | NULL, FK REFERENCES ProductCategory | Parent category |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

### 4.3 Indexes

```sql
-- SaleOrder indexes
CREATE INDEX idx_so_number ON SaleOrder(order_number);
CREATE INDEX idx_so_customer ON SaleOrder(customer_id);
CREATE INDEX idx_so_date ON SaleOrder(order_date);
CREATE INDEX idx_so_status ON SaleOrder(status);
CREATE INDEX idx_so_payment ON SaleOrder(payment_status);
CREATE INDEX idx_so_created ON SaleOrder(created_by);

-- SaleItem indexes
CREATE INDEX idx_si_order ON SaleItem(order_id);
CREATE INDEX idx_si_product ON SaleItem(product_id);

-- Customer indexes
CREATE INDEX idx_cust_code ON Customer(customer_code);
CREATE INDEX idx_cust_phone ON Customer(phone);
CREATE INDEX idx_cust_type ON Customer(customer_type);
CREATE INDEX idx_cust_status ON Customer(status);
CREATE INDEX idx_cust_price_list ON Customer(price_list_id);

-- POS_Session indexes
CREATE INDEX idx_pos_session_number ON POS_Session(session_number);
CREATE INDEX idx_pos_cashier ON POS_Session(cashier_id);
CREATE INDEX idx_pos_status ON POS_Session(status);
CREATE INDEX idx_pos_active ON POS_Session(status) WHERE status = 'open';

-- POS_Transaction indexes
CREATE INDEX idx_pos_tx_session ON POS_Transaction(session_id);
CREATE INDEX idx_pos_tx_number ON POS_Transaction(transaction_number);
CREATE INDEX idx_pos_tx_time ON POS_Transaction(transaction_time);
CREATE INDEX idx_pos_tx_customer ON POS_Transaction(customer_id);
CREATE INDEX idx_pos_tx_method ON POS_Transaction(payment_method);

-- Product indexes
CREATE INDEX idx_prod_code ON Product(product_code);
CREATE INDEX idx_prod_category ON Product(category_id);
CREATE INDEX idx_prod_barcode ON Product(barcode);
CREATE INDEX idx_prod_active ON Product(is_active) WHERE is_active = TRUE;
CREATE INDEX idx_prod_name_search ON Product USING GIN(to_tsvector('simple', product_name));

-- SaleInvoice indexes
CREATE INDEX idx_inv_number ON SaleInvoice(invoice_number);
CREATE INDEX idx_inv_order ON SaleInvoice(order_id);
CREATE INDEX idx_inv_date ON SaleInvoice(invoice_date);
CREATE INDEX idx_inv_status ON SaleInvoice(status);

-- SaleReturn indexes
CREATE INDEX idx_ret_number ON SaleReturn(return_number);
CREATE INDEX idx_ret_order ON SaleReturn(order_id);
CREATE INDEX idx_ret_date ON SaleReturn(return_date);
CREATE INDEX idx_ret_reason ON SaleReturn(reason);
```

### 4.4 Summary of Tables

| Table | Primary Key | Foreign Keys | Purpose | Estimated Rows |
|-------|-------------|--------------|---------|----------------|
| SaleOrder | order_id | Customer, User | Sale order headers | 50,000/year |
| SaleItem | item_id | SaleOrder, Product | Sale order line items | 150,000/year |
| Customer | customer_id | PriceList | Customer master data | 10,000 |
| PriceList | price_list_id | - | Price list definitions | 50 |
| Discount | discount_id | - | Discount/promotion rules | 100 |
| POS_Session | session_id | User, POSTerminal | POS session management | 5,000/year |
| POS_Transaction | transaction_id | POS_Session, Customer | POS transaction records | 500,000/year |
| SaleInvoice | invoice_id | SaleOrder | Sales invoices | 50,000/year |
| SaleReturn | return_id | SaleOrder, User | Sale returns | 5,000/year |
| Product | product_id | ProductCategory | Product catalog | 5,000 |
| ProductImage | image_id | Product | Product images | 10,000 |
| ProductCategory | category_id | ProductCategory (self) | Category hierarchy | 100 |

---

## 5. API Specifications

### 5.1 POS Transaction API

#### POST /api/v1/sales/pos/transactions

Process a POS transaction.

**Request Body**:
```json
{
  "session_id": 1201,
  "customer_phone": "+84912345678",
  "items": [
    {
      "product_id": 3001,
      "quantity": 2,
      "unit_price": 850000
    },
    {
      "product_id": 3002,
      "quantity": 1,
      "unit_price": 200000
    }
  ],
  "discount": {
    "type": "percentage",
    "value": 5
  },
  "payment": {
    "method": "cash",
    "amount_paid": 1700000
  }
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "transaction_id": 50001,
    "transaction_number": "TXN-2026-050001",
    "receipt_number": "RCP-050001",
    "session_id": 1201,
    "customer_id": 4501,
    "customer_name": "Nguyen Van A",
    "transaction_time": "2026-04-14T10:30:00Z",
    "items": [
      {"product_id": 3001, "product_name": "Ao thun nam", "quantity": 2, "unit_price": 850000, "line_total": 1700000},
      {"product_id": 3002, "product_name": "Quan jean", "quantity": 1, "unit_price": 200000, "line_total": 200000}
    ],
    "subtotal": 1900000,
    "discount_amount": 95000,
    "vat_amount": 180500,
    "grand_total": 1700000,
    "payment": {
      "method": "cash",
      "amount_paid": 1700000,
      "change_amount": 0
    },
    "status": "completed"
  }
}
```

### 5.2 Sale Order API

#### POST /api/v1/sales/orders

Create a sale order.

**Request Body**:
```json
{
  "customer_id": 4501,
  "delivery_date": "2026-04-20",
  "delivery_address": "123 Nguyen Trai, Thanh Xuan, Ha Noi",
  "items": [
    {
      "product_id": 3001,
      "quantity": 10,
      "unit_price": 850000
    }
  ],
  "payment_terms": "Net 30",
  "notes": "Deliver during business hours"
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "order_id": 5001,
    "order_number": "SO-2026-0050",
    "customer_id": 4501,
    "customer_name": "Nguyen Van A",
    "order_date": "2026-04-14",
    "delivery_date": "2026-04-20",
    "subtotal": 8500000,
    "discount_amount": 0,
    "vat_amount": 850000,
    "grand_total": 9350000,
    "status": "confirmed",
    "payment_status": "unpaid",
    "items": [
      {
        "item_id": 15001,
        "product_id": 3001,
        "product_name": "Ao thun nam",
        "quantity": 10,
        "unit_price": 850000,
        "line_total": 8500000
      }
    ],
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

### 5.3 Customer API

#### GET /api/v1/sales/customers/lookup

Look up customer by phone number.

**Query Parameters**:
- `phone` (string, required): Customer phone number

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "customer_id": 4501,
    "customer_code": "CUS-004501",
    "customer_type": "retail",
    "full_name": "Nguyen Van A",
    "phone": "+84912345678",
    "email": "nguyenvana@email.com",
    "address": "123 Nguyen Trai, Thanh Xuan, Ha Noi",
    "price_list_id": 1,
    "price_list_name": "Retail Standard",
    "credit_limit": 0,
    "outstanding_balance": 0,
    "total_purchases": 15000000,
    "status": "active"
  }
}
```

### 5.4 Dashboard API

#### GET /api/v1/sales/dashboard

Get sales dashboard summary.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "today_sales": 15000000,
    "orders_today": 25,
    "avg_order_value": 600000,
    "active_sessions": 3,
    "sales_trend": [
      {"date": "2026-04-08", "amount": 12000000},
      {"date": "2026-04-09", "amount": 14000000},
      {"date": "2026-04-10", "amount": 11000000},
      {"date": "2026-04-11", "amount": 13000000},
      {"date": "2026-04-12", "amount": 16000000},
      {"date": "2026-04-13", "amount": 14500000},
      {"date": "2026-04-14", "amount": 15000000}
    ],
    "top_products": [
      {"product_id": 3001, "product_name": "Ao thun nam", "quantity_sold": 150, "revenue": 127500000},
      {"product_id": 3002, "product_name": "Quan jean", "quantity_sold": 80, "revenue": 16000000}
    ],
    "top_customers": [
      {"customer_id": 4501, "customer_name": "Nguyen Van A", "total_purchases": 15000000}
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
    "message": "POS transaction validation failed",
    "details": [
      {
        "field": "items[0].quantity",
        "message": "Insufficient stock. Available: 1, Requested: 2"
      }
    ],
    "request_id": "req-pos123"
  }
}
```

### 5.6 API Summary Table

| Endpoint | Method | Auth Required | Rate Limit | Description |
|----------|--------|---------------|------------|-------------|
| /api/v1/sales/pos/transactions | POST | Yes | 200/min | Process POS transaction |
| /api/v1/sales/pos/transactions/{id}/void | POST | Yes | 30/min | Void transaction |
| /api/v1/sales/pos/sessions | POST | Yes | 10/min | Start POS session |
| /api/v1/sales/pos/sessions/{id}/close | POST | Yes | 10/min | Close POS session |
| /api/v1/sales/orders | POST | Yes | 100/min | Create sale order |
| /api/v1/sales/orders | GET | Yes | 200/min | List sale orders |
| /api/v1/sales/orders/{id} | GET | Yes | 200/min | Get order detail |
| /api/v1/sales/orders/{id}/status | PUT | Yes | 50/min | Update order status |
| /api/v1/sales/customers | POST | Yes | 50/min | Create customer |
| /api/v1/sales/customers | GET | Yes | 200/min | List customers |
| /api/v1/sales/customers/lookup | GET | Yes | 200/min | Lookup by phone |
| /api/v1/sales/price-lists | POST | Yes | 20/min | Create price list |
| /api/v1/sales/price-lists | GET | Yes | 200/min | List price lists |
| /api/v1/sales/discounts | POST | Yes | 20/min | Create discount |
| /api/v1/sales/discounts | GET | Yes | 200/min | List discounts |
| /api/v1/sales/returns | POST | Yes | 50/min | Process return |
| /api/v1/sales/returns | GET | Yes | 200/min | List returns |
| /api/v1/sales/invoices | GET | Yes | 200/min | List invoices |
| /api/v1/sales/dashboard | GET | Yes | 100/min | Dashboard summary |
| /api/v1/sales/reports | GET | Yes | 30/min | Sales reports |

---

## 6. Permissions Matrix

### 6.1 Role Definitions

| Role | Code | Description | Department |
|------|------|-------------|------------|
| Sales Manager | SALES_MGR | Full sales control | Sales |
| Sales Representative | SALES_REP | B2B sales | Sales |
| Cashier | CASHIER | POS operations | Sales |
| Customer Service | CS | Returns and support | Sales |
| Finance Officer | FIN_OFFICER | Revenue tracking | Finance |
| Marketing Manager | MKT_MGR | Pricing and promotions | Marketing |
| Director | DIRECTOR | Strategic oversight | Executive |

### 6.2 CRUD Permissions

| Resource | SALES_MGR | SALES_REP | CASHIER | CS | FIN_OFFICER | MKT_MGR | DIRECTOR |
|----------|-----------|-----------|---------|-----|-------------|---------|----------|
| SaleOrder | CRUD | CRUD | R | R | R | R | R |
| SaleItem | CRUD | CRUD | R | R | R | R | R |
| POS_Session | CRUD | - | CRUD | R | R | - | R |
| POS_Transaction | CRUD | - | CRUD | R | R | - | R |
| Customer | CRUD | CRUD | R (lookup) | CRUD | R | R | R |
| PriceList | CRUD | R | R | - | R | CRUD | R |
| Discount | CRUD | R | R (apply) | R | R | CRUD | R |
| SaleInvoice | CRUD | CRUD | R | R | CRUD | - | R |
| SaleReturn | CRUD | R | R | CRUD | CRUD | - | R |
| Product | CRUD | R | R | R | R | R | R |

### 6.3 Row-Level Security

| Resource | Rule | Description |
|----------|------|-------------|
| SaleOrder | Sales reps see only own orders | `created_by = user.id OR user.role IN ('SALES_MGR', 'DIRECTOR')` |
| POS_Session | Cashiers see only own sessions | `cashier_id = user.id OR user.role IN ('SALES_MGR', 'FIN_OFFICER')` |
| Customer | All sales staff see all customers | No restriction for sales team |

---

## 7. State Machine

### 7.1 Sale Order State Diagram

```
                    +---------+
                    |  DRAFT  |
                    +----+----+
                         |
                   (Confirm)
                         v
                    +---------+
                    |CONFIRMED|
                    +----+----+
                         |
                   (Pick)
                         v
                    +---------+
                    | PICKING |
                    +----+----+
                         |
                   (Ship)
                         v
                    +---------+
                    | SHIPPED |
                    +----+----+
                         |
                   (Deliver)
                         v
                    +---------+
                    |DELIVERED|
                    +----+----+
                         |
                   (Invoice)
                         v
                    +---------+
                    | INVOICED|
                    +----+----+
```

### 7.2 POS Session State Diagram

```
                    +------+
                    | OPEN |
                    +--+---+
                       |
                 (Close Session)
                       v
                  +--------+
                  | CLOSED |
                  +---+----+
                      |
                (Reconcile)
                      v
                 +-----------+
                 | RECONCILED|
                 +-----------+
```

### 7.3 State Transition Tables

#### Sale Order Transitions

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| DRAFT | CONFIRMED | Confirm order | Sales Rep, Sales Manager | Stock available |
| CONFIRMED | PICKING | Send to warehouse | Sales Rep | Order confirmed |
| PICKING | SHIPPED | Mark as shipped | Warehouse | Items picked and packed |
| SHIPPED | DELIVERED | Confirm delivery | Warehouse/Customer | Delivery confirmed |
| DELIVERED | INVOICED | Generate invoice | Sales Rep, Finance | Delivery confirmed |
| CONFIRMED | CANCELLED | Cancel | Sales Manager | Before picking |
| ANY | CANCELLED | Emergency cancel | Sales Manager | Justification required |

#### POS Transaction States

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| IN_PROGRESS | COMPLETED | Payment received | Cashier | Payment = total |
| IN_PROGRESS | HELD | F12 pressed | Cashier | Cart has items |
| HELD | IN_PROGRESS | Recalled | Cashier | Same session |
| COMPLETED | VOIDED | Void | Supervisor | Before session close |
| COMPLETED | REFUNDED | Return processed | Customer Service | Within return window |

### 7.4 Approval Workflow

#### Discount Approval

```
Cashier applies discount
     |
     v
Check discount amount vs authority
     /      |         \
   <=10%   10-25%     >25%
     |       |          |
     v       v          v
  Auto    Supervisor  Manager
  Apply   Approval    Approval
     |       |          |
     v       v          v
  Applied Applied   Applied
             |        or
             v       Rejected
          Applied
```

---

## 8. Reports

### 8.1 Report Inventory

| Report | Description | Dimensions | Metrics | Filters | Chart Type |
|--------|-------------|------------|---------|---------|------------|
| Sales Summary | Daily/monthly sales summary | Date, channel, cashier | Revenue, transaction count, avg value | Period, channel | Line chart |
| Product Performance | Product sales analysis | Product, category, period | Quantity sold, revenue, margin | Period, category | Bar chart |
| Customer Analysis | Customer purchasing patterns | Customer, segment, period | Total purchases, frequency, avg order | Period, segment | Scatter plot |
| POS Reconciliation | POS session reconciliation | Session, cashier, date | Expected vs actual, variance | Date, cashier | Table |
| Discount Effectiveness | Discount ROI analysis | Discount, period | Revenue lift, discount cost, net impact | Period, discount | Bar + line |
| Return Analysis | Return rate and reasons | Product, reason, period | Return count, return rate, refund amount | Period, product | Pie chart |
| Payment Method Analysis | Payment method breakdown | Method, period | Transaction count, total amount | Period | Pie chart |
| Sales by Channel | POS vs B2B vs online | Channel, period | Revenue, count, avg value | Period | Stacked bar |

### 8.2 Report Summary Table

| Report ID | Report Name | Schedule | Auto-Generate | Distribution | Retention |
|-----------|-------------|----------|---------------|--------------|-----------|
| RPT-SL-001 | Sales Summary | Daily | Yes | Sales Manager, Director | 5 years |
| RPT-SL-002 | Product Performance | Weekly | Yes | Sales Manager, Marketing | 2 years |
| RPT-SL-003 | Customer Analysis | Monthly | Yes | Sales Manager | 3 years |
| RPT-SL-004 | POS Reconciliation | Daily | Yes | Finance Officer, Manager | 3 years |
| RPT-SL-005 | Discount Effectiveness | Per Campaign | Yes | Marketing Manager | 2 years |
| RPT-SL-006 | Return Analysis | Weekly | Yes | Customer Service, Manager | 2 years |
| RPT-SL-007 | Payment Method Analysis | Monthly | Yes | Finance Officer | 2 years |
| RPT-SL-008 | Sales by Channel | Monthly | Yes | Sales Manager, Director | 3 years |

---

## 9. Integration Points

### 9.1 External API Integrations

| Integration | Provider | Protocol | Direction | Frequency | Data Exchanged |
|------------|----------|----------|-----------|-----------|----------------|
| Payment Gateway | VNPAY, MoMo, ZaloPay | HTTPS/API | Bidirectional | Real-time | Payment processing, confirmations |
| Barcode Scanner | Hardware | USB/Bluetooth | Inbound | Real-time | Barcode data |
| Receipt Printer | Hardware | USB/Network | Outbound | Real-time | Receipt print commands |
| E-Invoice | Tax Authority | HTTPS/API | Outbound | Real-time | E-invoice generation |
| Card Terminal | Bank POS | TCP/IP | Bidirectional | Real-time | Card payment processing |

### 9.2 Internal Module Integrations

| Source Module | Target | Data Flow | Trigger | Frequency |
|---------------|--------|-----------|---------|-----------|
| M08 Sales | M04 Accounting | Sale invoice -> AR + Revenue | Invoice issued | Real-time |
| M08 Sales | M09 Inventory | Sale -> Stock deduction | Order confirmed | Real-time |
| M09 Inventory | M08 Sales | Stock level -> Product availability | Stock change | Real-time |
| M04 Accounting | M08 Sales | Payment confirmation -> Order update | Payment posted | Real-time |
| M07 Purchasing | M08 Sales | Product cost -> Margin calculation | Cost update | Real-time |
| M11 Contract Mgmt | M08 Sales | Contract pricing -> Customer price list | Contract active | Real-time |

### 9.3 Webhook Events

| Event | Trigger | Payload | Subscribers |
|-------|---------|---------|-------------|
| pos.transaction_completed | POS transaction completed | transaction_id, amount, payment_method, cashier | Accounting, Inventory |
| sale_order.confirmed | Sale order confirmed | order_id, customer_id, total, items | Inventory, Warehouse |
| sale_order.shipped | Order shipped | order_id, items, delivery_date | Customer, Accounting |
| sale_return.processed | Return processed | return_id, order_id, refund_amount, reason | Accounting, Inventory |
| pos.session_closed | POS session closed | session_id, cashier, total_sales, variance | Finance, Manager |
| customer.created | New customer created | customer_id, name, phone, type | Marketing, Sales |

---

## 10. Technical Notes

### 10.1 Performance Requirements

| Metric | Target | Measurement | Notes |
|--------|--------|-------------|-------|
| POS Transaction | < 1s | P95 latency | From scan to receipt |
| Product Search | < 200ms | P95 latency | Barcode or text search |
| Customer Lookup | < 300ms | P95 latency | By phone number |
| Sale Order Save | < 500ms | P95 latency | With stock check |
| Dashboard Load | < 1.5s | P95 latency | All KPIs |
| Concurrent POS | 20+ terminals | Simultaneous | Each with independent sessions |
| Peak Throughput | 100 tx/min | Sustained | High-traffic periods |

### 10.2 Caching Strategy

| Cache Type | Data | TTL | Invalidation |
|------------|------|-----|--------------|
| Redis | Product catalog | 5 minutes | On product update |
| Redis | Price lists | 15 minutes | On price list change |
| Redis | Active discounts | 5 minutes | On discount change |
| Redis | POS session data | 1 minute | On transaction |
| Redis | Customer lookup | 10 minutes | On customer update |
| Application | Category tree | 1 hour | On category change |
| Browser | Product images | 1 day | On image update |
| CDN | Product images | 1 hour | On image update |

### 10.3 Key Files and Components

| File/Component | Path | Purpose |
|----------------|------|---------|
| POS Service | `src/modules/sales/services/pos.service.ts` | POS transaction processing |
| Sale Order Service | `src/modules/sales/services/sale-order.service.ts` | Sale order lifecycle |
| Customer Service | `src/modules/sales/services/customer.service.ts` | Customer management |
| Price List Service | `src/modules/sales/services/price-list.service.ts` | Price list management |
| Discount Service | `src/modules/sales/services/discount.service.ts` | Discount rules and application |
| Return Service | `src/modules/sales/services/return.service.ts` | Return processing |
| POS Session Service | `src/modules/sales/services/pos-session.service.ts` | Session management |
| Report Generator | `src/modules/sales/services/report-generator.service.ts` | Sales reports |
| Payment Gateway Service | `src/modules/sales/services/payment-gateway.service.ts` | Payment processing |
| E-Invoice Service | `src/modules/sales/services/e-invoice.service.ts` | E-invoice generation |

### 10.4 Localization

| Aspect | Configuration | Notes |
|--------|---------------|-------|
| Language | Vietnamese (primary), English | All labels, messages, receipts |
| Currency | VND | All monetary amounts |
| Date Format | DD/MM/YYYY | Vietnamese standard |
| Number Format | 1.234.567.890 (dot separator) | Vietnamese standard |
| Receipt Format | Vietnamese | Per local requirements |
| Tax Rates | 0%, 5%, 8%, 10% | Per Vietnamese tax law |

### 10.5 Mobile Support

| Feature | Mobile Support | Notes |
|---------|----------------|-------|
| POS Terminal | Tablet-optimized | Touch-friendly interface |
| Customer Lookup | Responsive | Search and view |
| Sale Order | Responsive | Create and track orders |
| Dashboard | Responsive | View KPIs and charts |
| Reports | Responsive | View-only, export to PDF |
| Notifications | Push | Low stock, large order alerts |

### 10.6 Security Considerations

| Aspect | Implementation |
|--------|----------------|
| POS Session Integrity | Cashier cannot void own transactions without approval |
| Price Override Audit | All price overrides logged with user and reason |
| Cash Reconciliation | Variance tracking with explanation requirement |
| Customer PII | Encrypted storage for phone, email, address |
| Payment Data | PCI-DSS compliance for card data |
| Discount Fraud Prevention | Authority levels with approval workflow |
| Session Timeout | Auto-logout after 15 minutes of inactivity |

### 10.7 Database Considerations

- POS transactions partitioned by month for performance
- Soft deletes for orders and customers (is_deleted flag)
- Materialized views for dashboard metrics
- Archive completed POS sessions after 1 year
- Full-text search on product name and customer name
- Unique constraints on order_number, transaction_number, invoice_number
- Stock deduction happens in same transaction as order creation for consistency

---

*Document Version: 1.0*
*Last Updated: 2026-04-14*
*Author: ERP Design Team*
*Review Status: Draft*
