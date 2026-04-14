# M34 - Payment Gateway (Cong thanh toan)

**Category:** Digital Office
**Priority:** P3
**Reference Images:** assets/image_27.png
**Dependencies:** M04 (Accounting), M08 (Sales), M41 (E-Banking), M33 (E-Invoice)
**Generated:** April 14, 2026

---

## 1. Module Overview

The Payment Gateway module (JetPay) provides a unified payment processing platform that consolidates multiple payment methods into a single integration point for the ABN ERP system. It enables businesses to accept payments from customers through domestic and international credit/debit cards, QR code payments, e-wallets (MOMO, Viettel Money, ZaloPay), and bank transfers -- all processed through a centralized gateway with real-time transaction tracking, refund management, fee calculation, and settlement reporting.

The JetPay dashboard (image_27) presents a comprehensive financial overview: total payments processed of 292.2 billion VND (contributing to 2,822 billion VND lifetime total) across 210 transactions, total refunds of 32 billion VND across 19 transactions, resulting in net transferred amount of 240 billion VND with 50.13 billion VND remaining in the gateway wallet. The payment method breakdown shows adoption across all supported channels: Domestic cards, International cards (Visa/Mastercard/JCB), QR code payments, MOMO e-wallet, Viettel Money, Bank Transfer, and ZaloPay. The system has processed a total of 47,074 transactions across all time, with transaction-level details showing status, timestamps, amounts, fees, and settlement state. The module integrates with the Accounting module (M04) for automatic journal entry generation on each transaction, the E-Banking module (M41) for bank account reconciliation, and the E-Invoice module (M33) for payment-linked invoice generation.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| Payment Manager | Oversees all payment operations, gateway configuration, settlement reconciliation | Full access to all payment data and configuration |
| Finance Accountant | Monitors transactions, processes refunds, reviews settlement reports | Full access to transaction data, refund processing |
| Sales Representative | Views payment status for orders, generates payment links | Read access to order-linked payments, payment link creation |
| Customer Support | Handles payment inquiries, processes refund requests | Read access to transactions, refund request capability |
| System Administrator | Manages gateway connections, API keys, method configuration | Full system access, read-only transaction data |
| Auditor | Reviews transaction history, fee analysis, compliance reports | Read-only access to all payment data |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M04 - Accounting | Transactions generate journal entries; fees posted as expenses |
| M08 - Sales | Orders linked to payment transactions; payment status flows back to orders |
| M41 - E-Banking | Settlement transfers to bank accounts; reconciliation |
| M33 - E-Invoice | Payment confirmation triggers invoice generation |
| M01 - Authentication | User access control and role-based permissions |

### Key Business Rules

1. All payment transactions must include a unique transaction reference number that is immutable after creation.
2. Refunds can only be processed against transactions that are in "settled" or "captured" status; pending transactions must be voided instead.
3. Partial refunds are allowed up to the original transaction amount; total refunds cannot exceed the original transaction value.
4. Transaction fees are calculated per payment method and deducted from the settlement amount; fee schedules are configurable per merchant account.
5. Settlements are processed on a T+1 or T+2 schedule depending on the payment method; settlement reports are generated daily.
6. Payment links expire after a configurable validity period (default: 24 hours); expired links cannot be used for payment.
7. Duplicate transaction detection is performed based on amount, merchant, and timestamp within a 5-minute window; suspected duplicates are flagged for review.
8. International card transactions are subject to additional 3D Secure verification; failed 3DS authentication results in transaction decline.
9. QR code payments must be confirmed by the payer's mobile banking app within 120 seconds; timeout results in transaction cancellation.
10. All payment data must be retained for minimum 5 years for audit and regulatory compliance purposes.

---

## 2. Screen Inventory

### 2.1 Screen: Payment Gateway Dashboard

**Route:** `/payment-gateway/dashboard` | **Purpose:** Display comprehensive payment overview with totals, method breakdown, and recent activity.

| UI Component | Description |
|--------------|-------------|
| Total Payments Card | 292.2B VND this period, 2,822B VND lifetime total |
| Transaction Count Card | 210 transactions this period, 47,074 lifetime |
| Refunds Card | 32B refunded, 19 refund transactions |
| Net Transfer Card | 240B transferred to bank, 50.13B remaining in wallet |
| Payment Method Breakdown | Pie/bar chart showing volume by method |
| Recent Transactions Table | Latest 10 transactions with status and amount |
| Quick Actions | New Payment Link, Process Refund, Download Settlement |

**User Interactions:**
- Click on any KPI card to drill down into detailed reports
- View payment method breakdown and compare performance
- Click recent transactions to view full details
- Click quick actions to navigate to relevant functions

**Data Displayed:**
- Payment totals (period and lifetime)
- Transaction counts and refund statistics
- Net transfer and wallet balance
- Payment method distribution

### 2.2 Screen: Transaction List

**Route:** `/payment-gateway/transactions` | **Purpose:** Browse, search, and manage all payment transactions.

| UI Component | Description |
|--------------|-------------|
| Transaction List Table | Columns: Reference, Date, Amount, Method, Status, Fee, Settlement Status |
| Advanced Filters | Filter by date range, method, status, amount range, settlement status |
| Bulk Actions | Export, batch refund, download receipts |
| Status Badges | Pending, Processing, Completed, Failed, Refunded, Voided |
| Method Icons | Visual indicators for payment method type |

**User Interactions:**
- Search transactions by reference number or amount
- Filter by multiple criteria simultaneously
- Click transaction to view full details
- Select multiple transactions for batch operations

**Data Displayed:**
- Transaction reference and timestamp
- Amount, currency, payment method
- Transaction status and fee amount
- Settlement status and date

### 2.3 Screen: Transaction Detail

**Route:** `/payment-gateway/transactions/{id}` | **Purpose:** View complete details of a single payment transaction.

| UI Component | Description |
|--------------|-------------|
| Transaction Header | Reference number, status badge, amount, date |
| Payment Details | Method, card/wallet info (masked), 3DS status |
| Merchant Information | Merchant account, terminal ID, order reference |
| Fee Breakdown | Gateway fee, processing fee, net amount |
| Settlement Information | Settlement date, batch reference, bank account |
| Refund History | List of refunds against this transaction |
| Timeline | Chronological event log (created, authorized, captured, settled) |
| Action Bar | Process Refund, Void, Resend Notification, Download Receipt |

**User Interactions:**
- View all transaction details and event history
- Process refund (full or partial)
- Void pending transactions
- Download transaction receipt as PDF

**Data Displayed:**
- Complete transaction lifecycle data
- Fee breakdown and net settlement amount
- Refund history with amounts
- Timeline of all status changes

### 2.4 Screen: Refund Management

**Route:** `/payment-gateway/refunds` | **Purpose:** Process and track payment refunds.

| UI Component | Description |
|--------------|-------------|
| Refund List Table | Columns: Refund ID, Original Transaction, Amount, Date, Status, Reason |
| Refund Form | Modal form: transaction selection, refund amount, reason, notes |
| Status Filter | Pending, Processing, Completed, Failed |
| Summary Cards | Total refunds this period, count, average refund amount |

**User Interactions:**
- Create new refund by selecting original transaction
- Enter refund amount (full or partial)
- Track refund processing status
- View refund history and trends

**Data Displayed:**
- Refund amounts and reasons
- Processing status and timelines
- Original transaction references

### 2.5 Screen: Payment Method Configuration

**Route:** `/payment-gateway/methods` | **Purpose:** Configure supported payment methods and fee schedules.

| UI Component | Description |
|--------------|-------------|
| Method Cards | Domestic Card, International Card, QR, MOMO, Viettel Money, Transfer, ZaloPay |
| Method Settings | Enable/disable, fee percentage, fixed fee, settlement schedule |
| Fee Schedule Table | Fee by transaction volume tier |
| API Configuration | API keys, webhook URLs, callback endpoints |

**User Interactions:**
- Enable or disable payment methods
- Configure fee percentages and fixed fees
- Set settlement schedule (T+1, T+2)
- Test payment method connectivity

**Data Displayed:**
- Method status (active/inactive)
- Fee configuration details
- Settlement schedule
- API connection status

### 2.6 Screen: Payment Link Generator

**Route:** `/payment-gateway/payment-links` | **Purpose:** Create shareable payment links for customers.

| UI Component | Description |
|--------------|-------------|
| Payment Link List | Link ID, Amount, Description, Status, Created, Expires, Payment Count |
| Link Creation Form | Amount, description, expiry date, customer email, redirect URL |
| QR Code Display | QR code for the payment link |
| Share Options | Copy link, send email, generate QR, embed code |

**User Interactions:**
- Create new payment link with amount and details
- Configure expiry and redirect settings
- Generate QR code for in-person payments
- Send payment link directly to customer email
- Track payment link usage and status

**Data Displayed:**
- Payment link details and status
- Payment count and amounts collected
- Expiry countdown

### 2.7 Screen: Settlement Report

**Route:** `/payment-gateway/settlements` | **Purpose:** Generate and review settlement reports for bank reconciliation.

| UI Component | Description |
|--------------|-------------|
| Settlement List | Date, Total Amount, Fee Deducted, Net Amount, Status, Bank Account |
| Settlement Detail | Transaction breakdown, fee calculation, transfer confirmation |
| Export Options | Export to CSV, Excel, bank-specific format |
| Reconciliation Status | Matched/unmatched with bank statement |

**User Interactions:**
- View settlement summaries by date
- Drill down into transaction-level breakdown
- Export settlement files for bank reconciliation
- Mark settlements as reconciled

**Data Displayed:**
- Settlement totals and net amounts
- Fee deductions by method
- Bank transfer confirmation status
- Reconciliation matching status

### 2.8 Screen: Merchant Account Management

**Route:** `/payment-gateway/merchants` | **Purpose:** Manage merchant accounts linked to the payment gateway.

| UI Component | Description |
|--------------|-------------|
| Merchant List | Name, Status, Total Transactions, Total Volume, Settlement Account |
| Merchant Detail | Configuration, API credentials, fee schedule, transaction history |
| Account Linking | Linked bank accounts for settlement |

**User Interactions:**
- Create and configure merchant accounts
- Assign fee schedules per merchant
- Link settlement bank accounts
- View merchant-specific transaction history

**Data Displayed:**
- Merchant account configuration
- Transaction volume and value
- Linked settlement accounts

---

## 3. Feature Specifications

### 3.1 Feature: Payment Processing
**Description:** Process payments through multiple methods (cards, QR, e-wallets, transfers) with real-time status tracking.

**User Stories:**
- As a Customer, I want to pay using my preferred payment method, so that checkout is convenient.
- As a Finance Accountant, I want to track all payment transactions in real-time, so that I can monitor cash flow.

**Acceptance Criteria:**
- Given a payment request is submitted, when the payment method processes the transaction, then the status updates to completed, failed, or pending based on the result.
- Given a transaction is completed, when the settlement runs, then the net amount (after fees) is queued for bank transfer.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Amount | Must be positive, max 1B per transaction | Amount must be between 1,000 and 1,000,000,000 VND |
| Payment method | Must be active and configured | Selected payment method is not available |
| Merchant account | Must be active | Merchant account is not active |
| Expiry (links) | Must be in the future | Expiry date must be in the future |

**Edge Cases:**
- Network timeout during payment processing results in pending status with auto-reconciliation
- Duplicate payment detection flags transactions within 5-minute window with same amount and merchant
- International card payments require 3D Secure; failure triggers decline

### 3.2 Feature: Refund Processing
**Description:** Process full or partial refunds against completed transactions with status tracking.

**User Stories:**
- As a Customer Support agent, I want to process refunds for returned orders, so that customers receive their money back.
- As a Finance Accountant, I want to track refund totals, so that I can reconcile against payments.

**Acceptance Criteria:**
- Given a completed transaction, when a refund is processed, then the refund amount is deducted from the next settlement.
- Given a partial refund, when the refund completes, then the remaining refundable amount is updated on the original transaction.

**Edge Cases:**
- Refunds exceeding the original transaction amount are rejected
- Refunds on already fully refunded transactions are blocked
- Refund processing failures trigger automatic retry and notification

### 3.3 Feature: Payment Link Generation
**Description:** Create shareable payment links with configurable expiry and automatic payment tracking.

**User Stories:**
- As a Sales Representative, I want to generate payment links for orders, so that customers can pay remotely.
- As a Finance Accountant, I want payment links to expire, so that outstanding payment requests are time-bounded.

**Acceptance Criteria:**
- Given a payment link is created, when the customer pays through the link, then a transaction is recorded and the link status updates.
- Given a payment link expires, when a customer attempts to access it, then an expired message is displayed.

**Edge Cases:**
- Payment links with partial payments can be configured to accept remaining balance or close
- Multiple payments to a single link are allowed if configured for recurring payments
- Expired links cannot be reactivated; new links must be created

### 3.4 Feature: Settlement and Reconciliation
**Description:** Generate daily settlement reports with fee deductions and bank transfer reconciliation.

**User Stories:**
- As a Finance Accountant, I want daily settlement reports, so that I can verify gateway-to-bank transfers.
- As a Payment Manager, I want automatic reconciliation with bank statements, so that discrepancies are detected quickly.

**Acceptance Criteria:**
- Given the end of a settlement day, when the settlement report is generated, then all completed transactions are aggregated with fee deductions.
- Given a settlement report, when the bank statement is imported, then transactions are matched and reconciliation status is updated.

**Edge Cases:**
- Settlement failures (bank account invalid, insufficient fee coverage) are flagged for manual review
- Cross-month settlements are allocated to the correct accounting period
- Fee schedule changes apply from the effective date forward, not retroactively

### 3.5 Feature: Multi-Method Fee Management
**Description:** Configure and calculate transaction fees per payment method with volume-based tiers.

**User Stories:**
- As a Payment Manager, I want to set different fees per payment method, so that pricing reflects processing costs.
- As a Finance Accountant, I want volume-based fee tiers, so that high-volume merchants receive discounted fees.

**Acceptance Criteria:**
- Given a transaction is processed, when the fee is calculated, then the method-specific fee schedule is applied.
- Given a merchant reaches a new volume tier, when the next transaction is processed, then the lower fee rate applies.

**Edge Cases:**
- Fee changes do not affect transactions already processed
- Minimum fee ensures no transaction is processed below cost
- Promotional fee periods (e.g., first month free) are tracked and expire automatically

### 3.6 Feature: Transaction Status Tracking
**Description:** Track transaction lifecycle from initiation through authorization, capture, settlement, and potential refund.

**User Stories:**
- As a Finance Accountant, I want to see the full lifecycle of each transaction, so that I can troubleshoot issues.
- As a Customer Support agent, I want to check transaction status for customer inquiries, so that I can provide accurate information.

**Acceptance Criteria:**
- Given a transaction progresses through its lifecycle, when each status change occurs, then the event is logged with timestamp and details.
- Given a transaction is in pending status for more than 30 minutes, when the timeout check runs, then the transaction is marked as failed.

**Edge Cases:**
- Transactions stuck in pending status are automatically reconciled with the payment provider
- Status changes are atomic to prevent inconsistent states
- Refunded transactions retain their original status with a refund sub-status

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+------------------+       +--------------------+       +------------------+
| PaymentTransaction|      | RefundTransaction  |       |  PaymentMethod   |
+------------------+       +--------------------+       +------------------+
| id (PK)          |<----->| id (PK)            |       | id (PK)          |
| reference        |       | transaction_id(FK) |       | name             |
| amount           |       | amount             |       | code             |
| fee              |       | status             |       | fee_percent      |
| net_amount       |       | reason             |       | fee_fixed        |
| status           |       | created_at         |       | settlement_days  |
| method_id(FK)    |       +--------------------+       | is_active        |
| merchant_id(FK)  |                                    +------------------+
| settlement_id(FK)|       +--------------------+              |
| created_at       |       |  PaymentGateway    |              |
+------------------+       +--------------------+              |
        |                | id (PK)            |              |
        v                | name               |              v
+------------------+     | provider           |       +------------------+
| TransactionFee   |     | api_config_json    |       | MerchantAccount  |
+------------------+     | status             |       +------------------+
| id (PK)          |     | created_at         |       | id (PK)          |
| transaction_id(FK)|    +--------------------+       | name             |
| fee_type         |                                  | settlement_acct  |
| amount           |                                  | status           |
| created_at       |                                  | created_at       |
+------------------+                                  +------------------+
        |                                                    |
        v                                                    v
+------------------+       +--------------------+       +------------------+
| TransactionStatus|       | SettlementReport   |       |  PaymentLink     |
+------------------+       +--------------------+       +------------------+
| id (PK)          |       | id (PK)            |       | id (PK)          |
| code             |       | settlement_date    |       | reference        |
| name             |       | total_amount       |       | amount           |
| description      |       | total_fees         |       | description      |
+------------------+       | net_amount         |       | expiry_date      |
                           | status             |       | status           |
                           | bank_account       |       | payment_count    |
                           | created_at         |       | created_at       |
                           +--------------------+       +------------------+
```

### 4.2 Table Schemas

#### Table: payment_transaction
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| reference | VARCHAR(50) | NOT NULL, UNIQUE | Unique transaction reference |
| amount | DECIMAL(18,2) | NOT NULL | Transaction amount |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Currency code |
| fee | DECIMAL(18,2) | DEFAULT 0 | Total fee deducted |
| net_amount | DECIMAL(18,2) | NOT NULL | Net amount after fees |
| status | VARCHAR(30) | NOT NULL | Status: pending, processing, completed, failed, refunded, voided |
| payment_method_id | BIGINT | FK -> payment_method.id | Payment method used |
| merchant_account_id | BIGINT | FK -> merchant_account.id | Merchant account |
| order_id | BIGINT | NULL | Associated order ID |
| card_last_four | VARCHAR(4) | NULL | Masked card number |
| card_type | VARCHAR(20) | NULL | Card type (Visa, MC, JCB) |
| threed_secure | TINYINT(1) | DEFAULT 0 | 3DS authentication flag |
| settlement_id | BIGINT | FK -> settlement_report.id | Settlement batch |
| gateway_response | JSON | NULL | Raw response from payment gateway |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| completed_at | TIMESTAMP | NULL | Completion timestamp |
| created_by | BIGINT | FK -> users.id | Creator |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_payment_txn_reference ON payment_transaction(reference);
CREATE INDEX idx_payment_txn_status ON payment_transaction(status);
CREATE INDEX idx_payment_txn_method ON payment_transaction(payment_method_id);
CREATE INDEX idx_payment_txn_merchant ON payment_transaction(merchant_account_id);
CREATE INDEX idx_payment_txn_date ON payment_transaction(created_at);
CREATE INDEX idx_payment_txn_order ON payment_transaction(order_id);
CREATE INDEX idx_payment_txn_settlement ON payment_transaction(settlement_id);
```

#### Table: refund_transaction
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| transaction_id | BIGINT | FK -> payment_transaction.id, NOT NULL | Original transaction |
| refund_reference | VARCHAR(50) | NOT NULL, UNIQUE | Refund reference number |
| amount | DECIMAL(18,2) | NOT NULL | Refund amount |
| reason | VARCHAR(500) | NULL | Refund reason |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'pending' | Status: pending, processing, completed, failed |
| gateway_response | JSON | NULL | Raw gateway response |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| completed_at | TIMESTAMP | NULL | Refund completion timestamp |
| created_by | BIGINT | FK -> users.id | Creator |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_refund_reference ON refund_transaction(refund_reference);
CREATE INDEX idx_refund_transaction ON refund_transaction(transaction_id);
CREATE INDEX idx_refund_status ON refund_transaction(status);
```

#### Table: payment_method
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL | Display name |
| code | VARCHAR(30) | NOT NULL, UNIQUE | Method code: domestic_card, intl_card, qr, momo, viettel_money, transfer, zalo_pay |
| fee_percent | DECIMAL(5,2) | DEFAULT 0 | Percentage fee |
| fee_fixed | DECIMAL(18,2) | DEFAULT 0 | Fixed fee per transaction |
| settlement_days | INT | DEFAULT 1 | T+N settlement days |
| min_amount | DECIMAL(18,2) | DEFAULT 1000 | Minimum transaction amount |
| max_amount | DECIMAL(18,2) | DEFAULT 1000000000 | Maximum transaction amount |
| is_active | TINYINT(1) | DEFAULT 0 | Active flag |
| config_json | JSON | NULL | Method-specific configuration |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_payment_method_code ON payment_method(code);
CREATE INDEX idx_payment_method_active ON payment_method(is_active);
```

#### Table: payment_gateway
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Gateway display name |
| provider | VARCHAR(100) | NOT NULL | Gateway provider name |
| api_config_json | JSON | NOT NULL | Encrypted API credentials and settings |
| webhook_url | VARCHAR(500) | NULL | Webhook callback URL |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'inactive' | Status: active, inactive, error |
| last_health_check | TIMESTAMP | NULL | Last health check timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_payment_gateway_status ON payment_gateway(status);
```

#### Table: transaction_fee
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| transaction_id | BIGINT | FK -> payment_transaction.id, NOT NULL | Associated transaction |
| fee_type | VARCHAR(30) | NOT NULL | Type: gateway, processing, method |
| amount | DECIMAL(18,2) | NOT NULL | Fee amount |
| description | VARCHAR(200) | NULL | Fee description |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX txn_fee_transaction ON transaction_fee(transaction_id);
CREATE INDEX txn_fee_type ON transaction_fee(fee_type);
```

#### Table: settlement_report
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| settlement_date | DATE | NOT NULL | Settlement date |
| total_amount | DECIMAL(18,2) | NOT NULL | Gross settlement amount |
| total_fees | DECIMAL(18,2) | NOT NULL | Total fees deducted |
| net_amount | DECIMAL(18,2) | NOT NULL | Net transfer amount |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'pending' | Status: pending, transferred, reconciled, failed |
| bank_account_id | BIGINT | FK -> bank_account.id | Destination bank account |
| transfer_reference | VARCHAR(100) | NULL | Bank transfer reference |
| transferred_at | TIMESTAMP | NULL | Transfer timestamp |
| reconciled_at | TIMESTAMP | NULL | Reconciliation timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_settlement_date ON settlement_report(settlement_date);
CREATE INDEX idx_settlement_status ON settlement_report(status);
CREATE INDEX idx_settlement_bank ON settlement_report(bank_account_id);
```

#### Table: payment_link
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| reference | VARCHAR(50) | NOT NULL, UNIQUE | Payment link reference |
| amount | DECIMAL(18,2) | NOT NULL | Payment amount |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Currency |
| description | TEXT | NULL | Payment description |
| customer_email | VARCHAR(200) | NULL | Customer email |
| redirect_url | VARCHAR(500) | NULL | Post-payment redirect URL |
| expiry_date | TIMESTAMP | NOT NULL | Link expiry |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, paid, expired, cancelled |
| payment_count | INT | DEFAULT 0 | Number of payments received |
| total_paid | DECIMAL(18,2) | DEFAULT 0 | Total amount paid |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_payment_link_reference ON payment_link(reference);
CREATE INDEX idx_payment_link_status ON payment_link(status);
CREATE INDEX idx_payment_link_expiry ON payment_link(expiry_date);
CREATE INDEX idx_payment_link_creator ON payment_link(created_by);
```

#### Table: merchant_account
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Merchant display name |
| merchant_code | VARCHAR(50) | NOT NULL, UNIQUE | Merchant code |
| settlement_bank_account_id | BIGINT | FK -> bank_account.id | Settlement destination |
| fee_schedule_json | JSON | NULL | Custom fee schedule |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, suspended, closed |
| api_key | VARCHAR(255) | NULL | Encrypted API key |
| api_secret | VARCHAR(255) | NULL | Encrypted API secret |
| total_transactions | INT | DEFAULT 0 | Lifetime transaction count |
| total_volume | DECIMAL(18,2) | DEFAULT 0 | Lifetime volume |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_merchant_code ON merchant_account(merchant_code);
CREATE INDEX idx_merchant_status ON merchant_account(status);
```

#### Table: transaction_status
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| code | VARCHAR(30) | NOT NULL, UNIQUE | Status code |
| name | VARCHAR(100) | NOT NULL | Display name |
| description | TEXT | NULL | Status description |
| is_terminal | TINYINT(1) | DEFAULT 0 | Whether this is a final state |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_txn_status_code ON transaction_status(code);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| payment_transaction | Payment transaction records | 500,000 |
| refund_transaction | Refund records | 50,000 |
| payment_method | Payment method configurations | 20 |
| payment_gateway | Gateway connection configs | 10 |
| transaction_fee | Fee breakdown per transaction | 1,000,000 |
| settlement_report | Daily settlement batches | 5,000 |
| payment_link | Payment link records | 100,000 |
| merchant_account | Merchant account records | 100 |
| transaction_status | Status definitions | 10 |

---

## 5. API Specifications

### 5.1 Transaction Endpoints

#### GET /api/v1/payment-gateway/transactions
**Description:** List payment transactions with filtering and pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| limit | integer | No | Items per page (default: 20, max: 100) |
| date_from | string | No | Filter by date from |
| date_to | string | No | Filter by date to |
| method | string | No | Filter by payment method code |
| status | string | No | Filter by status |
| amount_min | decimal | No | Minimum amount |
| amount_max | decimal | No | Maximum amount |
| sort | string | No | Sort field |
| order | string | No | asc/desc |

**Permission:** `payment:transaction:read`

#### POST /api/v1/payment-gateway/transactions
**Description:** Create a new payment transaction (initiate payment)

**Permission:** `payment:transaction:create`

#### GET /api/v1/payment-gateway/transactions/{id}
**Description:** Get single transaction details

**Permission:** `payment:transaction:read`

### 5.2 Refund Endpoints

#### POST /api/v1/payment-gateway/refunds
**Description:** Process a refund

**Request Body:**
```json
{
  "transaction_id": 12345,
  "amount": 5000000,
  "reason": "Customer returned product"
}
```

**Response (201 Created):**
```json
{
  "id": 67,
  "refund_reference": "REF-20260414-00067",
  "status": "pending",
  "amount": 5000000
}
```

**Permission:** `payment:refund:create`

### 5.3 Settlement Endpoints

#### GET /api/v1/payment-gateway/settlements
**Description:** List settlement reports

**Permission:** `payment:settlement:read`

#### GET /api/v1/payment-gateway/settlements/{id}/transactions
**Description:** Get transactions in a settlement batch

**Permission:** `payment:settlement:read`

### 5.4 Payment Link Endpoints

#### POST /api/v1/payment-gateway/payment-links
**Description:** Create a payment link

**Request Body:**
```json
{
  "amount": 15000000,
  "description": "Invoice #INV-2026-0042",
  "expiry_date": "2026-04-15T23:59:59Z",
  "customer_email": "customer@example.com"
}
```

**Response (201 Created):**
```json
{
  "id": 890,
  "reference": "PL-20260414-00890",
  "url": "https://pay.example.com/pl/PL-20260414-00890",
  "status": "active",
  "expiry_date": "2026-04-15T23:59:59Z"
}
```

**Permission:** `payment:link:create`

### 5.5 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/payment-gateway/transactions | List transactions | payment:transaction:read |
| POST | /api/v1/payment-gateway/transactions | Create transaction | payment:transaction:create |
| GET | /api/v1/payment-gateway/transactions/{id} | Get transaction | payment:transaction:read |
| POST | /api/v1/payment-gateway/refunds | Create refund | payment:refund:create |
| GET | /api/v1/payment-gateway/refunds | List refunds | payment:refund:read |
| GET | /api/v1/payment-gateway/settlements | List settlements | payment:settlement:read |
| GET | /api/v1/payment-gateway/settlements/{id}/transactions | Settlement transactions | payment:settlement:read |
| POST | /api/v1/payment-gateway/payment-links | Create payment link | payment:link:create |
| GET | /api/v1/payment-gateway/payment-links | List payment links | payment:link:read |
| GET | /api/v1/payment-gateway/methods | List payment methods | payment:method:read |
| PUT | /api/v1/payment-gateway/methods/{id} | Update method config | payment:method:update |
| GET | /api/v1/payment-gateway/merchants | List merchants | payment:merchant:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Payment Manager | payment_manager | Full access to all payment operations and configuration |
| Finance Accountant | finance_accountant | Monitor transactions, process refunds, review settlements |
| Sales Representative | sales_rep | View order-linked payments, create payment links |
| Customer Support | customer_support | View transactions, process refund requests |
| System Administrator | system_admin | Manage gateway connections and configuration |
| Auditor | auditor | Read-only access for compliance review |

### Permission Matrix
| Role | View Transactions | Create Payment Link | Process Refund | Configure Methods | View Settlements | Export Data |
|------|------------------|--------------------|---------------|------------------|------------------|------------|
| Payment Manager | Yes | Yes | Yes | Yes | Yes | Yes |
| Finance Accountant | Yes | No | Yes | No | Yes | Yes |
| Sales Representative | Linked orders | Yes | No | No | No | No |
| Customer Support | Yes | No | Request only | No | No | No |
| System Administrator | Yes | No | No | Yes | Yes | Yes |
| Auditor | Yes | No | No | No | Yes | Yes |

### Row-Level Security
- Sales Representatives can only view transactions linked to their orders.
- Finance Accountants have access to all transactions within their assigned company.
- Payment Managers have unrestricted access across all companies and merchant accounts.

---

## 7. State Machine / Workflows

### 7.1 Transaction Status Transitions

```
[PENDING] ──authorize──→ [PROCESSING] ──capture──→ [COMPLETED] ──settle──→ [SETTLED]
     │                        │                        │
     └──fail──────────────────┘                        └──refund──→ [REFUNDED]
     │                                                 │
     └──timeout──→ [FAILED]                            └──void──→ [VOIDED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| PENDING | PROCESSING | authorize | Payment Gateway | Funds reserved on customer account |
| PROCESSING | COMPLETED | capture | Payment Gateway | Funds transferred to merchant |
| PROCESSING | FAILED | fail | System | Transaction declined, customer notified |
| PENDING | FAILED | timeout | System | 30-minute timeout, auto-fail |
| COMPLETED | REFUNDED | refund | Finance Accountant | Refund transaction created |
| COMPLETED | VOIDED | void | Payment Manager | Transaction voided before settlement |

### 7.2 Refund Status Transitions

```
[PENDING] ──process──→ [PROCESSING] ──complete──→ [COMPLETED]
     │                      │
     └──reject──→ [REJECTED]└──fail──→ [FAILED]
```

### 7.3 Payment Link Status Transitions

```
[ACTIVE] ──pay──→ [PAID]
   │                │
   └──expire──→ [EXPIRED]
   │
   └──cancel──→ [CANCELLED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Payment Overview
**Type:** Real-time
**Purpose:** Display payment volume, method distribution, and transaction trends

**Metrics:**
- Total payments (period and lifetime)
- Transaction count and average value
- Method breakdown with percentages

**Chart Type:** Bar chart, Pie chart

### 8.2 Report: Refund Analysis
**Type:** Real-time
**Purpose:** Track refund volumes, amounts, and reasons

**Metrics:**
- Refund count and total amount
- Refund rate by method
- Top refund reasons

**Chart Type:** Bar chart, Table

### 8.3 Report: Settlement Summary
**Type:** Real-time
**Purpose:** Review settlement batches and bank reconciliation status

**Metrics:**
- Daily settlement totals
- Fee deductions by method
- Reconciliation match rate

**Chart Type:** Line chart, Table

### 8.4 Report: Method Performance
**Type:** Real-time
**Purpose:** Compare payment method adoption and success rates

**Metrics:**
- Transaction volume by method
- Success/failure rates
- Average transaction value

**Chart Type:** Stacked bar chart

### 8.5 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Payment Overview | Real-time | Daily | Chart, CSV |
| Refund Analysis | Real-time | Weekly | Table, PDF |
| Settlement Summary | Real-time | Daily | Line Chart, Excel |
| Method Performance | Real-time | Monthly | Bar Chart |
| Fee Analysis | Real-time | Monthly | Table |
| Transaction Trends | Real-time | Weekly | Line Chart |
| Merchant Performance | Real-time | Monthly | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Card Network API (Visa/MC/JCB) | Card payment processing | REST | API Key + Certificate |
| MOMO API | E-wallet payment | REST | API Key + HMAC signature |
| ZaloPay API | Zalo e-wallet payment | REST | API Key + OAuth |
| Viettel Money API | Viettel e-wallet payment | REST | API Key |
| VietQR API | QR code payment processing | REST | API Key |
| Bank Transfer API | Direct bank transfer | REST | OAuth 2.0 |
| 3D Secure API | Card authentication | REST | Certificate-based |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| payment.completed | {transaction_id, reference, amount, method} | Accounting (M04), Sales (M08) |
| payment.failed | {transaction_id, reference, error} | Monitoring, Customer notification |
| refund.completed | {refund_id, transaction_id, amount} | Accounting (M04) |
| settlement.generated | {settlement_id, date, net_amount} | E-Banking (M41) |
| payment_link.paid | {link_id, amount, paid_at} | Order management |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| order.placed | Sales (M08) | Generate payment link automatically |
| invoice.issued | E-Invoice (M33) | Link payment to invoice |
| bank.transfer.received | E-Banking (M41) | Reconcile settlement |

---

## 10. Technical Notes

### Performance
- Expected data volume: 500,000+ transactions, 50,000+ refunds, 100,000+ payment links
- Query complexity: Medium (aggregations for settlements, detail retrieval for transactions)
- Expected response time: Transaction list < 1s, Detail < 500ms, Settlement generation < 10s per 10,000 transactions
- Payment processing latency: < 3 seconds for card payments, < 5 seconds for e-wallets
- Webhook delivery: < 1 second for real-time notifications

### Caching Strategy
- Redis cache for payment method configurations, fee schedules, and merchant account data
- TTL: 30 minutes for static configuration, 5 minutes for settlement summaries
- Transaction status cached per transaction with 1-minute TTL during processing
- Cache invalidation on status change, fee update, or settlement generation

### File Attachments
- Supported types: PDF (receipts), CSV (settlement exports), XML (bank files)
- Max size: 5 MB per file
- Storage: S3-compatible object storage with CDN distribution

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Currency: VND with Vietnamese number formatting
- Multi-currency support for international card transactions

### Mobile Considerations
- Payment links are mobile-optimized with responsive design
- QR code payments designed for mobile scanning
- Mobile dashboard provides read-only access to transaction status
- Push notifications for payment confirmations and refund status

### Security
- Payment card data is never stored; only last 4 digits and token references
- API credentials for payment providers are encrypted at rest using AES-256
- All payment data is transmitted over TLS 1.2+
- PCI-DSS compliance: system operates at SAQ A level (redirect/hosted payment pages)
- 3D Secure authentication required for international card transactions
- Audit trail captures all transaction status changes with timestamps

### Compliance
- Payment data retained for minimum 5 years per Vietnamese financial regulations
- Refund processing follows State Bank of Vietnam guidelines
- Settlement reports comply with bank reconciliation requirements
- Transaction monitoring includes anti-fraud detection for suspicious patterns
