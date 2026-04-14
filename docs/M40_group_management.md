# M40 - Group Management (Quan ly Tap doan)

**Category:** Platform
**Priority:** P2
**Reference Images:** assets/image_30.png, assets/image_60.png
**Dependencies:** M03 (Company & Organization), M04 (Accounting), M01 (Authentication), M05 (Employee Management)
**Generated:** April 14, 2026

---

## 1. Module Overview

The Group Management module provides enterprise-grade support for multi-company organizational structures, enabling parent corporations to manage subsidiary companies with consolidated reporting, inter-company transactions, data synchronization, and centralized governance. It is designed for corporate groups where a parent company oversees multiple legally distinct subsidiary entities that operate semi-independently but require consolidated financial reporting, standardized processes, and shared infrastructure.

The group management dashboard (image_30) displays the parent company "Phuong Nam Food Corporation" with its 6 subsidiary companies organized in a hierarchical tree structure. Each subsidiary shows its data synchronization status, indicating whether data has been successfully synced to the consolidated group view. The dashboard prominently displays the message "Chua co du lieu dong bo tu Long An" (No synced data from Long An), highlighting a subsidiary that has not yet established data synchronization. The system admin view (image_60) shows the group structure in the context of overall system administration, with user counts per company, cross-company access patterns, and data consolidation status. The module supports multi-company data isolation while enabling consolidated financial reporting, inter-company transaction tracking, group-wide user management, and standardized configuration across all subsidiaries.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| Group CEO / Director | Oversees entire corporate group, views consolidated reports, sets group policies | Full access to all group-wide data and consolidated reports |
| Subsidiary Director | Manages individual subsidiary operations | Full access to subsidiary data, read access to group reports |
| Group Finance Manager | Prepares consolidated financial reports, manages inter-company reconciliations | Full access to consolidated financial data across all companies |
| Subsidiary Finance Manager | Manages subsidiary financial operations | Full access to subsidiary financial data, read access to group standards |
| Group IT Administrator | Manages group-wide system configuration, data sync, and user provisioning | Full system access across all companies |
| Subsidiary IT Staff | Manages subsidiary-level system configuration | Access limited to subsidiary systems |
| Auditor | Reviews group-wide compliance, inter-company transactions, and consolidated reports | Read-only access to all group and subsidiary data |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M03 - Company & Organization | Company hierarchy, department structures, location data |
| M04 - Accounting | Consolidated financial reporting, inter-company journal entries |
| M01 - Authentication | Group-wide user management, cross-company access control |
| M05 - Employee Management | Employee assignment across subsidiaries, group headcount |
| M19 - Workflow | Cross-company approval workflows |
| M39 - Operations Dashboard | Group-level operations monitoring |

### Key Business Rules

1. The parent company sits at the top of the organizational hierarchy; all subsidiaries roll up to the parent for consolidated reporting.
2. Each subsidiary maintains its own chart of accounts, fiscal year settings, and operational data while following group-standard configurations.
3. Inter-company transactions must be recorded in both companies' books; the system automatically creates the corresponding entry in the counterparty company.
4. Data synchronization from subsidiaries to the group consolidation engine runs on a scheduled basis (daily, weekly, or monthly); sync status is monitored and alerting on failures.
5. Consolidated financial reports include elimination entries to remove inter-company transactions and balances.
6. Users can be assigned to multiple companies within the group; cross-company access is controlled by role assignments per company.
7. Group-standard configurations (chart of accounts, approval workflows, reporting templates) can be pushed from parent to subsidiaries; subsidiaries can have local overrides with parent approval.
8. Subsidiaries that have not completed data sync setup display "No synced data" warnings on the group dashboard.
9. Inter-company receivables and payables are reconciled monthly; unreconciled differences are flagged for resolution.
10. Group hierarchy supports unlimited depth; a subsidiary can itself be a parent to other entities (sub-subsidiaries).

---

## 2. Screen Inventory

### 2.1 Screen: Group Hierarchy Dashboard

**Route:** `/group-management/hierarchy` | **Purpose:** Display the corporate group structure with parent company and subsidiaries, sync status, and consolidated metrics.

| UI Component | Description |
|--------------|-------------|
| Parent Company Header | Phuong Nam Food Corporation - parent company name, logo, tax code |
| Organization Tree | Hierarchical tree view showing parent -> 6 subsidiaries with expandable nodes |
| Sync Status Indicators | Green/yellow/red indicators per subsidiary showing data sync health |
| "No Synced Data" Alert | Warning for Long An subsidiary: "Chua co du lieu dong bo tu Long An" |
| Consolidated Metrics | Group-wide revenue, expenses, employee count, active users |
| Quick Actions | Add Subsidiary, Run Sync, View Consolidated Report, Configure Sync |

**User Interactions:**
- Expand/collapse subsidiary nodes in the org tree
- Click sync status indicator to view sync details
- Click subsidiary to view subsidiary-specific data
- Run manual data sync for specific subsidiaries
- View consolidated group metrics

**Data Displayed:**
- Parent company information
- Subsidiary list with sync status
- Data synchronization warnings
- Group-wide consolidated metrics

### 2.2 Screen: Subsidiary Detail

**Route:** `/group-management/subsidiaries/{id}` | **Purpose:** View and manage a specific subsidiary's configuration and data.

| UI Component | Description |
|--------------|-------------|
| Subsidiary Header | Name, tax code, status, sync status badge |
| Company Information | Address, legal representative, establishment date, industry |
| Sync Configuration | Sync frequency, last sync date, sync scope, field mappings |
| Sync History | Table of recent sync runs with status, records synced, errors |
| Financial Summary | Subsidiary revenue, expenses, profit with group comparison |
| Employee Summary | Headcount, department structure, key positions |

**User Interactions:**
- View subsidiary company details
- Configure data sync settings
- Review sync history and troubleshoot errors
- View subsidiary financial and HR summaries

**Data Displayed:**
- Subsidiary company profile
- Sync configuration and history
- Financial and HR summaries

### 2.3 Screen: Data Synchronization Monitor

**Route:** `/group-management/sync-monitor` | **Purpose:** Monitor and manage data synchronization between subsidiaries and the group consolidation engine.

| UI Component | Description |
|--------------|-------------|
| Sync Overview | Total subsidiaries, synced count, failed count, pending count |
| Per-Subsidiary Status | Table: Subsidiary, Last Sync, Status, Records, Errors, Next Scheduled |
| Sync Log Detail | Expandable log entries with timestamps, record counts, error messages |
| Manual Sync Trigger | Button to initiate immediate sync for selected subsidiaries |
| Sync Configuration | Global sync settings, retry policy, notification recipients |

**User Interactions:**
- View sync status across all subsidiaries
- Drill into sync logs to troubleshoot failures
- Trigger manual sync for specific subsidiaries
- Configure global sync settings and notifications

**Data Displayed:**
- Sync status per subsidiary
- Sync history with detailed logs
- Error messages and resolution hints
- Next scheduled sync times

### 2.4 Screen: Inter-Company Transaction Management

**Route:** `/group-management/inter-company` | **Purpose:** Track and reconcile inter-company transactions and balances.

| UI Component | Description |
|--------------|-------------|
| Transaction List | Date, From Company, To Company, Type, Amount, Status, Reconciliation |
| Transaction Creation | Form: companies, transaction type, amount, description, effective date |
| Reconciliation Status | Matched, unmatched, and difference amounts |
| Balance Summary | Inter-company receivables and payables by company pair |

**User Interactions:**
- Record new inter-company transactions
- View inter-company balances by company pair
- Reconcile inter-company accounts
- Export inter-company transaction reports

**Data Displayed:**
- Inter-company transaction details
- Reconciliation status and differences
- Balance summaries by company pair

### 2.5 Screen: Consolidated Financial Report

**Route:** `/group-management/consolidated-reports` | **Purpose:** Generate and view consolidated financial reports across all subsidiaries.

| UI Component | Description |
|--------------|-------------|
| Report Selector | Balance Sheet, Income Statement, Cash Flow, Trial Balance |
| Period Selector | Month, quarter, year selection |
| Consolidated View | Combined financials with elimination entries |
| Subsidiary Breakdown | Contribution by subsidiary to consolidated totals |
| Comparison View | Consolidated vs. prior period, budget comparison |
| Export | PDF, Excel export with group branding |

**User Interactions:**
- Select report type and period
- View consolidated financials with eliminations
- Drill into subsidiary-level contributions
- Compare consolidated results to prior periods and budgets
- Export reports for distribution

**Data Displayed:**
- Consolidated financial statements
- Elimination entry details
- Subsidiary contribution breakdown
- Period comparison data

### 2.6 Screen: Group Configuration Management

**Route:** `/group-management/configuration` | **Purpose:** Manage group-standard configurations and push to subsidiaries.

| UI Component | Description |
|--------------|-------------|
| Configuration Categories | Chart of accounts, approval workflows, reporting templates, user roles |
| Group Standard | Current group-standard configuration |
| Subsidiary Compliance | Table showing each subsidiary's compliance with group standards |
| Push Configuration | Select configurations to push, select target subsidiaries |
| Override Request | Subsidiary requests for local configuration overrides |

**User Interactions:**
- View and update group-standard configurations
- Push configurations to selected subsidiaries
- Review subsidiary compliance status
- Approve or reject override requests from subsidiaries

**Data Displayed:**
- Group-standard configuration details
- Subsidiary compliance status
- Override request queue

### 2.7 Screen: Cross-Company User Management

**Route:** `/group-management/users` | **Purpose:** Manage user access across multiple companies in the group.

| UI Component | Description |
|--------------|-------------|
| User List | Name, email, companies, roles per company, status |
| Company Assignment | Assign users to multiple companies with role per company |
| Access Summary | Total users per company, cross-company users count |
| Bulk Operations | Bulk role assignment, bulk company assignment |

**User Interactions:**
- Search and filter users across the group
- Assign users to multiple companies
- Configure role per company assignment
- Perform bulk user management operations

**Data Displayed:**
- User list with company assignments
- Role assignments per company
- Access summary statistics

---

## 3. Feature Specifications

### 3.1 Feature: Group Hierarchy Management
**Description:** Manage the corporate group structure with parent company, subsidiaries, and hierarchical relationships.

**User Stories:**
- As a Group IT Administrator, I want to define the group hierarchy with parent and subsidiary companies, so that the system reflects the organizational structure.
- As a Group CEO, I want to view the complete hierarchy, so that I understand the group composition.

**Acceptance Criteria:**
- Given a new subsidiary is added, when the hierarchy is updated, then the new entity appears in the org tree with appropriate roll-up to parent.
- Given a subsidiary is viewed, when the user expands the node, then subsidiary details (sync status, financial summary) are displayed.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Company name | Required, unique within group | Company name is required and must be unique within the group |
| Tax code | Valid 10/13 digit format | Tax code must be a valid 10 or 13 digit number |
| Parent company | Must exist in system | Parent company must be a registered entity |

**Edge Cases:**
- Multi-level hierarchies (sub-subsidiaries) are supported with unlimited depth
- Circular parent-child relationships are detected and prevented
- Deleting a subsidiary with active transactions is blocked

### 3.2 Feature: Data Synchronization
**Description:** Schedule and monitor data synchronization from subsidiaries to the group consolidation engine.

**User Stories:**
- As a Group IT Administrator, I want to configure sync schedules per subsidiary, so that data is consolidated regularly.
- As a Group Finance Manager, I want to see sync status for all subsidiaries, so that I know consolidated reports are complete.

**Acceptance Criteria:**
- Given a sync schedule is configured, when the scheduled time arrives, then data is synced from the subsidiary to the group engine.
- Given a sync fails, when the check runs, then the failure is logged and the subsidiary shows a red sync indicator.

**Edge Cases:**
- Sync failures are retried automatically up to 3 times before alerting
- Large sync payloads (>1GB) are processed in chunks with progress tracking
- Network outages at subsidiary sites are detected and sync is deferred

### 3.3 Feature: Inter-Company Transaction Tracking
**Description:** Record, track, and reconcile inter-company transactions across subsidiaries.

**User Stories:**
- As a Group Finance Manager, I want to record inter-company transactions, so that both companies' books are updated automatically.
- As an Auditor, I want to see unreconciled inter-company differences, so that I can verify financial accuracy.

**Acceptance Criteria:**
- Given an inter-company transaction is recorded, when it is saved, then corresponding entries are created in both companies' ledgers.
- Given inter-company balances are reviewed, when reconciliation runs, then matching transactions are paired and differences are flagged.

**Edge Cases:**
- Currency differences between companies are handled using exchange rate at transaction date
- Inter-company transactions spanning fiscal years are allocated correctly
- Unreconciled differences over threshold require management review

### 3.4 Feature: Consolidated Financial Reporting
**Description:** Generate consolidated financial reports with elimination of inter-company transactions.

**User Stories:**
- As a Group Finance Manager, I want consolidated financial statements, so that I can report group-wide performance.
- As a Group CEO, I want to see subsidiary contributions to consolidated results, so that I can evaluate each entity.

**Acceptance Criteria:**
- Given a consolidated report is generated, when inter-company eliminations are applied, then the report shows only external transactions.
- Given a period is selected, when the report renders, then all subsidiary data is aggregated with eliminations.

**Edge Cases:**
- Subsidiaries with different fiscal year-ends require period alignment
- Elimination entries are documented and auditable
- Partial subsidiary data (incomplete sync) is flagged in the report

### 3.5 Feature: Group-Standard Configuration Push
**Description:** Push standardized configurations from parent to subsidiaries with compliance tracking.

**User Stories:**
- As a Group IT Administrator, I want to push chart of accounts updates to all subsidiaries, so that reporting is standardized.
- As a Subsidiary Finance Manager, I want to request local overrides, so that subsidiary-specific needs are addressed.

**Acceptance Criteria:**
- Given a configuration is pushed, when subsidiaries receive it, then their local settings are updated accordingly.
- Given an override request is submitted, when the parent reviews, then it can be approved or rejected.

**Edge Cases:**
- Configuration pushes that conflict with local data are flagged for manual review
- Override approvals are logged with approver and timestamp
- Rollback of configuration pushes is supported

### 3.6 Feature: Cross-Company User Management
**Description:** Manage user access and roles across multiple companies within the group.

**User Stories:**
- As a Group IT Administrator, I want to assign users to multiple companies, so that shared staff have appropriate access.
- As a Subsidiary Director, I want to see which users have access to my subsidiary, so that I can manage security.

**Acceptance Criteria:**
- Given a user is assigned to multiple companies, when they log in, then they can switch between company contexts.
- Given a user's roles differ by company, when they access a subsidiary, then the appropriate role permissions apply.

**Edge Cases:**
- Users departing the group have access revoked across all companies simultaneously
- Role conflicts across companies are resolved by most restrictive permission
- Cross-company users count toward each company's user license

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+------------------+       +------------------+       +------------------+
|   GroupCompany   |       |    Subsidiary    |       |ConsolidatedData  |
+------------------+       +------------------+       +------------------+
| id (PK)          |       | id (PK)          |       | id (PK)          |
| name             |       | group_id(FK)     |       | period           |
| tax_code         |       | parent_id(FK)    |       | revenue          |
| type             |       | sync_status      |       | expenses         |
| is_parent        |       | last_sync        |       | profit           |
| created_at       |       | sync_config_json |       | eliminations     |
+------------------+       +------------------+       | created_at       |
        |                         |                  +------------------+
        v                         v
+------------------+       +------------------+
|InterCompanyTxn   |       |   DataSyncLog    |
+------------------+       +------------------+
| id (PK)          |       | id (PK)          |
| from_company(FK) |       | subsidiary_id(FK)|
| to_company(FK)   |       | run_number       |
| type             |       | status           |
| amount           |       | records_synced   |
| status           |       | error_details    |
| reconciliation   |       | started_at       |
+------------------+       +------------------+
        |
        v
+------------------+       +------------------+
|  GroupReport     |       | SubsidiaryConfig |
+------------------+       +------------------+
| id (PK)          |       | id (PK)          |
| report_type      |       | subsidiary_id(FK)|
| period           |       | config_key       |
| data_json        |       | config_value     |
| created_at       |       | is_group_std     |
+------------------+       +------------------+
        |
        v
+------------------+
|  GroupHierarchy  |
+------------------+
| id (PK)          |
| parent_id(FK)    |
| child_id(FK)     |
| level            |
+------------------+
```

### 4.2 Table Schemas

#### Table: group_company
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(300) | NOT NULL | Company name |
| tax_code | VARCHAR(20) | NOT NULL, UNIQUE | Tax identification code |
| type | VARCHAR(30) | NOT NULL | Type: parent, subsidiary |
| is_parent | TINYINT(1) | DEFAULT 0 | Parent company flag |
| legal_representative | VARCHAR(200) | NULL | Legal representative name |
| address | TEXT | NULL | Company address |
| industry | VARCHAR(100) | NULL | Industry classification |
| establishment_date | DATE | NULL | Date of establishment |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, inactive, dissolved |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_group_company_tax ON group_company(tax_code);
CREATE INDEX idx_group_company_type ON group_company(type);
CREATE INDEX idx_group_company_parent ON group_company(is_parent);
CREATE INDEX idx_group_company_status ON group_company(status);
```

#### Table: subsidiary
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| group_company_id | BIGINT | FK -> group_company.id, NOT NULL | Associated company |
| parent_id | BIGINT | FK -> group_company.id | Parent company |
| sync_status | VARCHAR(30) | NOT NULL, DEFAULT 'not_configured' | Status: not_configured, pending, synced, failed |
| last_sync_at | TIMESTAMP | NULL | Last successful sync |
| sync_config_json | JSON | NULL | Sync configuration |
| sync_frequency | VARCHAR(30) | DEFAULT 'daily' | Sync frequency: real-time, hourly, daily, weekly, monthly |
| compliance_status | VARCHAR(30) | DEFAULT 'unknown' | Compliance with group standards |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_subsidiary_company ON subsidiary(group_company_id);
CREATE INDEX idx_subsidiary_parent ON subsidiary(parent_id);
CREATE INDEX idx_subsidiary_sync ON subsidiary(sync_status);
CREATE INDEX idx_subsidiary_last_sync ON subsidiary(last_sync_at);
```

#### Table: consolidated_data
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| period_month | INT | NOT NULL | Period month |
| period_year | INT | NOT NULL | Period year |
| revenue | DECIMAL(18,2) | DEFAULT 0 | Consolidated revenue |
| expenses | DECIMAL(18,2) | DEFAULT 0 | Consolidated expenses |
| profit | DECIMAL(18,2) | DEFAULT 0 | Consolidated profit |
| eliminations | DECIMAL(18,2) | DEFAULT 0 | Inter-company eliminations |
| employee_count | INT | DEFAULT 0 | Group headcount |
| subsidiary_count | INT | DEFAULT 0 | Number of subsidiaries |
| data_completeness | DECIMAL(5,2) | DEFAULT 0 | Percentage of subsidiaries synced |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_consolidated_period ON consolidated_data(period_month, period_year);
```

#### Table: inter_company_txn
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| from_company_id | BIGINT | FK -> group_company.id, NOT NULL | Source company |
| to_company_id | BIGINT | FK -> group_company.id, NOT NULL | Destination company |
| type | VARCHAR(50) | NOT NULL | Type: sale, purchase, loan, service, royalty |
| amount | DECIMAL(18,2) | NOT NULL | Transaction amount |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Currency |
| description | TEXT | NULL | Transaction description |
| effective_date | DATE | NOT NULL | Transaction date |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'pending' | Status: pending, posted, reconciled, disputed |
| reconciliation_status | VARCHAR(30) | DEFAULT 'unreconciled' | Matched, unreconciled, difference |
| difference_amount | DECIMAL(18,2) | DEFAULT 0 | Reconciliation difference |
| reference_number | VARCHAR(50) | NULL | Transaction reference |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_ic_txn_from ON inter_company_txn(from_company_id);
CREATE INDEX idx_ic_txn_to ON inter_company_txn(to_company_id);
CREATE INDEX idx_ic_txn_status ON inter_company_txn(status);
CREATE INDEX idx_ic_txn_date ON inter_company_txn(effective_date);
CREATE INDEX idx_ic_txn_recon ON inter_company_txn(reconciliation_status);
```

#### Table: data_sync_log
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| subsidiary_id | BIGINT | FK -> subsidiary.id, NOT NULL | Subsidiary |
| run_number | INT | NOT NULL | Sequential run number |
| status | VARCHAR(30) | NOT NULL | Status: success, failed, partial |
| records_synced | INT | DEFAULT 0 | Number of records synced |
| error_count | INT | DEFAULT 0 | Number of errors |
| error_details | TEXT | NULL | Error messages |
| started_at | TIMESTAMP | NOT NULL | Sync start timestamp |
| completed_at | TIMESTAMP | NULL | Sync completion timestamp |
| duration_seconds | INT | NULL | Sync duration |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_sync_log_subsidiary ON data_sync_log(subsidiary_id);
CREATE INDEX idx_sync_log_status ON data_sync_log(status);
CREATE INDEX idx_sync_log_started ON data_sync_log(started_at);
CREATE INDEX idx_sync_log_run ON data_sync_log(subsidiary_id, run_number);
```

#### Table: group_report
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| report_type | VARCHAR(50) | NOT NULL | Type: balance_sheet, income_statement, cash_flow, trial_balance |
| period_month | INT | NOT NULL | Period month |
| period_year | INT | NOT NULL | Period year |
| data_json | JSON | NOT NULL | Report data |
| elimination_entries_json | JSON | NULL | Elimination entry details |
| subsidiary_breakdown_json | JSON | NULL | Per-subsidiary breakdown |
| generated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Generation timestamp |
| generated_by | BIGINT | FK -> users.id | Report generator |

**Indexes:**
```sql
CREATE INDEX idx_group_report_type ON group_report(report_type);
CREATE INDEX idx_group_report_period ON group_report(period_month, period_year);
```

#### Table: subsidiary_config
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| subsidiary_id | BIGINT | FK -> subsidiary.id, NOT NULL | Subsidiary |
| config_key | VARCHAR(100) | NOT NULL | Configuration key |
| config_value | TEXT | NULL | Configuration value |
| is_group_standard | TINYINT(1) | DEFAULT 0 | Whether this is group-standard |
| override_requested | TINYINT(1) | DEFAULT 0 | Override request flag |
| override_approved | TINYINT(1) | DEFAULT 0 | Override approval flag |
| pushed_at | TIMESTAMP | NULL | When pushed from group |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_sub_config_unique ON subsidiary_config(subsidiary_id, config_key);
CREATE INDEX idx_sub_config_group ON subsidiary_config(is_group_standard);
```

#### Table: group_hierarchy
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| parent_id | BIGINT | FK -> group_company.id, NOT NULL | Parent company |
| child_id | BIGINT | FK -> group_company.id, NOT NULL | Child company |
| level | INT | NOT NULL | Hierarchy level (0 = parent, 1 = direct subsidiary) |
| sort_order | INT | DEFAULT 0 | Display order |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_group_hierarchy_unique ON group_hierarchy(parent_id, child_id);
CREATE INDEX idx_group_hierarchy_parent ON group_hierarchy(parent_id);
CREATE INDEX idx_group_hierarchy_child ON group_hierarchy(child_id);
CREATE INDEX idx_group_hierarchy_level ON group_hierarchy(level);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| group_company | Company records | 50 |
| subsidiary | Subsidiary configurations | 50 |
| consolidated_data | Consolidated financial data | 500 |
| inter_company_txn | Inter-company transactions | 10,000 |
| data_sync_log | Sync run history | 50,000 |
| group_report | Consolidated reports | 2,000 |
| subsidiary_config | Subsidiary configurations | 5,000 |
| group_hierarchy | Hierarchy relationships | 100 |

---

## 5. API Specifications

### 5.1 Group Hierarchy Endpoints

#### GET /api/v1/group-management/companies
**Description:** List all companies in the group

**Permission:** `group:company:read`

#### POST /api/v1/group-management/companies
**Description:** Add a new subsidiary

**Permission:** `group:company:create`

#### GET /api/v1/group-management/hierarchy
**Description:** Get complete group hierarchy tree

**Response (200 OK):**
```json
{
  "parent": {
    "id": 1,
    "name": "Phuong Nam Food Corporation",
    "tax_code": "0123456789",
    "children": [
      {"id": 2, "name": "Long An Subsidiary", "sync_status": "failed"},
      {"id": 3, "name": "Dong Nai Subsidiary", "sync_status": "synced"}
    ]
  }
}
```

**Permission:** `group:hierarchy:read`

### 5.2 Sync Management Endpoints

#### GET /api/v1/group-management/sync-status
**Description:** Get sync status for all subsidiaries

**Permission:** `group:sync:read`

#### POST /api/v1/group-management/subsidiaries/{id}/sync
**Description:** Trigger manual sync for a subsidiary

**Permission:** `group:sync:execute`

#### GET /api/v1/group-management/sync-logs
**Description:** Get sync history with filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subsidiary_id | integer | No | Filter by subsidiary |
| date_from | string | No | Filter by date |
| status | string | No | Filter by status |

**Permission:** `group:sync:read`

### 5.3 Inter-Company Transaction Endpoints

#### POST /api/v1/group-management/inter-company-transactions
**Description:** Record inter-company transaction

**Permission:** `group:inter-company:create`

#### GET /api/v1/group-management/inter-company-transactions
**Description:** List inter-company transactions

**Permission:** `group:inter-company:read`

#### POST /api/v1/group-management/inter-company-transactions/{id}/reconcile
**Description:** Reconcile inter-company transaction

**Permission:** `group:inter-company:reconcile`

### 5.4 Consolidated Report Endpoints

#### GET /api/v1/group-management/consolidated-reports
**Description:** Generate consolidated financial report

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| report_type | string | Yes | Report type |
| period_month | integer | Yes | Period month |
| period_year | integer | Yes | Period year |

**Permission:** `group:report:read`

### 5.5 Configuration Endpoints

#### POST /api/v1/group-management/configuration/push
**Description:** Push group-standard configuration to subsidiaries

**Permission:** `group:config:push`

#### POST /api/v1/group-management/configuration/override-request
**Description:** Request configuration override

**Permission:** `group:config:override`

### 5.6 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/group-management/companies | List companies | group:company:read |
| POST | /api/v1/group-management/companies | Add company | group:company:create |
| GET | /api/v1/group-management/hierarchy | Get hierarchy | group:hierarchy:read |
| GET | /api/v1/group-management/sync-status | Sync status | group:sync:read |
| POST | /api/v1/group-management/subsidiaries/{id}/sync | Trigger sync | group:sync:execute |
| GET | /api/v1/group-management/sync-logs | Sync history | group:sync:read |
| POST | /api/v1/group-management/inter-company-transactions | Record txn | group:inter-company:create |
| GET | /api/v1/group-management/inter-company-transactions | List txns | group:inter-company:read |
| POST | /api/v1/group-management/inter-company-transactions/{id}/reconcile | Reconcile | group:inter-company:reconcile |
| GET | /api/v1/group-management/consolidated-reports | Get report | group:report:read |
| POST | /api/v1/group-management/configuration/push | Push config | group:config:push |
| POST | /api/v1/group-management/configuration/override-request | Request override | group:config:override |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Group Director | group_director | Full access to all group management features |
| Subsidiary Director | subsidiary_director | Full access to own subsidiary, read to group reports |
| Group Finance Manager | group_finance_manager | Full access to consolidated reports and inter-company |
| Subsidiary Finance Manager | subsidiary_finance_manager | Access to own subsidiary financials only |
| Group IT Admin | group_it_admin | Full access to sync, configuration, and user management |
| Auditor | auditor | Read-only access to all group data |

### Permission Matrix
| Role | Manage Hierarchy | View Consolidated | Manage Sync | Inter-Company | Push Config | Cross-Company Users |
|------|-----------------|-------------------|------------|--------------|------------|-------------------|
| Group Director | Yes | Yes | Yes | Yes | Yes | Yes |
| Subsidiary Director | No | Read only | No | Own only | No | Own subsidiary |
| Group Finance Manager | No | Yes | No | Yes | No | No |
| Subsidiary Finance Manager | No | No | No | Own only | No | No |
| Group IT Admin | Yes | Read only | Yes | No | Yes | Yes |
| Auditor | No | Read only | Read only | Read only | Read only | Read only |

### Row-Level Security
- Subsidiary Directors and Finance Managers are restricted to their own subsidiary's data.
- Group-level roles have access across all subsidiaries.
- Consolidated reports are visible only to Group Director, Group Finance Manager, and Auditor.
- Sync configuration is managed by Group IT Admin only.

---

## 7. State Machine / Workflows

### 7.1 Data Sync Status Transitions

```
[NOT_CONFIGURED] ──configure──→ [PENDING] ──sync──→ [SYNCED]
                                     │                │
                                     └──fail──→ [FAILED] ──retry──→ [PENDING]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| NOT_CONFIGURED | PENDING | configure | IT Admin | Sync settings saved |
| PENDING | SYNCED | sync | System | Data consolidated, metrics updated |
| PENDING | FAILED | fail | System | Error logged, alert sent |
| FAILED | PENDING | retry | System | Automatic retry (max 3) |

### 7.2 Inter-Company Transaction Status

```
[PENDING] ──post──→ [POSTED] ──reconcile──→ [RECONCILED]
     │                  │                       │
     └──cancel──→ [CANCELLED]                   └──dispute──→ [DISPUTED]
```

### 7.3 Configuration Push Workflow

```
[GROUP_STANDARD] ──push──→ [PUSHED] ──subsidiary apply──→ [APPLIED]
                                │
                                └──override_request──→ [OVERRIDE_PENDING]
                                                       │
                                                       └──approve──→ [OVERRIDE_APPLIED]
                                                       │
                                                       └──reject──→ [APPLIED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Group Hierarchy Overview
**Type:** Real-time
**Purpose:** Display group structure with sync status and consolidated metrics

**Metrics:**
- Subsidiary count, sync status breakdown
- Consolidated revenue, expenses, profit
- Data completeness percentage

**Chart Type:** Org tree, Table

### 8.2 Report: Consolidated Financial Statement
**Type:** Scheduled
**Purpose:** Generate consolidated balance sheet, income statement, cash flow

**Metrics:**
- Consolidated totals with eliminations
- Subsidiary contribution breakdown
- Prior period comparison

**Chart Type:** Table, Bar chart

### 8.3 Report: Inter-Company Reconciliation
**Type:** Real-time
**Purpose:** Track inter-company transaction reconciliation status

**Metrics:**
- Matched vs. unreconciled counts
- Difference amounts by company pair
- Aging of unreconciled items

**Chart Type:** Table, Bar chart

### 8.4 Report: Sync Performance
**Type:** Real-time
**Purpose:** Monitor data sync reliability across subsidiaries

**Metrics:**
- Sync success rate per subsidiary
- Average sync duration
- Error frequency

**Chart Type:** Table, Line chart

### 8.5 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Group Hierarchy Overview | Real-time | On-demand | Org Tree, CSV |
| Consolidated Financial Statement | Scheduled | Monthly | Table, PDF, Excel |
| Inter-Company Reconciliation | Real-time | Monthly | Table |
| Sync Performance | Real-time | Weekly | Table |
| Configuration Compliance | Real-time | Monthly | Table |
| Cross-Company User Access | Real-time | On-demand | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| M04 Accounting API (per subsidiary) | Financial data consolidation | REST | OAuth 2.0 |
| M05 Employee API (per subsidiary) | Headcount data aggregation | REST | OAuth 2.0 |
| M01 Auth API (per subsidiary) | User access synchronization | REST | OAuth 2.0 |
| Exchange Rate API | Currency conversion for multi-currency groups | REST | API Key |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| sync.completed | {subsidiary_id, records_synced, status} | Operations Dashboard (M39) |
| sync.failed | {subsidiary_id, error} | Group IT Admin notification |
| inter-company.posted | {txn_id, from_company, to_company, amount} | Accounting (M04) |
| config.pushed | {config_key, subsidiary_ids} | Subsidiary IT notification |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| journal.posted | Accounting (M04) per subsidiary | Check for inter-company entries |
| user.created | Auth (M01) | Sync user to group directory |
| company.activated | Organization (M03) | Add to group hierarchy |

---

## 10. Technical Notes

### Performance
- Expected data volume: 50+ companies, 10,000+ inter-company transactions, 50,000+ sync log entries
- Query complexity: High (multi-company aggregations, elimination calculations)
- Expected response time: Hierarchy load < 1s, Consolidated report < 5s, Sync status < 500ms
- Sync jobs: Per-subsidiary jobs running on staggered schedules to avoid resource contention
- Large subsidiary data syncs (>1GB) processed in parallel chunks

### Caching Strategy
- Redis cache for group hierarchy, sync status, and consolidated metrics
- TTL: 15 minutes for hierarchy, 5 minutes for sync status, 1 hour for consolidated reports
- Consolidated report data cached per period with invalidation on new sync
- Cache invalidation on sync completion, inter-company transaction posting, or config push

### File Attachments
- Consolidated report exports: PDF, Excel
- Sync configuration files: JSON
- Max file size: 10 MB
- Storage: S3-compatible object storage

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Currency: VND with multi-currency support for international subsidiaries
- Company names and addresses support Vietnamese characters

### Mobile Considerations
- Group hierarchy viewable on mobile with collapsible tree
- Sync status alerts via push notification
- Consolidated financial summaries viewable on mobile
- Full configuration management requires web browser

### Security
- Data isolation: each subsidiary's data is logically separated with company-scoped queries
- Cross-company access requires explicit role assignment per company
- Inter-company transactions are auditable with dual-entry enforcement
- Sync credentials stored in encrypted vault per subsidiary
- API rate limiting: 100 requests per minute for read, 20 for write
- Audit trail captures all group-level configuration changes

### Scalability
- Sync engine supports parallel processing across multiple subsidiaries
- Consolidated report generation uses pre-aggregated data for fast rendering
- Inter-company reconciliation runs as a scheduled batch job
- Group hierarchy supports unlimited depth with recursive query optimization
- Multi-tenant architecture allows adding subsidiaries without system changes
