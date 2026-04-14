# M39 - Operations Dashboard (Bang dieu hanh)

**Category:** Platform
**Priority:** P1
**Reference Images:** assets/image_60.png, assets/image_61.png, assets/image_62.png, assets/image_63.png
**Dependencies:** M01 (Authentication), M04 (Accounting), M08 (Sales), M05 (Employee Management), M19 (Workflow)
**Generated:** April 14, 2026

---

## 1. Module Overview

The Operations Dashboard module serves as the centralized command center for the entire ABN ERP platform, providing real-time visibility into system-wide operations, cross-module metrics, user activity, workflow performance, financial summaries, and HR analytics. It aggregates data from all connected modules into configurable dashboard widgets, enabling executives, managers, and administrators to monitor the health and performance of the entire organization from a single interface.

The system admin dashboard (image_60) displays total user count of 2,800 with access range tracking from 5 to 80 concurrent users per module, a usage-per-application table showing active users across all ERP modules, a product usage chart with adoption trends, and real-time event feeds showing new tasks (3), new workflow runs (5), and new posts (5). The financial dashboard (image_62) presents consolidated financial metrics: Revenue 65,338M VND (+15%), Expenses 51,212M VND (-6%), Profit 5,126M VND (+10%), with monthly trend charts and cash balance breakdowns by branch. The HR dashboard (image_63) shows 130 employees with age distribution charts, employee movement trends (new hires, transfers, resignations), and resignation reason analysis (benefits, work environment, adaptation difficulty, career change, performance). The workflow dashboard (image_61) tracks 750 total workflow runs with 120 in progress and 600 completed, providing process performance visibility. The platform is built on generated/aggregated data with configurable widgets, cross-module metric calculation, and automated report generation.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| CEO / Executive | Views organization-wide KPIs, financial performance, and operational health | Read access to all aggregated data across all modules |
| System Administrator | Monitors system health, user activity, application usage, and integration status | Full access to technical metrics, read access to business data |
| Finance Director | Monitors financial KPIs, revenue, expenses, and profitability | Full access to financial dashboards and reports |
| HR Director | Monitors workforce analytics, employee metrics, and HR trends | Full access to HR dashboards and workforce data |
| Operations Manager | Monitors workflow performance, process efficiency, and cross-module operations | Read access to workflow and operational dashboards |
| Department Manager | Views department-specific metrics and team performance | Read access to department-scoped dashboards |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M01 - Authentication | User activity tracking, access metrics |
| M04 - Accounting | Financial data aggregation (revenue, expenses, profit) |
| M08 - Sales | Sales metrics and order data |
| M05 - Employee Management | HR analytics, employee count, movement data |
| M19 - Workflow | Workflow run statistics, process performance |
| M06 - Task Management | Task creation and completion metrics |
| M20 - Communication | Post and message activity metrics |

### Key Business Rules

1. Dashboard data is refreshed at configurable intervals (real-time, 5 minutes, 15 minutes, hourly) based on widget type and data source performance impact.
2. Cross-module metrics are calculated using data aggregation jobs that run on scheduled intervals; real-time display may show data up to the aggregation delay.
3. User access to dashboard data follows the same permission model as the underlying modules; users can only see data they have permission to access in the source module.
4. Dashboard configurations are user-specific by default; shared dashboards can be published for team-wide or organization-wide access.
5. Financial data displayed on the operations dashboard is sourced from the Accounting module (M04) and must match the authoritative financial reports.
6. Workflow metrics are sourced from the Workflow module (M19) and reflect real-time process execution status.
7. HR data is sourced from the Employee Management module (M05) and is subject to data privacy restrictions; aggregated data is shown, individual data requires explicit permission.
8. All dashboard widgets support filtering by date range, department, company, and custom dimensions defined by the data source.
9. Dashboard data exports include a timestamp indicating when the underlying aggregation was last refreshed.
10. System-generated data (usage metrics, access logs) is retained for 12 months; business data retention follows the source module's retention policy.

---

## 2. Screen Inventory

### 2.1 Screen: System Admin Dashboard

**Route:** `/operations/admin-dashboard` | **Purpose:** Monitor system-wide user activity, application usage, and real-time events.

| UI Component | Description |
|--------------|-------------|
| Total Users Card | 2,800 total registered users |
| Access Range Card | 5 to 80 concurrent users per module |
| Usage Per App Table | Table showing each module with active user count, total users, trend |
| Product Usage Chart | Bar/line chart showing module adoption over time |
| Real-Time Events Feed | Live feed: 3 new tasks, 5 new workflow runs, 5 new posts |
| System Health Indicators | API response times, database load, cache hit rate |
| New Entity Events | Recently created users, workflows, posts, tasks |

**User Interactions:**
- View system-wide user activity at a glance
- Click module in usage table to drill into module-specific metrics
- Monitor real-time event feed for system activity
- View system health indicators for performance monitoring

**Data Displayed:**
- Total user count and access patterns
- Per-module usage statistics
- Product adoption trends
- Real-time system events

### 2.2 Screen: Financial Dashboard

**Route:** `/operations/financial` | **Purpose:** Display consolidated financial metrics with revenue, expenses, and profit analysis.

| UI Component | Description |
|--------------|-------------|
| Revenue Card | 65,338M VND with +15% trend indicator |
| Expenses Card | 51,212M VND with -6% trend indicator |
| Profit Card | 5,126M VND with +10% trend indicator |
| Monthly Revenue Chart | Bar chart showing monthly revenue trend |
| Monthly Expenses Chart | Stacked bar chart showing expense breakdown |
| Cash Balance by Branch | Table showing cash balances per branch/location |
| Cash Flow Chart | Line chart showing cash flow over time |
| Budget vs. Actual | Comparison of budgeted vs. actual figures |

**User Interactions:**
- Click on financial cards to drill into detailed reports
- View monthly trends with configurable time ranges
- Compare budget vs. actual performance
- Filter by company, department, or branch

**Data Displayed:**
- Revenue, expenses, and profit with trends
- Monthly financial charts
- Cash balance breakdown by location
- Budget comparison data

### 2.3 Screen: HR Dashboard

**Route:** `/operations/hr` | **Purpose:** Display workforce analytics with employee count, demographics, and movement trends.

| UI Component | Description |
|--------------|-------------|
| Total Employees Card | 130 employees with period comparison |
| Age Distribution Chart | Pie/bar chart showing employee age groups |
| Employee Movement Chart | Line chart showing new hires, transfers, resignations over time |
| Resignation Reasons Chart | Pie chart: Benefits (60), Work environment (35), Adaptation (30), Career change (25), Performance (20) |
| Department Structure | Org chart showing employee distribution by department |
| Contract Type Distribution | Pie chart showing contract type breakdown |
| Headcount Trend | Monthly headcount change trend |

**User Interactions:**
- View workforce demographics and distribution
- Analyze resignation patterns and reasons
- Track employee movement trends
- Filter by department, location, or period

**Data Displayed:**
- Employee count and demographics
- Age distribution
- Movement trends (hires, transfers, resignations)
- Resignation reason analysis

### 2.4 Screen: Workflow Dashboard

**Route:** `/operations/workflow` | **Purpose:** Monitor workflow execution with run counts, status breakdown, and performance metrics.

| UI Component | Description |
|--------------|-------------|
| Total Runs Card | 750 total workflow runs |
| In Progress Card | 120 workflows currently running |
| Completed Card | 600 completed workflows |
| Workflow Status Chart | Donut chart showing status distribution |
| Performance Metrics | Average completion time, bottleneck analysis |
| Top Workflows | Most frequently executed workflows |
| Failed Workflow Alert | List of failed or stalled workflows |

**User Interactions:**
- View workflow execution status at a glance
- Click on status segments to view workflow details
- Monitor performance metrics and bottlenecks
- Investigate failed workflows

**Data Displayed:**
- Workflow run counts and status breakdown
- Performance metrics (completion time, bottlenecks)
- Top workflows by frequency
- Failed workflow alerts

### 2.5 Screen: Dashboard Configuration

**Route:** `/operations/dashboard-config` | **Purpose:** Create, edit, and manage dashboard widgets and layouts.

| UI Component | Description |
|--------------|-------------|
| Widget Catalog | Available widget types organized by category |
| Layout Editor | Drag-drop grid for arranging widgets |
| Widget Settings | Data source, refresh interval, filters, chart type |
| Sharing Controls | Private, team-shared, organization-wide |
| Template Library | Pre-built dashboard templates by role |

**User Interactions:**
- Browse widget catalog and add widgets to dashboard
- Arrange widgets using drag-drop layout editor
- Configure widget data sources, filters, and refresh intervals
- Share dashboards with team or organization
- Use pre-built templates for quick setup

**Data Displayed:**
- Available widget types and descriptions
- Current dashboard layout
- Widget configuration options
- Sharing and template settings

### 2.6 Screen: Usage Metrics

**Route:** `/operations/usage-metrics` | **Purpose:** Detailed analysis of system usage across all modules and users.

| UI Component | Description |
|--------------|-------------|
| Module Usage Table | Per-module: active users, total users, sessions, avg duration |
| User Activity Ranking | Top active users with module usage breakdown |
| Usage Trend Chart | Daily/weekly/monthly usage trends |
| Peak Hours Chart | Usage distribution by hour of day |
| Adoption Rate | New module adoption over time |

**User Interactions:**
- View detailed usage statistics per module
- Analyze user activity patterns
- Track usage trends over time
- Identify peak usage periods

**Data Displayed:**
- Module-level usage metrics
- User activity rankings
- Usage trends and patterns
- Peak hour distribution

### 2.7 Screen: Cross-Module Reports

**Route:** `/operations/reports` | **Purpose:** Generate and view aggregated reports spanning multiple modules.

| UI Component | Description |
|--------------|-------------|
| Report Catalog | Available cross-module reports |
| Report Viewer | Report with filters, data table, and chart |
| Schedule Configuration | Set up recurring report generation |
| Export Options | PDF, Excel, CSV, scheduled email delivery |

**User Interactions:**
- Browse and select cross-module reports
- Apply filters and generate reports
- Schedule recurring report delivery
- Export reports in various formats

**Data Displayed:**
- Cross-module aggregated data
- Trend analysis across modules
- Scheduled report status
- Export history

---

## 3. Feature Specifications

### 3.1 Feature: Configurable Dashboard Widgets
**Description:** Create and arrange dashboard widgets with configurable data sources, refresh intervals, and visualization types.

**User Stories:**
- As an Executive, I want to configure my dashboard with key metrics from all modules, so that I can monitor the organization at a glance.
- As a System Administrator, I want to see real-time system events, so that I can respond to issues quickly.

**Acceptance Criteria:**
- Given a user adds a widget to their dashboard, when the widget is configured with a data source and filters, then the widget displays data from that source.
- Given a widget has a refresh interval, when the interval elapses, then the widget data is automatically refreshed.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Data source | Must be accessible to user | You do not have permission to access this data source |
| Refresh interval | Minimum 30 seconds, maximum 24 hours | Refresh interval must be between 30 seconds and 24 hours |
| Widget dimensions | Minimum 1x1, maximum 12x6 grid units | Widget size must be within allowed dimensions |

**Edge Cases:**
- Widgets with unavailable data sources display an error state with retry option
- Dashboard layouts are responsive and adapt to different screen sizes
- Shared dashboard changes are visible to all viewers on next refresh

### 3.2 Feature: Cross-Module Metric Aggregation
**Description:** Calculate and display metrics that span multiple modules with scheduled data aggregation.

**User Stories:**
- As an Operations Manager, I want to see workflow performance alongside financial results, so that I can understand process impact on business outcomes.
- As a Finance Director, I want revenue data combined from Sales and Accounting, so that I have a complete financial picture.

**Acceptance Criteria:**
- Given a cross-module metric is configured, when the aggregation job runs, then data from all source modules is combined and displayed.
- Given an aggregation job fails, when the next run occurs, then missed data is backfilled.

**Edge Cases:**
- Aggregation delays are communicated through widget status indicators
- Data from modules with different refresh rates is aligned to the slowest source
- Historical aggregation is re-run when source data is corrected

### 3.3 Feature: Real-Time Event Feed
**Description:** Display real-time system events from all modules in a chronological feed.

**User Stories:**
- As a System Administrator, I want to see new tasks, workflow runs, and posts as they happen, so that I can monitor system activity.
- As an Operations Manager, I want to filter events by type and module, so that I can focus on relevant activities.

**Acceptance Criteria:**
- Given an event occurs in any module, when the event is published, then it appears in the real-time feed within 5 seconds.
- Given a user filters the feed, when filters are applied, then only matching events are displayed.

**Edge Cases:**
- High-volume event periods (>100 events/minute) are batched for display
- Event feed is truncated to last 1,000 events for performance
- WebSocket connection drops trigger automatic reconnection with event replay

### 3.4 Feature: Dashboard Sharing and Templates
**Description:** Share dashboards with team members and use pre-built templates for quick setup.

**User Stories:**
- As a Team Manager, I want to share a standardized dashboard with my team, so that everyone monitors the same metrics.
- As a new user, I want to use a pre-built dashboard template for my role, so that I can start monitoring quickly.

**Acceptance Criteria:**
- Given a dashboard is shared, when team members access it, then they see the same widgets and layout with their own data scope.
- Given a user applies a template, when the template is loaded, then all widgets are created with pre-configured settings.

**Edge Cases:**
- Shared dashboard modifications by the owner are propagated to all viewers
- Users with insufficient permissions see placeholder widgets with permission requests
- Template updates do not affect dashboards already created from the template

### 3.5 Feature: Financial Data Consolidation
**Description:** Aggregate financial data from Accounting module with trend analysis and budget comparison.

**User Stories:**
- As a Finance Director, I want to see revenue, expenses, and profit in a single view, so that I can assess financial health.
- As a CEO, I want to compare financial trends across periods, so that I can track performance over time.

**Acceptance Criteria:**
- Given financial data is aggregated, when the dashboard loads, then revenue, expenses, and profit are displayed with trend indicators.
- Given a date range is selected, when the view updates, then all financial metrics are filtered to the selected period.

**Edge Cases:**
- Financial data from multiple companies is consolidated with currency conversion
- Budget comparison uses the latest approved budget version
- Data discrepancies between dashboard and detailed reports are flagged for review

### 3.6 Feature: HR Analytics Dashboard
**Description:** Display workforce analytics with employee demographics, movement trends, and resignation analysis.

**User Stories:**
- As an HR Director, I want to see employee distribution by department and age, so that I can plan workforce strategy.
- As a CEO, I want to understand resignation patterns, so that I can address retention issues.

**Acceptance Criteria:**
- Given HR data is aggregated, when the dashboard loads, then employee count, demographics, and trends are displayed.
- Given resignation data is analyzed, when the chart renders, then reasons are categorized and quantified.

**Edge Cases:**
- Individual employee data is only visible to users with HR module permissions
- Small department sizes (<5 employees) show aggregated data to protect privacy
- Historical HR data is preserved even after employee departure

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+------------------+       +------------------+       +------------------+
| DashboardWidget  |       | DashboardConfig  |       |   UsageMetric    |
+------------------+       +------------------+       +------------------+
| id (PK)          |       | id (PK)          |       | id (PK)          |
| widget_type(FK)  |       | user_id(FK)      |       | module_code      |
| config_json      |       | name             |       | active_users     |
| position_x       |       | layout_json      |       | total_users      |
| position_y       |       | is_shared        |       | session_count    |
| refresh_interval |       | shared_with_json |       | avg_duration     |
| data_source      |       | created_at       |       | date             |
+------------------+       +------------------+       +------------------+
        |                           |                         |
        v                           v                         v
+------------------+       +------------------+       +------------------+
|   WidgetType     |       | DataAggregation  |       | SystemLog        |
+------------------+       +------------------+       +------------------+
| id (PK)          |       | id (PK)          |       | id (PK)          |
| code             |       | source_module    |       | event_type       |
| name             |       | metric_name      |       | event_data_json  |
| category         |       | aggregated_value |       | user_id(FK)      |
| data_source_type |       | aggregation_date |       | module_code      |
+------------------+       | created_at       |       | timestamp        |
                           +------------------+       +------------------+
        |
        v
+------------------+       +------------------+
| AggregatedReport |       |CrossModuleMetric |
+------------------+       +------------------+
| id (PK)          |       | id (PK)          |
| report_type      |       | metric_name      |
| filters_json     |       | source_modules   |
| schedule         |       | calculation_expr |
| last_generated   |       | refresh_interval |
+------------------+       +------------------+
```

### 4.2 Table Schemas

#### Table: dashboard_widget
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| dashboard_config_id | BIGINT | FK -> dashboard_config.id, NOT NULL | Parent dashboard |
| widget_type_id | BIGINT | FK -> widget_type.id, NOT NULL | Widget type |
| title | VARCHAR(200) | NULL | Widget display title |
| config_json | JSON | NOT NULL | Widget configuration (data source, filters, chart type) |
| position_x | INT | NOT NULL | Grid X position |
| position_y | INT | NOT NULL | Grid Y position |
| width | INT | NOT NULL | Grid width (1-12) |
| height | INT | NOT NULL | Grid height (1-6) |
| refresh_interval | INT | DEFAULT 300 | Refresh interval in seconds |
| data_source | VARCHAR(100) | NOT NULL | Data source identifier |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_dash_widget_config ON dashboard_widget(dashboard_config_id);
CREATE INDEX idx_dash_widget_type ON dashboard_widget(widget_type_id);
CREATE INDEX idx_dash_widget_position ON dashboard_widget(position_x, position_y);
```

#### Table: dashboard_config
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | FK -> users.id, NOT NULL | Dashboard owner |
| name | VARCHAR(200) | NOT NULL | Dashboard name |
| description | TEXT | NULL | Dashboard description |
| layout_json | JSON | NOT NULL | Layout configuration |
| is_shared | TINYINT(1) | DEFAULT 0 | Shared flag |
| shared_with_json | JSON | NULL | Shared user/role IDs |
| scope | VARCHAR(30) | DEFAULT 'private' | Scope: private, team, organization |
| template_id | BIGINT | FK -> dashboard_template.id | Source template |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_dash_config_user ON dashboard_config(user_id);
CREATE INDEX idx_dash_config_shared ON dashboard_config(is_shared);
CREATE INDEX idx_dash_config_scope ON dashboard_config(scope);
```

#### Table: usage_metric
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| module_code | VARCHAR(50) | NOT NULL | Module identifier |
| active_users | INT | DEFAULT 0 | Current active users |
| total_users | INT | DEFAULT 0 | Total users with access |
| session_count | INT | DEFAULT 0 | Session count for period |
| avg_duration_minutes | DECIMAL(10,2) | DEFAULT 0 | Average session duration |
| date | DATE | NOT NULL | Metric date |
| hour | INT | NULL | Metric hour (for hourly metrics) |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_usage_module ON usage_metric(module_code);
CREATE INDEX idx_usage_date ON usage_metric(date);
CREATE INDEX idx_usage_module_date ON usage_metric(module_code, date);
```

#### Table: widget_type
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| code | VARCHAR(50) | NOT NULL, UNIQUE | Widget type code |
| name | VARCHAR(200) | NOT NULL | Widget display name |
| category | VARCHAR(50) | NOT NULL | Category: financial, hr, workflow, system, usage, custom |
| data_source_type | VARCHAR(50) | NOT NULL | Source type: api, query, event, aggregated |
| default_config_json | JSON | NULL | Default widget configuration |
| icon | VARCHAR(50) | NULL | Widget icon |
| is_active | TINYINT(1) | DEFAULT 1 | Active flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_widget_type_code ON widget_type(code);
CREATE INDEX idx_widget_type_category ON widget_type(category);
```

#### Table: data_aggregation
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| source_module | VARCHAR(50) | NOT NULL | Source module code |
| metric_name | VARCHAR(100) | NOT NULL | Metric identifier |
| aggregated_value | DECIMAL(18,4) | NOT NULL | Aggregated value |
| aggregation_date | DATE | NOT NULL | Aggregation date |
| aggregation_hour | INT | NULL | Hour for hourly aggregations |
| dimension_json | JSON | NULL | Dimension filters applied |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_agg_module ON data_aggregation(source_module);
CREATE INDEX idx_agg_metric ON data_aggregation(metric_name);
CREATE INDEX idx_agg_date ON data_aggregation(aggregation_date);
CREATE INDEX idx_agg_module_date ON data_aggregation(source_module, aggregation_date);
```

#### Table: system_log
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| event_type | VARCHAR(50) | NOT NULL | Event type: task.created, workflow.run, post.created, user.login |
| event_data_json | JSON | NOT NULL | Event payload |
| user_id | BIGINT | FK -> users.id | Triggering user |
| module_code | VARCHAR(50) | NOT NULL | Source module |
| timestamp | TIMESTAMP | NOT NULL, DEFAULT NOW() | Event timestamp |
| ip_address | VARCHAR(45) | NULL | User IP address |

**Indexes:**
```sql
CREATE INDEX idx_sys_log_type ON system_log(event_type);
CREATE INDEX idx_sys_log_module ON system_log(module_code);
CREATE INDEX idx_sys_log_timestamp ON system_log(timestamp);
CREATE INDEX idx_sys_log_user ON system_log(user_id);
```

#### Table: aggregated_report
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| report_type | VARCHAR(50) | NOT NULL | Report type identifier |
| name | VARCHAR(200) | NOT NULL | Report display name |
| filters_json | JSON | NULL | Default filter configuration |
| schedule | VARCHAR(30) | NULL | Schedule: daily, weekly, monthly |
| last_generated_at | TIMESTAMP | NULL | Last generation timestamp |
| recipients_json | JSON | NULL | Email recipients for scheduled reports |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| created_by | BIGINT | FK -> users.id | Creator |

**Indexes:**
```sql
CREATE INDEX idx_agg_report_type ON aggregated_report(report_type);
CREATE INDEX idx_agg_report_schedule ON aggregated_report(schedule);
```

#### Table: cross_module_metric
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| metric_name | VARCHAR(100) | NOT NULL, UNIQUE | Metric identifier |
| display_name | VARCHAR(200) | NOT NULL | Display name |
| source_modules_json | JSON | NOT NULL | Source module codes |
| calculation_expression | TEXT | NOT NULL | Calculation formula/expression |
| refresh_interval | INT | DEFAULT 300 | Refresh interval in seconds |
| category | VARCHAR(50) | NOT NULL | Metric category |
| is_active | TINYINT(1) | DEFAULT 1 | Active flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_cross_metric_name ON cross_module_metric(metric_name);
CREATE INDEX idx_cross_metric_category ON cross_module_metric(category);
```

#### Table: dashboard_template
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Template name |
| role | VARCHAR(50) | NULL | Target role |
| description | TEXT | NULL | Template description |
| widgets_json | JSON | NOT NULL | Widget definitions |
| layout_json | JSON | NOT NULL | Layout configuration |
| is_public | TINYINT(1) | DEFAULT 0 | Public availability |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_dash_template_role ON dashboard_template(role);
CREATE INDEX idx_dash_template_public ON dashboard_template(is_public);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| dashboard_widget | Widget instances | 50,000 |
| dashboard_config | User dashboard configurations | 10,000 |
| usage_metric | Module usage metrics | 500,000 |
| widget_type | Widget type definitions | 100 |
| data_aggregation | Aggregated data records | 5,000,000 |
| system_log | System event logs | 10,000,000 |
| aggregated_report | Report configurations | 500 |
| cross_module_metric | Cross-module metric definitions | 100 |
| dashboard_template | Dashboard templates | 50 |

---

## 5. API Specifications

### 5.1 Dashboard Endpoints

#### GET /api/v1/operations/dashboards
**Description:** List user's dashboards

**Permission:** `ops:dashboard:read`

#### POST /api/v1/operations/dashboards
**Description:** Create a new dashboard

**Permission:** `ops:dashboard:create`

#### GET /api/v1/operations/dashboards/{id}
**Description:** Get dashboard with widgets

**Permission:** `ops:dashboard:read`

### 5.2 Widget Endpoints

#### POST /api/v1/operations/dashboards/{id}/widgets
**Description:** Add widget to dashboard

**Permission:** `ops:widget:create`

#### PUT /api/v1/operations/widgets/{id}
**Description:** Update widget configuration

**Permission:** `ops:widget:update`

#### DELETE /api/v1/operations/widgets/{id}
**Description:** Remove widget from dashboard

**Permission:** `ops:widget:delete`

### 5.3 Usage Metrics Endpoints

#### GET /api/v1/operations/usage-metrics
**Description:** Get module usage metrics

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date_from | string | Yes | Start date |
| date_to | string | Yes | End date |
| module_code | string | No | Filter by module |

**Permission:** `ops:usage:read`

### 5.4 System Events Endpoints

#### GET /api/v1/operations/events
**Description:** Get real-time system events (WebSocket/streaming)

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| limit | integer | No | Number of recent events (default: 50) |
| event_type | string | No | Filter by event type |
| module_code | string | No | Filter by module |

**Permission:** `ops:event:read`

### 5.5 Financial Summary Endpoints

#### GET /api/v1/operations/financial-summary
**Description:** Get consolidated financial summary

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date_from | string | Yes | Start date |
| date_to | string | Yes | End date |
| company_id | integer | No | Filter by company |

**Response (200 OK):**
```json
{
  "revenue": 65338000000,
  "revenue_trend": 15.0,
  "expenses": 51212000000,
  "expenses_trend": -6.0,
  "profit": 5126000000,
  "profit_trend": 10.0,
  "period": {"from": "2026-01-01", "to": "2026-04-14"}
}
```

**Permission:** `ops:financial:read`

### 5.6 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/operations/dashboards | List dashboards | ops:dashboard:read |
| POST | /api/v1/operations/dashboards | Create dashboard | ops:dashboard:create |
| GET | /api/v1/operations/dashboards/{id} | Get dashboard | ops:dashboard:read |
| POST | /api/v1/operations/dashboards/{id}/widgets | Add widget | ops:widget:create |
| PUT | /api/v1/operations/widgets/{id} | Update widget | ops:widget:update |
| DELETE | /api/v1/operations/widgets/{id} | Delete widget | ops:widget:delete |
| GET | /api/v1/operations/usage-metrics | Usage metrics | ops:usage:read |
| GET | /api/v1/operations/events | System events | ops:event:read |
| GET | /api/v1/operations/financial-summary | Financial summary | ops:financial:read |
| GET | /api/v1/operations/hr-summary | HR summary | ops:hr:read |
| GET | /api/v1/operations/workflow-summary | Workflow summary | ops:workflow:read |
| GET | /api/v1/operations/widget-types | Widget types | ops:widget:read |
| GET | /api/v1/operations/templates | Dashboard templates | ops:template:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| CEO / Executive | executive | Read access to all dashboards and metrics |
| System Administrator | system_admin | Full access to system metrics, read access to business data |
| Finance Director | finance_director | Full access to financial dashboards |
| HR Director | hr_director | Full access to HR dashboards |
| Operations Manager | operations_manager | Read access to workflow and operational dashboards |
| Department Manager | department_manager | Read access to department-scoped dashboards |

### Permission Matrix
| Role | Create Dashboard | View All Dashboards | System Metrics | Financial Data | HR Data | Workflow Data | Share Dashboard |
|------|-----------------|--------------------|---------------|--------------|---------|-------------|----------------|
| Executive | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| System Administrator | Yes | Yes | Yes | Read only | Read only | Yes | Yes |
| Finance Director | Yes | Team only | No | Yes | No | No | Yes |
| HR Director | Yes | Team only | No | No | Yes | No | Yes |
| Operations Manager | Yes | Team only | Yes | Read only | Read only | Yes | Yes |
| Department Manager | Yes | Own only | No | Dept only | Dept only | Dept only | Team only |

### Row-Level Security
- Users can only view data from modules they have permission to access.
- Department Managers see data scoped to their department and sub-departments.
- Financial and HR data requires specific module permissions beyond dashboard access.
- Shared dashboards respect the viewer's data scope, not the owner's.

---

## 7. State Machine / Workflows

### 7.1 Data Aggregation Lifecycle

```
[SCHEDULED] ──trigger──→ [RUNNING] ──complete──→ [COMPLETED]
                              │
                              └──fail──→ [FAILED] ──retry──→ [RUNNING]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| SCHEDULED | RUNNING | trigger | Scheduler | Aggregation job starts |
| RUNNING | COMPLETED | complete | System | Dashboard widgets refresh |
| RUNNING | FAILED | fail | System | Error logged, retry scheduled |
| FAILED | RUNNING | retry | System | Automatic retry (max 3) |

### 7.2 Dashboard Sharing Workflow

```
[PRIVATE] ──share──→ [SHARED_TEAM] ──publish──→ [SHARED_ORG]
     │                      │
     └──unshare─────────────┘
```

### 7.3 Widget Refresh Cycle

```
[LOADED] ──interval──→ [REFRESHING] ──data──→ [LOADED]
                            │
                            └──error──→ [ERROR] ──retry──→ [REFRESHING]
```

---

## 8. Reports & Analytics

### 8.1 Report: System Usage Overview
**Type:** Real-time
**Purpose:** Display module usage patterns, active users, and adoption trends

**Metrics:**
- Active users per module
- Session counts and duration
- Adoption trends over time

**Chart Type:** Bar chart, Line chart, Table

### 8.2 Report: Financial Performance
**Type:** Real-time
**Purpose:** Consolidated revenue, expenses, and profit analysis

**Metrics:**
- Revenue, expenses, profit with trends
- Monthly financial breakdowns
- Budget vs. actual comparison

**Chart Type:** Bar chart, Line chart, Table

### 8.3 Report: HR Analytics
**Type:** Real-time
**Purpose:** Workforce demographics, movement, and retention analysis

**Metrics:**
- Employee count and distribution
- Age demographics
- Resignation reasons and trends

**Chart Type:** Pie chart, Bar chart, Line chart

### 8.4 Report: Workflow Performance
**Type:** Real-time
**Purpose:** Workflow execution metrics and process efficiency

**Metrics:**
- Total runs, in progress, completed
- Average completion time
- Failed workflow analysis

**Chart Type:** Donut chart, Bar chart

### 8.5 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| System Usage Overview | Real-time | Weekly | Chart, CSV |
| Financial Performance | Real-time | Monthly | Bar Chart, Excel |
| HR Analytics | Real-time | Monthly | Pie Chart, PDF |
| Workflow Performance | Real-time | Weekly | Donut Chart |
| Cross-Module KPI | Real-time | Daily | Dashboard |
| User Activity Report | Real-time | Weekly | Table |
| Event Summary | Real-time | Daily | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| M04 Accounting API | Financial data aggregation | REST | OAuth 2.0 |
| M05 Employee API | HR data aggregation | REST | OAuth 2.0 |
| M19 Workflow API | Workflow run statistics | REST | OAuth 2.0 |
| M06 Task API | Task creation and completion metrics | REST | OAuth 2.0 |
| M20 Communication API | Post and message activity | REST | OAuth 2.0 |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| dashboard.created | {dashboard_id, user_id, name} | Notification service |
| dashboard.shared | {dashboard_id, shared_with} | Shared recipients |
| aggregation.completed | {metric_name, date, value} | Dependent widgets |
| event.published | {event_type, module, timestamp} | Real-time feed subscribers |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| *.created (any module) | All modules | Log to system events, update real-time feed |
| *.completed (any module) | All modules | Update aggregated metrics |
| user.login | Auth (M01) | Update active user count |
| workflow.run | Workflow (M19) | Update workflow metrics |

---

## 10. Technical Notes

### Performance
- Expected data volume: 10,000,000+ system events, 5,000,000+ aggregation records, 500,000+ usage metrics
- Query complexity: High (cross-module joins, multi-dimensional aggregations)
- Expected response time: Dashboard load < 2s, Widget refresh < 1s, Event feed < 500ms
- Aggregation jobs: 50+ scheduled jobs running at various intervals (5min to daily)
- WebSocket event delivery: < 1 second latency

### Caching Strategy
- Multi-layer caching: Redis for aggregation results, browser cache for widget configurations
- TTL: 5 minutes for real-time widgets, 15 minutes for financial data, 1 hour for HR analytics
- Dashboard layouts cached per user with 30-minute TTL
- Cache invalidation triggered by aggregation completion, data source updates, or manual refresh
- Cross-module metric cache uses dependency graph for targeted invalidation

### File Attachments
- Dashboard exports: PDF, Excel, CSV
- Max export size: 100 MB
- Storage: S3-compatible object storage with CDN for exported files

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Currency: VND with Vietnamese number formatting
- Number formatting adapts to user locale preferences

### Mobile Considerations
- Dashboard layouts adapt to mobile screens with responsive widget grid
- Real-time event feed accessible on mobile with push notifications
- Financial and HR summaries viewable on mobile with simplified charts
- Mobile-specific widget types optimized for small screens
- Touch-friendly widget reordering on mobile

### Security
- Dashboard data respects source module permissions; no data exposure beyond user's module access
- Sensitive data (individual HR records, detailed financial entries) is never exposed through dashboard
- Shared dashboards enforce viewer's data scope, preventing privilege escalation
- API rate limiting: 100 requests per minute for dashboard data, 20 for configuration changes
- Event feed data is sanitized to prevent information leakage

### Scalability
- Aggregation jobs use distributed task queue for parallel processing
- Real-time events use WebSocket with fallback to Server-Sent Events (SSE)
- Widget data fetching is parallelized with configurable concurrency limits
- Dashboard configurations are stored in document format for fast retrieval
- Historical data is partitioned by month for efficient querying
