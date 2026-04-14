# M32 - CRM (Quan he Khach hang)

**Category:** Business
**Priority:** P2
**Reference Images:** assets/image_60.png, assets/image_67.png
**Dependencies:** M08 (Sales), M31 (AI Marketing), M01 (Authentication), M20 (Communication)
**Generated:** April 14, 2026

---

## 1. Module Overview

The CRM (Customer Relationship Management) module serves as the central hub for managing all customer-facing interactions throughout the entire sales lifecycle. It provides a unified platform for tracking leads from initial capture through qualification, opportunity management, deal closure, and post-sale relationship management. The CRM module integrates bidirectionally with the AI Marketing module (M31) -- receiving qualified leads from marketing campaigns and feeding back conversion data for ROI attribution. It also connects with the Sales module (M08) for order processing, the Communication module (M20) for activity logging, and the Accounting module (M04) for invoice and payment visibility.

The system currently serves approximately 1,130 daily active users across sales, marketing, and customer success teams. Integration monitoring shows CRM-Warehouse synchronization has executed 36 runs with 30 successful and 6 failed attempts, while CRM-Accounting synchronization maintains the same ratio. These integration health metrics are tracked on the operations dashboard (image_60) alongside system-wide user activity, new entity creation events, and cross-module data flow status. The CRM pipeline (image_67) visualizes deal progression through configurable stages, providing real-time visibility into the sales funnel, win rates, and revenue forecasts.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| Sales Director | Oversees entire sales operation, pipeline forecasting, team performance | Full access to all CRM data, reports, and configuration |
| Sales Manager | Manages sales team, reviews pipeline, approves quotes and deals | Full access to team data, read access to other teams |
| Sales Representative | Manages own accounts, contacts, opportunities, and deals | Read/write own records, read shared records |
| Account Manager | Manages post-sale customer relationships, upselling | Read/write on assigned accounts, read-only on pipeline |
| Marketing Manager | Views lead conversion data, campaign attribution | Read access to lead and opportunity data |
| Customer Support | Logs customer interactions, views account history | Read access to accounts and contacts, write access to activities |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M31 - AI Marketing | Receives qualified leads; sends back conversion and revenue data |
| M08 - Sales | Opportunities convert to sales orders; product and pricing data shared |
| M04 - Accounting | Invoice and payment history visible on account profiles |
| M20 - Communication | Activity logging (calls, emails, meetings) tied to CRM records |
| M01 - Authentication | User access control and role-based permissions |
| M10 - Asset Management | Asset data linked to customer accounts for service history |

### Key Business Rules

1. A Lead must be qualified and converted to an Opportunity before a Deal can be created; direct Lead-to-Deal conversion is not permitted.
2. Each Account must have at least one primary Contact; orphaned accounts are flagged for review after 30 days.
3. Opportunities automatically move to "Closed Lost" status after 90 days of inactivity with no logged activities.
4. Deal stages are configurable per pipeline but must include at minimum: Qualification, Proposal, Negotiation, Closed Won, Closed Lost.
5. A Sales Representative can only view and edit records they own or that are explicitly shared with them.
6. Duplicate Accounts are detected based on tax code, company name similarity (>85% match), or domain name; duplicates are flagged for manual review.
7. Competitor information is tracked at the Opportunity level to analyze win/loss patterns.
8. Quotes associated with a Deal must be approved by a Sales Manager before being sent to the customer if the total exceeds 50,000,000 VND.
9. All customer interactions (calls, emails, meetings, notes) are logged as Activities and displayed on the account timeline.
10. Pipeline data is recalculated in real-time as opportunities progress; historical snapshots are preserved weekly for trend analysis.

---

## 2. Screen Inventory

### 2.1 Screen: CRM Dashboard Overview

**Route:** `/crm/dashboard` | **Purpose:** Display key CRM metrics including pipeline value, win rates, activity counts, and integration health.

| UI Component | Description |
|--------------|-------------|
| Pipeline Value Card | Total value of all open opportunities with breakdown by stage |
| Win Rate Card | Monthly win rate percentage with trend vs. prior month |
| New Leads Card | Count of new leads this period with source breakdown |
| Activities Card | Total logged activities (calls, meetings, emails) this week |
| Integration Health | CRM-Warehouse: 30/36 success, CRM-Accounting: 30/36 success |
| Top Deals Table | Highest-value opportunities approaching close date |
| Activity Feed | Recent activities across the team |
| Quick Actions | New Lead, New Opportunity, New Contact, New Activity |

**User Interactions:**
- Click on any KPI card to drill down into detailed reports
- View integration health details and troubleshoot failures
- Click quick actions to navigate to creation forms
- Filter dashboard by date range and team

**Data Displayed:**
- Pipeline value and stage distribution
- Win rate trends
- Lead volume and sources
- Integration synchronization status

### 2.2 Screen: Lead Management

**Route:** `/crm/leads` | **Purpose:** Manage incoming leads from marketing campaigns, website forms, and manual entry.

| UI Component | Description |
|--------------|-------------|
| Lead List Table | Columns: Name, Company, Email, Phone, Source, Score, Status, Assigned To, Created |
| Kanban View | Drag-drop board organized by status: New, Contacted, Qualified, Disqualified |
| Advanced Filters | Filter by source, score range, date, assigned user, company |
| Bulk Actions | Assign, qualify, disqualify, merge duplicates, export |
| Lead Detail Panel | Slide-out panel with full lead profile and activity timeline |
| Import Button | CSV/Excel bulk import with mapping wizard |

**User Interactions:**
- Switch between list and kanban views
- Drag leads between status columns in kanban view
- Click lead to open detail panel with activity history
- Perform bulk operations on selected leads
- Import leads from CSV with field mapping

**Data Displayed:**
- Lead profile information (name, company, contact details)
- Lead score and source attribution
- Activity timeline (emails, calls, notes)
- Campaign attribution data

### 2.3 Screen: Account & Contact Management

**Route:** `/crm/accounts` | **Purpose:** Centralized database of customer accounts and individual contacts.

| UI Component | Description |
|--------------|-------------|
| Account List Table | Columns: Account Name, Tax Code, Industry, Revenue, Owner, Created |
| Contact List Table | Columns: Name, Account, Email, Phone, Title, Owner, Last Activity |
| Account Detail View | Full profile with tabs: Overview, Contacts, Opportunities, Activities, Invoices |
| Hierarchy View | Parent-child account relationships displayed as org tree |
| Duplicate Detection | Flagged potential duplicates with match score and merge action |
| Activity Timeline | Chronological feed of all interactions on the account |

**User Interactions:**
- Search accounts by name, tax code, or industry
- Click account to view full profile with related records
- View account hierarchy tree for group companies
- Merge duplicate accounts after review
- Log activities directly from account profile

**Data Displayed:**
- Account profile (company name, tax code, industry, address)
- Associated contacts and their roles
- Open and closed opportunities
- Invoice and payment history from accounting

### 2.4 Screen: Opportunity Pipeline

**Route:** `/crm/opportunities` | **Purpose:** Visualize and manage the sales pipeline with configurable stages and deal tracking.

| UI Component | Description |
|--------------|-------------|
| Pipeline Kanban | Board view with columns for each stage: Qualification, Needs Analysis, Proposal, Negotiation, Closed Won, Closed Lost |
| Deal Cards | Card shows: Deal name, account, value, probability, close date, owner |
| Pipeline Summary | Total value, weighted value, count per stage |
| Forecast View | Revenue forecast based on opportunity values and probabilities |
| Filter Bar | Filter by stage, owner, close date range, value range |
| Competitor Tracking | Competitor information displayed on deal cards |

**User Interactions:**
- Drag deals between stages to update progress
- Click deal card to edit details and log activities
- Adjust probability percentage manually or use stage defaults
- View forecast report by team and time period
- Add competitor information to deals

**Data Displayed:**
- Deal name, associated account, value
- Stage, probability, expected close date
- Weighted value (value x probability)
- Competitor names and comparison notes

### 2.5 Screen: Deal Detail

**Route:** `/crm/deals/{id}` | **Purpose:** Comprehensive view and edit of a single deal/transaction.

| UI Component | Description |
|--------------|-------------|
| Deal Header | Deal name, account, stage badge, value, close date |
| Detail Tabs | Overview, Products/Services, Quotes, Activities, Competitors, Notes |
| Product Line Items | Table: Product, quantity, unit price, discount, total |
| Quote List | Associated quotes with status and actions (send, approve, clone) |
| Activity Log | Timeline of all activities related to this deal |
| Competitor Comparison | Table comparing our offering vs. competitors |
| Probability Meter | Visual gauge showing current win probability |
| Action Bar | Next Stage, Close Won, Close Lost, Edit, Clone |

**User Interactions:**
- Add/remove products and services to deal
- Create new quotes or clone existing ones
- Submit quote for manager approval
- Log calls, meetings, emails, notes
- Advance deal to next stage or close

**Data Displayed:**
- Deal value and weighted value
- Product/service line items with pricing
- Quote history and status
- Full activity timeline
- Competitor analysis

### 2.6 Screen: Activity Management

**Route:** `/crm/activities` | **Purpose:** Log, track, and review all customer interactions and internal tasks.

| UI Component | Description |
|--------------|-------------|
| Activity List | Table with type, subject, related record, due date, status, owner |
| Activity Types | Call, Meeting, Email, Task, Note |
| Calendar View | Monthly calendar showing scheduled activities |
| Activity Form | Modal form: Type, subject, description, related record, date, duration, outcome |
| Template Library | Pre-built activity templates for common scenarios |

**User Interactions:**
- Log activities manually or sync from email/calendar
- Schedule future activities with reminders
- Link activities to accounts, contacts, opportunities, or deals
- Use templates for consistent activity logging

**Data Displayed:**
- Activity type, subject, description
- Related CRM record
- Date, time, duration, outcome
- Participants and attendees

### 2.7 Screen: Pipeline Configuration

**Route:** `/crm/settings/pipelines` | **Purpose:** Configure sales pipelines, stages, probabilities, and automation rules.

| UI Component | Description |
|--------------|-------------|
| Pipeline List | Named pipelines with stage counts and deal counts |
| Stage Editor | Drag-drop stage ordering, name, probability, color |
| Automation Rules | Trigger actions on stage change (notify, create task, update field) |
| Default Values | Set default probability, expected duration per stage |
| Access Control | Define which roles can view/edit each pipeline |

**User Interactions:**
- Create new pipelines for different sales processes
- Add, remove, reorder stages within each pipeline
- Set default probability percentages
- Configure automation rules for stage transitions

**Data Displayed:**
- Pipeline names and associated deals
- Stage definitions with probabilities
- Automation rule configurations

### 2.8 Screen: Competitor Tracking

**Route:** `/crm/competitors` | **Purpose:** Maintain competitor database and track competitive intelligence.

| UI Component | Description |
|--------------|-------------|
| Competitor List | Name, industry, strengths, weaknesses, threat level |
| Competitor Detail | Profile with products, pricing, market position |
| Win/Loss Analysis | Report showing win rates against each competitor |

**User Interactions:**
- Add and update competitor profiles
- Link competitors to lost opportunities
- View competitive analysis reports

**Data Displayed:**
- Competitor profile information
- Win/loss statistics
- Pricing comparison data

### 2.9 Screen: Product Catalog

**Route:** `/crm/products` | **Purpose:** Manage products and services available for quoting and deal creation.

| UI Component | Description |
|--------------|-------------|
| Product List | Name, SKU, category, price, status |
| Product Categories | Hierarchical category tree |
| Price List | Multiple price lists by customer type or region |
| Product Detail | Description, pricing, images, related products |

**User Interactions:**
- Create and edit product records
- Organize products into categories
- Configure multiple price lists
- Set product availability by region or customer type

**Data Displayed:**
- Product details and pricing
- Category hierarchy
- Price list configurations

### 2.10 Screen: Quote Management

**Route:** `/crm/quotes` | **Purpose:** Create, manage, and track customer quotes and proposals.

| UI Component | Description |
|--------------|-------------|
| Quote List | Number, customer, value, status, date, expiry |
| Quote Builder | Line item editor with product selection, pricing, discounts, taxes |
| Template Selector | Quote templates with company branding |
| Approval Status | Current approval state with timeline |
| PDF Preview | Generate quote document as PDF |

**User Interactions:**
- Create quotes from deals or standalone
- Add products, adjust pricing, apply discounts
- Submit for approval when threshold exceeded
- Generate and send PDF quotes to customers
- Track quote acceptance and rejection

**Data Displayed:**
- Quote line items with pricing
- Discount and tax calculations
- Approval workflow status
- Quote validity period

### 2.11 Screen: Integration Health Monitor

**Route:** `/crm/integrations` | **Purpose:** Monitor and manage integrations with Warehouse and Accounting modules.

| UI Component | Description |
|--------------|-------------|
| Integration Cards | CRM-Warehouse, CRM-Accounting with status indicators |
| Sync History | Table with run number, date, status, records synced, errors |
| Error Details | Expandable error messages with resolution steps |
| Manual Sync Trigger | Button to initiate immediate synchronization |
| Configuration | Field mappings, sync frequency, conflict resolution rules |

**User Interactions:**
- View integration health at a glance
- Review sync history and error details
- Trigger manual sync for specific integration
- Configure field mappings and rules

**Data Displayed:**
- Integration status (green/yellow/red)
- Sync run statistics (36 total, 30 success, 6 failed)
- Error messages and timestamps
- Record counts per sync

---

## 3. Feature Specifications

### 3.1 Feature: Lead Management
**Description:** Capture, score, qualify, and convert leads from multiple sources including marketing campaigns, website forms, and manual entry.

**User Stories:**
- As a Sales Representative, I want to view and manage my assigned leads, so that I can prioritize follow-up activities.
- As a Sales Manager, I want to see lead conversion rates by source, so that I can optimize marketing spend.

**Acceptance Criteria:**
- Given a new lead is created, when it meets the scoring threshold, then it is automatically flagged as Marketing Qualified.
- Given a qualified lead, when the Sales Rep marks it as qualified, then an Opportunity is created and linked to the appropriate Account.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Email | Valid email format, unique within 30 days | Please enter a valid email address. Duplicate detected |
| Phone | Valid phone format | Please enter a valid phone number |
| Company | Required for B2B leads | Company name is required for business leads |

**Edge Cases:**
- Duplicate leads from the same email within 30 days are merged automatically
- Leads from inactive campaigns are archived after 180 days
- Lead scoring recalculates when source campaign data changes

### 3.2 Feature: Account & Contact Management
**Description:** Maintain a centralized database of customer accounts and contacts with hierarchy support, duplicate detection, and relationship mapping.

**User Stories:**
- As an Account Manager, I want to view the complete account profile including contacts, opportunities, and activities, so that I have full context for customer interactions.
- As a Sales Representative, I want to detect duplicate accounts before creating new ones, so that the database remains clean.

**Acceptance Criteria:**
- Given an account creation request, when a similar account exists (tax code match or name similarity >85%), then a duplicate warning is shown.
- Given an account profile, when the user views the timeline, then all related activities, opportunities, and invoices are displayed chronologically.

**Edge Cases:**
- Merging accounts transfers all related contacts, opportunities, and activities to the master account
- Deleting an account is blocked if it has open opportunities or invoices
- Account hierarchy supports unlimited nesting levels

### 3.3 Feature: Opportunity Pipeline Management
**Description:** Manage sales opportunities through configurable pipeline stages with probability-based forecasting and competitor tracking.

**User Stories:**
- As a Sales Representative, I want to track my opportunities through pipeline stages, so that I can manage my sales process effectively.
- As a Sales Director, I want to view weighted pipeline forecasts, so that I can predict revenue accurately.

**Acceptance Criteria:**
- Given an opportunity advances to a new stage, when the stage change is saved, then the probability is updated to the stage default and notifications are sent.
- Given a pipeline view, when the user switches to forecast mode, then weighted values are calculated and aggregated by team and period.

**Edge Cases:**
- Opportunities inactive for 90 days are auto-closed as lost
- Stage transitions can be restricted by role configuration
- Probability can be manually overridden from stage default

### 3.4 Feature: Deal Management
**Description:** Track individual deals with product line items, pricing, competitor analysis, and activity logging.

**User Stories:**
- As a Sales Representative, I want to add products and services to a deal with pricing, so that I can calculate total deal value.
- As a Sales Manager, I want to review deal details before approval, so that I can ensure pricing accuracy.

**Acceptance Criteria:**
- Given a deal has products added, when the total is calculated, then line item prices, discounts, and taxes are applied correctly.
- Given a deal is marked as Closed Won, when the status is saved, then the associated account and contact are updated.

**Edge Cases:**
- Deals with zero value are allowed for service agreements
- Product prices can be overridden from catalog price with manager approval
- Closing a deal as Won triggers workflow to create sales order

### 3.5 Feature: Quote Management
**Description:** Create professional quotes with product line items, pricing, discounts, and approval workflow.

**User Stories:**
- As a Sales Representative, I want to generate quotes from deals, so that I can present pricing to customers.
- As a Sales Manager, I want to approve quotes exceeding threshold, so that pricing is controlled.

**Acceptance Criteria:**
- Given a quote is created from a deal, when products are added, then pricing flows from the deal line items.
- Given a quote total exceeds 50,000,000 VND, when the user attempts to send, then manager approval is required.

**Edge Cases:**
- Quotes expire after configured validity period and are auto-marked as expired
- Approved quotes cannot be modified without creating a revision
- Multiple quotes can be associated with a single deal

### 3.6 Feature: Activity Tracking
**Description:** Log all customer interactions (calls, meetings, emails, notes) with linking to CRM records.

**User Stories:**
- As a Sales Representative, I want to log calls and meetings, so that interaction history is maintained.
- As a Sales Manager, I want to review team activity levels, so that I can assess engagement.

**Acceptance Criteria:**
- Given an activity is logged, when it is linked to a record, then it appears on that record's timeline.
- Given a scheduled activity, when the due date arrives, then a reminder notification is sent.

**Edge Cases:**
- Activities can be linked to multiple records simultaneously
- Recurring activities can be configured for regular check-ins
- Email sync captures inbound and outbound emails automatically

### 3.7 Feature: CRM-Warehouse Integration
**Description:** Synchronize customer and order data between CRM and Warehouse modules for unified inventory visibility.

**User Stories:**
- As a Sales Representative, I want to check stock availability before quoting, so that I don't promise unavailable products.
- As a Warehouse Manager, I want to see incoming orders from CRM, so that I can prepare inventory.

**Acceptance Criteria:**
- Given the CRM-Warehouse sync runs, when order data is synchronized, then stock levels are visible on CRM product records.
- Given a sync failure, when 6 out of 36 runs fail, then an alert is sent to the system administrator.

**Edge Cases:**
- Sync conflicts are resolved using last-write-wins with audit trail
- Failed syncs are retried automatically up to 3 times
- Manual sync trigger available for on-demand synchronization

### 3.8 Feature: CRM-Accounting Integration
**Description:** Synchronize customer, deal, and invoice data between CRM and Accounting modules for financial visibility.

**User Stories:**
- As an Account Manager, I want to see invoice and payment history on the account profile, so that I understand the customer's financial status.
- As a Sales Director, I want to compare quoted amounts with invoiced amounts, so that I can track deal realization.

**Acceptance Criteria:**
- Given the CRM-Accounting sync runs, when invoice data is synchronized, then payment history is visible on account profiles.
- Given a sync failure, when error rate exceeds threshold, then the integration status changes to warning.

**Edge Cases:**
- Currency differences are handled using exchange rate at invoice date
- Partial payments are reflected proportionally on CRM records
- Tax data flows from accounting to CRM for accurate quote calculations

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|     Lead       |       |   Account      |       |   Contact      |
+----------------+       +----------------+       +----------------+
| id (PK)        |       | id (PK)        |       | id (PK)        |
| email          |       | name           |       | account_id(FK) |
| company        |       | tax_code       |       | first_name     |
| score          |       | industry       |       | last_name      |
| source         |       | annual_revenue |       | email          |
| status         |       | parent_id(FK)  |       | phone          |
| assigned_to(FK)|       | owner_id(FK)   |       | title          |
| created_at     |       | created_at     |       | owner_id(FK)   |
+----------------+       +----------------+       | created_at     |
        |                         |               +----------------+
        v                         v                      |
+----------------+       +----------------+               |
|  Opportunity   |<------|    Deal        |<--------------+
+----------------+       +----------------+
| id (PK)        |       | id (PK)        |
| account_id(FK) |       | opportunity_id |
| stage          |       | account_id(FK) |
| value          |       | value          |
| probability    |       | probability    |
| close_date     |       | close_date     |
| owner_id(FK)   |       | status         |
| created_at     |       | owner_id(FK)   |
+----------------+       | created_at     |
        |                +----------------+
        v                         |
+----------------+                |
|   Pipeline     |                v
+----------------+       +----------------+
| id (PK)        |       |    Quote       |
| name           |       +----------------+
| stages_json    |       | id (PK)        |
| created_at     |       | deal_id(FK)    |
+----------------+       | quote_number   |
                         | total          |
                         | status         |
                         | valid_until    |
                         | created_at     |
                         +----------------+
        |
        v
+----------------+       +----------------+       +----------------+
|  Activity      |       |  Competitor    |       |   Product      |
+----------------+       +----------------+       +----------------+
| id (PK)        |       | id (PK)        |       | id (PK)        |
| type           |       | name           |       | name           |
| subject        |       | industry       |       | sku            |
| related_id(FK) |       | threat_level   |       | category_id(FK)|
| related_type   |       | strengths      |       | price          |
| due_date       |       | weaknesses     |       | status         |
| status         |       | created_at     |       | created_at     |
| owner_id(FK)   |       +----------------+       +----------------+
| created_at     |
+----------------+
```

### 4.2 Table Schemas

#### Table: lead
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| first_name | VARCHAR(100) | NULL | First name |
| last_name | VARCHAR(100) | NULL | Last name |
| email | VARCHAR(200) | NOT NULL | Email address |
| phone | VARCHAR(30) | NULL | Phone number |
| company | VARCHAR(200) | NULL | Company name |
| job_title | VARCHAR(100) | NULL | Job title |
| score | INT | DEFAULT 0 | Lead score |
| source | VARCHAR(50) | NULL | Lead source |
| campaign_id | BIGINT | FK -> campaign.id | Source marketing campaign |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'new' | Status: new, contacted, qualified, disqualified, converted |
| assigned_to | BIGINT | FK -> users.id | Assigned sales rep |
| converted_to_opportunity_id | BIGINT | FK -> opportunity.id | Converted opportunity |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_lead_status ON lead(status);
CREATE INDEX idx_lead_score ON lead(score);
CREATE INDEX idx_lead_source ON lead(source);
CREATE INDEX idx_lead_assigned ON lead(assigned_to);
CREATE INDEX idx_lead_email ON lead(email);
CREATE INDEX idx_lead_campaign ON lead(campaign_id);
```

#### Table: account
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Account name |
| tax_code | VARCHAR(50) | NULL, UNIQUE | Tax identification code |
| industry | VARCHAR(100) | NULL | Industry classification |
| annual_revenue | DECIMAL(18,2) | NULL | Annual revenue |
| website | VARCHAR(255) | NULL | Website URL |
| phone | VARCHAR(30) | NULL | Main phone |
| billing_address | TEXT | NULL | Billing address |
| shipping_address | TEXT | NULL | Shipping address |
| parent_id | BIGINT | FK -> account.id | Parent account |
| owner_id | BIGINT | FK -> users.id | Account owner |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_account_tax_code ON account(tax_code);
CREATE INDEX idx_account_parent ON account(parent_id);
CREATE INDEX idx_account_owner ON account(owner_id);
CREATE INDEX idx_account_industry ON account(industry);
```

#### Table: contact
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| account_id | BIGINT | FK -> account.id | Associated account |
| first_name | VARCHAR(100) | NOT NULL | First name |
| last_name | VARCHAR(100) | NOT NULL | Last name |
| email | VARCHAR(200) | NOT NULL | Email address |
| phone | VARCHAR(30) | NULL | Phone number |
| title | VARCHAR(100) | NULL | Job title |
| department | VARCHAR(100) | NULL | Department |
| is_primary | TINYINT(1) | DEFAULT 0 | Primary contact flag |
| owner_id | BIGINT | FK -> users.id | Contact owner |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_contact_account ON contact(account_id);
CREATE INDEX idx_contact_owner ON contact(owner_id);
CREATE INDEX idx_contact_email ON contact(email);
```

#### Table: opportunity
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| account_id | BIGINT | FK -> account.id, NOT NULL | Associated account |
| lead_id | BIGINT | FK -> lead.id | Source lead |
| name | VARCHAR(200) | NOT NULL | Opportunity name |
| stage | VARCHAR(50) | NOT NULL | Pipeline stage |
| value | DECIMAL(18,2) | NOT NULL | Expected deal value |
| probability | DECIMAL(5,2) | DEFAULT 0 | Win probability percentage |
| close_date | DATE | NULL | Expected close date |
| owner_id | BIGINT | FK -> users.id | Opportunity owner |
| pipeline_id | BIGINT | FK -> pipeline.id | Associated pipeline |
| next_step | TEXT | NULL | Next action required |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_opportunity_account ON opportunity(account_id);
CREATE INDEX idx_opportunity_stage ON opportunity(stage);
CREATE INDEX idx_opportunity_owner ON opportunity(owner_id);
CREATE INDEX idx_opportunity_close_date ON opportunity(close_date);
CREATE INDEX idx_opportunity_pipeline ON opportunity(pipeline_id);
CREATE INDEX idx_opportunity_lead ON opportunity(lead_id);
```

#### Table: deal
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| opportunity_id | BIGINT | FK -> opportunity.id | Associated opportunity |
| account_id | BIGINT | FK -> account.id | Associated account |
| contact_id | BIGINT | FK -> contact.id | Primary contact |
| name | VARCHAR(200) | NOT NULL | Deal name |
| value | DECIMAL(18,2) | NOT NULL | Deal value |
| probability | DECIMAL(5,2) | DEFAULT 0 | Win probability |
| close_date | DATE | NULL | Close date |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'open' | Status: open, closed_won, closed_lost |
| loss_reason | TEXT | NULL | Reason for loss |
| owner_id | BIGINT | FK -> users.id | Deal owner |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_deal_opportunity ON deal(opportunity_id);
CREATE INDEX idx_deal_account ON deal(account_id);
CREATE INDEX idx_deal_status ON deal(status);
CREATE INDEX idx_deal_owner ON deal(owner_id);
```

#### Table: quote
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| deal_id | BIGINT | FK -> deal.id | Associated deal |
| quote_number | VARCHAR(50) | NOT NULL, UNIQUE | Quote reference number |
| total | DECIMAL(18,2) | NOT NULL | Total quote amount |
| discount | DECIMAL(18,2) | DEFAULT 0 | Total discount |
| tax | DECIMAL(18,2) | DEFAULT 0 | Total tax |
| grand_total | DECIMAL(18,2) | NOT NULL | Grand total |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status: draft, pending_approval, approved, sent, accepted, rejected, expired |
| valid_until | DATE | NULL | Quote validity date |
| notes | TEXT | NULL | Quote notes |
| approved_by | BIGINT | FK -> users.id | Approving manager |
| approved_at | TIMESTAMP | NULL | Approval timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_quote_number ON quote(quote_number);
CREATE INDEX idx_quote_deal ON quote(deal_id);
CREATE INDEX idx_quote_status ON quote(status);
```

#### Table: quote_item
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| quote_id | BIGINT | FK -> quote.id, NOT NULL | Parent quote |
| product_id | BIGINT | FK -> product.id | Product |
| description | TEXT | NULL | Line item description |
| quantity | DECIMAL(10,2) | NOT NULL | Quantity |
| unit_price | DECIMAL(18,2) | NOT NULL | Unit price |
| discount | DECIMAL(5,2) | DEFAULT 0 | Discount percentage |
| tax | DECIMAL(5,2) | DEFAULT 0 | Tax percentage |
| total | DECIMAL(18,2) | NOT NULL | Line total |
| sort_order | INT | DEFAULT 0 | Display order |

**Indexes:**
```sql
CREATE INDEX idx_quote_item_quote ON quote_item(quote_id);
CREATE INDEX idx_quote_item_product ON quote_item(product_id);
```

#### Table: activity
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| type | VARCHAR(30) | NOT NULL | Type: call, meeting, email, task, note |
| subject | VARCHAR(200) | NOT NULL | Activity subject |
| description | TEXT | NULL | Activity description |
| related_id | BIGINT | NOT NULL | Related record ID |
| related_type | VARCHAR(50) | NOT NULL | Related record type: account, contact, opportunity, deal, lead |
| due_date | TIMESTAMP | NULL | Scheduled date/time |
| completed_at | TIMESTAMP | NULL | Completion timestamp |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'pending' | Status: pending, completed, cancelled |
| duration_minutes | INT | NULL | Activity duration |
| outcome | TEXT | NULL | Activity outcome |
| owner_id | BIGINT | FK -> users.id | Activity owner |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_activity_related ON activity(related_type, related_id);
CREATE INDEX idx_activity_type ON activity(type);
CREATE INDEX idx_activity_status ON activity(status);
CREATE INDEX idx_activity_owner ON activity(owner_id);
CREATE INDEX idx_activity_due_date ON activity(due_date);
```

#### Table: pipeline
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Pipeline name |
| stages_json | JSON | NOT NULL | Stage definitions with order, name, probability, color |
| is_default | TINYINT(1) | DEFAULT 0 | Default pipeline flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_pipeline_default ON pipeline(is_default);
```

#### Table: competitor
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Competitor name |
| industry | VARCHAR(100) | NULL | Industry |
| strengths | TEXT | NULL | Known strengths |
| weaknesses | TEXT | NULL | Known weaknesses |
| threat_level | VARCHAR(20) | DEFAULT 'medium' | Threat level: low, medium, high |
| website | VARCHAR(255) | NULL | Website |
| notes | TEXT | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_competitor_threat ON competitor(threat_level);
CREATE INDEX idx_competitor_industry ON competitor(industry);
```

#### Table: deal_competitor
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| deal_id | BIGINT | FK -> deal.id, NOT NULL | Associated deal |
| competitor_id | BIGINT | FK -> competitor.id, NOT NULL | Competitor |
| comparison_notes | TEXT | NULL | Comparison notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_deal_competitor_unique ON deal_competitor(deal_id, competitor_id);
```

#### Table: product
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Product name |
| sku | VARCHAR(50) | NULL, UNIQUE | Stock keeping unit |
| category_id | BIGINT | FK -> product_category.id | Product category |
| description | TEXT | NULL | Product description |
| price | DECIMAL(18,2) | NOT NULL | Base price |
| unit | VARCHAR(20) | DEFAULT 'unit' | Unit of measure |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, inactive |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_product_sku ON product(sku);
CREATE INDEX idx_product_category ON product(category_id);
CREATE INDEX idx_product_status ON product(status);
```

#### Table: integration_sync_log
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| integration_type | VARCHAR(50) | NOT NULL | Type: warehouse, accounting |
| run_number | INT | NOT NULL | Sequential run number |
| status | VARCHAR(30) | NOT NULL | Status: success, failed, partial |
| records_synced | INT | DEFAULT 0 | Number of records synced |
| error_count | INT | DEFAULT 0 | Number of errors |
| error_details | TEXT | NULL | Error messages |
| started_at | TIMESTAMP | NOT NULL | Sync start |
| completed_at | TIMESTAMP | NULL | Sync completion |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_sync_log_type ON integration_sync_log(integration_type);
CREATE INDEX idx_sync_log_status ON integration_sync_log(status);
CREATE INDEX idx_sync_log_started ON integration_sync_log(started_at);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| lead | Lead records | 500,000 |
| account | Customer accounts | 50,000 |
| contact | Individual contacts | 200,000 |
| opportunity | Sales opportunities | 100,000 |
| deal | Deal records | 80,000 |
| quote | Customer quotes | 60,000 |
| quote_item | Quote line items | 300,000 |
| activity | Activity logs | 1,000,000 |
| pipeline | Pipeline configurations | 20 |
| competitor | Competitor database | 500 |
| deal_competitor | Deal-competitor mapping | 20,000 |
| product | Product catalog | 5,000 |
| integration_sync_log | Sync run history | 10,000 |

---

## 5. API Specifications

### 5.1 Lead Endpoints

#### GET /api/v1/crm/leads
**Description:** List leads with filtering and pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| limit | integer | No | Items per page (default: 20, max: 100) |
| status | string | No | Filter by status |
| source | string | No | Filter by source |
| assigned_to | integer | No | Filter by assigned user |
| score_min | integer | No | Minimum score |
| sort | string | No | Sort field |
| order | string | No | asc/desc |

**Permission:** `crm:lead:read`

#### POST /api/v1/crm/leads
**Description:** Create a new lead

**Permission:** `crm:lead:create`

#### PUT /api/v1/crm/leads/{id}/convert
**Description:** Convert a lead to an opportunity

**Request Body:**
```json
{
  "account_id": 123,
  "opportunity_name": "Q2 Software License Deal",
  "value": 500000000,
  "close_date": "2026-06-30"
}
```

**Permission:** `crm:lead:convert`

### 5.2 Account Endpoints

#### GET /api/v1/crm/accounts
**Description:** List accounts with filtering

**Permission:** `crm:account:read`

#### POST /api/v1/crm/accounts
**Description:** Create a new account

**Permission:** `crm:account:create`

#### GET /api/v1/crm/accounts/{id}/hierarchy
**Description:** Get account hierarchy tree

**Response (200 OK):**
```json
{
  "account_id": 1,
  "name": "Phuong Nam Food Corp",
  "children": [
    {"id": 2, "name": "Long An Subsidiary"},
    {"id": 3, "name": "Dong Nai Subsidiary"}
  ]
}
```

### 5.3 Opportunity Endpoints

#### GET /api/v1/crm/opportunities
**Description:** List opportunities with pipeline filtering

**Permission:** `crm:opportunity:read`

#### PUT /api/v1/crm/opportunities/{id}/stage
**Description:** Advance opportunity to next stage

**Request Body:**
```json
{
  "stage": "Negotiation",
  "next_step": "Schedule demo with CTO"
}
```

**Permission:** `crm:opportunity:update`

### 5.4 Quote Endpoints

#### POST /api/v1/crm/quotes/{id}/submit-for-approval
**Description:** Submit quote for manager approval

**Permission:** `crm:quote:submit`

#### POST /api/v1/crm/quotes/{id}/approve
**Description:** Approve a pending quote

**Permission:** `crm:quote:approve`

### 5.5 Activity Endpoints

#### GET /api/v1/crm/activities
**Description:** List activities with filtering

**Permission:** `crm:activity:read`

#### POST /api/v1/crm/activities
**Description:** Log a new activity

**Permission:** `crm:activity:create`

### 5.6 Integration Endpoints

#### GET /api/v1/crm/integrations/status
**Description:** Get integration health status

**Response (200 OK):**
```json
{
  "warehouse": {"status": "warning", "total_runs": 36, "success": 30, "failed": 6},
  "accounting": {"status": "warning", "total_runs": 36, "success": 30, "failed": 6}
}
```

#### POST /api/v1/crm/integrations/{type}/sync
**Description:** Trigger manual sync

**Permission:** `crm:integration:sync`

### 5.7 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/crm/leads | List leads | crm:lead:read |
| POST | /api/v1/crm/leads | Create lead | crm:lead:create |
| PUT | /api/v1/crm/leads/{id}/convert | Convert lead | crm:lead:convert |
| GET | /api/v1/crm/accounts | List accounts | crm:account:read |
| POST | /api/v1/crm/accounts | Create account | crm:account:create |
| GET | /api/v1/crm/accounts/{id}/hierarchy | Account hierarchy | crm:account:read |
| GET | /api/v1/crm/opportunities | List opportunities | crm:opportunity:read |
| PUT | /api/v1/crm/opportunities/{id}/stage | Update stage | crm:opportunity:update |
| GET | /api/v1/crm/deals | List deals | crm:deal:read |
| POST | /api/v1/crm/deals | Create deal | crm:deal:create |
| GET | /api/v1/crm/quotes | List quotes | crm:quote:read |
| POST | /api/v1/crm/quotes | Create quote | crm:quote:create |
| POST | /api/v1/crm/quotes/{id}/submit-for-approval | Submit for approval | crm:quote:submit |
| POST | /api/v1/crm/quotes/{id}/approve | Approve quote | crm:quote:approve |
| GET | /api/v1/crm/activities | List activities | crm:activity:read |
| POST | /api/v1/crm/activities | Create activity | crm:activity:create |
| GET | /api/v1/crm/competitors | List competitors | crm:competitor:read |
| GET | /api/v1/crm/products | List products | crm:product:read |
| GET | /api/v1/crm/integrations/status | Integration status | crm:integration:read |
| POST | /api/v1/crm/integrations/{type}/sync | Trigger sync | crm:integration:sync |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Sales Director | sales_director | Full access to all CRM data, reports, and configuration |
| Sales Manager | sales_manager | Full access to team data, approval authority |
| Sales Representative | sales_rep | Access to own records and shared records |
| Account Manager | account_manager | Post-sale account management |
| Marketing Manager | marketing_manager | Read access to lead and opportunity data |

### Permission Matrix
| Role | Create | Read | Update | Delete | Export | Approve Quote | View All Pipelines |
|------|--------|------|--------|--------|--------|--------------|-------------------|
| Sales Director | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Sales Manager | Yes | Yes | Yes | No | Yes | Yes | Yes (team only) |
| Sales Representative | Yes | Own + Shared | Own | No | No | No | No |
| Account Manager | No | Assigned | Yes (notes) | No | No | No | No |
| Marketing Manager | No | Read Only | No | No | Yes | No | No |

### Row-Level Security
- Sales Representatives can only view and edit leads, accounts, opportunities, and deals they own.
- Records explicitly shared with a user via the sharing feature become readable/writable by that user.
- Sales Managers can view all records owned by their direct reports.
- Sales Directors have unrestricted read access to all CRM data across all teams and companies.

---

## 7. State Machine / Workflows

### 7.1 Lead Status Transitions

```
[NEW] ──contact──→ [CONTACTED] ──qualify──→ [QUALIFIED] ──convert──→ [CONVERTED]
  │                     │                      │
  └──disqualify────────┘                      └──disqualify──→ [DISQUALIFIED]
```

### 7.2 Opportunity Stage Transitions

```
[QUALIFICATION] ──advance──→ [NEEDS_ANALYSIS] ──advance──→ [PROPOSAL] ──advance──→ [NEGOTIATION]
      │                           │                          │                        │
      └──close_lost───────────────┴──────────────────────────┴────────────────────────┤
                                                                                      v
                                                                       [CLOSED_WON] / [CLOSED_LOST]
```

### 7.3 Quote Approval Workflow

```
[DRAFT] ──submit──→ [PENDING_APPROVAL] ──approve──→ [APPROVED] ──send──→ [SENT]
     │                    │                          │
     └──send(<50M)────────┘                          └──reject──→ [REJECTED]
```

### 7.4 Integration Sync Workflow

```
[TRIGGERED] ──sync──→ [IN_PROGRESS] ──complete──→ [SUCCESS]
     │                      │
     │                      └──error──→ [FAILED] ──retry(3x)──→ [IN_PROGRESS]
     │                                          │
     └──manual──────────────────────────────────┘
```

---

## 8. Reports & Analytics

### 8.1 Report: Pipeline Overview
**Type:** Real-time
**Purpose:** Display sales pipeline value, stage distribution, and forecast

**Metrics:**
- Total pipeline value, weighted value
- Deal count by stage
- Average deal size, average sales cycle

**Chart Type:** Funnel chart, Bar chart (value by stage)

### 8.2 Report: Win Rate Analysis
**Type:** Real-time
**Purpose:** Analyze win rates by team, product, source, and time period

**Metrics:**
- Win rate percentage
- Won vs. lost counts
- Average sales cycle for won deals

**Chart Type:** Donut chart, Line chart (trend)

### 8.3 Report: Sales Forecast
**Type:** Real-time
**Purpose:** Predict revenue based on opportunity values and probabilities

**Metrics:**
- Weighted pipeline value
- Forecast by month
- Best-case vs. commit vs. pipeline scenarios

**Chart Type:** Bar chart, Table

### 8.4 Report: Activity Analysis
**Type:** Real-time
**Purpose:** Monitor team activity levels and engagement patterns

**Metrics:**
- Activities by type and user
- Completion rate
- Average response time

**Chart Type:** Stacked bar chart, Table

### 8.5 Report: Competitor Analysis
**Type:** Real-time
**Purpose:** Track win/loss patterns against competitors

**Metrics:**
- Win rate per competitor
- Lost deals by competitor
- Average deal value vs. competitors

**Chart Type:** Bar chart, Table

### 8.6 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Pipeline Overview | Real-time | On-demand | Chart, CSV |
| Win Rate Analysis | Real-time | Weekly | Donut, PDF |
| Sales Forecast | Real-time | Monthly | Bar Chart, Excel |
| Activity Analysis | Real-time | Weekly | Table, CSV |
| Competitor Analysis | Real-time | Monthly | Chart, PDF |
| Lead Conversion | Real-time | On-demand | Funnel, CSV |
| Revenue Attribution | Real-time | Monthly | Table |
| Sales Team Performance | Real-time | Weekly | Table |
| Account Health | Real-time | On-demand | Dashboard |
| Quote Conversion Rate | Real-time | Monthly | Chart |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| MISA Warehouse API | Stock level sync, order fulfillment status | REST | API Key |
| MISA Accounting API | Invoice and payment data sync | REST | OAuth 2.0 |
| Email Provider API | Email activity sync (sent, opened, clicked) | REST | API Key |
| Calendar API | Meeting sync (Google Calendar, Outlook) | REST | OAuth 2.0 |
| Phone System API | Call log integration | REST | API Key |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| lead.converted | {lead_id, opportunity_id, account_id} | Marketing (M31), Workflow (M19) |
| deal.closed_won | {deal_id, account_id, value, close_date} | Accounting (M04), Sales (M08) |
| opportunity.stage_changed | {opportunity_id, old_stage, new_stage} | Reporting, Workflow |
| quote.approved | {quote_id, deal_id, total} | Sales (M08) |
| account.created | {account_id, name, tax_code} | Warehouse, Accounting |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| lead.created | Marketing (M31) | Create CRM lead record |
| email.sent | Communication (M20) | Log email activity on contact timeline |
| invoice.paid | Accounting (M04) | Update account payment history |
| order.fulfilled | Warehouse | Update deal fulfillment status |

---

## 10. Technical Notes

### Performance
- Expected data volume: 500,000+ leads, 50,000+ accounts, 200,000+ contacts, 100,000+ opportunities
- Query complexity: High (multi-table joins for pipeline reports, hierarchy queries for account trees)
- Expected response time: Dashboard < 2s, List views < 1s, Detail views < 500ms, Pipeline Kanban < 1.5s
- Integration sync runs process 10,000+ records per run with < 5 minute execution time

### Caching Strategy
- Redis cache for pipeline snapshots, account hierarchies, and competitor analysis
- TTL: 5 minutes for real-time dashboards, 30 minutes for static reference data
- Cache invalidation on record update, stage change, or manual refresh
- Account hierarchy tree cached per user with 15-minute TTL

### File Attachments
- Supported types: PDF, DOC, DOCX, XLS, XLSX, PNG, JPG
- Max size: 25 MB per file (quotes, proposals, meeting notes)
- Storage: S3-compatible object storage with CDN distribution

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY (Vietnamese), YYYY-MM-DD (ISO)
- Currency: VND (Vietnamese Dong) with multi-currency support for international deals
- Number formatting: Vietnamese locale conventions

### Mobile Considerations
- CRM dashboard has responsive layout for tablet access
- Mobile app supports lead creation, activity logging, and opportunity stage updates
- Offline mode caches assigned records for field sales use
- Mobile-optimized quote viewing and approval

### Security
- Customer data is encrypted at rest using AES-256
- Row-level security enforces ownership-based access control
- Audit trail tracks all record changes with before/after values
- API rate limiting: 100 requests per minute per user for read, 20 for write
- Integration credentials stored in encrypted vault

### Scalability
- Activity logging uses async event processing to prevent timeline bottlenecks
- Pipeline Kanban data is pre-aggregated and cached for fast loading
- Bulk lead import is processed in background with chunked batches
- Real-time notifications use WebSocket connections for instant updates
