# M36 - Promotion Management (Quan ly Khuyen mai)

**Category:** Business
**Priority:** P3
**Reference Images:** assets/image_33.png
**Dependencies:** M08 (Sales), M04 (Accounting), M37 (Store & POS)
**Generated:** April 14, 2026

---

## 1. Module Overview

The Promotion Management module provides comprehensive tools for designing, executing, tracking, and analyzing promotional programs and discount campaigns across all sales channels. It enables businesses to create structured promotion programs with specific conditions, schedules, budgets, and product-level discount rules, then track their performance in real-time through detailed reporting dashboards.

The promotion dashboard (image_33) presents a detailed view of active promotion programs with filtering capabilities by time period, date range, program name, and product. A pie chart visualizes promotion distribution by promotion code, showing the relative usage of each code. The detailed table displays individual promotion items with their associated metrics: for example, HH1834 (Standard 1st year) shows 93 units sold, 30 orders, and 16M VND revenue; HH1835 (Standard 2nd+ year) shows 61 units, 70 orders, and 25M VND; HH1836 (Professional 1st year) shows 61 units, 53 orders, and 54M VND; HH1837 (Professional 2nd+ year) shows 36 units, 42 orders, and 26M VND. The totals across all promotion items are 251 units, 195 orders, and 121M VND in revenue. The module integrates with the Sales module (M08) for applying discounts at order creation, the Accounting module (M04) for tracking promotion costs and revenue impact, and the Store & POS module (M37) for in-store promotion application.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| Marketing Manager | Designs and manages all promotional programs and budgets | Full access to promotion creation, configuration, and reporting |
| Sales Manager | Views active promotions, applies to sales orders, monitors performance | Read access to promotions, apply capability on orders |
| Finance Accountant | Tracks promotion costs, revenue impact, and budget utilization | Read access to all promotion financial data |
| Store Manager | Applies promotions at POS, monitors in-store promotion usage | Read/write for POS-level promotion application |
| Business Analyst | Analyzes promotion effectiveness, generates performance reports | Read access to all promotion data and reports |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M08 - Sales | Discounts applied during order creation; revenue attribution |
| M04 - Accounting | Promotion costs posted as expenses; revenue impact tracking |
| M37 - Store & POS | In-store promotion application and tracking |
| M31 - AI Marketing | Campaign-linked promotions for marketing attribution |
| M01 - Authentication | User access control and role-based permissions |

### Key Business Rules

1. Each promotion program must have a defined start date, end date, budget, and set of applicable products or product categories.
2. Promotion codes must be unique across all active programs; expired codes are archived and cannot be reused within the same fiscal year.
3. Discounts can be configured as percentage-based, fixed amount, buy-X-get-Y, or tiered (volume-based); only one discount type applies per order line unless explicitly stacked.
4. Promotion budgets are enforced at the program level; once the budget is exhausted, the program automatically pauses and notifications are sent.
5. Promotion conditions can include: minimum order value, specific products, customer segments, date ranges, purchase history, and channel restrictions.
6. Promotion performance is tracked in real-time: units sold, order count, revenue generated, and discount cost are updated with each qualifying transaction.
7. Promotion reports are available in real-time with filtering by program, product, time period, and channel; historical data is preserved for trend analysis.
8. Promotion schedules can be configured for recurring programs (e.g., monthly, quarterly) with automatic activation and deactivation.
9. Stacked promotions (multiple discounts on same order) require explicit configuration and are subject to maximum discount cap (default: 50% of order value).
10. Promotion data must be retained for minimum 3 years for audit and performance analysis purposes.

---

## 2. Screen Inventory

### 2.1 Screen: Promotion Dashboard

**Route:** `/promotions/dashboard` | **Purpose:** Display promotion program overview with filtering, pie chart by code, and detailed performance table.

| UI Component | Description |
|--------------|-------------|
| Filter Bar | Filter by time period, date range, program name, product |
| Pie Chart | Distribution of promotion usage by code (HH1834, HH1835, HH1836, HH1837) |
| Summary Cards | Total units (251), Total orders (195), Total revenue (121M) |
| Performance Table | Detailed rows per promotion item: code, name, units, orders, revenue |
| Budget Utilization | Budget spent vs. total for each program |
| Quick Actions | New Program, New Promotion Code, Import Promotions |

**User Interactions:**
- Apply filters to narrow down promotion data
- Click pie chart segments to filter by specific code
- Click table rows to view program detail
- View budget utilization and spending trends
- Click quick actions to create new programs

**Data Displayed:**
- HH1834: Standard 1st year - 93 units, 30 orders, 16M
- HH1835: Standard 2nd+ year - 61 units, 70 orders, 25M
- HH1836: Professional 1st year - 61 units, 53 orders, 54M
- HH1837: Professional 2nd+ year - 36 units, 42 orders, 26M
- Totals: 251 units, 195 orders, 121M

### 2.2 Screen: Promotion Program List

**Route:** `/promotions/programs` | **Purpose:** Browse, search, and manage promotion programs.

| UI Component | Description |
|--------------|-------------|
| Program List Table | Columns: Code, Name, Type, Status, Start Date, End Date, Budget, Units, Revenue |
| Status Filters | Active, Scheduled, Paused, Completed, Cancelled |
| Type Filters | Percentage, Fixed Amount, Buy-X-Get-Y, Tiered, Bundle |
| Search | Search by code, name, or product |

**User Interactions:**
- Search programs by code or name
- Filter by status and type
- Click program to view full details and performance
- Activate, pause, or cancel programs

**Data Displayed:**
- Program identification (code, name, type)
- Schedule and budget information
- Performance metrics (units, orders, revenue)

### 2.3 Screen: Promotion Program Detail

**Route:** `/promotions/programs/{id}` | **Purpose:** View and edit a single promotion program with full configuration.

| UI Component | Description |
|--------------|-------------|
| Program Header | Code, name, status badge, type |
| General Information | Description, start/end dates, budget, priority |
| Conditions Section | Minimum order value, applicable products, customer segments, channel restrictions |
| Discount Configuration | Type (percentage/fixed/tiered), value, stacking rules, maximum discount cap |
| Schedule Section | Recurrence pattern, auto-activation settings |
| Performance Panel | Real-time metrics: units, orders, revenue, discount cost, ROI |
| Promotion Codes Table | Associated codes with usage counts |
| Action Bar | Activate, Pause, Edit, Clone, Archive |

**User Interactions:**
- Edit program configuration while in draft or paused status
- View real-time performance metrics
- Manage associated promotion codes
- Clone program for reuse
- Archive completed programs

**Data Displayed:**
- Complete program configuration
- Condition and discount rules
- Performance data and ROI
- Code-level usage statistics

### 2.4 Screen: Promotion Code Management

**Route:** `/promotions/codes` | **Purpose:** Create and manage promotion codes within programs.

| UI Component | Description |
|--------------|-------------|
| Code List Table | Code, Program, Usage Count, Revenue, Status, Expiry |
| Code Generator | Bulk code generation with prefix, pattern, quantity |
| Usage Tracking | Per-code usage statistics |
| Validation Rules | Code format, character restrictions, length |

**User Interactions:**
- Generate promotion codes in bulk
- Configure individual code settings
- Track usage per code
- Deactivate specific codes

**Data Displayed:**
- Code values and usage counts
- Associated program information
- Expiry and status information

### 2.5 Screen: Promotion Budget Management

**Route:** `/promotions/budgets` | **Purpose:** Configure and monitor promotion program budgets.

| UI Component | Description |
|--------------|-------------|
| Budget List | Program, Total Budget, Spent, Remaining, Utilization %, Status |
| Budget Allocation | Allocate budget across channels or products |
| Alert Configuration | Budget threshold alerts (80%, 90%, 100%) |
| Spending Timeline | Cumulative spending over program duration |

**User Interactions:**
- Set and adjust program budgets
- Configure budget alerts
- View spending timeline and utilization
- Allocate budget across sub-categories

**Data Displayed:**
- Budget amounts and utilization
- Spending trends over time
- Alert status and history

### 2.6 Screen: Promotion Type Configuration

**Route:** `/promotions/types` | **Purpose:** Define and configure promotion discount types and rules.

| UI Component | Description |
|--------------|-------------|
| Type List | Percentage, Fixed Amount, Buy-X-Get-Y, Tiered, Bundle |
| Rule Editor | Configure discount rules per type |
| Stacking Configuration | Define which types can be combined |
| Maximum Discount Cap | Set overall discount limits |

**User Interactions:**
- Create custom promotion types
- Configure discount calculation rules
- Set stacking and capping rules
- Test promotion calculations

**Data Displayed:**
- Promotion type definitions
- Rule configurations
- Stacking and cap settings

---

## 3. Feature Specifications

### 3.1 Feature: Promotion Program Management
**Description:** Create, configure, and manage promotion programs with conditions, schedules, and discount rules.

**User Stories:**
- As a Marketing Manager, I want to create promotion programs with specific conditions and discount rules, so that I can drive sales during targeted periods.
- As a Sales Manager, I want to view active promotions, so that I can apply them to customer orders.

**Acceptance Criteria:**
- Given a promotion program is created, when conditions and discount rules are configured, then the program is ready for activation.
- Given an active promotion program, when a qualifying order is created, then the discount is automatically calculated and applied.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Program code | Unique, alphanumeric with hyphens | Code must be unique and contain only letters, numbers, and hyphens |
| Start date | Must be before end date | Start date must be before end date |
| Budget | Must be positive | Budget must be greater than zero |
| Discount value | Percentage: 1-100, Fixed: positive | Discount value is invalid for the selected type |

**Edge Cases:**
- Programs with overlapping date ranges are allowed but stacking rules determine combined application
- Budget exhaustion automatically pauses the program with notification
- Retroactive program activation does not apply to past orders

### 3.2 Feature: Promotion Code Generation
**Description:** Generate and manage promotion codes with usage tracking and validation.

**User Stories:**
- As a Marketing Manager, I want to generate promotion codes in bulk, so that I can distribute them across channels.
- As a Business Analyst, I want to track usage per code, so that I can measure channel effectiveness.

**Acceptance Criteria:**
- Given a bulk code generation request, when the generator runs, then unique codes are created with the specified prefix and pattern.
- Given a promotion code is used, when the order completes, then the usage count increments and revenue is attributed.

**Edge Cases:**
- Code generation with 10,000+ codes is processed asynchronously
- Expired codes are archived and cannot be reused within the same fiscal year
- Invalid code formats are rejected at creation time

### 3.3 Feature: Promotion Budget Tracking
**Description:** Monitor promotion budgets with real-time spending tracking and alert configuration.

**User Stories:**
- As a Finance Accountant, I want to track promotion spending against budgets, so that I can control marketing costs.
- As a Marketing Manager, I want budget threshold alerts, so that I can adjust programs before overspending.

**Acceptance Criteria:**
- Given a promotion program has a budget, when qualifying orders are processed, then the spent amount increments in real-time.
- Given spending reaches a configured threshold (80%, 90%, 100%), when the check runs, then notifications are sent to configured recipients.

**Edge Cases:**
- Budget overruns are blocked by default but can be configured to allow with warning
- Budget adjustments retroactively change utilization calculations
- Multi-channel budget allocation supports reallocation between channels

### 3.4 Feature: Promotion Condition Engine
**Description:** Evaluate promotion conditions against orders to determine discount eligibility.

**User Stories:**
- As a Marketing Manager, I want to set complex conditions (minimum order value, specific products, customer segments), so that promotions target the right audience.
- As a Sales Manager, I want automatic discount application when conditions are met, so that checkout is seamless.

**Acceptance Criteria:**
- Given an order is being created, when the condition engine evaluates, then all applicable promotions are identified and the best discount is applied.
- Given multiple promotions qualify, when stacking rules are checked, then compatible promotions are combined within the maximum discount cap.

**Edge Cases:**
- Conflicting promotions (same product, different discounts) are resolved by priority and best-value rules
- Condition evaluation is cached for performance with 5-minute TTL
- Manual override allows sales staff to apply or remove promotions on specific orders

### 3.5 Feature: Promotion Performance Reporting
**Description:** Track and report on promotion program performance with real-time metrics.

**User Stories:**
- As a Business Analyst, I want to see units, orders, and revenue per promotion, so that I can evaluate program effectiveness.
- As a Marketing Manager, I want to compare promotion ROI, so that I can optimize future programs.

**Acceptance Criteria:**
- Given a promotion program is active, when performance data is requested, then units sold, order count, revenue, and discount cost are displayed.
- Given a date range filter is applied, when the report is generated, then data is filtered to the specified period.

**Edge Cases:**
- Real-time metrics may have a 30-second delay during high-volume periods
- Historical reports preserve data even after programs are archived
- Export of large reports (>100,000 rows) is processed asynchronously

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+------------------+       +------------------+       +------------------+
| PromotionProgram |       |  PromotionItem   |       | PromotionDiscount|
+------------------+       +------------------+       +------------------+
| id (PK)          |<----->| id (PK)          |       | id (PK)          |
| code             |       | program_id(FK)   |       | program_id(FK)   |
| name             |       | product_id(FK)   |       | type             |
| type             |       | units_sold       |       | value            |
| status           |       | order_count      |       | max_discount     |
| start_date       |       | revenue          |       | stacking_allowed |
| end_date         |       +------------------+       +------------------+
| budget           |
| spent            |       +------------------+
| priority         |       | PromotionReport  |
| created_at       |       +------------------+
+------------------+       | id (PK)          |
        |                  | program_id(FK)   |
        v                  | metric_type      |
+------------------+       | metric_value     |
|PromotionCondition|       | snapshot_date    |
+------------------+       +------------------+
| id (PK)          |
| program_id(FK)   |       +------------------+
| condition_type   |       | PromotionSchedule|
| condition_value  |       +------------------+
| operator         |       | id (PK)          |
+------------------+       | program_id(FK)   |
        |                  | recurrence       |
        v                  | auto_activate    |
+------------------+       +------------------+
| PromotionBudget  |
+------------------+              |
| id (PK)          |              v
| program_id(FK)   |       +------------------+
| total_budget     |       |  PromotionType   |
| allocated_json   |       +------------------+
| alert_thresholds |       | id (PK)          |
+------------------+       | name             |
                           | code             |
                           | rules_json       |
                           +------------------+
```

### 4.2 Table Schemas

#### Table: promotion_program
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| code | VARCHAR(50) | NOT NULL, UNIQUE | Program code |
| name | VARCHAR(300) | NOT NULL | Program name |
| type | VARCHAR(30) | NOT NULL | Promotion type |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status: draft, scheduled, active, paused, completed, cancelled |
| description | TEXT | NULL | Program description |
| start_date | TIMESTAMP | NOT NULL | Program start date |
| end_date | TIMESTAMP | NOT NULL | Program end date |
| budget | DECIMAL(18,2) | NOT NULL | Total budget |
| spent | DECIMAL(18,2) | DEFAULT 0 | Amount spent |
| priority | INT | DEFAULT 0 | Priority (higher = applied first) |
| max_discount_cap | DECIMAL(5,2) | DEFAULT 50 | Maximum discount percentage cap |
| stacking_allowed | TINYINT(1) | DEFAULT 0 | Allow stacking with other promotions |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_promo_program_code ON promotion_program(code);
CREATE INDEX idx_promo_program_status ON promotion_program(status);
CREATE INDEX idx_promo_program_dates ON promotion_program(start_date, end_date);
CREATE INDEX idx_promo_program_type ON promotion_program(type);
```

#### Table: promotion_item
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| program_id | BIGINT | FK -> promotion_program.id, NOT NULL | Parent program |
| promotion_code | VARCHAR(50) | NOT NULL | Specific promotion code (e.g., HH1834) |
| name | VARCHAR(300) | NOT NULL | Item name (e.g., "Standard 1st year") |
| product_id | BIGINT | FK -> product.id | Applicable product |
| units_sold | INT | DEFAULT 0 | Total units sold with this promotion |
| order_count | INT | DEFAULT 0 | Total orders using this promotion |
| revenue | DECIMAL(18,2) | DEFAULT 0 | Total revenue attributed |
| discount_cost | DECIMAL(18,2) | DEFAULT 0 | Total discount cost |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_promo_item_program ON promotion_item(program_id);
CREATE INDEX idx_promo_item_code ON promotion_item(promotion_code);
CREATE INDEX idx_promo_item_product ON promotion_item(product_id);
```

#### Table: promotion_discount
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| program_id | BIGINT | FK -> promotion_program.id, NOT NULL | Parent program |
| type | VARCHAR(30) | NOT NULL | Type: percentage, fixed, buy_x_get_y, tiered, bundle |
| value | DECIMAL(18,2) | NOT NULL | Discount value (percentage or amount) |
| min_order_value | DECIMAL(18,2) | NULL | Minimum order value to qualify |
| max_discount | DECIMAL(18,2) | NULL | Maximum discount per order |
| stacking_allowed | TINYINT(1) | DEFAULT 0 | Can stack with other promotions |
| applicable_products_json | JSON | NULL | Product/category restrictions |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_promo_discount_program ON promotion_discount(program_id);
CREATE INDEX idx_promo_discount_type ON promotion_discount(type);
```

#### Table: promotion_report
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| program_id | BIGINT | FK -> promotion_program.id, NOT NULL | Associated program |
| metric_type | VARCHAR(50) | NOT NULL | Metric: units, orders, revenue, discount_cost, roi |
| metric_value | DECIMAL(18,2) | NOT NULL | Metric value |
| snapshot_date | DATE | NOT NULL | Snapshot date |
| period_type | VARCHAR(20) | DEFAULT 'daily' | Period: daily, weekly, monthly |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_promo_report_program ON promotion_report(program_id);
CREATE INDEX idx_promo_report_date ON promotion_report(snapshot_date);
CREATE INDEX idx_promo_report_type ON promotion_report(metric_type);
```

#### Table: promotion_condition
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| program_id | BIGINT | FK -> promotion_program.id, NOT NULL | Parent program |
| condition_type | VARCHAR(50) | NOT NULL | Type: min_order_value, product, customer_segment, channel, date_range |
| condition_value | TEXT | NOT NULL | Condition value (JSON for complex values) |
| operator | VARCHAR(10) | DEFAULT '=' | Operator: =, !=, >, <, >=, <=, IN, NOT IN |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_promo_condition_program ON promotion_condition(program_id);
CREATE INDEX idx_promo_condition_type ON promotion_condition(condition_type);
```

#### Table: promotion_schedule
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| program_id | BIGINT | FK -> promotion_program.id, NOT NULL | Associated program |
| recurrence | VARCHAR(30) | NOT NULL | Recurrence: none, daily, weekly, monthly, quarterly |
| recurrence_config_json | JSON | NULL | Recurrence configuration |
| auto_activate | TINYINT(1) | DEFAULT 0 | Auto-activate on schedule |
| next_activation | TIMESTAMP | NULL | Next activation timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_promo_schedule_program ON promotion_schedule(program_id);
CREATE INDEX idx_promo_schedule_next ON promotion_schedule(next_activation);
```

#### Table: promotion_budget
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| program_id | BIGINT | FK -> promotion_program.id, NOT NULL | Associated program |
| total_budget | DECIMAL(18,2) | NOT NULL | Total program budget |
| allocated_json | JSON | NULL | Budget allocation by channel/product |
| alert_threshold_80 | TINYINT(1) | DEFAULT 1 | Alert at 80% utilization |
| alert_threshold_90 | TINYINT(1) | DEFAULT 1 | Alert at 90% utilization |
| alert_threshold_100 | TINYINT(1) | DEFAULT 1 | Alert at 100% utilization |
| overrun_policy | VARCHAR(20) | DEFAULT 'block' | Policy: block, warn_and_allow |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_promo_budget_program ON promotion_budget(program_id);
```

#### Table: promotion_type
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL | Type display name |
| code | VARCHAR(30) | NOT NULL, UNIQUE | Type code |
| rules_json | JSON | NULL | Default rules for this type |
| is_active | TINYINT(1) | DEFAULT 1 | Active flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_promo_type_code ON promotion_type(code);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| promotion_program | Promotion program records | 500 |
| promotion_item | Promotion item performance | 5,000 |
| promotion_discount | Discount rule configurations | 1,000 |
| promotion_report | Performance snapshots | 100,000 |
| promotion_condition | Condition rules | 2,000 |
| promotion_schedule | Schedule configurations | 500 |
| promotion_budget | Budget tracking | 500 |
| promotion_type | Promotion type definitions | 20 |

---

## 5. API Specifications

### 5.1 Promotion Program Endpoints

#### GET /api/v1/promotions/programs
**Description:** List promotion programs with filtering and pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| limit | integer | No | Items per page (default: 20, max: 100) |
| status | string | No | Filter by status |
| type | string | No | Filter by type |
| date_from | string | No | Filter by start date |
| date_to | string | No | Filter by end date |
| product_id | integer | No | Filter by applicable product |

**Permission:** `promotion:program:read`

#### POST /api/v1/promotions/programs
**Description:** Create a new promotion program

**Permission:** `promotion:program:create`

#### POST /api/v1/promotions/programs/{id}/activate
**Description:** Activate a promotion program

**Permission:** `promotion:program:activate`

#### POST /api/v1/promotions/programs/{id}/pause
**Description:** Pause an active promotion program

**Permission:** `promotion:program:pause`

### 5.2 Promotion Item Endpoints

#### GET /api/v1/promotions/programs/{id}/items
**Description:** Get promotion items for a program

**Permission:** `promotion:program:read`

### 5.3 Budget Endpoints

#### GET /api/v1/promotions/programs/{id}/budget
**Description:** Get budget information for a program

**Permission:** `promotion:budget:read`

#### PUT /api/v1/promotions/programs/{id}/budget
**Description:** Update program budget

**Permission:** `promotion:budget:update`

### 5.4 Report Endpoints

#### GET /api/v1/promotions/reports/summary
**Description:** Get promotion performance summary

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date_from | string | Yes | Start date |
| date_to | string | Yes | End date |
| program_id | integer | No | Filter by program |
| product_id | integer | No | Filter by product |

**Response (200 OK):**
```json
{
  "total_units": 251,
  "total_orders": 195,
  "total_revenue": 121000000,
  "by_code": [
    {"code": "HH1834", "name": "Standard 1st year", "units": 93, "orders": 30, "revenue": 16000000},
    {"code": "HH1835", "name": "Standard 2nd+ year", "units": 61, "orders": 70, "revenue": 25000000},
    {"code": "HH1836", "name": "Professional 1st year", "units": 61, "orders": 53, "revenue": 54000000},
    {"code": "HH1837", "name": "Professional 2nd+ year", "units": 36, "orders": 42, "revenue": 26000000}
  ]
}
```

**Permission:** `promotion:report:read`

### 5.5 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/promotions/programs | List programs | promotion:program:read |
| POST | /api/v1/promotions/programs | Create program | promotion:program:create |
| GET | /api/v1/promotions/programs/{id} | Get program | promotion:program:read |
| PUT | /api/v1/promotions/programs/{id} | Update program | promotion:program:update |
| POST | /api/v1/promotions/programs/{id}/activate | Activate | promotion:program:activate |
| POST | /api/v1/promotions/programs/{id}/pause | Pause | promotion:program:pause |
| GET | /api/v1/promotions/programs/{id}/items | Program items | promotion:program:read |
| GET | /api/v1/promotions/programs/{id}/budget | Budget info | promotion:budget:read |
| PUT | /api/v1/promotions/programs/{id}/budget | Update budget | promotion:budget:update |
| GET | /api/v1/promotions/reports/summary | Performance summary | promotion:report:read |
| GET | /api/v1/promotions/types | List promotion types | promotion:type:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Marketing Manager | marketing_manager | Full access to promotion programs and budgets |
| Sales Manager | sales_manager | Read access, apply promotions to orders |
| Finance Accountant | finance_accountant | Read access to budgets and financial data |
| Store Manager | store_manager | Apply promotions at POS level |
| Business Analyst | business_analyst | Read access to all data and reports |

### Permission Matrix
| Role | Create Program | Read Program | Update Budget | Activate/Pause | Apply at POS | View Reports | Export |
|------|---------------|-------------|--------------|---------------|-------------|-------------|--------|
| Marketing Manager | Yes | Yes | Yes | Yes | No | Yes | Yes |
| Sales Manager | No | Yes | No | No | Yes | Yes | Yes |
| Finance Accountant | No | Yes | Read only | No | No | Yes | Yes |
| Store Manager | No | Yes | No | No | Yes | No | No |
| Business Analyst | No | Yes | No | No | No | Yes | Yes |

### Row-Level Security
- Marketing Managers have full access to all programs within their assigned company.
- Sales Representatives can only view active programs applicable to their sales channel.
- Store Managers can only apply promotions at their assigned store's POS.

---

## 7. State Machine / Workflows

### 7.1 Promotion Program Status Transitions

```
[DRAFT] ──schedule──→ [SCHEDULED] ──auto-activate──→ [ACTIVE] ──complete──→ [COMPLETED]
   │                       │                           │
   └──activate─────────────┘                           └──pause──→ [PAUSED]
   │                                                   │
   └──cancel───────────────────────────────────────────┘
                                                       │
                                                       └──resume──→ [ACTIVE]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | SCHEDULED | schedule | Marketing Manager | Sets activation timer |
| DRAFT | ACTIVE | activate | Marketing Manager | Program becomes available for orders |
| SCHEDULED | ACTIVE | auto-activate | System | Triggered at start date |
| ACTIVE | COMPLETED | complete | System | End date reached, archives data |
| ACTIVE | PAUSED | pause | Marketing Manager | Stops discount application |
| PAUSED | ACTIVE | resume | Marketing Manager | Resumes discount application |
| DRAFT | CANCELLED | cancel | Marketing Manager | Program discarded |

### 7.2 Budget Alert Workflow

```
[0%] ──80%──→ [WARNING_80] ──90%──→ [WARNING_90] ──100%──→ [EXHAUSTED]
                                                    │
                                                    └──block──→ [BLOCKED]
                                                    │
                                                    └──warn──→ [OVERRUN]
```

---

## 8. Reports & Analytics

### 8.1 Report: Promotion Performance Summary
**Type:** Real-time
**Purpose:** Display promotion program performance with units, orders, and revenue

**Metrics:**
- Total units sold, order count, revenue
- Performance by promotion code
- Revenue by program

**Chart Type:** Pie chart (by code), Table (detailed)

### 8.2 Report: Budget Utilization
**Type:** Real-time
**Purpose:** Track budget spending across promotion programs

**Metrics:**
- Budget spent vs. total
- Utilization percentage
- Spending timeline

**Chart Type:** Bar chart, Line chart

### 8.3 Report: Promotion ROI Analysis
**Type:** Real-time
**Purpose:** Evaluate return on investment for promotion programs

**Metrics:**
- Revenue generated, discount cost, net revenue
- ROI percentage by program
- ROI trend over time

**Chart Type:** Bar chart, Line chart

### 8.4 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Performance Summary | Real-time | On-demand | Pie Chart, Table, CSV |
| Budget Utilization | Real-time | Daily | Bar Chart, Excel |
| ROI Analysis | Real-time | Weekly | Bar Chart, PDF |
| Code Usage | Real-time | On-demand | Table |
| Channel Comparison | Real-time | Monthly | Bar Chart |
| Product Performance | Real-time | On-demand | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| MISA Sales API | Order data sync for promotion application | REST | OAuth 2.0 |
| MISA POS API | In-store promotion application | REST | OAuth 2.0 |
| Email/SMS API | Budget alert notifications | REST | API Key |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| promotion.activated | {program_id, code, start_date} | Sales (M08), POS (M37) |
| promotion.paused | {program_id, code, reason} | Sales (M08), POS (M37) |
| budget.threshold_reached | {program_id, threshold, spent, total} | Finance team, Marketing Manager |
| promotion.completed | {program_id, final_metrics} | Reporting, Accounting (M04) |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| order.created | Sales (M08) | Evaluate applicable promotions |
| order.completed | Sales (M08) | Update promotion performance metrics |
| pos.transaction | Store & POS (M37) | Apply and track in-store promotions |

---

## 10. Technical Notes

### Performance
- Expected data volume: 500+ programs, 5,000+ promotion items, 100,000+ performance snapshots
- Query complexity: Medium (aggregations for reports, condition evaluation for orders)
- Expected response time: Program list < 500ms, Report summary < 1s, Condition evaluation < 100ms per order
- Budget alert checks run every 5 minutes during active program periods

### Caching Strategy
- Redis cache for active promotion programs and condition rules
- TTL: 5 minutes for active programs, 15 minutes for historical reports
- Performance metrics cached per program with 30-second TTL
- Cache invalidation on program status change, budget update, or new order

### File Attachments
- Supported types: CSV (code imports), XLSX (bulk program imports)
- Max size: 10 MB per file
- Storage: S3-compatible object storage

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Currency: VND with Vietnamese number formatting
- Promotion codes support Vietnamese characters

### Mobile Considerations
- Promotion dashboard is responsive for tablet access
- POS promotion application optimized for mobile screens
- Push notifications for budget alerts and program status changes

### Security
- Promotion configuration changes are logged with before/after values
- Budget adjustments require audit trail with approver information
- API rate limiting: 100 requests per minute for read, 20 for write
- Promotion code validation prevents injection attacks

### Scalability
- Condition evaluation engine processes promotions in parallel for high-volume order periods
- Performance metric aggregation uses background jobs to prevent order processing delays
- Report exports with large datasets are processed asynchronously
