# M31 - AI Marketing (Tiep thi Thong minh)

**Category:** Business
**Priority:** P2
**Reference Images:** assets/image_09.png, assets/image_10.png, assets/image_14.png, assets/image_31.png, assets/image_32.png, assets/image_60.png
**Dependencies:** M08 (Sales), M32 (CRM), M20 (Communication)
**Generated:** April 14, 2026

---

## 1. Module Overview

The AI Marketing module provides a comprehensive marketing automation platform integrated directly into the ABN ERP system. It enables marketing teams to plan, execute, track, and optimize campaigns across multiple channels -- email, landing pages, SMS, social media (Facebook, Zalo), and web forms -- all from a single unified interface. The module leverages AI-powered features to segment audiences, optimize send times, predict lead scores, and recommend content personalization.

The marketing dashboard (image_09) presents a high-level overview of campaign performance with real-time metrics: total revenue generated of 40,435,500,000 VND (+15.2% vs. prior period), total marketing costs of 20,121,500,000 VND, and an overall marketing ROI of 28.55%. The funnel visualization (image_10) tracks the complete customer journey from initial Lead capture through Marketing Qualified Lead (MQL), Sales Accepted Lead (SAL), to Sales Qualified Lead (SQL), showing conversion rates at each stage: Lead 1,852,371 -> MQL 1,239,091 (66.9%) -> SAL 1,055,108 (85.1%) -> SQL 1,012,242 (96.0%). The customer care dashboard (image_14) monitors opportunity conversion at 38%, revenue contribution of 25.1%, and average order value (AOV) of 58,950,000 VND. The platform serves approximately 1,130 daily active users across marketing, sales, and customer success teams.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| Marketing Director | Oversees all marketing campaigns, budgets, ROI analysis, team performance | Full access to all marketing data, reports, and configuration |
| Campaign Manager | Creates and manages individual campaigns across email, social, landing pages | Full access to assigned campaigns, read access to others |
| Content Creator | Designs email templates, landing page content, form layouts | Write access to templates and content, read access to campaign data |
| Sales Representative | Receives qualified leads, updates lead status, tracks conversion | Read/write on assigned leads, read-only on campaign data |
| Marketing Analyst | Monitors funnel metrics, generates reports, performs A/B test analysis | Read access to all data, write access to reports and dashboards |
| System Administrator | Manages integrations, API keys, user access, system configuration | Full system access, read-only marketing analytics |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M32 - CRM | Leads flow from marketing into CRM pipeline; opportunity data feeds back to marketing ROI |
| M08 - Sales | Order and revenue data from sales module used to calculate marketing-attributed revenue |
| M20 - Communication | SMS and messaging infrastructure used for SMS campaigns |
| M01 - Authentication | User access control and role-based permissions |
| M38 - OneAI Platform | AI models used for content generation, lead scoring, and audience segmentation |

### Key Business Rules

1. A Lead can only be converted to MQL when it meets the minimum scoring threshold configured in the lead scoring rules.
2. Duplicate leads are detected and merged based on email address matching; the first created lead is retained as the master record.
3. Email campaigns must pass spam filtering checks (SPF, DKIM, DMARC) before being sent; campaigns failing validation are automatically paused.
4. Landing pages must be published before they can accept form submissions; unpublished pages return a 404 response.
5. A/B tests must run for a minimum of 48 hours or until statistical significance (95% confidence) is reached, whichever occurs later.
6. Marketing costs are allocated to campaigns based on actual spend; budget overruns trigger automatic alerts to the Marketing Director.
7. Lead data must comply with data protection regulations; consent flags are required before any marketing communication is sent.
8. The conversion funnel is recalculated in real-time as new leads enter or progress through stages; historical snapshots are preserved for trend analysis.
9. SMS campaigns are limited to 160 characters per message; longer messages are split into multiple segments and charged accordingly.
10. Facebook campaign posts require valid API tokens and page access tokens that expire every 60 days; expired tokens trigger notification to the Campaign Manager.

---

## 2. Screen Inventory

### 2.1 Screen: Marketing Dashboard Overview

**Route:** `/marketing/dashboard` | **Purpose:** Display high-level marketing performance metrics, revenue, costs, and ROI at a glance.

| UI Component | Description |
|--------------|-------------|
| Revenue Card | Total revenue: 40,435,500,000 VND with +15.2% trend indicator vs. prior period |
| Cost Card | Total marketing cost: 20,121,500,000 VND with breakdown by channel |
| ROI Card | Marketing ROI: 28.55% calculated as (Revenue - Cost) / Cost |
| Campaign Count | Total active campaigns with status breakdown (active/paused/completed) |
| Daily Users | 1,130 active users currently interacting with marketing features |
| Revenue Trend Chart | Monthly line chart showing revenue trajectory over last 12 months |
| Cost Breakdown Chart | Stacked bar chart showing cost allocation by channel (email, social, landing page, SMS) |
| Quick Actions | Buttons: New Campaign, New Email, New Landing Page, New Lead Form |

**User Interactions:**
- Click on any KPI card to drill down into detailed reports
- Select date range filter (Today, This Week, This Month, Custom) to adjust all metrics
- Click quick action buttons to navigate to creation forms
- Hover over trend chart data points to see exact values

**Data Displayed:**
- Revenue, cost, ROI totals and trends
- Active campaign count and user activity
- Channel-level cost breakdowns

### 2.2 Screen: Conversion Funnel

**Route:** `/marketing/funnel` | **Purpose:** Visualize the lead-to-customer conversion pipeline with stage-by-stage metrics.

| UI Component | Description |
|--------------|-------------|
| Funnel Visualization | Horizontal funnel chart with 4 stages: Lead (1,852,371), MQL (1,239,091), SAL (1,055,108), SQL (1,012,242) |
| Stage Conversion Rate | Percentage labels between each stage (66.9%, 85.1%, 96.0%) |
| Time Period Selector | Filter funnel data by date range |
| Stage Detail Table | For each stage: count, conversion rate, average time in stage, top source channels |
| Drop-off Analysis | Shows number and percentage of leads lost at each transition |
| Source Attribution | Pie chart showing lead sources: Website (45%), Social (25%), Referral (15%), Email (10%), Other (5%) |

**User Interactions:**
- Click on a funnel stage to view the list of leads at that stage
- Hover over drop-off areas to see reasons for conversion loss
- Export funnel data as CSV or PDF
- Compare funnel across different time periods

**Data Displayed:**
- Absolute counts at each funnel stage
- Conversion rates between stages
- Average time spent in each stage
- Source channel distribution

### 2.3 Screen: Customer Care Dashboard

**Route:** `/marketing/customer-care` | **Purpose:** Monitor customer engagement metrics and opportunity pipeline.

| UI Component | Description |
|--------------|-------------|
| Opportunity Conversion Rate | 38% of marketing-generated opportunities converted to deals |
| Revenue Contribution | 25.1% of total company revenue attributed to marketing efforts |
| Average Order Value | 58,950,000 VND per marketing-sourced order |
| Customer Lifetime Value | Projected LTV chart by cohort |
| Engagement Timeline | Recent customer touchpoints and interactions |
| Satisfaction Score | NPS and CSAT metrics from post-campaign surveys |
| Top Performing Campaigns | Table ranking campaigns by revenue generated |

**User Interactions:**
- Click opportunity rate to see list of converted vs. lost opportunities
- View individual customer journeys by clicking engagement timeline
- Filter by campaign, channel, or time period

**Data Displayed:**
- Opportunity-to-deal conversion metrics
- Marketing-attributed revenue percentage
- Average order value and customer lifetime value
- Campaign performance rankings

### 2.4 Screen: Email Campaign Manager

**Route:** `/marketing/email-campaigns` | **Purpose:** Create, manage, and monitor email marketing campaigns with drag-drop template builder.

| UI Component | Description |
|--------------|-------------|
| Campaign List Table | Columns: Name, Status, Sent, Opened, Clicked, Bounced, Unsubscribed, Date |
| Template Gallery | Pre-built email templates organized by industry and use case |
| Drag-Drop Builder | Visual editor with content blocks (text, image, button, divider, social, spacer) |
| Recipient Selector | Choose from contact lists, segments, or dynamic queries |
| Spam Filter Checker | Validates SPF, DKIM, DMARC alignment before sending |
| Send Scheduler | Schedule send date/time with timezone support |
| A/B Test Configuration | Set up split tests for subject lines, content, send times |

**User Interactions:**
- Drag content blocks from sidebar onto canvas to build email
- Click on any block to edit content, styling, and properties
- Preview email in desktop and mobile views
- Test send to self or colleagues before full campaign launch
- Pause/resume active campaigns
- Clone existing campaigns for reuse

**Data Displayed:**
- Campaign performance metrics (open rate, click rate, bounce rate)
- Template preview thumbnails
- Recipient list counts

### 2.5 Screen: Landing Page Builder

**Route:** `/marketing/landing-pages` | **Purpose:** Design and publish no-code landing pages for lead capture and campaign promotion.

| UI Component | Description |
|--------------|-------------|
| Page List Table | Columns: Name, URL, Status, Views, Submissions, Conversion Rate, Last Modified |
| Template Library | Landing page templates by industry: SaaS, E-commerce, Event, Webinar, Product Launch |
| Visual Builder | Drag-drop page builder with sections (hero, features, testimonials, CTA, footer) |
| SEO Settings | Meta title, description, Open Graph tags, canonical URL |
| Publishing Controls | Publish/Unpublish toggle, custom domain configuration |
| Form Builder | Embedded lead capture form builder with field configuration |
| Analytics Panel | Real-time visitor count, heat map, scroll depth tracking |

**User Interactions:**
- Select template and customize using drag-drop builder
- Add/remove sections, reorder via drag
- Configure embedded form fields and validation rules
- Preview on desktop, tablet, and mobile viewports
- Publish page with single click
- View real-time analytics dashboard

**Data Displayed:**
- Page URL and status
- Visitor count and conversion rate
- Form submission count and field-level analytics

### 2.6 Screen: Lead Form Manager

**Route:** `/marketing/lead-forms` | **Purpose:** Create and manage web forms for capturing leads from website and landing pages.

| UI Component | Description |
|--------------|-------------|
| Form List Table | Columns: Name, Embed Code, Submissions, Conversion Rate, Status |
| Form Builder | Drag-drop form builder with field types (text, email, phone, dropdown, checkbox, textarea) |
| Embed Code Generator | Generates JavaScript embed snippet for website integration |
| Field Configuration | Required/optional toggle, validation rules, placeholder text |
| Thank You Page | Configure redirect URL or inline thank-you message |
| Auto-Responder | Set up automatic confirmation email on form submission |
| Spam Protection | reCAPTCHA integration, honeypot fields, IP rate limiting |

**User Interactions:**
- Build form by dragging fields onto canvas
- Configure field properties and validation
- Generate and copy embed code for website deployment
- Test form submission in preview mode
- View submission data in table format

**Data Displayed:**
- Form submission count and conversion rate
- Field-level completion rates
- Source URL of each submission

### 2.7 Screen: Contact & Data Management

**Route:** `/marketing/contacts` | **Purpose:** Centralized contact database with segmentation, journey tracking, and data management.

| UI Component | Description |
|--------------|-------------|
| Contact List Table | Columns: Name, Email, Phone, Company, Score, Status, Last Activity, Source |
| Advanced Search | Full-text search across all contact fields and custom attributes |
| Segment Builder | Rule-based contact segmentation (e.g., "Opened email in last 30 days AND score > 50") |
| Journey Timeline | Visual timeline showing each contact's interactions across all channels |
| Bulk Actions | Add to segment, assign to campaign, export, delete, update fields |
| Custom Fields | Define and manage custom contact attributes |
| Data Quality Dashboard | Duplicate count, incomplete records, bounce status, consent status |

**User Interactions:**
- Search and filter contacts using multiple criteria
- Create dynamic segments that auto-update based on rules
- Click contact to view full profile and journey timeline
- Perform bulk operations on selected contacts
- Export contact lists to CSV

**Data Displayed:**
- Contact profile data and activity history
- Segment membership counts
- Data quality metrics

### 2.8 Screen: Marketing Reports

**Route:** `/marketing/reports` | **Purpose:** Access 20+ real-time marketing reports with filtering, charting, and export capabilities.

| UI Component | Description |
|--------------|-------------|
| Report Catalog | Grid of 20+ report cards organized by category (Campaign, Funnel, Revenue, Channel) |
| Report Viewer | Dynamic report with filters, data table, and chart visualization |
| Chart Type Selector | Switch between bar, line, pie, table views |
| Date Range Filter | Apply date range to all reports |
| Export Options | Export to PDF, Excel, CSV, or schedule recurring email delivery |
| Comparison Mode | Compare two time periods side by side |
| Saved Reports | Bookmark and organize frequently used reports |

**User Interactions:**
- Browse report catalog and select report
- Apply filters (date range, campaign, channel, segment)
- Switch chart types to view data differently
- Export report or schedule automated delivery
- Save report configuration for quick access

**Data Displayed:**
- 20+ marketing reports including: Campaign Performance, Email Analytics, Funnel Analysis, ROI by Channel, Lead Source Report, A/B Test Results, Cost per Acquisition, Revenue Attribution, Contact Growth, Engagement Trends

### 2.9 Screen: Integration Hub

**Route:** `/marketing/integrations` | **Purpose:** Manage connections to external channels and platforms.

| UI Component | Description |
|--------------|-------------|
| Integration Cards | CRM, SMS Gateway, Facebook, Zalo, Website, API |
| Connection Status | Green/yellow/red indicators for each integration |
| Configuration Forms | API keys, webhooks, field mappings, sync frequency |
| Sync Log | Recent synchronization events with status and error details |
| Test Connection | Button to verify integration connectivity |

**User Interactions:**
- Configure API credentials for each channel
- Enable/disable integrations
- View sync history and troubleshoot errors
- Test connection before going live

**Data Displayed:**
- Integration health status
- Last sync timestamp and record counts
- Error messages and resolution hints

---

## 3. Feature Specifications

### 3.1 Feature: Email Marketing Campaign
**Description:** Create, send, and track email marketing campaigns with drag-drop template builder, bulk sending, and spam filtering.

**User Stories:**
- As a Campaign Manager, I want to build email templates using drag-drop blocks, so that I can create professional emails without coding.
- As a Marketing Director, I want to schedule bulk email sends to segmented contact lists, so that I can reach the right audience at the right time.
- As a Content Creator, I want spam filtering to validate my emails before sending, so that deliverability rates remain high.

**Acceptance Criteria:**
- Given a campaign is in draft status, when the user builds an email using the drag-drop editor and clicks Save, then the email is saved as a draft template.
- Given a draft email campaign, when the user passes spam filter validation and schedules the send, then the campaign is queued for delivery at the specified time.
- Given an active email campaign, when recipients open or click the email, then the open and click events are tracked and reflected in real-time metrics.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Subject line | Required, max 100 characters | Subject line is required and must be under 100 characters |
| Recipient list | At least 1 contact required | Please select at least one recipient |
| From email | Must match verified domain | From email must use a verified and authenticated domain |
| Send date | Must be in the future | Scheduled send date must be in the future |
| SPF/DKIM | Domain must pass validation | Email authentication failed. Please verify your domain settings |

**Edge Cases:**
- Campaign sent to a segment with zero matching contacts shows warning and prevents send
- Email bounced due to recipient mailbox full is logged and contact is flagged
- Unsubscribe link must be present; if missing, system auto-injects before sending
- If API rate limit is reached during bulk send, system pauses and resumes automatically

### 3.2 Feature: Landing Page Builder
**Description:** Design and publish no-code landing pages with visual drag-drop builder, SEO optimization, and embedded lead capture forms.

**User Stories:**
- As a Content Creator, I want to build landing pages without writing code, so that I can quickly launch campaign pages.
- As a Campaign Manager, I want landing pages to track visitor analytics, so that I can measure campaign effectiveness.

**Acceptance Criteria:**
- Given a user selects a template, when they customize content and publish, then the page is live at the configured URL.
- Given a published landing page, when a visitor submits the embedded form, then a new Lead record is created in the system.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Page title | Required, max 200 characters | Page title is required |
| URL slug | Alphanumeric and hyphens only, unique | URL must be unique and contain only letters, numbers, and hyphens |
| Meta description | Max 160 characters | Meta description must be under 160 characters for optimal SEO |

**Edge Cases:**
- Publishing a page with a duplicate URL slug appends a numeric suffix
- High traffic volumes are handled via CDN caching with configurable TTL
- Form submissions during page downtime are queued and processed when page is restored

### 3.3 Feature: Lead Form Builder
**Description:** Create web forms for capturing leads with customizable fields, validation, spam protection, and auto-responders.

**User Stories:**
- As a Campaign Manager, I want to embed lead capture forms on any website, so that I can grow my contact database.
- As a Content Creator, I want form submissions to automatically create Lead records, so that no manual data entry is needed.

**Acceptance Criteria:**
- Given a form is configured with required fields, when a visitor submits incomplete data, then validation errors are displayed inline.
- Given a form submission passes validation, when it is submitted, then a Lead record is created and the auto-responder email is sent.

**Edge Cases:**
- Duplicate submissions from the same email within 5 minutes are blocked
- Forms with more than 20 fields show a progress indicator to reduce abandonment
- reCAPTCHA failure shows a user-friendly error without exposing security details

### 3.4 Feature: Contact Data Management
**Description:** Centralized contact database with segmentation, journey tracking, custom fields, and data quality management.

**User Stories:**
- As a Marketing Analyst, I want to segment contacts by behavior and attributes, so that I can target campaigns precisely.
- As a Campaign Manager, I want to see each contact's full journey, so that I can personalize interactions.

**Acceptance Criteria:**
- Given segment rules are defined, when contact data changes, then segment membership is updated within 15 minutes.
- Given a contact profile, when the user opens the journey timeline, then all interactions (email opens, clicks, form submissions, page views) are displayed chronologically.

**Edge Cases:**
- Merging duplicate contacts preserves all historical interactions and attributes
- Deleting a contact removes them from all segments and stops future communications
- Import of 100,000+ contacts is processed asynchronously with progress notification

### 3.5 Feature: Marketing Reports (20+ Real-time)
**Description:** Access over 20 real-time marketing reports with interactive charts, filtering, and export capabilities.

**User Stories:**
- As a Marketing Director, I want to view ROI reports by channel, so that I can allocate budget effectively.
- As a Marketing Analyst, I want to export report data for further analysis, so that I can share insights with stakeholders.

**Acceptance Criteria:**
- Given a report is selected, when filters are applied, then the data refreshes within 3 seconds.
- Given a report view, when the user clicks Export, then the data is downloaded in the selected format.

**Edge Cases:**
- Reports with more than 1 million rows are paginated and export is processed asynchronously
- Scheduled report emails are queued and retry up to 3 times on delivery failure

### 3.6 Feature: Multi-Channel Integration
**Description:** Integrate with CRM, SMS gateways, Facebook, Zalo, and other platforms for unified marketing orchestration.

**User Stories:**
- As a Campaign Manager, I want to send SMS campaigns alongside email campaigns, so that I can reach contacts through their preferred channel.
- As a Marketing Director, I want Facebook ad performance data to flow into marketing reports, so that I can see full-funnel attribution.

**Acceptance Criteria:**
- Given an SMS integration is configured, when an SMS campaign is sent, then delivery and response events are tracked.
- Given a Facebook integration, when ad spend data is synced, then it appears in the cost breakdown report.

**Edge Cases:**
- Expired API tokens trigger automatic notification and pause affected campaigns
- Rate-limited APIs are retried with exponential backoff
- Data schema changes on external platforms are detected and flagged for mapping review

### 3.7 Feature: A/B Testing
**Description:** Run A/B tests on email subject lines, content variations, and send times with statistical significance tracking.

**User Stories:**
- As a Campaign Manager, I want to test two email subject lines, so that I can determine which drives higher open rates.
- As a Marketing Analyst, I want to see statistical significance scores, so that I can trust test results.

**Acceptance Criteria:**
- Given an A/B test is configured with two variants, when the test reaches 95% confidence, then the winner is declared and the remaining contacts receive the winning variant.
- Given an active A/B test, when the user views results, then open rates, click rates, and confidence scores are displayed for each variant.

**Edge Cases:**
- Tests ending with no significant difference after 14 days are auto-closed with a tie notification
- If one variant has a dramatically higher bounce rate, the test is paused for review

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|    Campaign    |       |  EmailTemplate |       |  LandingPage   |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |       | id (PK)        |
| name           |       | campaign_id(FK)|       | campaign_id(FK)|
| type           |       | name           |       | name           |
| status         |       | html_content   |       | url_slug       |
| start_date     |       | plain_text     |       | html_content   |
| end_date       |       | thumbnail      |       | status         |
| budget         |       | created_at     |       | seo_meta       |
| created_at     |       | updated_at     |       | created_at     |
+----------------+       +----------------+       +----------------+
        |                          |                      |
        v                          v                      v
+----------------+       +----------------+       +----------------+
|      Lead      |       |    Contact     |       |   LeadForm     |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |       | id (PK)        |
| contact_id(FK) |       | email          |       | name           |
| campaign_id(FK)|       | phone          |       | fields_json    |
| stage          |       | company        |       | embed_code     |
| score          |       | score          |       | status         |
| source         |       | consent        |       | created_at     |
| created_at     |       | created_at     |       +----------------+
+----------------+       +----------------+              |
        |                          |                     v
        v                          v              +----------------+
+----------------+       +----------------+       |  FormSubmission|
|   Conversion   |       | ContactGroup   |       +----------------+
|    Funnel      |       +----------------+       | id (PK)        |
+----------------+       | id (PK)        |       | form_id(FK)    |
| id (PK)        |       | name           |       | contact_id(FK) |
| stage_name     |       | rules_json     |       | submitted_at   |
| count          |       | contact_count  |       | source_url     |
| conversion_rate|       | created_at     |       +----------------+
| created_at     |       +----------------+
+----------------+
        |
        v
+----------------+       +----------------+       +----------------+
| MarketingRpt   |       |  AdChannel     |       |  EmailCampaign |
+----------------+       +----------------+       +----------------+
| id (PK)        |       | id (PK)        |       | id (PK)        |
| report_type    |       | name           |       | campaign_id(FK)|
| filters_json   |       | platform       |       | template_id(FK)|
| schedule       |       | api_key        |       | segment_id     |
| created_at     |       | status         |       | send_date      |
+----------------+       +----------------+       | status         |
                                                 +----------------+
        |                                              |
        v                                              v
+----------------+       +----------------+       +----------------+
|  SMSCampaign   |       | FacebookCamp   |       |   ABTest       |
+----------------+       +----------------+       +----------------+
| id (PK)        |       | id (PK)        |       | id (PK)        |
| campaign_id(FK)|       | campaign_id(FK)|       | campaign_id(FK)|
| message        |       | page_id        |       | variant_a      |
| recipient_list |       | ad_spend       |       | variant_b      |
| status         |       | status         |       | confidence     |
| sent_at        |       | sync_date      |       | winner         |
+----------------+       +----------------+       +----------------+
```

### 4.2 Table Schemas

#### Table: campaign
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Campaign name |
| type | VARCHAR(50) | NOT NULL | Type: email, sms, facebook, landing_page, multi_channel |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status: draft, active, paused, completed, archived |
| start_date | DATE | NULL | Campaign start date |
| end_date | DATE | NULL | Campaign end date |
| budget | DECIMAL(18,2) | NULL | Allocated budget |
| actual_cost | DECIMAL(18,2) | NULL, DEFAULT 0 | Actual spend |
| revenue_attributed | DECIMAL(18,2) | NULL, DEFAULT 0 | Revenue attributed to campaign |
| description | TEXT | NULL | Campaign description |
| owner_id | BIGINT | FK -> users.id | Campaign owner |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_campaign_status ON campaign(status);
CREATE INDEX idx_campaign_type ON campaign(type);
CREATE INDEX idx_campaign_owner ON campaign(owner_id);
CREATE INDEX idx_campaign_dates ON campaign(start_date, end_date);
```

#### Table: email_template
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Template name |
| html_content | LONGTEXT | NOT NULL | Email HTML body |
| plain_text | TEXT | NULL | Plain text fallback |
| thumbnail_url | VARCHAR(500) | NULL | Preview image URL |
| category | VARCHAR(50) | NULL | Template category |
| is_system | TINYINT(1) | DEFAULT 0 | System template flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_email_template_category ON email_template(category);
CREATE INDEX idx_email_template_system ON email_template(is_system);
```

#### Table: landing_page
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Page name |
| url_slug | VARCHAR(200) | NOT NULL, UNIQUE | URL path segment |
| html_content | LONGTEXT | NOT NULL | Page HTML content |
| seo_meta_title | VARCHAR(100) | NULL | SEO meta title |
| seo_meta_desc | VARCHAR(200) | NULL | SEO meta description |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status: draft, published, unpublished |
| view_count | BIGINT | DEFAULT 0 | Total page views |
| conversion_count | BIGINT | DEFAULT 0 | Total form conversions |
| custom_domain | VARCHAR(255) | NULL | Custom domain mapping |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_landing_page_slug ON landing_page(url_slug);
CREATE INDEX idx_landing_page_status ON landing_page(status);
```

#### Table: lead
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| contact_id | BIGINT | FK -> contact.id, NOT NULL | Associated contact |
| campaign_id | BIGINT | FK -> campaign.id | Source campaign |
| stage | VARCHAR(30) | NOT NULL, DEFAULT 'lead' | Stage: lead, mql, sal, sql, customer, disqualified |
| score | INT | DEFAULT 0 | Lead score value |
| source | VARCHAR(50) | NULL | Lead source channel |
| source_url | VARCHAR(500) | NULL | URL where lead was captured |
| assigned_to | BIGINT | FK -> users.id | Assigned sales rep |
| converted_at | TIMESTAMP | NULL | Stage conversion timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_lead_stage ON lead(stage);
CREATE INDEX idx_lead_campaign ON lead(campaign_id);
CREATE INDEX idx_lead_score ON lead(score);
CREATE INDEX idx_lead_assigned ON lead(assigned_to);
CREATE INDEX idx_lead_contact ON lead(contact_id);
```

#### Table: contact
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| first_name | VARCHAR(100) | NULL | First name |
| last_name | VARCHAR(100) | NULL | Last name |
| email | VARCHAR(200) | NOT NULL, UNIQUE | Email address |
| phone | VARCHAR(30) | NULL | Phone number |
| company | VARCHAR(200) | NULL | Company name |
| job_title | VARCHAR(100) | NULL | Job title |
| score | INT | DEFAULT 0 | Contact score |
| consent_given | TINYINT(1) | DEFAULT 0 | Marketing consent flag |
| consent_date | TIMESTAMP | NULL | Consent timestamp |
| last_activity_at | TIMESTAMP | NULL | Last interaction timestamp |
| source | VARCHAR(50) | NULL | Original source |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_contact_email ON contact(email);
CREATE INDEX idx_contact_score ON contact(score);
CREATE INDEX idx_contact_source ON contact(source);
CREATE INDEX idx_contact_company ON contact(company);
```

#### Table: contact_group
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Group name |
| description | TEXT | NULL | Group description |
| rules_json | JSON | NULL | Dynamic segmentation rules |
| contact_count | INT | DEFAULT 0 | Cached member count |
| is_dynamic | TINYINT(1) | DEFAULT 0 | Dynamic vs static group |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_contact_group_dynamic ON contact_group(is_dynamic);
```

#### Table: lead_form
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Form name |
| fields_json | JSON | NOT NULL | Form field definitions |
| embed_code | TEXT | NULL | Generated embed JavaScript |
| redirect_url | VARCHAR(500) | NULL | Thank you page URL |
| auto_responder_template_id | BIGINT | FK -> email_template.id | Auto-responder email |
| spam_protection | VARCHAR(30) | DEFAULT 'recaptcha' | Spam protection method |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, inactive |
| submission_count | INT | DEFAULT 0 | Total submissions |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_lead_form_status ON lead_form(status);
```

#### Table: form_submission
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| form_id | BIGINT | FK -> lead_form.id, NOT NULL | Source form |
| contact_id | BIGINT | FK -> contact.id | Associated contact |
| data_json | JSON | NOT NULL | Submitted form data |
| source_url | VARCHAR(500) | NULL | Page URL where form was embedded |
| ip_address | VARCHAR(45) | NULL | Submitter IP address |
| submitted_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Submission timestamp |

**Indexes:**
```sql
CREATE INDEX idx_form_submission_form ON form_submission(form_id);
CREATE INDEX idx_form_submission_contact ON form_submission(contact_id);
CREATE INDEX idx_form_submission_date ON form_submission(submitted_at);
```

#### Table: marketing_report
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Report name |
| report_type | VARCHAR(50) | NOT NULL | Report category |
| filters_json | JSON | NULL | Saved filter configuration |
| chart_type | VARCHAR(30) | DEFAULT 'bar' | Default chart type |
| schedule | VARCHAR(30) | NULL | Delivery schedule: daily, weekly, monthly |
| last_generated_at | TIMESTAMP | NULL | Last generation timestamp |
| is_public | TINYINT(1) | DEFAULT 0 | Shared with all users |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_marketing_report_type ON marketing_report(report_type);
CREATE INDEX idx_marketing_report_public ON marketing_report(is_public);
```

#### Table: ad_channel
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Channel display name |
| platform | VARCHAR(50) | NOT NULL | Platform: facebook, google, zalo, sms |
| api_key | VARCHAR(500) | NULL | Encrypted API key |
| api_secret | VARCHAR(500) | NULL | Encrypted API secret |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'inactive' | Status: active, inactive, error |
| last_sync_at | TIMESTAMP | NULL | Last data sync timestamp |
| error_message | TEXT | NULL | Last error message |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_ad_channel_platform ON ad_channel(platform);
CREATE INDEX idx_ad_channel_status ON ad_channel(status);
```

#### Table: conversion_funnel
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| campaign_id | BIGINT | FK -> campaign.id, NULL | Associated campaign (NULL for global) |
| stage_name | VARCHAR(50) | NOT NULL | Stage identifier |
| stage_order | INT | NOT NULL | Stage sequence number |
| count | BIGINT | NOT NULL | Contact count at this stage |
| conversion_rate | DECIMAL(5,2) | NULL | Conversion rate from previous stage |
| avg_time_in_stage_hours | DECIMAL(10,2) | NULL | Average hours in stage |
| snapshot_date | DATE | NOT NULL | Snapshot date |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_conversion_funnel_campaign ON conversion_funnel(campaign_id);
CREATE INDEX idx_conversion_funnel_date ON conversion_funnel(snapshot_date);
CREATE INDEX idx_conversion_funnel_stage ON conversion_funnel(stage_name);
```

#### Table: email_campaign
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| campaign_id | BIGINT | FK -> campaign.id, NOT NULL | Parent campaign |
| template_id | BIGINT | FK -> email_template.id | Email template used |
| segment_id | BIGINT | FK -> contact_group.id | Target contact segment |
| subject_line | VARCHAR(200) | NOT NULL | Email subject |
| from_name | VARCHAR(100) | NOT NULL | Sender display name |
| from_email | VARCHAR(200) | NOT NULL | Sender email |
| send_date | TIMESTAMP | NOT NULL | Scheduled send time |
| sent_count | INT | DEFAULT 0 | Total sent |
| opened_count | INT | DEFAULT 0 | Total opened |
| clicked_count | INT | DEFAULT 0 | Total clicked |
| bounced_count | INT | DEFAULT 0 | Total bounced |
| unsubscribed_count | INT | DEFAULT 0 | Total unsubscribed |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status: draft, scheduled, sending, sent, paused, failed |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_email_campaign_status ON email_campaign(status);
CREATE INDEX idx_email_campaign_send_date ON email_campaign(send_date);
CREATE INDEX idx_email_campaign_campaign ON email_campaign(campaign_id);
```

#### Table: sms_campaign
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| campaign_id | BIGINT | FK -> campaign.id, NOT NULL | Parent campaign |
| message | VARCHAR(480) | NOT NULL | SMS message content (3 segments max) |
| segment_id | BIGINT | FK -> contact_group.id | Target segment |
| send_date | TIMESTAMP | NOT NULL | Scheduled send time |
| sent_count | INT | DEFAULT 0 | Total sent |
| delivered_count | INT | DEFAULT 0 | Total delivered |
| failed_count | INT | DEFAULT 0 | Total failed |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status: draft, scheduled, sending, sent, failed |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_sms_campaign_status ON sms_campaign(status);
CREATE INDEX idx_sms_campaign_date ON sms_campaign(send_date);
```

#### Table: facebook_campaign
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| campaign_id | BIGINT | FK -> campaign.id, NOT NULL | Parent campaign |
| page_id | VARCHAR(100) | NOT NULL | Facebook page ID |
| post_id | VARCHAR(100) | NULL | Facebook post ID |
| ad_spend | DECIMAL(18,2) | DEFAULT 0 | Ad spend amount |
| impressions | BIGINT | DEFAULT 0 | Total impressions |
| clicks | INT | DEFAULT 0 | Total clicks |
| conversions | INT | DEFAULT 0 | Total conversions |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status |
| last_sync_at | TIMESTAMP | NULL | Last metric sync |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_fb_campaign_page ON facebook_campaign(page_id);
CREATE INDEX idx_fb_campaign_status ON facebook_campaign(status);
```

#### Table: ab_test
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| campaign_id | BIGINT | FK -> campaign.id, NOT NULL | Parent campaign |
| test_type | VARCHAR(30) | NOT NULL | Type: subject_line, content, send_time |
| variant_a_json | JSON | NOT NULL | Variant A configuration |
| variant_b_json | JSON | NOT NULL | Variant B configuration |
| sample_size | INT | NOT NULL | Number of contacts in test |
| confidence_level | DECIMAL(5,2) | DEFAULT 0 | Current confidence percentage |
| winner | VARCHAR(1) | NULL | Winning variant: A or B |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'running' | Status: running, completed, cancelled |
| started_at | TIMESTAMP | NOT NULL | Test start time |
| completed_at | TIMESTAMP | NULL | Test completion time |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_ab_test_campaign ON ab_test(campaign_id);
CREATE INDEX idx_ab_test_status ON ab_test(status);
CREATE INDEX idx_ab_test_type ON ab_test(test_type);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| campaign | Master campaign records | 5,000 |
| email_template | Email template library | 500 |
| landing_page | Published landing pages | 1,000 |
| lead | Lead pipeline records | 2,000,000 |
| contact | Contact database | 3,000,000 |
| contact_group | Contact segments/groups | 500 |
| lead_form | Lead capture forms | 200 |
| form_submission | Form submission records | 500,000 |
| marketing_report | Saved report configs | 100 |
| ad_channel | External channel integrations | 50 |
| conversion_funnel | Funnel snapshot data | 50,000 |
| email_campaign | Email send records | 10,000 |
| sms_campaign | SMS send records | 2,000 |
| facebook_campaign | Facebook ad records | 1,000 |
| ab_test | A/B test configurations | 500 |

---

## 5. API Specifications

### 5.1 Campaign Endpoints

#### GET /api/v1/marketing/campaigns
**Description:** List all marketing campaigns with pagination and filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| limit | integer | No | Items per page (default: 20, max: 100) |
| type | string | No | Filter by campaign type |
| status | string | No | Filter by status |
| date_from | string | No | Filter by start date (ISO 8601) |
| date_to | string | No | Filter by end date (ISO 8601) |
| sort | string | No | Sort field (name, start_date, created_at) |
| order | string | No | Sort direction: asc/desc |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "name": "Q1 Product Launch",
      "type": "email",
      "status": "active",
      "start_date": "2026-01-15",
      "end_date": "2026-03-31",
      "budget": 500000000,
      "actual_cost": 325000000,
      "revenue_attributed": 2500000000,
      "created_at": "2026-01-10T08:00:00Z",
      "updated_at": "2026-04-14T10:30:00Z"
    }
  ],
  "meta": {
    "total": 245,
    "page": 1,
    "limit": 20,
    "last_page": 13
  }
}
```

**Permission:** `marketing:campaign:read`

#### POST /api/v1/marketing/campaigns
**Description:** Create a new marketing campaign

**Request Body:**
```json
{
  "name": "Summer Sale 2026",
  "type": "multi_channel",
  "start_date": "2026-06-01",
  "end_date": "2026-08-31",
  "budget": 1000000000,
  "description": "Summer promotional campaign across email, SMS, and Facebook"
}
```

**Response (201 Created):**
```json
{
  "id": 246,
  "name": "Summer Sale 2026",
  "type": "multi_channel",
  "status": "draft",
  "created_at": "2026-04-14T11:00:00Z"
}
```

**Permission:** `marketing:campaign:create`

#### GET /api/v1/marketing/campaigns/{id}
**Description:** Get a single campaign by ID with full details

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Q1 Product Launch",
  "type": "email",
  "status": "active",
  "start_date": "2026-01-15",
  "end_date": "2026-03-31",
  "budget": 500000000,
  "actual_cost": 325000000,
  "revenue_attributed": 2500000000,
  "email_campaigns": [...],
  "funnel_data": {...},
  "created_at": "2026-01-10T08:00:00Z"
}
```

#### PUT /api/v1/marketing/campaigns/{id}
**Description:** Update a campaign

**Permission:** `marketing:campaign:update`

#### DELETE /api/v1/marketing/campaigns/{id}
**Description:** Soft delete a campaign

**Permission:** `marketing:campaign:delete`

### 5.2 Email Campaign Endpoints

#### POST /api/v1/marketing/email-campaigns/{id}/send
**Description:** Send or schedule an email campaign

**Request Body:**
```json
{
  "send_date": "2026-04-20T09:00:00Z",
  "segment_id": 15,
  "subject_line": "Exclusive Summer Deals Inside"
}
```

**Response (202 Accepted):**
```json
{
  "status": "scheduled",
  "send_date": "2026-04-20T09:00:00Z",
  "estimated_recipients": 25000
}
```

**Permission:** `marketing:email:send`

#### GET /api/v1/marketing/email-campaigns/{id}/analytics
**Description:** Get real-time analytics for an email campaign

**Response (200 OK):**
```json
{
  "sent": 25000,
  "opened": 8750,
  "clicked": 2625,
  "bounced": 125,
  "unsubscribed": 50,
  "open_rate": 35.0,
  "click_rate": 10.5,
  "bounce_rate": 0.5,
  "unsubscribe_rate": 0.2
}
```

### 5.3 Contact Endpoints

#### GET /api/v1/marketing/contacts
**Description:** List contacts with search and filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page |
| search | string | No | Full-text search |
| segment_id | integer | No | Filter by segment |
| score_min | integer | No | Minimum score |
| score_max | integer | No | Maximum score |

**Permission:** `marketing:contact:read`

#### POST /api/v1/marketing/contacts/import
**Description:** Bulk import contacts from CSV/Excel

**Request:** Multipart file upload

**Response (202 Accepted):**
```json
{
  "import_id": "imp_abc123",
  "status": "processing",
  "estimated_completion": "2026-04-14T11:15:00Z"
}
```

**Permission:** `marketing:contact:import`

#### GET /api/v1/marketing/contacts/{id}/journey
**Description:** Get the complete journey timeline for a contact

**Response (200 OK):**
```json
{
  "contact_id": 12345,
  "journey": [
    {
      "type": "form_submission",
      "timestamp": "2026-03-01T10:00:00Z",
      "details": {"form": "Newsletter Signup", "url": "/landing/newsletter"}
    },
    {
      "type": "email_opened",
      "timestamp": "2026-03-02T09:30:00Z",
      "details": {"campaign": "Welcome Series", "subject": "Welcome to our community"}
    },
    {
      "type": "email_clicked",
      "timestamp": "2026-03-02T09:32:00Z",
      "details": {"campaign": "Welcome Series", "link": "/products"}
    }
  ]
}
```

### 5.4 Funnel Endpoints

#### GET /api/v1/marketing/funnel
**Description:** Get conversion funnel data

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date_from | string | Yes | Start date |
| date_to | string | Yes | End date |
| campaign_id | integer | No | Filter by campaign |

**Response (200 OK):**
```json
{
  "stages": [
    {"name": "Lead", "count": 1852371, "conversion_rate": null},
    {"name": "MQL", "count": 1239091, "conversion_rate": 66.9},
    {"name": "SAL", "count": 1055108, "conversion_rate": 85.1},
    {"name": "SQL", "count": 1012242, "conversion_rate": 96.0}
  ],
  "total_conversion": 54.6,
  "period": {"from": "2026-01-01", "to": "2026-04-14"}
}
```

### 5.5 Landing Page Endpoints

#### POST /api/v1/marketing/landing-pages/{id}/publish
**Description:** Publish a landing page

**Response (200 OK):**
```json
{
  "status": "published",
  "url": "https://marketing.example.com/summer-sale",
  "published_at": "2026-04-14T11:00:00Z"
}
```

**Permission:** `marketing:landing-page:publish`

### 5.6 A/B Test Endpoints

#### POST /api/v1/marketing/ab-tests
**Description:** Create a new A/B test

**Request Body:**
```json
{
  "campaign_id": 1,
  "test_type": "subject_line",
  "variant_a": {"subject_line": "Deal of the Year"},
  "variant_b": {"subject_line": "You Won't Believe These Prices"},
  "sample_size": 5000
}
```

**Response (201 Created):**
```json
{
  "id": 55,
  "status": "running",
  "started_at": "2026-04-14T11:00:00Z"
}
```

### 5.7 Report Endpoints

#### GET /api/v1/marketing/reports
**Description:** List available marketing reports

**Permission:** `marketing:report:read`

#### GET /api/v1/marketing/reports/{type}/data
**Description:** Get data for a specific report type

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date_from | string | Yes | Start date |
| date_to | string | Yes | End date |
| filters | string | No | JSON-encoded filters |

**Permission:** `marketing:report:read`

### 5.8 Integration Endpoints

#### GET /api/v1/marketing/integrations
**Description:** List all configured integrations

**Permission:** `marketing:integration:read`

#### POST /api/v1/marketing/integrations/{id}/test
**Description:** Test integration connectivity

**Response (200 OK):**
```json
{
  "status": "connected",
  "message": "Successfully connected to Facebook API",
  "last_sync_at": "2026-04-14T10:00:00Z"
}
```

### 5.9 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/marketing/campaigns | List campaigns | marketing:campaign:read |
| POST | /api/v1/marketing/campaigns | Create campaign | marketing:campaign:create |
| GET | /api/v1/marketing/campaigns/{id} | Get campaign | marketing:campaign:read |
| PUT | /api/v1/marketing/campaigns/{id} | Update campaign | marketing:campaign:update |
| DELETE | /api/v1/marketing/campaigns/{id} | Delete campaign | marketing:campaign:delete |
| POST | /api/v1/marketing/email-campaigns | Create email campaign | marketing:email:create |
| POST | /api/v1/marketing/email-campaigns/{id}/send | Send email campaign | marketing:email:send |
| GET | /api/v1/marketing/email-campaigns/{id}/analytics | Email analytics | marketing:email:read |
| GET | /api/v1/marketing/contacts | List contacts | marketing:contact:read |
| POST | /api/v1/marketing/contacts/import | Import contacts | marketing:contact:import |
| GET | /api/v1/marketing/contacts/{id}/journey | Contact journey | marketing:contact:read |
| GET | /api/v1/marketing/funnel | Funnel data | marketing:funnel:read |
| GET | /api/v1/marketing/landing-pages | List landing pages | marketing:landing-page:read |
| POST | /api/v1/marketing/landing-pages | Create landing page | marketing:landing-page:create |
| POST | /api/v1/marketing/landing-pages/{id}/publish | Publish page | marketing:landing-page:publish |
| GET | /api/v1/marketing/lead-forms | List lead forms | marketing:lead-form:read |
| POST | /api/v1/marketing/lead-forms | Create lead form | marketing:lead-form:create |
| POST | /api/v1/marketing/ab-tests | Create A/B test | marketing:ab-test:create |
| GET | /api/v1/marketing/reports | List reports | marketing:report:read |
| GET | /api/v1/marketing/reports/{type}/data | Report data | marketing:report:read |
| GET | /api/v1/marketing/integrations | List integrations | marketing:integration:read |
| POST | /api/v1/marketing/integrations/{id}/test | Test integration | marketing:integration:configure |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Marketing Director | marketing_director | Full access to all marketing features and reports |
| Campaign Manager | campaign_manager | Manage campaigns, emails, landing pages, forms |
| Content Creator | content_creator | Create and edit templates, content, forms |
| Sales Representative | sales_rep | View leads, update lead status, read campaign data |
| Marketing Analyst | marketing_analyst | Read access to all data, create reports and segments |
| System Administrator | system_admin | Manage integrations, system configuration |

### Permission Matrix
| Role | Create Campaign | Read Campaign | Update Campaign | Delete Campaign | Send Email | View Reports | Manage Integrations | Export Data |
|------|----------------|---------------|-----------------|----------------|-----------|-------------|--------------------|-------------|
| Marketing Director | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Campaign Manager | Yes | Yes | Yes | No | Yes | Yes | No | Yes |
| Content Creator | No | Yes | No | No | No | Yes | No | No |
| Sales Representative | No | Yes | No | No | No | No | No | No |
| Marketing Analyst | No | Yes | No | No | No | Yes | No | Yes |
| System Administrator | No | Yes | No | No | No | Yes | Yes | Yes |

### Row-Level Security
- Campaign Managers can only edit campaigns they own or are assigned to.
- Sales Representatives can only view leads assigned to them.
- Marketing Analysts have read access to all campaigns and contacts across all companies.
- Content Creators can only edit templates they created or that are shared with them.
- Marketing Directors have unrestricted read access to all marketing data within their company.

---

## 7. State Machine / Workflows

### 7.1 Campaign Status Transitions

```
[DRAFT] ──schedule──→ [SCHEDULED] ──auto-send──→ [ACTIVE] ──complete──→ [COMPLETED]
   │                       │                       │
   └──send_now─────────────┘                       └──pause──→ [PAUSED]
   │                                               │
   └──delete────────────────────────────────────────┘
                                                     │
                                                     └──resume──→ [ACTIVE]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | SCHEDULED | schedule | Campaign Manager | Validates template, checks segment size, queues for sending |
| DRAFT | ACTIVE | send_now | Campaign Manager | Immediately starts sending to segment |
| SCHEDULED | ACTIVE | auto-send | System | Triggered at scheduled time |
| ACTIVE | COMPLETED | complete | System | All emails sent, finalizes metrics |
| ACTIVE | PAUSED | pause | Campaign Manager | Stops sending, preserves progress |
| PAUSED | ACTIVE | resume | Campaign Manager | Continues sending from paused point |
| DRAFT | DELETED | delete | Campaign Manager | Soft deletes campaign and all associated data |

### 7.2 Lead Stage Transitions

```
[NEW] ──score──→ [LEAD] ──qualify──→ [MQL] ──accept──→ [SAL] ──qualify──→ [SQL] ──convert──→ [CUSTOMER]
  │                │                  │                 │                  │
  └──disqualify───→[DISQUALIFIED]←───┘←──reject────────┘←──reject────────┘
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| NEW | LEAD | auto-score | System | Lead scoring rules applied |
| LEAD | MQL | qualify | System/Manager | Score threshold met, notification sent to sales |
| MQL | SAL | accept | Sales Rep | Sales accepts lead for follow-up |
| MQL | DISQUALIFIED | reject | Sales Rep | Lead marked as not fitting criteria |
| SAL | SQL | qualify | Sales Rep | Lead verified as qualified opportunity |
| SAL | DISQUALIFIED | reject | Sales Rep | Lead rejected after initial review |
| SQL | CUSTOMER | convert | System | Opportunity won, deal closed |
| SQL | DISQUALIFIED | reject | Sales Rep | Lead rejected after qualification |

### 7.3 Email Campaign Send Workflow

```
[DRAFT] ──validate──→ [VALIDATION_CHECK] ──pass──→ [SCHEDULED] ──send──→ [SENDING] ──done──→ [SENT]
                            │                         │                    │
                            └──fail──→ [FAILED]←───────┘←──error───────────┘
```

---

## 8. Reports & Analytics

### 8.1 Report: Campaign Performance Overview
**Type:** Real-time
**Purpose:** Display performance metrics for all active and completed campaigns

**Dimensions:**
- Campaign name, type, status
- Date range
- Channel (email, SMS, Facebook, landing page)

**Metrics:**
- Total spend, attributed revenue, ROI
- Sends, opens, clicks, conversions
- Cost per acquisition (CPA)

**Filters:**
- Date range, campaign type, status, owner

**Chart Type:** Bar chart (revenue by campaign), Table (detailed metrics)

### 8.2 Report: Funnel Analysis
**Type:** Real-time
**Purpose:** Track lead-to-customer conversion pipeline

**Dimensions:**
- Funnel stage
- Source channel
- Campaign

**Metrics:**
- Count at each stage
- Conversion rates between stages
- Average time in stage

**Chart Type:** Funnel chart, Line chart (trend over time)

### 8.3 Report: Email Analytics
**Type:** Real-time
**Purpose:** Monitor email campaign deliverability and engagement

**Metrics:**
- Sent, delivered, opened, clicked, bounced, unsubscribed
- Open rate, click rate, bounce rate, unsubscribe rate
- Top performing subject lines, best send times

**Chart Type:** Line chart (rates over time), Table (campaign comparison)

### 8.4 Report: ROI by Channel
**Type:** Real-time
**Purpose:** Compare return on investment across marketing channels

**Metrics:**
- Total cost, attributed revenue, ROI percentage
- Revenue per channel
- Cost per lead by channel

**Chart Type:** Stacked bar chart, Pie chart (cost allocation)

### 8.5 Report: Lead Source Attribution
**Type:** Real-time
**Purpose:** Identify which channels and campaigns generate the most valuable leads

**Metrics:**
- Leads by source
- MQL conversion rate by source
- Revenue attributed to each source

**Chart Type:** Pie chart, Table

### 8.6 Report: Contact Growth Trends
**Type:** Real-time
**Purpose:** Track contact database growth over time

**Metrics:**
- New contacts per period
- Active vs. inactive contacts
- Consent rate

**Chart Type:** Line chart, Area chart

### 8.7 Report: A/B Test Results
**Type:** Real-time
**Purpose:** Analyze A/B test outcomes with statistical significance

**Metrics:**
- Variant performance (open rate, click rate, conversion rate)
- Confidence level
- Winner declaration

**Chart Type:** Bar chart (variant comparison)

### 8.8 Report: Cost per Acquisition
**Type:** Real-time
**Purpose:** Track efficiency of marketing spend

**Metrics:**
- Total cost, total conversions, CPA
- CPA by channel, by campaign

**Chart Type:** Line chart (CPA trend), Table

### 8.9 Report: Revenue Attribution
**Type:** Real-time
**Purpose:** Map closed deals back to marketing touchpoints

**Metrics:**
- Total marketing-attributed revenue
- Revenue by campaign, by channel
- Multi-touch attribution model results

**Chart Type:** Waterfall chart, Table

### 8.10 Report: Engagement Trends
**Type:** Real-time
**Purpose:** Monitor overall audience engagement patterns

**Metrics:**
- Email open/click rates over time
- Landing page views and conversions
- Form submission rates

**Chart Type:** Multi-line chart

### 8.11 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Campaign Performance | Real-time | On-demand | Chart, Table, PDF, Excel |
| Funnel Analysis | Real-time | On-demand | Funnel Chart, CSV |
| Email Analytics | Real-time | On-demand | Line Chart, CSV |
| ROI by Channel | Real-time | On-demand | Bar Chart, PDF |
| Lead Source Attribution | Real-time | On-demand | Pie Chart, Table |
| Contact Growth | Real-time | On-demand | Line Chart |
| A/B Test Results | Real-time | On-demand | Bar Chart, Table |
| Cost per Acquisition | Real-time | On-demand | Line Chart, CSV |
| Revenue Attribution | Real-time | On-demand | Waterfall, Table |
| Engagement Trends | Real-time | On-demand | Multi-line Chart |
| Customer Care Overview | Real-time | Daily | Dashboard, PDF |
| Budget vs. Actual | Real-time | Weekly | Bar Chart, Excel |
| Template Performance | Real-time | Monthly | Table |
| Landing Page Conversion | Real-time | On-demand | Table, Chart |
| Form Analytics | Real-time | On-demand | Table, Chart |
| Segment Analysis | Real-time | On-demand | Table, Chart |
| SMS Campaign Report | Real-time | On-demand | Table |
| Facebook Ad Report | Real-time | On-demand | Table, Chart |
| Contact Activity | Real-time | On-demand | Table, CSV |
| Campaign Comparison | Real-time | On-demand | Table, Chart |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| SendGrid / Mailgun API | Email delivery and tracking | REST | API Key |
| Facebook Marketing API | Ad campaign management and metrics | REST | OAuth 2.0 |
| Twilio / Vietnam SMS Gateway | SMS campaign delivery | REST | API Key |
| Zalo API | Vietnamese messaging platform integration | REST | OAuth 2.0 |
| Google Analytics API | Landing page traffic data | REST | Service Account |
| reCAPTCHA API | Form spam protection | REST | Site/Secret Key |
| MISA CRM API | Lead and opportunity sync | REST | OAuth 2.0 |
| OneAI API (M38) | AI content generation and lead scoring | REST | API Key |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| lead.created | {lead_id, contact_id, source, campaign_id} | CRM (M32), Workflow (M19) |
| lead.stage_changed | {lead_id, old_stage, new_stage, timestamp} | CRM (M32), Sales (M08) |
| email.sent | {email_campaign_id, contact_id, timestamp} | Contact timeline |
| email.opened | {email_campaign_id, contact_id, timestamp, device} | Contact timeline, analytics |
| email.clicked | {email_campaign_id, contact_id, link, timestamp} | Contact timeline, analytics |
| email.bounced | {email_campaign_id, contact_id, bounce_type} | Contact data quality |
| form.submitted | {form_id, contact_id, data, source_url} | Lead pipeline, CRM |
| landing_page.published | {page_id, url, published_at} | CDN cache invalidation |
| campaign.completed | {campaign_id, final_metrics} | Workflow, reporting |
| ab_test.completed | {test_id, winner, confidence} | Campaign manager |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| deal.won | CRM (M32) | Attribute revenue to marketing campaign |
| order.created | Sales (M08) | Update campaign revenue attribution |
| contact.updated | CRM (M32) | Sync contact data changes |
| user.created | Auth (M01) | Initialize marketing user preferences |

---

## 10. Technical Notes

### Performance
- Expected data volume: 3,000,000+ contacts, 2,000,000+ leads, 50,000+ funnel snapshots per day
- Query complexity: High (multi-table joins for funnel analysis, real-time aggregation for dashboards)
- Expected response time: Dashboard < 2s, Report generation < 3s, List views < 1s, Bulk email send < 50ms per recipient
- Email sending throughput: 10,000 emails per minute via SendGrid batch API
- Landing page load time: < 500ms with CDN caching (TTL: 5 minutes)

### Caching Strategy
- Redis cache layer for funnel snapshots, campaign metrics, and contact segment counts
- TTL: 5 minutes for real-time dashboards, 15 minutes for historical reports
- Landing page HTML cached at CDN edge with 5-minute TTL
- Cache invalidation: Triggered on campaign status change, form submission, or manual refresh
- Contact segment membership cached and recalculated on 15-minute schedule or on-demand

### File Attachments
- Supported types: PNG, JPG, GIF, SVG, PDF, HTML, CSV, XLSX
- Max size: 10 MB per file (email templates, landing page assets)
- Storage: S3-compatible object storage with CDN distribution
- Email template images optimized and resized automatically

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY (Vietnamese), YYYY-MM-DD (ISO)
- Currency: VND (Vietnamese Dong) with support for USD
- Number formatting: Vietnamese locale (period as thousands separator, comma as decimal)
- Email templates support RTL and multi-language content blocks

### Mobile Considerations
- Landing pages are responsive and tested on mobile viewports (320px+)
- Email templates include mobile-optimized variants
- Marketing dashboard has a responsive layout for tablet access
- Mobile app provides read-only access to campaign metrics and funnel data
- Lead form embeds work on mobile websites with touch-friendly field inputs

### Security
- API keys for external integrations are encrypted at rest using AES-256
- Email sending respects unsubscribe preferences and consent flags
- Contact data is subject to data protection compliance (opt-in tracking, right to delete)
- A/B test data is isolated and not accessible outside the test scope
- Rate limiting: 100 API requests per minute per user for read endpoints, 20 for write

### Scalability
- Email sending is handled by async job queue (RabbitMQ/Redis) with worker pool
- Bulk contact import (100,000+) is processed in background with chunked batches
- Landing page serving is offloaded to CDN for high-traffic campaigns
- Funnel data is pre-aggregated hourly and stored in materialized views for fast dashboard loading
