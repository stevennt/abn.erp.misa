# M30 - URL Shortener (MISA ShortLink)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_65.png
**Dependencies:** M01 (User & Authentication)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The URL Shortener module (MISA ShortLink) provides a service for creating shortened URLs with QR code generation and click analytics. It enables users to convert long, complex URLs into short, manageable links suitable for sharing in emails, documents, chat messages, social posts, and printed materials. Each shortened URL is accompanied by a QR code for easy mobile scanning and comprehensive analytics tracking click counts, referrer sources, geographic locations, device types, and time-based trends. The module serves marketing, HR, and communication teams who need trackable links for campaigns, announcements, and internal resource sharing. The mobile preview (img_65) shows the shortened URL interface with QR code and analytics visualization, demonstrating the module's focus on mobile-friendly link sharing and performance tracking.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Link Creator | Any employee who creates shortened URLs | Create + manage (own) |
| Marketing User | Creates tracked links for campaigns | Create + analytics |
| Analyst | Views click analytics and reports | Read-only analytics |
| System Administrator | Manages global settings and domain configuration | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication)
- **Used by:** M20 (Internal Chat), M23 (Social Network), M31 (aiMarketing), M36 (Promotion Management)

### Key Business Rules
1. Shortened URLs follow the format: `https://short.misa.vn/{code}` where code is a 6-8 character alphanumeric string.
2. Each shortened URL can have a custom alias (e.g., `https://short.misa.vn/q3-report`) instead of auto-generated code.
3. Custom aliases must be unique, alphanumeric with hyphens, and 3-30 characters long.
4. QR codes are auto-generated for every shortened URL in PNG and SVG formats.
5. Click analytics track: total clicks, unique clicks, referrer source, device type, browser, operating system, geographic location, and timestamp.
6. URLs can be assigned to campaigns for grouped analytics (e.g., "Q3 Marketing Campaign").
7. Tags can be applied to URLs for organization and filtering.
8. Shortened URLs can have an expiration date after which they redirect to a configurable expired page.
9. Click fraud detection prevents bot/crawler clicks from inflating analytics.
10. URL validation ensures the destination URL is accessible and not malicious before shortening.
11. Users can view analytics for their own URLs; marketing users and admins can view all.
12. The mobile preview shows the shortened URL with QR code and basic analytics (img_65).

---

## 2. Screen Inventory

### 2.1 Screen: URL Shortener Dashboard
**Route:** `/shortlink`
**Purpose:** Main dashboard for creating and managing shortened URLs (img_65).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| URL Input | Text field | Enter long URL to shorten |
| Custom Alias Input | Text field | Optional custom short code |
| Campaign Selector | Dropdown | Assign to campaign (optional) |
| Tags Input | Multi-tag | Add tags |
| Expiration Date | Date picker | Optional expiration |
| Shorten Button | CTA | Generate shortened URL |
| Result Card | Card | Shortened URL, QR code, copy button |
| My URLs List | Table | User's shortened URLs with click counts |

**User Interactions:**
- Enter a long URL and click Shorten.
- Optionally set a custom alias, campaign, and tags.
- Copy the shortened URL to clipboard.
- Download the QR code.
- View click analytics for each URL.

### 2.2 Screen: URL Analytics
**Route:** `/shortlink/urls/{id}/analytics`
**Purpose:** Detailed click analytics for a shortened URL (img_65).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Summary Cards | Metrics | Total clicks, unique clicks, today's clicks, conversion rate |
| Click Trend | Line chart | Clicks over time (daily/weekly/monthly) |
| Referrer Breakdown | Pie chart | Traffic sources (direct, social, email, search, other) |
| Device Breakdown | Bar chart | Desktop, mobile, tablet distribution |
| Geographic Map | Map | Click locations by country/city |
| Browser/OS Table | Table | Top browsers and operating systems |
| Export Button | CTA | Export analytics to CSV/PDF |

**User Interactions:**
- View click trends over selectable time periods.
- Analyze traffic sources and device distribution.
- View geographic click distribution.
- Export analytics data.

### 2.3 Screen: QR Code Viewer
**Route:** Modal or `/shortlink/urls/{id}/qr`
**Purpose:** View and download QR code for a shortened URL (img_65).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| QR Code Display | Large image | QR code at various sizes |
| Size Options | Radio buttons | Small (200x200), Medium (400x400), Large (800x800) |
| Format Options | Toggle | PNG, SVG |
| Download Button | CTA | Download QR code |
| Preview | Card | Mock phone showing QR scan result |
| Short URL Display | Text | Shortened URL below QR code |

**User Interactions:**
- View QR code at different sizes.
- Download in PNG or SVG format.
- Preview how QR code appears on mobile.
- Copy shortened URL.

### 2.4 Screen: Campaign Management
**Route:** `/shortlink/campaigns`
**Purpose:** Manage URL campaigns for grouped analytics.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Campaign List | Card list | Campaign name, URL count, total clicks, date range |
| Create Campaign | Button | Create new campaign |
| Campaign Detail | Card | Campaign info, URL list, aggregated analytics |
| Edit/Delete | Actions | Modify or remove campaigns |

**User Interactions:**
- Create campaigns with name and date range.
- View all URLs within a campaign.
- View aggregated campaign analytics.
- Edit or delete campaigns.

### 2.5 Screen: My URLs List
**Route:** `/shortlink/my-urls`
**Purpose:** List of all URLs created by the current user.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| URL Table | Data grid | Short URL, destination, clicks, campaign, tags, created date, actions |
| Search | Input | Search by short URL or destination |
| Filter | Multi-select | Filter by campaign, tag, status |
| Sort | Dropdown | Sort by clicks, created date |
| Bulk Actions | Button group | Delete selected, export selected |

**User Interactions:**
- View all created URLs.
- Search and filter the list.
- Click analytics icon for detailed view.
- Delete or edit URLs.

### 2.6 Screen: Mobile URL Preview
**Route:** Mobile app view (img_65)
**Purpose:** Mobile-optimized URL management and preview.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Short URL Display | Card | Shortened URL with copy button |
| QR Code | Image | Scannable QR code |
| Click Counter | Badge | Total click count |
| Share Button | FAB | Share URL via messaging apps |

---

## 3. Feature Specifications

### 3.1 Feature: URL Shortening
**Description:** Convert long URLs into short, shareable links with optional custom aliases.

**User Stories:**
- As a user, I want to shorten a long URL, so that it's easier to share in emails and documents.
- As a marketing user, I want to create a custom alias like "q3-report", so that the link is memorable.

**Acceptance Criteria:**
- Given a user enters `https://company.com/reports/quarterly/2024/q3/financial-summary`
- When they click Shorten
- Then a short URL like `https://short.misa.vn/a7x9k2` is generated

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Destination URL | Required, valid URL format | "Please enter a valid URL" |
| Custom Alias | 3-30 chars, alphanumeric + hyphens, unique | "Alias must be 3-30 characters, alphanumeric with hyphens, and unique" |
| Max URLs per User | 1,000 | "You have reached the maximum URL limit" |

**Edge Cases:**
- Duplicate destination URLs are allowed (each gets a unique short code).
- Custom aliases are case-insensitive (Q3-Report = q3-report).
- Reserved words (admin, login, api) cannot be used as aliases.

### 3.2 Feature: QR Code Generation
**Description:** Auto-generate QR codes for every shortened URL.

**User Stories:**
- As a user, I want a QR code for my shortened URL, so that I can include it in printed materials.
- As a user, I want to download the QR code in different sizes, so that it fits my design needs.

**Acceptance Criteria:**
- Given a shortened URL is created
- When the user views the QR code
- Then a scannable QR code is displayed that redirects to the destination URL

**Edge Cases:**
- QR codes encode the short URL, not the destination (allows analytics tracking).
- QR code error correction level set to "M" (medium) for reliable scanning.

### 3.3 Feature: Click Analytics
**Description:** Track and display comprehensive click analytics for shortened URLs.

**User Stories:**
- As a marketing user, I want to see how many times my link was clicked, so that I can measure engagement.
- As a user, I want to know where my clicks are coming from, so that I can optimize my sharing strategy.

**Acceptance Criteria:**
- Given a shortened URL is clicked
- When the redirect occurs
- Then the click is recorded with referrer, device, browser, location, and timestamp

**Metrics Tracked:**
- Total clicks
- Unique clicks (deduplicated by IP + user agent per 24 hours)
- Referrer source (direct, Google, Facebook, email, etc.)
- Device type (desktop, mobile, tablet)
- Browser (Chrome, Safari, Firefox, etc.)
- Operating system (Windows, macOS, iOS, Android, etc.)
- Geographic location (country, city)
- Timestamp (for trend analysis)

**Edge Cases:**
- Bot/crawler clicks detected via user agent and excluded from analytics.
- Same-user rapid clicks (>10 per minute) are throttled and flagged.

### 3.4 Feature: Campaign Management
**Description:** Group URLs into campaigns for aggregated analytics.

**User Stories:**
- As a marketing user, I want to group all campaign links together, so that I can see overall campaign performance.

**Acceptance Criteria:**
- Given a campaign "Q3 Product Launch" is created
- When URLs are assigned to it
- Then the campaign analytics show aggregated clicks across all URLs

**Edge Cases:**
- URLs can be reassigned between campaigns.
- Deleting a campaign doesn't delete its URLs.

### 3.5 Feature: URL Expiration
**Description:** Set expiration dates for shortened URLs.

**User Stories:**
- As a user, I want my shortened URL to expire after an event, so that old links don't remain active indefinitely.

**Acceptance Criteria:**
- Given a URL has an expiration date of 2024-12-31
- When someone clicks the short URL after that date
- Then they are redirected to the configured expired page

**Edge Cases:**
- Expired URLs can be reactivated by the owner.
- Expired page shows: "This link has expired" with optional custom message.

### 3.6 Feature: URL Tagging
**Description:** Apply tags to URLs for organization and filtering.

**User Stories:**
- As a user, I want to tag my URLs, so that I can organize and filter them later.

**Acceptance Criteria:**
- Given a user adds tags "marketing, email, q3" to a URL
- When they filter by "email" tag
- Then all URLs with the "email" tag appear

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|    ShortURL    |       |   URLClick     |       |  QRCode        |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |       | id (PK)        |
| original_url   |       | short_url(FK)  |       | short_url(FK)  |
| short_code     |       | ip_address     |       | format         |
| custom_alias   |       | user_agent     |       | file_url       |
| title          |       | referrer       |       | size           |
| campaign(FK)   |       | device_type    |       | created_at     |
| creator_id(FK) |       | browser        |       +----------------+
| click_count    |       | os             |
| expires_at     |       | country        |
| created_at     |       | city           |
+----------------+       | clicked_at     |
                         +----------------+
+----------------+
|  URLCampaign   |
+----------------+
| id (PK)        |
| name           |
| description    |
| start_date     |
| end_date       |
| created_by(FK) |
| created_at     |
+----------------+

+----------------+       +----------------+
|    URLTag      |       |ShortURLTag     |
+----------------+       +----------------+
| id (PK)        |       | short_url(FK)  |
| name           |       | tag_id (FK)    |
| created_at     |       +----------------+
+----------------+
```

### 4.2 Table Schemas

#### Table: short_urls
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| original_url | VARCHAR(2000) | NOT NULL | Destination URL |
| short_code | VARCHAR(10) | NOT NULL, UNIQUE | Auto-generated code |
| custom_alias | VARCHAR(30) | NULL, UNIQUE | Custom alias |
| title | VARCHAR(300) | NULL | URL title/description |
| campaign_id | BIGINT | NULL, FK -> url_campaigns.id | Campaign assignment |
| creator_id | BIGINT | NOT NULL, FK -> users.id | Creator |
| click_count | INT | NOT NULL, DEFAULT 0 | Total clicks |
| unique_click_count | INT | NOT NULL, DEFAULT 0 | Unique clicks |
| expires_at | TIMESTAMP | NULL, DEFAULT NULL | Expiration timestamp |
| expired_redirect_url | VARCHAR(2000) | NULL | Custom expired page URL |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_short_urls_code ON short_urls(short_code);
CREATE UNIQUE INDEX idx_short_urls_alias ON short_urls(custom_alias);
CREATE INDEX idx_short_urls_creator ON short_urls(creator_id);
CREATE INDEX idx_short_urls_campaign ON short_urls(campaign_id);
CREATE INDEX idx_short_urls_clicks ON short_urls(click_count DESC);
CREATE INDEX idx_short_urls_created ON short_urls(created_at DESC);
CREATE INDEX idx_short_urls_active ON short_urls(is_active, expires_at);
```

#### Table: url_clicks
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| short_url_id | BIGINT | NOT NULL, FK -> short_urls.id | Short URL |
| ip_address | VARCHAR(45) | NOT NULL | Clicker IP address |
| user_agent | VARCHAR(500) | NULL | Browser user agent |
| referrer | VARCHAR(500) | NULL | Referrer URL |
| referrer_domain | VARCHAR(200) | NULL | Referrer domain (parsed) |
| device_type | ENUM('desktop','mobile','tablet','bot') | NOT NULL, DEFAULT 'desktop' | Device type |
| browser | VARCHAR(100) | NULL | Browser name |
| os | VARCHAR(100) | NULL | Operating system |
| country | VARCHAR(100) | NULL | Country (from IP geolocation) |
| city | VARCHAR(100) | NULL | City (from IP geolocation) |
| is_bot | BOOLEAN | NOT NULL, DEFAULT FALSE | Bot detection flag |
| clicked_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Click timestamp |

**Indexes:**
```sql
CREATE INDEX idx_url_clicks_short_url ON url_clicks(short_url_id);
CREATE INDEX idx_url_clicks_clicked_at ON url_clicks(clicked_at DESC);
CREATE INDEX idx_url_clicks_device ON url_clicks(device_type);
CREATE INDEX idx_url_clicks_referrer ON url_clicks(referrer_domain);
CREATE INDEX idx_url_clicks_country ON url_clicks(country);
CREATE INDEX idx_url_clicks_bot ON url_clicks(is_bot);
```

#### Table: url_campaigns
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Campaign name |
| description | TEXT | NULL | Campaign description |
| start_date | DATE | NULL | Campaign start |
| end_date | DATE | NULL | Campaign end |
| created_by | BIGINT | NOT NULL, FK -> users.id | Creator |
| url_count | INT | NOT NULL, DEFAULT 0 | URL count |
| total_clicks | INT | NOT NULL, DEFAULT 0 | Aggregated clicks |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_url_campaigns_created_by ON url_campaigns(created_by);
CREATE INDEX idx_url_campaigns_dates ON url_campaigns(start_date, end_date);
```

#### Table: url_tags
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL | Tag name |
| creator_id | BIGINT | NOT NULL, FK -> users.id | Tag creator |
| usage_count | INT | NOT NULL, DEFAULT 0 | How many URLs use this tag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Tag creation time |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_url_tags_creator_name ON url_tags(creator_id, name);
```

#### Table: short_url_tags
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| short_url_id | BIGINT | NOT NULL, FK -> short_urls.id | Short URL |
| tag_id | BIGINT | NOT NULL, FK -> url_tags.id | Tag |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_sut_url_tag ON short_url_tags(short_url_id, tag_id);
CREATE INDEX idx_sut_tag_id ON short_url_tags(tag_id);
```

#### Table: qr_codes
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| short_url_id | BIGINT | NOT NULL, FK -> short_urls.id | Target short URL |
| format | ENUM('png','svg') | NOT NULL | QR code format |
| size | VARCHAR(20) | NOT NULL | Size label (small, medium, large) |
| file_url | VARCHAR(500) | NOT NULL | QR code file URL |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Generation timestamp |

**Indexes:**
```sql
CREATE INDEX idx_qr_codes_short_url ON qr_codes(short_url_id);
CREATE UNIQUE INDEX idx_qr_codes_url_format_size ON qr_codes(short_url_id, format, size);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| short_urls | Shortened URL records | 100,000 |
| url_clicks | Click analytics records | 5,000,000 |
| url_campaigns | Campaign definitions | 1,000 |
| url_tags | Tag definitions | 5,000 |
| short_url_tags | URL-tag mapping | 200,000 |
| qr_codes | Generated QR codes | 300,000 |

---

## 5. API Specifications

### 5.1 URL Shortening Endpoints

#### POST /api/v1/shortlink/shorten
**Description:** Create a shortened URL

**Request Body:**
```json
{
  "original_url": "https://company.com/reports/quarterly/2024/q3/financial-summary",
  "custom_alias": "q3-report",
  "campaign_id": 5,
  "tags": ["report", "q3", "finance"],
  "expires_at": "2024-12-31T23:59:59Z",
  "title": "Q3 2024 Financial Summary"
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "short_url": "https://short.misa.vn/q3-report",
  "short_code": "q3-report",
  "original_url": "https://company.com/reports/quarterly/2024/q3/financial-summary",
  "qr_code_url": "https://short.misa.vn/qr/q3-report.png",
  "created_at": "2024-04-14T09:00:00Z"
}
```

**Permission:** `shortlink:create`

#### GET /api/v1/shortlink/urls
**Description:** List user's shortened URLs

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page (default: 20) |
| campaign_id | bigint | No | Filter by campaign |
| tag | string | No | Filter by tag |
| q | string | No | Search by alias or destination |
| sort | string | No | created_at, click_count |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "short_url": "https://short.misa.vn/q3-report",
      "original_url": "https://company.com/reports/...",
      "title": "Q3 2024 Financial Summary",
      "click_count": 245,
      "unique_click_count": 180,
      "campaign": {"id": 5, "name": "Q3 Reports"},
      "tags": ["report", "q3", "finance"],
      "created_at": "2024-04-14T09:00:00Z"
    }
  ],
  "meta": {"total": 50, "page": 1, "limit": 20}
}
```

**Permission:** `shortlink:read`

#### GET /api/v1/shortlink/urls/{id}/analytics
**Description:** Get click analytics for a URL

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date_from | date | No | Start date |
| date_to | date | No | End date |
| group_by | string | No | day, week, month |

**Response (200 OK):**
```json
{
  "summary": {
    "total_clicks": 245,
    "unique_clicks": 180,
    "today_clicks": 12
  },
  "trend": [
    {"date": "2024-04-14", "clicks": 12, "unique": 10},
    {"date": "2024-04-13", "clicks": 18, "unique": 15}
  ],
  "referrers": [
    {"domain": "direct", "count": 100},
    {"domain": "google.com", "count": 80},
    {"domain": "facebook.com", "count": 40}
  ],
  "devices": [
    {"type": "desktop", "count": 150},
    {"type": "mobile", "count": 80},
    {"type": "tablet", "count": 15}
  ],
  "countries": [
    {"country": "Vietnam", "count": 200},
    {"country": "United States", "count": 30}
  ]
}
```

**Permission:** `shortlink:analytics:read`

### 5.2 QR Code Endpoint

#### GET /api/v1/shortlink/urls/{id}/qr
**Description:** Get QR code for a shortened URL

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| format | string | No | png (default), svg |
| size | string | No | small (200x200), medium (400x400), large (800x800) |

**Response:** Image file (PNG/SVG) or JSON with URL

**Permission:** `shortlink:qr:read`

### 5.3 Redirect Endpoint

#### GET /{short_code}
**Description:** Redirect to original URL (public endpoint)

**Response:** 302 Redirect to original URL

**Side Effects:** Click recorded in analytics

### 5.4 Campaign Endpoints

#### POST /api/v1/shortlink/campaigns
**Description:** Create a new campaign

**Permission:** `shortlink:campaign:create`

#### GET /api/v1/shortlink/campaigns/{id}/analytics
**Description:** Get aggregated analytics for a campaign

**Permission:** `shortlink:campaign:analytics`

### 5.5 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| POST | /api/v1/shortlink/shorten | Create short URL | shortlink:create |
| GET | /api/v1/shortlink/urls | List URLs | shortlink:read |
| GET | /api/v1/shortlink/urls/{id} | Get URL detail | shortlink:read |
| PUT | /api/v1/shortlink/urls/{id} | Update URL | shortlink:update |
| DELETE | /api/v1/shortlink/urls/{id} | Delete URL | shortlink:delete |
| GET | /api/v1/shortlink/urls/{id}/analytics | Get analytics | shortlink:analytics:read |
| GET | /api/v1/shortlink/urls/{id}/qr | Get QR code | shortlink:qr:read |
| GET | /{short_code} | Redirect | Public |
| POST | /api/v1/shortlink/campaigns | Create campaign | shortlink:campaign:create |
| GET | /api/v1/shortlink/campaigns/{id}/analytics | Campaign analytics | shortlink:campaign:analytics |

---

## 6. Permissions Matrix

### Roles
| Role | Create URLs | View Own URLs | View All URLs | View Analytics | Manage Campaigns | Export Data |
|------|-----------|-------------|-------------|---------------|-----------------|------------|
| Employee | ✅ | ✅ | ❌ | Own only | ❌ | ❌ |
| Marketing User | ✅ | ✅ | ✅ | All | ✅ | ✅ |
| Analyst | ❌ | ❌ | ✅ | All | Read-only | ✅ |
| System Admin | ✅ | ✅ | ✅ | All | ✅ | ✅ |

### Row-Level Security
- Users can only view and manage their own URLs unless they have elevated permissions.
- Campaign analytics visible to campaign creator and marketing users.

---

## 7. State Machine / Workflows

### 7.1 URL Status Transitions

```
[ACTIVE] ──expire──→ [EXPIRED] ──reactivate──→ [ACTIVE]
   │
   └──delete──→ [DELETED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| ACTIVE | EXPIRED | expire | System (auto on date) | Redirect to expired page |
| EXPIRED | ACTIVE | reactivate | Owner | Restore normal redirect |
| ACTIVE | DELETED | delete | Owner or Admin | Remove from listings, preserve clicks |

---

## 8. Reports & Analytics

### 8.1 Report: URL Performance Report
**Type:** Real-time
**Purpose:** Track shortened URL performance and engagement.

**Dimensions:**
- Time period
- Campaign
- Tag
- Creator

**Metrics:**
- Total URLs created
- Total clicks across all URLs
- Average clicks per URL
- Top performing URLs
- Click-through rate trends

**Filters:**
- Date range
- Campaign
- Tag
- Creator

**Chart Type:** Line chart (trends), Bar chart (top URLs), Table

### 8.2 Report: Campaign Performance Report
**Type:** Real-time
**Purpose:** Aggregated campaign analytics.

**Metrics:**
- Total URLs in campaign
- Total campaign clicks
- Unique clicks
- Best performing URL in campaign
- Click trend over campaign duration

**Chart Type:** Summary cards, line chart, bar chart

### 8.3 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| URL Performance | Real-time | On-demand | Line chart, bar chart |
| Campaign Performance | Real-time | On-demand | Cards, line chart |
| Referrer Analysis | Real-time | On-demand | Pie chart |
| Device Distribution | Real-time | On-demand | Bar chart |
| Geographic Analysis | Real-time | On-demand | Map |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| QR Code Generator (qrcode library) | Generate QR codes | Local | N/A |
| IP Geolocation (MaxMind) | Resolve IP to location | Local DB | License |
| URL Validation | Check destination URL validity | Local | N/A |
| M31 aiMarketing | Share tracked links in campaigns | REST | JWT |
| M20 Chat | Share shortened URLs in messages | REST | JWT |
| M23 Social Network | Share shortened URLs in posts | REST | JWT |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| shortlink.created | {short_url_id, short_code, original_url, creator_id} | M31 aiMarketing |
| shortlink.click | {short_url_id, ip, referrer, device, timestamp} | M31 aiMarketing, M39 Dashboard |
| shortlink.expired | {short_url_id, short_code} | M31 aiMarketing |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| campaign.created | M31 aiMarketing | Create corresponding shortlink campaign |

---

## 10. Technical Notes

### Performance
- Expected data volume: 100,000+ shortened URLs, 5,000,000+ click records
- Query complexity: Low for redirect, Medium for analytics aggregation
- Expected response time: <50ms for redirect, <200ms for analytics
- Click recording uses async queue to avoid blocking redirect

### Caching Strategy
- Short URL to destination mapping cached with 1-hour TTL (Redis)
- QR codes cached permanently (immutable after generation)
- Analytics aggregation cached with 5-minute TTL
- Cache invalidation on URL create/update/delete

### Redirect Architecture
- Redirect endpoint is the highest-traffic endpoint; optimized for minimal latency
- Click recording done asynchronously via message queue (RabbitMQ/Redis Streams)
- Bot detection performed inline to avoid recording bot clicks
- 302 redirect (temporary) to allow analytics on every click

### QR Code Generation
- QR codes generated using standard QR library with error correction level M
- Three sizes pre-generated: 200x200, 400x400, 800x800
- PNG for web use, SVG for print/design use
- QR codes encode the short URL (not the destination) for analytics tracking
- QR codes stored in S3 with CDN distribution

### Bot Detection
- User agent analysis to identify known bots and crawlers
- IP reputation checking to filter datacenter/VPN traffic
- Click rate limiting (>10 clicks per minute from same IP = suspicious)
- Bot clicks are redirected but not counted in analytics

### URL Validation
- Destination URL validated for format (must be valid HTTP/HTTPS URL)
- Optional accessibility check (HEAD request to verify URL responds)
- Malware/phishing URL detection via blocklist
- Redirect chains detected (>3 redirects flagged)

### Domain Configuration
- Default short domain: short.misa.vn (configurable)
- Custom domains supported for enterprise clients
- SSL/TLS required for all short URLs
- HSTS enabled for redirect domain

### Mobile Considerations
- Mobile-optimized URL creation form
- QR code display optimized for mobile screens
- One-tap copy to clipboard for shortened URLs
- Share button for native mobile sharing (WhatsApp, Zalo, etc.)
- Mobile preview showing shortened URL with QR code (img_65)

### Data Retention
- Click records retained for 2 years (configurable)
- Aggregated analytics (daily summaries) retained indefinitely
- Deleted URLs preserved for 90 days before permanent deletion
- GDPR-compliant: IP addresses anonymized after 90 days
