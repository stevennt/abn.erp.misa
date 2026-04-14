# M41 - E-Banking (Ngan hang Dien tu)

**Category:** Digital Office
**Priority:** P3
**Reference Images:** assets/image_15.png, assets/image_27.png
**Dependencies:** M04 (Accounting), M34 (Payment Gateway), M40 (Group Management), M33 (E-Invoice)
**Generated:** April 14, 2026

---

## 1. Module Overview

The E-Banking module provides integrated electronic banking capabilities within the ABN ERP accounting system, enabling businesses to manage bank accounts, track deposits, process payments, reconcile bank statements, and monitor balances -- all without leaving the ERP platform. The module connects directly to bank APIs for real-time transaction feeds, automated statement imports, and payment order submission, creating a seamless bridge between the company's financial records and its banking operations.

The financial dashboard (image_15) shows banking-related metrics integrated within the broader accounting view: Cash on hand 7,500M, Deposit at bank 1,000M, with receivable/payable tracking that ties to bank transactions. The payment gateway integration (image_27) shows how bank transfer is offered as a payment method alongside cards, e-wallets, and QR payments, with the banking module processing incoming transfer payments and matching them to invoices and orders. The module supports deposit tracking (term deposits, savings accounts), real-time balance monitoring across multiple bank accounts, automated bank statement import and reconciliation, payment order creation and submission, and multi-bank connectivity through a unified interface. It integrates with the Accounting module (M04) for automatic journal entry generation on bank transactions, the Payment Gateway module (M34) for bank transfer payment processing, the Group Management module (M40) for multi-company bank account management, and the E-Invoice module (M33) for payment-linked invoice status updates.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| Finance Director | Oversees all banking operations, approves payment orders, monitors cash position | Full access to all banking data and payment approvals |
| Treasury Manager | Manages bank accounts, executes payments, performs reconciliation | Full access to bank operations, payment execution |
| Accountant | Records bank transactions, performs bank reconciliation, monitors balances | Read/write access to bank transactions and reconciliation |
| Cashier | Processes day-to-day bank payments, deposits, and withdrawals | Write access to payment processing within limits |
| Auditor | Reviews bank transaction history, reconciliation records, and compliance | Read-only access to all banking data |
| System Administrator | Manages bank connections, API configurations, and provider integrations | Full system access, read-only transaction data |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M04 - Accounting | Bank transactions generate journal entries; cash/deposit balances |
| M34 - Payment Gateway | Bank transfer payments flow into banking module |
| M40 - Group Management | Multi-company bank account management |
| M33 - E-Invoice | Payment status sync updates invoice payment state |
| M01 - Authentication | User access control, payment approval workflows |

### Key Business Rules

1. Each bank account in the system must be linked to a real bank account with valid account number, bank name, and currency.
2. Bank transactions are imported automatically via API connection or manually via statement file import; all imported transactions must be categorized.
3. Bank reconciliation matches system transactions (payments, receipts) against bank statement lines; unmatched items are flagged for review.
4. Payment orders require approval based on configured thresholds: single approval for amounts under 100M VND, dual approval for 100M-500M VND, director approval for over 500M VND.
5. Bank balances are updated in real-time as transactions are posted; available balance = ledger balance - outstanding payment orders.
6. Deposit accounts (term deposits, savings) track maturity dates and interest accrual; maturity alerts are sent 30 days before maturity.
7. Bank connections use secure API integrations with OAuth 2.0 or certificate-based authentication; credentials are stored encrypted.
8. Multi-currency bank accounts are supported with daily exchange rate updates; foreign currency transactions are converted to base currency for reporting.
9. Inter-company bank transfers within the group (M40) are tracked as inter-company transactions for consolidation.
10. Bank transaction categories are configurable and used for cash flow reporting and analysis.

---

## 2. Screen Inventory

### 2.1 Screen: Banking Dashboard

**Route:** `/ebanking/dashboard` | **Purpose:** Display banking overview with balances, recent transactions, and quick actions.

| UI Component | Description |
|--------------|-------------|
| Balance Summary Cards | Cash 7,500M, Deposit 1,000M across all bank accounts |
| Bank Account List | Account name, bank, balance, currency, status |
| Recent Transactions | Last 10 bank transactions with date, amount, description |
| Pending Payments | Payment orders awaiting approval or execution |
| Deposit Maturity Alerts | Upcoming deposit maturities with countdown |
| Quick Actions | New Payment, Import Statement, Reconcile, Transfer |

**User Interactions:**
- View balances across all bank accounts
- Click account to view transaction history
- Review and approve pending payment orders
- Check deposit maturity alerts
- Click quick actions to initiate banking operations

**Data Displayed:**
- Total cash and deposit balances
- Per-account balance breakdown
- Recent transaction activity
- Pending payment order status

### 2.2 Screen: Bank Account Management

**Route:** `/ebanking/accounts` | **Purpose:** Configure and manage bank accounts linked to the system.

| UI Component | Description |
|--------------|-------------|
| Account List Table | Account name, bank, account number, currency, balance, status, connection |
| Account Detail View | Full account information, transaction history, balance trend |
| Connection Status | API connection health, last sync timestamp |
| Balance History | Line chart showing balance over time |

**User Interactions:**
- Add new bank accounts with connection configuration
- View account details and transaction history
- Test bank connection connectivity
- Monitor balance trends over time

**Data Displayed:**
- Bank account configuration
- Connection status and health
- Balance history and trends

### 2.3 Screen: Bank Transaction List

**Route:** `/ebanking/transactions` | **Purpose:** Browse, search, and categorize all bank transactions.

| UI Component | Description |
|--------------|-------------|
| Transaction List Table | Date, Account, Description, Debit, Credit, Balance, Category, Status |
| Advanced Filters | Filter by date range, account, amount, category, status |
| Bulk Actions | Categorize, match to invoice, export |
| Uncategorised Count | Highlighted count of uncategorized transactions |

**User Interactions:**
- Search transactions by date, amount, or description
- Filter by account and category
- Categorize uncategorized transactions
- Match transactions to invoices or orders
- Export transaction data

**Data Displayed:**
- Transaction date, description, amounts
- Running balance
- Category assignment
- Matching status to system records

### 2.4 Screen: Bank Statement Import & Reconciliation

**Route:** `/ebanking/reconciliation` | **Purpose:** Import bank statements and reconcile against system transactions.

| UI Component | Description |
|--------------|-------------|
| Import Form | File upload (CSV, Excel, OFX, MT940), account selection, date range |
| Reconciliation Workspace | Side-by-side view: bank statement lines vs. system transactions |
| Matching Engine | Auto-suggest matches based on amount, date, and reference |
| Match Status | Matched, unmatched, partially matched indicators |
| Difference Summary | Total difference amount, unmatched count |
| Complete Button | Finalize reconciliation with variance posting |

**User Interactions:**
- Upload bank statement file for import
- Review auto-suggested matches and confirm or adjust
- Manually match unmatched statement lines to system transactions
- Create adjusting entries for differences
- Complete reconciliation and lock the period

**Data Displayed:**
- Bank statement line items
- System transaction records
- Match suggestions with confidence scores
- Reconciliation difference summary

### 2.5 Screen: Payment Order Management

**Route:** `/ebanking/payments` | **Purpose:** Create, approve, and submit payment orders to banks.

| UI Component | Description |
|--------------|-------------|
| Payment Order List | Order number, date, payee, amount, status, approver |
| Payment Creation Form | Payee, bank account, amount, purpose, priority, execution date |
| Approval Workflow | Current approval stage, approver history, status |
| Submission Status | Bank submission status, reference number, execution confirmation |

**User Interactions:**
- Create new payment orders with payee and amount details
- Submit payment orders for approval
- Approve or reject payment orders (based on role and threshold)
- Track submission status to bank
- View execution confirmation and reference

**Data Displayed:**
- Payment order details and status
- Approval workflow progress
- Bank submission and execution status

### 2.6 Screen: Deposit Management

**Route:** `/ebanking/deposits` | **Purpose:** Manage term deposits and savings accounts with maturity tracking.

| UI Component | Description |
|--------------|-------------|
| Deposit List | Bank, account, principal, interest rate, start date, maturity date, status |
| Deposit Detail | Full deposit information, interest accrual history, maturity projection |
| Maturity Calendar | Calendar view of upcoming deposit maturities |
| Interest Calculator | Projected interest earnings |

**User Interactions:**
- Create new deposit records with terms and rates
- Track interest accrual over time
- View maturity calendar and alerts
- Process deposit renewal or withdrawal at maturity

**Data Displayed:**
- Deposit principal, rate, and term
- Accrued interest to date
- Maturity date and projected value
- Renewal history

### 2.7 Screen: Bank Transfer Payment (in Marketplace)

**Route:** `/ebanking/bank-transfer` | **Purpose:** Process bank transfer payments received from the payment gateway and marketplace.

| UI Component | Description |
|--------------|-------------|
| Transfer List | Reference, date, amount, sender, receiver bank, status, matched order |
| Transfer Detail | Full transfer information, matching to order/invoice |
| Auto-Match Status | Successfully matched, pending review, unmatched |
| Manual Match Form | Link transfer to order or invoice manually |

**User Interactions:**
- View incoming bank transfers from payment gateway
- Review auto-matched transfers to orders
- Manually match unmatched transfers
- Confirm transfer receipt and update order status

**Data Displayed:**
- Bank transfer details (sender, receiver, amount)
- Matching status to orders/invoices
- Auto-match confidence scores

### 2.8 Screen: Bank Connection Configuration

**Route:** `/ebanking/connections` | **Purpose:** Configure and manage bank API connections.

| UI Component | Description |
|--------------|-------------|
| Connection List | Bank name, provider, status, last sync, accounts connected |
| Connection Setup | Bank selection, API credentials, OAuth flow, test connection |
| Sync Configuration | Sync frequency, data scope, notification settings |
| Provider Directory | List of supported banks and connection methods |

**User Interactions:**
- Set up new bank connections via API or OAuth
- Test connection connectivity
- Configure sync frequency and scope
- View supported bank provider directory

**Data Displayed:**
- Connection status and health
- Sync configuration details
- Supported bank providers

---

## 3. Feature Specifications

### 3.1 Feature: Bank Account Management
**Description:** Configure and manage bank accounts with real-time balance tracking and connection health monitoring.

**User Stories:**
- As a Treasury Manager, I want to see all bank accounts and their balances in one place, so that I can manage the company's cash position.
- As a Finance Director, I want to monitor bank connection health, so that I can ensure data is flowing correctly.

**Acceptance Criteria:**
- Given a bank account is configured, when the connection is active, then transactions are imported automatically at the configured frequency.
- Given a bank account balance changes, when the balance is viewed, then the current balance reflects all posted transactions.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Account number | Required, valid format | Account number is required and must be valid |
| Bank selection | Must be from supported provider list | Please select a supported bank |
| Currency | Must be configured currency | Currency must match the bank account currency |

**Edge Cases:**
- Bank connection outages are detected and alerts are sent
- Account number changes (bank migration) require new account setup
- Closed accounts are archived but transaction history is preserved

### 3.2 Feature: Bank Transaction Import & Categorization
**Description:** Import bank transactions via API or file with automatic categorization and manual override.

**User Stories:**
- As an Accountant, I want bank transactions to be imported automatically, so that I don't need to manually enter them.
- As an Accountant, I want transactions to be categorized automatically, so that reporting is accurate.

**Acceptance Criteria:**
- Given the bank API connection is active, when the sync runs, then new transactions are imported and categorized based on rules.
- Given a transaction is uncategorized, when the user assigns a category, then the category is saved and used for future similar transactions.

**Edge Cases:**
- Duplicate transactions (imported via both API and file) are detected and merged
- Large statement files (>10,000 lines) are processed in batches with progress tracking
- Categorization rules learn from user corrections over time

### 3.3 Feature: Bank Reconciliation
**Description:** Reconcile bank statements against system transactions with auto-matching and manual override.

**User Stories:**
- As an Accountant, I want to reconcile bank statements against our records, so that I can verify accuracy.
- As a Finance Director, I want to see reconciliation status, so that I know our records are reliable.

**Acceptance Criteria:**
- Given a bank statement is imported, when the reconciliation engine runs, then transactions are auto-matched based on amount, date, and reference.
- Given reconciliation is complete, when finalized, then the reconciliation status is locked and any difference is posted.

**Edge Cases:**
- Transactions spanning reconciliation periods are handled with cut-off date logic
- Bank fees and interest that have no matching system entry are flagged for journal entry creation
- Reconciliation can be reopened within a configurable grace period

### 3.4 Feature: Payment Order with Approval Workflow
**Description:** Create payment orders with multi-level approval based on amount thresholds.

**User Stories:**
- As a Cashier, I want to create payment orders and submit for approval, so that payments are properly authorized.
- As a Finance Director, I want to approve large payments, so that cash outflow is controlled.

**Acceptance Criteria:**
- Given a payment order is created, when the amount exceeds the configured threshold, then it requires the appropriate approval level.
- Given a payment order is approved, when it is submitted to the bank, then the submission status is tracked until execution confirmation.

**Edge Cases:**
- Urgent payments can be expedited with skip-approval (requires special role)
- Rejected payment orders return to drafter with reason
- Failed bank submissions are retried or returned to draft for correction

### 3.5 Feature: Deposit & Term Deposit Management
**Description:** Track term deposits with maturity alerts, interest accrual, and renewal processing.

**User Stories:**
- As a Treasury Manager, I want to track deposit maturity dates, so that I can plan cash flow.
- As a Finance Director, I want to see interest accrual, so that I can forecast income.

**Acceptance Criteria:**
- Given a deposit is created, when the maturity date approaches (30 days), then an alert is sent to the Treasury Manager.
- Given a deposit matures, when the user processes it, then principal and interest are recorded and the deposit status updates.

**Edge Cases:**
- Auto-renewal deposits are tracked with renewal terms
- Early withdrawal penalties are calculated and recorded
- Interest rate changes during the deposit term are tracked

### 3.6 Feature: Bank Transfer Payment Processing
**Description:** Process incoming bank transfers from payment gateway with auto-matching to orders.

**User Stories:**
- As an Accountant, I want bank transfers to be matched to orders automatically, so that order status updates correctly.
- As a Sales Representative, I want to see when a customer's bank transfer is received, so that I can confirm the order.

**Acceptance Criteria:**
- Given a bank transfer is received from the payment gateway, when the auto-match engine runs, then the transfer is matched to the order based on reference and amount.
- Given a transfer is matched, when confirmed, then the order payment status updates to "paid."

**Edge Cases:**
- Transfers with partial amounts are matched with remaining balance tracking
- Transfers without matching orders are held for manual review
- Duplicate transfers are detected and flagged

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+------------------+       +------------------+       +------------------+
|   BankAccount    |       | BankTransaction  |       |  BankStatement   |
+------------------+       +------------------+       +------------------+
| id (PK)          |       | id (PK)          |       | id (PK)          |
| account_number   |       | account_id(FK)   |       | account_id(FK)   |
| bank_name        |       | transaction_date |       | statement_date   |
| currency         |       | description      |       | opening_balance  |
| balance          |       | debit            |       | closing_balance  |
| status           |       | credit           |       | line_count       |
| connection_id(FK)|       | category_id(FK)  |       | status           |
| created_at       |       | status           |       | imported_at      |
+------------------+       | matched_to_id    |       +------------------+
        |                  +------------------+              |
        v                         |                          v
+------------------+              |                 +------------------+
| BankConnection   |              v                 |BankReconciliation|
+------------------+       +------------------+    +------------------+
| id (PK)          |       |TransactionCategory|   | id (PK)          |
| bank_provider    |       +------------------+   | account_id(FK)   |
| api_config_json  |       | id (PK)          |   | period           |
| status           |       | name             |   | status           |
| last_sync_at     |       | code             |   | matched_count    |
+------------------+       | rule_json        |   | unmatched_count  |
        |                  +------------------+   | difference       |
        v                                         | completed_at     |
+------------------+              |                +------------------+
| BankProvider     |              |
+------------------+              v
| id (PK)          |       +------------------+
| name             |       |  PaymentOrder    |
| connection_type  |       +------------------+
| supported        |       | id (PK)          |
+------------------+       | order_number     |
                           | payee_name       |
        |                  | amount           |
        v                  | bank_account_id  |
+------------------+       | status           |
|  BankBalance     |       | approval_status  |
+------------------+       | approved_by(FK)  |
| id (PK)          |       | submitted_at     |
| account_id(FK)   |       | bank_reference   |
| date             |       | created_at       |
| balance          |       +------------------+
| currency         |
+------------------+              |
        |                         v
        v                  +------------------+
+------------------+       |   BankTransfer   |
|    Deposit       |       +------------------+
+------------------+       | id (PK)          |
| id (PK)          |       | reference        |
| account_id(FK)   |       | amount           |
| principal        |       | sender_bank      |
| interest_rate    |       | receiver_bank    |
| start_date       |       | order_id(FK)     |
| maturity_date    |       | match_status     |
| status           |       | created_at       |
+------------------+       +------------------+
```

### 4.2 Table Schemas

#### Table: bank_account
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| account_name | VARCHAR(200) | NOT NULL | Account display name |
| account_number | VARCHAR(50) | NOT NULL | Bank account number |
| bank_name | VARCHAR(200) | NOT NULL | Bank name |
| bank_provider_id | BIGINT | FK -> bank_provider.id | Bank provider |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Account currency |
| balance | DECIMAL(18,2) | DEFAULT 0 | Current balance |
| available_balance | DECIMAL(18,2) | DEFAULT 0 | Available balance (minus pending) |
| account_type | VARCHAR(30) | NOT NULL, DEFAULT 'checking' | Type: checking, savings, deposit, loan |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, inactive, closed |
| connection_id | BIGINT | FK -> bank_connection.id | API connection |
| company_id | BIGINT | FK -> company.id | Owning company |
| opened_date | DATE | NULL | Account opening date |
| closed_date | DATE | NULL | Account closing date |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_bank_account_number ON bank_account(account_number);
CREATE INDEX idx_bank_account_provider ON bank_account(bank_provider_id);
CREATE INDEX idx_bank_account_status ON bank_account(status);
CREATE INDEX idx_bank_account_company ON bank_account(company_id);
CREATE INDEX idx_bank_account_connection ON bank_account(connection_id);
```

#### Table: bank_transaction
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| account_id | BIGINT | FK -> bank_account.id, NOT NULL | Bank account |
| transaction_date | DATE | NOT NULL | Transaction date |
| value_date | DATE | NULL | Value date |
| description | TEXT | NULL | Transaction description |
| reference | VARCHAR(100) | NULL | Bank reference number |
| debit | DECIMAL(18,2) | DEFAULT 0 | Debit amount |
| credit | DECIMAL(18,2) | DEFAULT 0 | Credit amount |
| running_balance | DECIMAL(18,2) | NULL | Balance after transaction |
| category_id | BIGINT | FK -> transaction_category.id | Transaction category |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'imported' | Status: imported, categorized, matched, reconciled |
| matched_to_type | VARCHAR(50) | NULL | Matched record type: invoice, order, payment_order |
| matched_to_id | BIGINT | NULL | Matched record ID |
| import_source | VARCHAR(30) | DEFAULT 'api' | Source: api, file_import, manual |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_bank_txn_account ON bank_transaction(account_id);
CREATE INDEX idx_bank_txn_date ON bank_transaction(transaction_date);
CREATE INDEX idx_bank_txn_status ON bank_transaction(status);
CREATE INDEX idx_bank_txn_category ON bank_transaction(category_id);
CREATE INDEX idx_bank_txn_reference ON bank_transaction(reference);
CREATE INDEX idx_bank_txn_matched ON bank_transaction(matched_to_type, matched_to_id);
```

#### Table: bank_statement
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| account_id | BIGINT | FK -> bank_account.id, NOT NULL | Bank account |
| statement_date | DATE | NOT NULL | Statement period date |
| opening_balance | DECIMAL(18,2) | NOT NULL | Opening balance |
| closing_balance | DECIMAL(18,2) | NOT NULL | Closing balance |
| total_debits | DECIMAL(18,2) | DEFAULT 0 | Total debits |
| total_credits | DECIMAL(18,2) | DEFAULT 0 | Total credits |
| line_count | INT | DEFAULT 0 | Number of line items |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'imported' | Status: imported, reconciling, reconciled |
| file_url | VARCHAR(500) | NULL | Imported statement file URL |
| import_format | VARCHAR(20) | NULL | Format: csv, excel, ofx, mt940 |
| imported_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Import timestamp |
| imported_by | BIGINT | FK -> users.id | Import user |

**Indexes:**
```sql
CREATE INDEX idx_bank_stmt_account ON bank_statement(account_id);
CREATE INDEX idx_bank_stmt_date ON bank_statement(statement_date);
CREATE INDEX idx_bank_stmt_status ON bank_statement(status);
```

#### Table: bank_reconciliation
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| account_id | BIGINT | FK -> bank_account.id, NOT NULL | Bank account |
| period_from | DATE | NOT NULL | Reconciliation period start |
| period_to | DATE | NOT NULL | Reconciliation period end |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'in_progress' | Status: in_progress, completed, reopened |
| bank_balance | DECIMAL(18,2) | NOT NULL | Bank statement closing balance |
| system_balance | DECIMAL(18,2) | NOT NULL | System ledger balance |
| matched_count | INT | DEFAULT 0 | Number of matched items |
| unmatched_count | INT | DEFAULT 0 | Number of unmatched items |
| difference | DECIMAL(18,2) | DEFAULT 0 | Reconciliation difference |
| completed_at | TIMESTAMP | NULL | Completion timestamp |
| completed_by | BIGINT | FK -> users.id | Reconciliation user |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_bank_recon_account ON bank_reconciliation(account_id);
CREATE INDEX idx_bank_recon_status ON bank_reconciliation(status);
CREATE INDEX idx_bank_recon_period ON bank_reconciliation(period_from, period_to);
```

#### Table: bank_connection
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| bank_provider_id | BIGINT | FK -> bank_provider.id, NOT NULL | Bank provider |
| name | VARCHAR(200) | NOT NULL | Connection display name |
| api_config_json | JSON | NOT NULL | Encrypted API credentials |
| auth_type | VARCHAR(30) | NOT NULL | Auth: oauth2, certificate, api_key |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'inactive' | Status: active, inactive, error |
| last_sync_at | TIMESTAMP | NULL | Last sync timestamp |
| sync_frequency | VARCHAR(30) | DEFAULT 'hourly' | Sync frequency |
| error_message | TEXT | NULL | Last error message |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_bank_conn_provider ON bank_connection(bank_provider_id);
CREATE INDEX idx_bank_conn_status ON bank_connection(status);
CREATE INDEX idx_bank_conn_last_sync ON bank_connection(last_sync_at);
```

#### Table: transaction_category
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Category name |
| code | VARCHAR(30) | NOT NULL, UNIQUE | Category code |
| type | VARCHAR(20) | NOT NULL | Type: income, expense, transfer |
| rule_json | JSON | NULL | Auto-categorization rules |
| is_active | TINYINT(1) | DEFAULT 1 | Active flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_txn_category_code ON transaction_category(code);
CREATE INDEX idx_txn_category_type ON transaction_category(type);
```

#### Table: payment_order
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| order_number | VARCHAR(50) | NOT NULL, UNIQUE | Payment order number |
| payee_name | VARCHAR(300) | NOT NULL | Payee name |
| payee_account | VARCHAR(50) | NULL | Payee bank account |
| payee_bank | VARCHAR(200) | NULL | Payee bank name |
| amount | DECIMAL(18,2) | NOT NULL | Payment amount |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Currency |
| bank_account_id | BIGINT | FK -> bank_account.id | Source bank account |
| purpose | TEXT | NULL | Payment purpose/description |
| priority | VARCHAR(20) | DEFAULT 'normal' | Priority: low, normal, high, urgent |
| execution_date | DATE | NOT NULL | Scheduled execution date |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status: draft, pending_approval, approved, rejected, submitted, executed, failed |
| approval_status | VARCHAR(30) | DEFAULT 'pending' | Approval: pending, approved, rejected |
| approved_by | BIGINT | FK -> users.id | Approving user |
| approved_at | TIMESTAMP | NULL | Approval timestamp |
| submitted_at | TIMESTAMP | NULL | Bank submission timestamp |
| bank_reference | VARCHAR(100) | NULL | Bank reference number |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_payment_order_number ON payment_order(order_number);
CREATE INDEX idx_payment_order_status ON payment_order(status);
CREATE INDEX idx_payment_order_approval ON payment_order(approval_status);
CREATE INDEX idx_payment_order_account ON payment_order(bank_account_id);
CREATE INDEX idx_payment_order_date ON payment_order(execution_date);
```

#### Table: bank_balance
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| account_id | BIGINT | FK -> bank_account.id, NOT NULL | Bank account |
| date | DATE | NOT NULL | Balance date |
| balance | DECIMAL(18,2) | NOT NULL | End-of-day balance |
| currency | VARCHAR(3) | NOT NULL | Currency |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_bank_balance_unique ON bank_balance(account_id, date);
CREATE INDEX idx_bank_balance_date ON bank_balance(date);
```

#### Table: deposit
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| account_id | BIGINT | FK -> bank_account.id, NOT NULL | Associated bank account |
| principal | DECIMAL(18,2) | NOT NULL | Deposit principal amount |
| interest_rate | DECIMAL(5,2) | NOT NULL | Annual interest rate |
| start_date | DATE | NOT NULL | Deposit start date |
| maturity_date | DATE | NOT NULL | Maturity date |
| accrued_interest | DECIMAL(18,2) | DEFAULT 0 | Accrued interest to date |
| auto_renew | TINYINT(1) | DEFAULT 0 | Auto-renewal flag |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, matured, withdrawn, closed |
| maturity_alert_sent | TINYINT(1) | DEFAULT 0 | 30-day alert sent flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_deposit_account ON deposit(account_id);
CREATE INDEX idx_deposit_maturity ON deposit(maturity_date);
CREATE INDEX idx_deposit_status ON deposit(status);
CREATE INDEX idx_deposit_alert ON deposit(maturity_alert_sent);
```

#### Table: bank_transfer
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| reference | VARCHAR(100) | NOT NULL, UNIQUE | Transfer reference |
| amount | DECIMAL(18,2) | NOT NULL | Transfer amount |
| sender_name | VARCHAR(300) | NULL | Sender name |
| sender_bank | VARCHAR(200) | NULL | Sender bank |
| sender_account | VARCHAR(50) | NULL | Sender account number |
| receiver_bank | VARCHAR(200) | NULL | Receiver bank |
| receiver_account | VARCHAR(50) | NULL | Receiver account number |
| order_id | BIGINT | FK -> sales_order.id | Matched order |
| match_status | VARCHAR(30) | NOT NULL, DEFAULT 'unmatched' | Status: unmatched, matched, confirmed, rejected |
| match_confidence | DECIMAL(5,2) | NULL | Auto-match confidence score |
| transfer_date | TIMESTAMP | NOT NULL | Transfer timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_bank_transfer_reference ON bank_transfer(reference);
CREATE INDEX idx_bank_transfer_match ON bank_transfer(match_status);
CREATE INDEX idx_bank_transfer_order ON bank_transfer(order_id);
CREATE INDEX idx_bank_transfer_date ON bank_transfer(transfer_date);
```

#### Table: bank_provider
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Provider display name |
| code | VARCHAR(30) | NOT NULL, UNIQUE | Provider code |
| connection_type | VARCHAR(30) | NOT NULL | Type: api, oauth, file, manual |
| country | VARCHAR(50) | DEFAULT 'Vietnam' | Country |
| is_supported | TINYINT(1) | DEFAULT 1 | Support flag |
| api_documentation_url | VARCHAR(500) | NULL | API docs URL |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_bank_provider_code ON bank_provider(code);
CREATE INDEX idx_bank_provider_supported ON bank_provider(is_supported);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| bank_account | Bank account records | 500 |
| bank_transaction | Bank transaction entries | 5,000,000 |
| bank_statement | Imported statements | 10,000 |
| bank_reconciliation | Reconciliation records | 5,000 |
| bank_connection | Bank API connections | 50 |
| transaction_category | Transaction categories | 100 |
| payment_order | Payment orders | 100,000 |
| bank_balance | Daily balance snapshots | 200,000 |
| deposit | Term deposit records | 2,000 |
| bank_transfer | Bank transfer payments | 500,000 |
| bank_provider | Bank provider directory | 50 |

---

## 5. API Specifications

### 5.1 Bank Account Endpoints

#### GET /api/v1/ebanking/accounts
**Description:** List bank accounts with filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page |
| status | string | No | Filter by status |
| company_id | integer | No | Filter by company |

**Permission:** `ebanking:account:read`

#### GET /api/v1/ebanking/accounts/{id}/balance
**Description:** Get current balance for an account

**Permission:** `ebanking:account:read`

### 5.2 Transaction Endpoints

#### GET /api/v1/ebanking/transactions
**Description:** List bank transactions

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page |
| account_id | integer | No | Filter by account |
| date_from | string | No | Filter by date |
| date_to | string | No | Filter by date |
| category_id | integer | No | Filter by category |
| status | string | No | Filter by status |

**Permission:** `ebanking:transaction:read`

#### PUT /api/v1/ebanking/transactions/{id}/categorize
**Description:** Categorize a transaction

**Request Body:**
```json
{
  "category_id": 5,
  "notes": "Office supplies payment"
}
```

**Permission:** `ebanking:transaction:categorize`

### 5.3 Reconciliation Endpoints

#### POST /api/v1/ebanking/statements/import
**Description:** Import a bank statement

**Request:** Multipart file upload with account_id and date range

**Permission:** `ebanking:statement:import`

#### POST /api/v1/ebanking/reconciliations
**Description:** Start reconciliation for an account

**Permission:** `ebanking:reconciliation:create`

#### POST /api/v1/ebanking/reconciliations/{id}/match
**Description:** Match a statement line to a system transaction

**Permission:** `ebanking:reconciliation:match`

#### POST /api/v1/ebanking/reconciliations/{id}/complete
**Description:** Complete reconciliation

**Permission:** `ebanking:reconciliation:complete`

### 5.4 Payment Order Endpoints

#### POST /api/v1/ebanking/payment-orders
**Description:** Create a payment order

**Permission:** `ebanking:payment-order:create`

#### POST /api/v1/ebanking/payment-orders/{id}/approve
**Description:** Approve a payment order

**Permission:** `ebanking:payment-order:approve`

#### POST /api/v1/ebanking/payment-orders/{id}/submit
**Description:** Submit payment order to bank

**Permission:** `ebanking:payment-order:submit`

### 5.5 Bank Transfer Endpoints

#### GET /api/v1/ebanking/transfers
**Description:** List bank transfers from payment gateway

**Permission:** `ebanking:transfer:read`

#### POST /api/v1/ebanking/transfers/{id}/match
**Description:** Manually match transfer to order

**Permission:** `ebanking:transfer:match`

### 5.6 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/ebanking/accounts | List accounts | ebanking:account:read |
| GET | /api/v1/ebanking/accounts/{id}/balance | Get balance | ebanking:account:read |
| GET | /api/v1/ebanking/transactions | List transactions | ebanking:transaction:read |
| PUT | /api/v1/ebanking/transactions/{id}/categorize | Categorize | ebanking:transaction:categorize |
| POST | /api/v1/ebanking/statements/import | Import statement | ebanking:statement:import |
| POST | /api/v1/ebanking/reconciliations | Start reconciliation | ebanking:reconciliation:create |
| POST | /api/v1/ebanking/reconciliations/{id}/match | Match items | ebanking:reconciliation:match |
| POST | /api/v1/ebanking/reconciliations/{id}/complete | Complete recon | ebanking:reconciliation:complete |
| POST | /api/v1/ebanking/payment-orders | Create payment | ebanking:payment-order:create |
| POST | /api/v1/ebanking/payment-orders/{id}/approve | Approve payment | ebanking:payment-order:approve |
| POST | /api/v1/ebanking/payment-orders/{id}/submit | Submit to bank | ebanking:payment-order:submit |
| GET | /api/v1/ebanking/transfers | List transfers | ebanking:transfer:read |
| POST | /api/v1/ebanking/transfers/{id}/match | Match transfer | ebanking:transfer:match |
| GET | /api/v1/ebanking/deposits | List deposits | ebanking:deposit:read |
| GET | /api/v1/ebanking/connections | List connections | ebanking:connection:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Finance Director | finance_director | Full access to all banking operations and approvals |
| Treasury Manager | treasury_manager | Execute payments, manage accounts, perform reconciliation |
| Accountant | accountant | Record transactions, perform reconciliation, view balances |
| Cashier | cashier | Process payments within limit, view balances |
| Auditor | auditor | Read-only access for compliance review |
| System Administrator | system_admin | Manage bank connections and configuration |

### Permission Matrix
| Role | View Accounts | Create Payment | Approve Payment | Import Statement | Reconcile | Manage Connections |
|------|-------------|---------------|---------------|-----------------|----------|-------------------|
| Finance Director | Yes | Yes | Yes | Yes | Yes | No |
| Treasury Manager | Yes | Yes | Yes (< threshold) | Yes | Yes | No |
| Accountant | Yes | No | No | Yes | Yes | No |
| Cashier | Yes | Yes (< limit) | No | No | No | No |
| Auditor | Yes | No | No | No | Read only | No |
| System Administrator | Yes | No | No | No | No | Yes |

### Row-Level Security
- Users can only access bank accounts assigned to their company.
- Cashiers have payment limits; amounts exceeding their limit require manager approval.
- Auditors have read-only access to all banking data within their audit scope.

---

## 7. State Machine / Workflows

### 7.1 Payment Order Approval Workflow

```
[DRAFT] ──submit──→ [PENDING_APPROVAL] ──approve──→ [APPROVED] ──submit_to_bank──→ [SUBMITTED] ──execute──→ [EXECUTED]
                         │                            │                               │
                         └──reject──→ [REJECTED]      └──fail──→ [FAILED]             └──fail──→ [FAILED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | PENDING_APPROVAL | submit | Cashier/Treasury | Approval notification sent |
| PENDING_APPROVAL | APPROVED | approve | Finance Director | Payment ready for submission |
| PENDING_APPROVAL | REJECTED | reject | Finance Director | Returned to drafter with reason |
| APPROVED | SUBMITTED | submit_to_bank | Treasury Manager | Payment sent to bank API |
| SUBMITTED | EXECUTED | execute | Bank | Bank confirms execution |
| SUBMITTED | FAILED | fail | Bank | Payment failed, notification sent |

### 7.2 Reconciliation Status Transitions

```
[IN_PROGRESS] ──complete──→ [COMPLETED]
     │
     └──reopen──→ [IN_PROGRESS]
```

### 7.3 Bank Transfer Match Status

```
[UNMATCHED] ──auto_match──→ [MATCHED] ──confirm──→ [CONFIRMED]
     │                           │
     └──manual_match─────────────┘
     │
     └──reject──→ [REJECTED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Cash Position Summary
**Type:** Real-time
**Purpose:** Display cash and bank balances across all accounts

**Metrics:**
- Total cash, total deposits
- Balance by account and bank
- Available balance vs. ledger balance

**Chart Type:** Table, Bar chart

### 8.2 Report: Transaction Analysis
**Type:** Real-time
**Purpose:** Analyze bank transactions by category, account, and time period

**Metrics:**
- Transaction volume by category
- Income vs. expense trends
- Top transactions by amount

**Chart Type:** Bar chart, Line chart

### 8.3 Report: Reconciliation Status
**Type:** Real-time
**Purpose:** Monitor bank reconciliation completion and differences

**Metrics:**
- Reconciled vs. unreconciled accounts
- Difference amounts
- Aging of unreconciled items

**Chart Type:** Table, Bar chart

### 8.4 Report: Payment Order Summary
**Type:** Real-time
**Purpose:** Track payment order status and approval pipeline

**Metrics:**
- Payment count by status
- Total approved, pending, executed
- Approval turnaround time

**Chart Type:** Donut chart, Table

### 8.5 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Cash Position Summary | Real-time | Daily | Table, CSV |
| Transaction Analysis | Real-time | Weekly | Bar Chart, Excel |
| Reconciliation Status | Real-time | Monthly | Table |
| Payment Order Summary | Real-time | Weekly | Donut Chart |
| Deposit Maturity Report | Real-time | Monthly | Table |
| Bank Fee Analysis | Real-time | Quarterly | Table |
| Cash Flow Forecast | Real-time | Weekly | Line Chart |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Bank API (Vietcombank, BIDV, VietinBank, etc.) | Transaction feed, balance, payment submission | REST | OAuth 2.0 / Certificate |
| Open Banking API | Standardized bank data access | REST | OAuth 2.0 |
| Exchange Rate API | Foreign currency conversion | REST | API Key |
| MISA Accounting API | Journal entry sync | REST | OAuth 2.0 |
| MISA Payment Gateway API | Bank transfer payment processing | REST | API Key |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| bank_transaction.imported | {account_id, transaction_id, amount, date} | Accounting (M04), Categorization engine |
| payment_order.approved | {order_id, amount, payee} | Bank submission queue |
| payment_order.executed | {order_id, bank_reference} | Accounting (M04), Invoice (M33) |
| reconciliation.completed | {account_id, period, difference} | Finance team notification |
| deposit.maturity_alert | {deposit_id, maturity_date, principal} | Treasury Manager notification |
| bank_transfer.received | {transfer_id, amount, sender} | Order matching engine |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| invoice.due | E-Invoice (M33) | Queue payment order if auto-pay configured |
| order.paid | Payment Gateway (M34) | Create bank transfer record |
| journal.posted | Accounting (M04) | Link to bank transaction if applicable |

---

## 10. Technical Notes

### Performance
- Expected data volume: 5,000,000+ bank transactions, 100,000+ payment orders, 500,000+ bank transfers
- Query complexity: Medium (aggregations for reports, matching algorithms for reconciliation)
- Expected response time: Account list < 500ms, Transaction list < 1s, Balance retrieval < 200ms
- Bank API sync: < 30 seconds per account for transaction import
- Reconciliation auto-match: < 5 seconds for 1,000 statement lines

### Caching Strategy
- Redis cache for account balances, connection status, and transaction categories
- TTL: 1 minute for balances (real-time), 5 minutes for connection status, 1 hour for categories
- Bank statement data cached during reconciliation session
- Cache invalidation on transaction import, payment execution, or reconciliation completion

### File Attachments
- Supported types: CSV, XLSX, OFX, MT940 (bank statement imports)
- Max size: 50 MB per file
- Storage: S3-compatible object storage
- Imported statement files preserved for audit purposes

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Currency: VND with multi-currency support
- Bank names and descriptions in Vietnamese
- MT940 and OFX format support for international banks

### Mobile Considerations
- Mobile dashboard shows account balances and recent transactions
- Payment order approval supported on mobile
- Push notifications for payment order status changes and deposit maturity alerts
- Bank statement import and reconciliation require web browser

### Security
- Bank API credentials encrypted at rest using AES-256
- Payment orders require re-authentication for approval
- All bank API communications use TLS 1.2+ with certificate pinning
- Audit trail captures all payment order actions with user, timestamp, and IP
- Bank transaction data is subject to financial data retention policies (minimum 10 years)
- API rate limiting: 100 requests per minute for read, 20 for write

### Compliance
- Bank reconciliation follows Vietnamese accounting standards
- Payment order approval workflow meets internal control requirements
- Transaction categorization supports State Bank of Vietnam reporting requirements
- Data retention: bank statements and reconciliation records retained for minimum 10 years
- Multi-currency reporting complies with Vietnamese foreign exchange regulations
