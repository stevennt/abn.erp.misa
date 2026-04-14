# M37 - Store & POS (Cua hang va Ban le)

**Category:** Business
**Priority:** P3
**Reference Images:** assets/image_34.png
**Dependencies:** M08 (Sales), M09 (Inventory & Warehouse), M36 (Promotion Management), M34 (Payment Gateway)
**Generated:** April 14, 2026

---

## 1. Module Overview

The Store & POS module provides a complete point-of-sale and retail management solution for physical store operations within the ABN ERP system. It combines a modern POS interface for fast checkout with comprehensive retail management features including inventory tracking, customer management, price list configuration, discount application, product returns, and multi-store support. The module is designed for high-speed transaction processing with keyboard shortcuts, barcode scanning, and real-time inventory synchronization.

The POS interface (image_34) presents a split-screen layout optimized for retail operations. The left panel displays the shopping cart with line items including product names (men's shirts, women's shirts, combo sets), quantities, unit prices, and line totals. Customer lookup is available via phone number input for loyalty tracking and receipt printing. The current cart total is 1,700,000 VND. The right panel displays a visual product grid with product images, names, and prices ranging from 280,000 to 699,000 VND, organized by categories such as "Women's Fashion" and "New Arrivals." The bottom action bar includes Cancel, Save (F10), and Collect Payment (F12) buttons for quick keyboard-driven operations. The top bar provides price list selector, sales staff selector, sales channel dropdown, and quick search (F3). The module integrates with the Inventory module (M09) for real-time stock level tracking, the Sales module (M08) for order synchronization, the Promotion Management module (M36) for discount application, and the Payment Gateway module (M34) for multi-method payment processing.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| Store Manager | Manages store operations, POS sessions, staff scheduling, and reporting | Full access to store data, POS configuration, and reports |
| Cashier | Processes transactions, handles payments, manages returns | Read/write access to POS operations within their session |
| Sales Associate | Assists customers, looks up products, applies promotions | Read access to products, write access to cart during active session |
| Inventory Clerk | Monitors stock levels, processes stock adjustments at store level | Read/write access to store inventory |
| Regional Manager | Oversees multiple stores, compares performance across locations | Read access to all store data and consolidated reports |
| System Administrator | Configures POS hardware, price lists, store settings | Full system access, read-only transaction data |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M08 - Sales | POS transactions sync to sales module; customer data shared |
| M09 - Inventory & Warehouse | Real-time stock tracking, product availability, stock adjustments |
| M36 - Promotion Management | Discount application at POS, promotion tracking |
| M34 - Payment Gateway | Payment processing (cards, e-wallets, QR, cash) |
| M01 - Authentication | User access control, session management |
| M41 - E-Banking | Cash deposit tracking, end-of-day bank deposits |

### Key Business Rules

1. Each POS session must be opened by an authorized cashier with a starting cash float amount; sessions cannot process transactions until opened.
2. Product prices are determined by the active price list; multiple price lists can be configured by customer type, store location, or promotional period.
3. Barcode scanning must resolve to a unique product variant; ambiguous scans require manual selection from matching products.
4. Customer lookup by phone number is the primary method for loyalty tracking; new customers can be created directly from the POS interface.
5. Returns must reference the original transaction receipt number; returns without receipt require manager approval and are subject to store return policy.
6. Discounts from the Promotion Management module are automatically applied when conditions are met; manual discount override requires manager authorization.
7. End-of-day reconciliation compares expected cash (based on transactions) against actual cash count; discrepancies are logged and reported.
8. Inventory levels are updated in real-time as transactions are processed; low stock alerts trigger when product quantity falls below configured threshold.
9. POS transactions are immutable after completion; corrections must be made through return transactions or adjustment entries.
10. Multi-store support includes consolidated reporting, inter-store transfers, and store-level performance comparison.

---

## 2. Screen Inventory

### 2.1 Screen: POS Terminal

**Route:** `/pos/terminal` | **Purpose:** Main point-of-sale interface for processing retail transactions.

| UI Component | Description |
|--------------|-------------|
| Shopping Cart (Left Panel) | Line items with product name, quantity, unit price, line total; customer phone lookup; running total (1,700,000) |
| Product Grid (Right Panel) | Visual product cards with images, names, prices (280k-699k); category tabs |
| Top Bar | Price list selector, sales staff selector, sales channel dropdown, search (F3) |
| Bottom Action Bar | Cancel, Save (F10), Collect Payment (F12) buttons |
| Category Navigation | Category tabs: Women's Fashion, Men's Fashion, Accessories, New Arrivals |
| Quick Keys | F1-F12 function key shortcuts for common operations |

**User Interactions:**
- Click products on grid or scan barcode to add to cart
- Adjust quantities using + / - buttons or direct input
- Look up customer by phone number
- Select price list and sales channel
- Apply discounts and promotions
- Process payment via F12 or click Collect Payment
- Save cart as draft via F10
- Cancel transaction

**Data Displayed:**
- Cart items with quantities and prices
- Running subtotal, discount, tax, and grand total
- Customer information if selected
- Available products with images and prices
- Category organization

### 2.2 Screen: Product Lookup & Grid

**Route:** `/pos/products` | **Purpose:** Browse and search the product catalog with visual grid display.

| UI Component | Description |
|--------------|-------------|
| Product Grid | Image cards with name, price, stock indicator |
| Search Bar | Quick search by name, SKU, barcode |
| Category Filters | Category tree for filtering |
| Price Display | Current price based on selected price list |
| Stock Indicator | In stock / Low stock / Out of stock badges |

**User Interactions:**
- Search products by name, SKU, or barcode
- Filter by category
- Click product to add to cart
- View product detail including variants and images

**Data Displayed:**
- Product images and names
- Current prices per price list
- Stock availability status
- Product variants and options

### 2.3 Screen: Customer Lookup

**Route:** `/pos/customers` | **Purpose:** Find or create customer records for loyalty tracking and receipt printing.

| UI Component | Description |
|--------------|-------------|
| Phone Search | Input field for phone number lookup |
| Customer Result | Display name, phone, loyalty points, purchase history |
| New Customer Form | Quick form: name, phone, email, birthday |
| Loyalty Info | Points balance, tier, available rewards |

**User Interactions:**
- Enter phone number to search
- Select customer from results
- Create new customer if not found
- View loyalty points and rewards

**Data Displayed:**
- Customer name and contact information
- Loyalty program details
- Recent purchase history
- Available rewards and discounts

### 2.4 Screen: Payment Processing

**Route:** `/pos/payment` | **Purpose:** Process payment with multiple payment methods.

| UI Component | Description |
|--------------|-------------|
| Amount Due | Grand total display |
| Payment Method Selector | Cash, Card (Domestic/Intl), QR, MOMO, Viettel Money, ZaloPay, Transfer |
| Cash Payment | Amount tendered input, change calculation |
| Card Payment | Terminal integration status, card swipe prompt |
| Split Payment | Multiple payment methods for single transaction |
| Receipt Options | Print, email, SMS receipt |

**User Interactions:**
- Select payment method
- Enter cash amount and view change due
- Process card payment via terminal
- Split payment across methods
- Choose receipt delivery method

**Data Displayed:**
- Total amount due
- Payment method availability
- Change calculation for cash payments
- Payment processing status

### 2.5 Screen: POS Session Management

**Route:** `/pos/sessions` | **Purpose:** Open, manage, and close POS sessions.

| UI Component | Description |
|--------------|-------------|
| Session List | Session ID, Cashier, Date, Status, Transaction Count, Total Sales |
| Open Session Form | Cashier selection, starting cash float amount |
| Session Summary | Transactions, cash collected, card payments, refunds, discrepancies |
| Close Session | End-of-day reconciliation form |

**User Interactions:**
- Open new session with cash float
- View active session status
- Close session with reconciliation
- View session history

**Data Displayed:**
- Session details and status
- Transaction counts and totals
- Cash reconciliation results
- Discrepancy amounts

### 2.6 Screen: Transaction History

**Route:** `/pos/transactions` | **Purpose:** Browse and manage POS transaction history.

| UI Component | Description |
|--------------|-------------|
| Transaction List | Receipt number, date, cashier, total, payment method, status |
| Transaction Detail | Line items, customer, payment breakdown, discounts applied |
| Return Action | Process return against original transaction |
| Receipt Reprint | Print duplicate receipt |

**User Interactions:**
- Search transactions by receipt number, date, or customer
- View transaction details
- Process returns
- Reprint receipts

**Data Displayed:**
- Transaction line items and totals
- Payment method breakdown
- Applied discounts and promotions
- Customer information

### 2.7 Screen: Product Return Processing

**Route:** `/pos/returns` | **Purpose:** Process product returns with receipt lookup and refund handling.

| UI Component | Description |
|--------------|-------------|
| Receipt Lookup | Receipt number input, date range search |
| Return Items | Select items from original transaction for return |
| Refund Calculation | Refund amount based on original prices |
| Refund Method | Cash refund, store credit, exchange |
| Manager Approval | Approval prompt for returns without receipt |

**User Interactions:**
- Look up original receipt
- Select items to return
- Choose refund method
- Process manager approval if required
- Complete return transaction

**Data Displayed:**
- Original transaction details
- Returnable items and quantities
- Refund amount calculation
- Approval status

### 2.8 Screen: Price List Management

**Route:** `/pos/price-lists` | **Purpose:** Configure and manage price lists for different customer types and periods.

| UI Component | Description |
|--------------|-------------|
| Price List List | Name, type, status, product count, effective dates |
| Price Configuration | Product-level price setting per list |
| Customer Type Mapping | Map price lists to customer types |
| Effective Date Range | Start and end dates for price validity |

**User Interactions:**
- Create and configure price lists
- Set product prices per list
- Map price lists to customer types
- Activate/deactivate price lists

**Data Displayed:**
- Price list configuration
- Product prices per list
- Customer type mappings
- Effective date ranges

### 2.9 Screen: Store Inventory Dashboard

**Route:** `/pos/inventory` | **Purpose:** Monitor store-level inventory with stock levels and alerts.

| UI Component | Description |
|--------------|-------------|
| Stock Level Table | Product, current stock, reserved, available, reorder point |
| Low Stock Alerts | Products below reorder point with suggested order quantity |
| Stock Adjustment Form | Adjust stock for damage, loss, or correction |
| Barcode/Label Printing | Print product barcodes and shelf labels |

**User Interactions:**
- View current stock levels
- Process stock adjustments
- Print barcodes and labels
- View low stock alerts

**Data Displayed:**
- Current stock quantities
- Reserved vs. available stock
- Reorder point and suggested quantities
- Adjustment history

---

## 3. Feature Specifications

### 3.1 Feature: POS Transaction Processing
**Description:** Process retail transactions with product selection, cart management, customer lookup, and payment processing.

**User Stories:**
- As a Cashier, I want to quickly add products to cart by scanning or clicking, so that I can serve customers efficiently.
- As a Cashier, I want to look up customers by phone number, so that I can apply loyalty benefits and print receipts with customer info.

**Acceptance Criteria:**
- Given a product is scanned or clicked, when it is added to cart, then the item appears with correct price from the active price list.
- Given a customer phone number is entered, when a match is found, then the customer information is displayed and loyalty benefits are applied.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Product stock | Must have available stock | Product is out of stock |
| Quantity | Must be positive integer or decimal | Quantity must be greater than zero |
| Customer phone | Valid 10-digit Vietnamese format | Please enter a valid 10-digit phone number |
| Payment amount | Must equal or exceed total | Payment amount must cover the total |

**Edge Cases:**
- Scanning a product with multiple variants opens variant selection modal
- Network outage during transaction processing caches transaction locally and syncs when restored
- Price list changes mid-transaction do not affect items already in cart

### 3.2 Feature: Price List Management
**Description:** Configure multiple price lists for different customer types, promotional periods, and store locations.

**User Stories:**
- As a Store Manager, I want to configure different prices for wholesale and retail customers, so that pricing reflects customer type.
- As a Regional Manager, I want to set store-specific prices, so that regional market differences are reflected.

**Acceptance Criteria:**
- Given a price list is active, when a customer of the mapped type checks out, then prices from the correct price list are applied.
- Given a price list expires, when the end date is reached, then the price list is automatically deactivated.

**Edge Cases:**
- Products without a price in the active price list fall back to the default price list
- Price list conflicts (customer qualifies for multiple) are resolved by best-price rule
- Price changes during active sessions only apply to new transactions

### 3.3 Feature: POS Session Management
**Description:** Open, manage, and close POS sessions with cash float tracking and end-of-day reconciliation.

**User Stories:**
- As a Cashier, I want to open a session with a starting cash float, so that I can begin processing transactions.
- As a Store Manager, I want to reconcile sessions at end of day, so that cash discrepancies are detected.

**Acceptance Criteria:**
- Given a session is opened, when the cashier enters the starting cash amount, then the session is active and ready for transactions.
- Given a session is being closed, when the actual cash count is entered, then the discrepancy (expected vs. actual) is calculated and logged.

**Edge Cases:**
- Sessions with discrepancies over threshold require manager sign-off
- Unclosed sessions auto-close at configured time with notification to manager
- Multiple sessions per store are supported for shift changes

### 3.4 Feature: Product Return Processing
**Description:** Process product returns with original receipt lookup, refund calculation, and inventory adjustment.

**User Stories:**
- As a Cashier, I want to process returns against original receipts, so that returns are accurately tracked.
- As a Store Manager, I want to approve returns without receipt, so that exceptional returns are controlled.

**Acceptance Criteria:**
- Given a receipt number is entered, when the return form loads, then all items from the original transaction are displayed for selection.
- Given a return is processed, when it is completed, then inventory levels are adjusted and refund is recorded.

**Edge Cases:**
- Partial returns (some items from receipt) are supported with remaining quantity tracking
- Returns beyond the return policy window require manager approval
- Refunded items are returned to inventory in "returned" condition status

### 3.5 Feature: Barcode Scanning and Product Resolution
**Description:** Resolve scanned barcodes to specific products and variants with disambiguation support.

**User Stories:**
- As a Cashier, I want products to be added automatically when I scan the barcode, so that checkout is fast.
- As a Store Manager, I want barcode scanning to handle multiple variants, so that the correct product is always selected.

**Acceptance Criteria:**
- Given a barcode is scanned, when it matches a unique product, then the product is added to cart automatically.
- Given a barcode matches multiple products, when the scan resolves, then a selection modal is displayed for the cashier to choose.

**Edge Cases:**
- Damaged barcodes can be manually entered by product code
- New products without barcodes can be added by search
- Barcode-to-product mapping is maintained in the product master data

### 3.6 Feature: Real-Time Inventory Synchronization
**Description:** Update inventory levels in real-time as POS transactions are processed with low stock alerting.

**User Stories:**
- As an Inventory Clerk, I want stock levels to update automatically with each sale, so that inventory accuracy is maintained.
- As a Store Manager, I want low stock alerts, so that I can reorder before products run out.

**Acceptance Criteria:**
- Given a POS transaction is completed, when the items are processed, then stock levels are decremented in real-time.
- Given a product stock falls below reorder point, when the check runs, then a low stock alert is generated.

**Edge Cases:**
- Concurrent transactions on the same product use atomic stock deduction to prevent overselling
- Reserved stock (in cart but not yet paid) is tracked separately from available stock
- Stock sync with warehouse may have brief delay during high-volume periods

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|   POS_Session  |       |POS_Transaction |       |   POS_Item     |
+----------------+       +----------------+       +----------------+
| id (PK)        |       | id (PK)        |       | id (PK)        |
| store_id(FK)   |<----->| session_id(FK) |<----->| transaction_id |
| cashier_id(FK) |       | receipt_number |       | product_id(FK) |
| opened_at      |       | total          |       | quantity       |
| closed_at      |       | discount       |       | unit_price     |
| starting_cash  |       | tax            |       | line_total     |
| closing_cash   |       | payment_json   |       +----------------+
| status         |       | customer_id(FK)|
+----------------+       | status         |
        |                | created_at     |
        v                +----------------+
+----------------+               |
|  POS_Customer  |               v
+----------------+       +----------------+
| id (PK)        |       |  POS_PriceList |
| phone          |       +----------------+
| name           |       | id (PK)        |
| email          |       | name           |
| loyalty_points |       | type           |
| tier           |       | status         |
+----------------+       | effective_from |
                         | effective_to   |
                         +----------------+
        |
        v
+----------------+       +----------------+       +----------------+
| POS_Discount   |       | POS_Return     |       |   POS_Payment  |
+----------------+       +----------------+       +----------------+
| id (PK)        |       | id (PK)        |       | id (PK)        |
| transaction_id |       | transaction_id |       | transaction_id |
| type           |       | reason         |       | method         |
| value          |       | refund_amount  |       | amount         |
| source         |       | status         |       | reference      |
+----------------+       | approved_by    |       | status         |
                         +----------------+       +----------------+
        |
        v
+----------------+       +----------------+       +----------------+
|    Product     |       | ProductVariant |       | ProductImage   |
+----------------+       +----------------+       +----------------+
| id (PK)        |       | id (PK)        |       | id (PK)        |
| name           |       | product_id(FK) |       | product_id(FK) |
| sku            |       | variant_code   |       | image_url      |
| category_id(FK)|       | barcode        |       | is_primary     |
| base_price     |       | price_adjust   |       | sort_order     |
| status         |       | stock_qty      |       +----------------+
+----------------+       +----------------+
        |
        v
+----------------+
|    Barcode     |
+----------------+
| id (PK)        |
| product_id(FK) |
| code           |
| type           |
+----------------+
```

### 4.2 Table Schemas

#### Table: pos_session
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| store_id | BIGINT | FK -> store.id, NOT NULL | Store location |
| cashier_id | BIGINT | FK -> users.id, NOT NULL | Assigned cashier |
| opened_at | TIMESTAMP | NOT NULL | Session open timestamp |
| closed_at | TIMESTAMP | NULL | Session close timestamp |
| starting_cash | DECIMAL(18,2) | NOT NULL | Starting cash float |
| closing_cash | DECIMAL(18,2) | NULL | Actual cash count at close |
| expected_cash | DECIMAL(18,2) | NULL | Expected cash based on transactions |
| discrepancy | DECIMAL(18,2) | NULL | Cash discrepancy amount |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'open' | Status: open, closed, voided |
| notes | TEXT | NULL | Session notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_pos_session_store ON pos_session(store_id);
CREATE INDEX idx_pos_session_cashier ON pos_session(cashier_id);
CREATE INDEX idx_pos_session_status ON pos_session(status);
CREATE INDEX idx_pos_session_opened ON pos_session(opened_at);
```

#### Table: pos_transaction
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| session_id | BIGINT | FK -> pos_session.id, NOT NULL | POS session |
| receipt_number | VARCHAR(50) | NOT NULL, UNIQUE | Receipt number |
| customer_id | BIGINT | FK -> pos_customer.id | Customer |
| subtotal | DECIMAL(18,2) | NOT NULL | Pre-discount subtotal |
| discount | DECIMAL(18,2) | DEFAULT 0 | Total discount |
| tax | DECIMAL(18,2) | DEFAULT 0 | Total tax |
| total | DECIMAL(18,2) | NOT NULL | Grand total |
| payment_json | JSON | NULL | Payment breakdown by method |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'completed' | Status: completed, voided, returned |
| cashier_id | BIGINT | FK -> users.id | Transaction cashier |
| channel | VARCHAR(30) | DEFAULT 'pos' | Sales channel |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_pos_txn_receipt ON pos_transaction(receipt_number);
CREATE INDEX idx_pos_txn_session ON pos_transaction(session_id);
CREATE INDEX idx_pos_txn_customer ON pos_transaction(customer_id);
CREATE INDEX idx_pos_txn_date ON pos_transaction(created_at);
CREATE INDEX idx_pos_txn_status ON pos_transaction(status);
```

#### Table: pos_item
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| transaction_id | BIGINT | FK -> pos_transaction.id, NOT NULL | Parent transaction |
| product_id | BIGINT | FK -> product.id, NOT NULL | Product |
| variant_id | BIGINT | FK -> product_variant.id | Product variant |
| quantity | DECIMAL(10,2) | NOT NULL | Quantity |
| unit_price | DECIMAL(18,2) | NOT NULL | Unit price at time of sale |
| discount | DECIMAL(5,2) | DEFAULT 0 | Line discount percentage |
| line_total | DECIMAL(18,2) | NOT NULL | Line total |
| sort_order | INT | DEFAULT 0 | Display order |

**Indexes:**
```sql
CREATE INDEX idx_pos_item_transaction ON pos_item(transaction_id);
CREATE INDEX idx_pos_item_product ON pos_item(product_id);
```

#### Table: pos_customer
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| phone | VARCHAR(20) | NOT NULL, UNIQUE | Phone number |
| name | VARCHAR(200) | NOT NULL | Customer name |
| email | VARCHAR(200) | NULL | Email address |
| birthday | DATE | NULL | Date of birth |
| loyalty_points | INT | DEFAULT 0 | Loyalty points balance |
| tier | VARCHAR(30) | DEFAULT 'standard' | Loyalty tier |
| total_purchases | DECIMAL(18,2) | DEFAULT 0 | Lifetime purchase amount |
| visit_count | INT | DEFAULT 0 | Total store visits |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_pos_customer_phone ON pos_customer(phone);
CREATE INDEX idx_pos_customer_tier ON pos_customer(tier);
CREATE INDEX idx_pos_customer_name ON pos_customer(name);
```

#### Table: pos_price_list
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Price list name |
| type | VARCHAR(30) | NOT NULL | Type: retail, wholesale, promotional, member |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, inactive |
| effective_from | DATE | NOT NULL | Effective start date |
| effective_to | DATE | NULL | Effective end date |
| customer_type | VARCHAR(30) | NULL | Applicable customer type |
| store_id | BIGINT | FK -> store.id | Applicable store (NULL = all) |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_pos_price_list_status ON pos_price_list(status);
CREATE INDEX idx_pos_price_list_type ON pos_price_list(type);
CREATE INDEX idx_pos_price_list_dates ON pos_price_list(effective_from, effective_to);
```

#### Table: pos_price_list_item
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| price_list_id | BIGINT | FK -> pos_price_list.id, NOT NULL | Parent price list |
| product_id | BIGINT | FK -> product.id, NOT NULL | Product |
| price | DECIMAL(18,2) | NOT NULL | Product price in this list |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_pos_price_item_unique ON pos_price_list_item(price_list_id, product_id);
CREATE INDEX idx_pos_price_item_product ON pos_price_list_item(product_id);
```

#### Table: pos_discount
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| transaction_id | BIGINT | FK -> pos_transaction.id, NOT NULL | Parent transaction |
| type | VARCHAR(30) | NOT NULL | Type: promotion, manual, loyalty, coupon |
| code | VARCHAR(50) | NULL | Promotion/coupon code |
| value | DECIMAL(18,2) | NOT NULL | Discount amount |
| source | VARCHAR(100) | NULL | Discount source reference |
| applied_by | BIGINT | FK -> users.id | User who applied discount |

**Indexes:**
```sql
CREATE INDEX idx_pos_discount_transaction ON pos_discount(transaction_id);
CREATE INDEX idx_pos_discount_type ON pos_discount(type);
CREATE INDEX idx_pos_discount_code ON pos_discount(code);
```

#### Table: pos_return
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| transaction_id | BIGINT | FK -> pos_transaction.id, NOT NULL | Original transaction |
| return_receipt | VARCHAR(50) | NOT NULL, UNIQUE | Return receipt number |
| reason | VARCHAR(200) | NULL | Return reason |
| refund_amount | DECIMAL(18,2) | NOT NULL | Total refund amount |
| refund_method | VARCHAR(30) | NOT NULL | Method: cash, store_credit, exchange |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'completed' | Status: completed, pending_approval, rejected |
| approved_by | BIGINT | FK -> users.id | Approving manager |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| created_by | BIGINT | FK -> users.id | Return processor |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_pos_return_receipt ON pos_return(return_receipt);
CREATE INDEX idx_pos_return_transaction ON pos_return(transaction_id);
CREATE INDEX idx_pos_return_status ON pos_return(status);
```

#### Table: pos_payment
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| transaction_id | BIGINT | FK -> pos_transaction.id, NOT NULL | Parent transaction |
| method | VARCHAR(30) | NOT NULL | Method: cash, domestic_card, intl_card, qr, momo, viettel_money, zalo_pay, transfer |
| amount | DECIMAL(18,2) | NOT NULL | Payment amount |
| reference | VARCHAR(100) | NULL | Payment reference (card auth code, transfer ID) |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'completed' | Status: completed, failed, pending |

**Indexes:**
```sql
CREATE INDEX idx_pos_payment_transaction ON pos_payment(transaction_id);
CREATE INDEX idx_pos_payment_method ON pos_payment(method);
```

#### Table: product
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(300) | NOT NULL | Product name |
| sku | VARCHAR(50) | NULL, UNIQUE | Stock keeping unit |
| category_id | BIGINT | FK -> product_category.id | Product category |
| base_price | DECIMAL(18,2) | NOT NULL | Base/default price |
| description | TEXT | NULL | Product description |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, inactive |
| barcode | VARCHAR(50) | NULL | Primary barcode |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_product_sku ON product(sku);
CREATE INDEX idx_product_category ON product(category_id);
CREATE INDEX idx_product_barcode ON product(barcode);
CREATE INDEX idx_product_status ON product(status);
CREATE INDEX idx_product_name ON product(name);
```

#### Table: product_variant
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| product_id | BIGINT | FK -> product.id, NOT NULL | Parent product |
| variant_code | VARCHAR(50) | NOT NULL, UNIQUE | Variant SKU |
| barcode | VARCHAR(50) | NULL, UNIQUE | Variant barcode |
| attributes_json | JSON | NULL | Variant attributes (size, color, etc.) |
| price_adjust | DECIMAL(18,2) | DEFAULT 0 | Price adjustment from base |
| stock_qty | DECIMAL(10,2) | DEFAULT 0 | Current stock quantity |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_variant_code ON product_variant(variant_code);
CREATE UNIQUE INDEX idx_variant_barcode ON product_variant(barcode);
CREATE INDEX idx_variant_product ON product_variant(product_id);
```

#### Table: product_image
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| product_id | BIGINT | FK -> product.id, NOT NULL | Parent product |
| variant_id | BIGINT | FK -> product_variant.id | Variant-specific image |
| image_url | VARCHAR(500) | NOT NULL | Image URL |
| is_primary | TINYINT(1) | DEFAULT 0 | Primary image flag |
| alt_text | VARCHAR(200) | NULL | Alt text |
| sort_order | INT | DEFAULT 0 | Display order |

**Indexes:**
```sql
CREATE INDEX idx_product_image_product ON product_image(product_id);
CREATE INDEX idx_product_image_variant ON product_image(variant_id);
CREATE INDEX idx_product_image_primary ON product_image(is_primary);
```

#### Table: barcode
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| product_id | BIGINT | FK -> product.id, NOT NULL | Associated product |
| variant_id | BIGINT | FK -> product_variant.id | Associated variant |
| code | VARCHAR(50) | NOT NULL | Barcode value |
| type | VARCHAR(20) | DEFAULT 'EAN13' | Barcode type: EAN13, EAN8, UPC, Code128, QR |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_barcode_code ON barcode(code);
CREATE INDEX idx_barcode_product ON barcode(product_id);
CREATE INDEX idx_barcode_variant ON barcode(variant_id);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| pos_session | POS session records | 10,000 |
| pos_transaction | POS transaction records | 1,000,000 |
| pos_item | Transaction line items | 5,000,000 |
| pos_customer | Customer records | 100,000 |
| pos_price_list | Price list configurations | 100 |
| pos_price_list_item | Price list product prices | 50,000 |
| pos_discount | Applied discounts | 200,000 |
| pos_return | Return transactions | 50,000 |
| pos_payment | Payment records | 1,500,000 |
| product | Product catalog | 10,000 |
| product_variant | Product variants | 50,000 |
| product_image | Product images | 100,000 |
| barcode | Barcode mappings | 60,000 |

---

## 5. API Specifications

### 5.1 POS Session Endpoints

#### POST /api/v1/pos/sessions/open
**Description:** Open a new POS session

**Request Body:**
```json
{
  "store_id": 1,
  "cashier_id": 42,
  "starting_cash": 5000000
}
```

**Permission:** `pos:session:open`

#### POST /api/v1/pos/sessions/{id}/close
**Description:** Close a POS session with reconciliation

**Request Body:**
```json
{
  "closing_cash": 12500000,
  "notes": "All transactions accounted for"
}
```

**Permission:** `pos:session:close`

### 5.2 Transaction Endpoints

#### POST /api/v1/pos/transactions
**Description:** Create a new POS transaction

**Permission:** `pos:transaction:create`

#### GET /api/v1/pos/transactions
**Description:** List POS transactions

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page |
| session_id | integer | No | Filter by session |
| date_from | string | No | Filter by date |
| date_to | string | No | Filter by date |
| customer_phone | string | No | Filter by customer phone |

**Permission:** `pos:transaction:read`

### 5.3 Customer Endpoints

#### GET /api/v1/pos/customers/search
**Description:** Search customer by phone number

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| phone | string | Yes | Phone number to search |

**Permission:** `pos:customer:read`

#### POST /api/v1/pos/customers
**Description:** Create new customer from POS

**Permission:** `pos:customer:create`

### 5.4 Return Endpoints

#### POST /api/v1/pos/returns
**Description:** Process a product return

**Request Body:**
```json
{
  "transaction_id": 12345,
  "items": [{"item_id": 1, "quantity": 1}],
  "reason": "Defective product",
  "refund_method": "cash"
}
```

**Permission:** `pos:return:create`

### 5.5 Price List Endpoints

#### GET /api/v1/pos/price-lists/{id}/products
**Description:** Get products with prices for a price list

**Permission:** `pos:price-list:read`

### 5.6 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| POST | /api/v1/pos/sessions/open | Open session | pos:session:open |
| POST | /api/v1/pos/sessions/{id}/close | Close session | pos:session:close |
| GET | /api/v1/pos/sessions | List sessions | pos:session:read |
| POST | /api/v1/pos/transactions | Create transaction | pos:transaction:create |
| GET | /api/v1/pos/transactions | List transactions | pos:transaction:read |
| GET | /api/v1/pos/customers/search | Search customer | pos:customer:read |
| POST | /api/v1/pos/customers | Create customer | pos:customer:create |
| POST | /api/v1/pos/returns | Process return | pos:return:create |
| GET | /api/v1/pos/price-lists | List price lists | pos:price-list:read |
| GET | /api/v1/pos/price-lists/{id}/products | Price list products | pos:price-list:read |
| GET | /api/v1/pos/products | List products | pos:product:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Store Manager | store_manager | Full access to store operations and reporting |
| Cashier | cashier | POS transaction processing within active session |
| Sales Associate | sales_associate | Product lookup, cart assistance |
| Inventory Clerk | inventory_clerk | Store inventory management |
| Regional Manager | regional_manager | Read access to all stores in region |
| System Administrator | system_admin | Configuration and hardware setup |

### Permission Matrix
| Role | Open/Close Session | Process Transaction | Process Return | Manage Price Lists | View Reports | Manage Inventory |
|------|-------------------|--------------------|---------------|-------------------|-------------|-----------------|
| Store Manager | Yes | Yes | Yes | Yes | Yes | Yes |
| Cashier | Yes (own) | Yes | Yes (with approval) | No | Own session only | No |
| Sales Associate | No | View only | No | No | No | No |
| Inventory Clerk | No | No | No | No | No | Yes |
| Regional Manager | No | No | No | No | All stores | No |
| System Administrator | No | No | No | Yes | Yes | Yes |

### Row-Level Security
- Cashiers can only access their own POS sessions and transactions.
- Store Managers have access to all sessions and transactions within their assigned store.
- Regional Managers have read-only access to all stores within their assigned region.

---

## 7. State Machine / Workflows

### 7.1 POS Session Status Transitions

```
[OPEN] ──process txn──→ [OPEN] ──close──→ [CLOSED]
  │                        │
  └──void──→ [VOIDED]      └──discrepancy──→ [CLOSED_WITH_NOTES]
```

### 7.2 Transaction Status Transitions

```
[CART] ──payment──→ [PAYMENT_PROCESSING] ──complete──→ [COMPLETED]
  │                      │
  └──cancel──→ [CANCELLED]└──fail──→ [FAILED]
```

### 7.3 Return Workflow

```
[REQUESTED] ──has_receipt──→ [APPROVED] ──process──→ [COMPLETED]
     │                             │
     └──no_receipt──→ [PENDING_APPROVAL] ──approve──→ [APPROVED]
                                          │
                                          └──reject──→ [REJECTED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Sales Summary
**Type:** Real-time
**Purpose:** Display POS sales totals by store, cashier, and time period

**Metrics:**
- Transaction count and total revenue
- Average transaction value
- Sales by payment method

**Chart Type:** Bar chart, Table

### 8.2 Report: Product Performance
**Type:** Real-time
**Purpose:** Track product sales volume and revenue at store level

**Metrics:**
- Units sold by product
- Revenue by product
- Top-selling products

**Chart Type:** Bar chart, Table

### 8.3 Report: Cashier Performance
**Type:** Real-time
**Purpose:** Monitor cashier transaction volume and accuracy

**Metrics:**
- Transactions per cashier
- Average transaction value
- Cash discrepancy rate

**Chart Type:** Table, Bar chart

### 8.4 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Sales Summary | Real-time | Daily | Bar Chart, CSV |
| Product Performance | Real-time | Weekly | Bar Chart, Excel |
| Cashier Performance | Real-time | Weekly | Table |
| Session Reconciliation | Real-time | Daily | Table |
| Return Analysis | Real-time | Monthly | Table |
| Payment Method Analysis | Real-time | Monthly | Pie Chart |
| Inventory Movement | Real-time | Daily | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Payment Gateway API (M34) | Card, e-wallet, QR payment processing | REST | API Key |
| Barcode Scanner | Hardware barcode scanner input | USB/HID | N/A |
| Receipt Printer | Thermal receipt printing | USB/Network | N/A |
| Cash Drawer | Cash drawer control | USB/Serial | N/A |
| Customer Display | Secondary display for customer | USB/Serial | N/A |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| pos_transaction.completed | {transaction_id, receipt_number, total, store_id} | Sales (M08), Inventory (M09) |
| pos_session.closed | {session_id, cashier_id, total_sales, discrepancy} | Store management, Accounting |
| pos_return.completed | {return_id, transaction_id, refund_amount} | Inventory (M09), Accounting (M04) |
| low_stock.alert | {product_id, current_stock, reorder_point} | Inventory (M09), Purchasing (M07) |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| promotion.activated | Promotion Management (M36) | Apply promotion at POS |
| product.price_changed | Inventory (M09) | Update POS price list cache |
| stock.replenished | Inventory (M09) | Update POS stock availability |

---

## 10. Technical Notes

### Performance
- Expected data volume: 1,000,000+ transactions, 5,000,000+ line items, 100,000+ customers
- Query complexity: Medium (aggregations for reports, real-time stock checks)
- Expected response time: Product grid load < 500ms, Transaction processing < 2s, Customer search < 300ms
- Barcode scan resolution: < 100ms
- Payment processing: < 5 seconds (depends on payment provider)

### Caching Strategy
- Redis cache for active price lists, product catalog, and stock levels
- TTL: 5 minutes for price lists, 1 minute for stock levels, 1 hour for product catalog
- POS terminal caches product data locally for offline operation
- Cache invalidation on price list change, stock update, or product modification

### File Attachments
- Supported types: PNG, JPG (product images), PDF (receipt exports)
- Max size: 2 MB per image
- Storage: S3-compatible object storage with CDN distribution
- Product images optimized and resized for POS grid display (200x200px thumbnails)

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Currency: VND with Vietnamese number formatting
- Receipt format complies with Vietnamese retail regulations

### Mobile Considerations
- POS interface optimized for tablet use (iPad, Android tablets)
- Mobile-friendly product grid with touch-optimized buttons
- Keyboard shortcuts (F1-F12) supplemented by on-screen action buttons
- Offline mode supports transaction queuing with automatic sync

### Hardware Integration
- Barcode scanners: USB HID keyboard emulation, Bluetooth
- Receipt printers: ESC/POS protocol over USB or network
- Cash drawers: Triggered via receipt printer signal
- Card terminals: Integrated via Payment Gateway API
- Customer displays: Secondary display via USB/serial

### Security
- POS sessions are tied to specific cashiers with authentication
- Transaction data is immutable after completion
- Cash discrepancies are logged with full audit trail
- Payment data (card numbers) is never stored; only token references
- API rate limiting: 200 requests per minute per terminal for read, 50 for write

### Scalability
- Multi-store architecture supports centralized product catalog with store-level pricing and inventory
- Transaction processing uses optimistic locking for concurrent cart operations
- Stock updates use atomic database operations to prevent race conditions
- End-of-day reconciliation runs asynchronously for large session volumes
