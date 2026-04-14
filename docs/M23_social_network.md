# M23 - Internal Social Network (Mang xa hoi)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_19.png, assets/image_58.png
**Dependencies:** M01 (User & Authentication), M03 (Company & Organization), M20 (Internal Chat)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Internal Social Network module provides a Facebook-like social platform for employees to share updates, ideas, news, and company announcements in an engaging feed-based interface. It serves as the cultural and social hub of the organization, fostering employee engagement through posts with images, likes, comments, shares, birthday celebrations, and company-wide announcements. The module features multiple feed sections including a general News Feed for personal shares, a News section for company announcements, an Opportunity Pool for business leads, and an Ideas section for innovation suggestions. With engagement metrics tracking (17 likes, 34 comments, 5 shares on sample posts) and birthday/notifications sidebars, the module creates a vibrant internal community that complements formal communication channels.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Employee | Any staff member who posts and engages | Full (own content) |
| Content Manager | HR/Marketing managing news and announcements | Full (news section) |
| Department Manager | Monitors department-related social activity | Read + participate |
| Community Moderator | Manages content quality and guidelines | Full moderation |
| Executive | Posts company-wide announcements | Full (announcements) |
| System Administrator | Configures global settings and moderation rules | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication), M03 (Company & Organization)
- **Used by:** M20 (Internal Chat), M31 (aiMarketing), M39 (Operations Dashboard)

### Key Business Rules
1. Posts can be created in four feed types: Share (personal updates), Ideas (innovation suggestions), News (company announcements), and Opportunity Pool (business leads).
2. All employees can create Share and Idea posts; News posts are restricted to Content Managers and Executives.
3. Likes are limited to one per user per post; users can unlike to remove their like.
4. Comments support threading with one level of replies (comment + reply).
5. Shares propagate the original post to the sharer's feed with an optional comment.
6. Birthday notifications are auto-generated from employee profile data; birthdays show on the sidebar with employee name and age milestone (e.g., "Hoang Trung Kien - Turning 27").
7. The notifications feed shows company events: new employees joined, commendations, promotions, marriages, resignations, transfers.
8. Image attachments per post support JPG, PNG, GIF with up to 10 images per post displayed in a grid layout.
9. Engagement metrics are tracked per post: like count, comment count, share count.
10. Posts can be edited within 30 minutes of creation; deleted posts show as "Post deleted" to preserve comment context.
11. The Opportunity Pool allows employees to submit business leads that can be converted to CRM opportunities.
12. Content moderation includes keyword filtering and report functionality for inappropriate content.

---

## 2. Screen Inventory

### 2.1 Screen: Social Feed (News Feed)
**Route:** `/social/feed`
**Purpose:** Main social feed showing posts from all employees (img_19).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Post Composer | Card | "What do you want to share?" input with quick actions (Share, Ideas, News) |
| Feed Stream | Scrollable list | Chronological posts with author, content, images, engagement |
| Left Navigation | Sidebar | News Feed (active), News, Opportunity Pool, Ideas |
| Right Sidebar - Birthdays | Widget | Today's birthdays with employee names and ages |
| Right Sidebar - Notifications | Widget | Company events (new hires, commendations, promotions, marriages, resignations) |
| Like/Comment/Share | Action bar | Per-post engagement buttons with counts |
| Image Grid | Gallery | Multi-image display in posts (up to 10 images) |

**User Interactions:**
- Create new posts with text and images.
- Like, comment on, and share posts.
- Browse different feed sections via left navigation.
- View birthdays and company notifications in sidebar.

**Data Displayed:**
- Sample post by Dang Thuy Anh (30 minutes ago) with beach photo
- Engagement: 17 likes, 34 comments, 5 shares
- Birthdays: Hoang Trung Kien (27), Nguyen Hanh Nguyen (23), Hoang Hai Nam (24)
- Notifications: New hires, commendations, promotions, marriages, resignations, transfers

### 2.2 Screen: News Feed
**Route:** `/social/news`
**Purpose:** Company news and official announcements.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| News List | Card list | Official news posts from HR/Management |
| Category Filter | Tabs | All, Company News, Policy Updates, Events |
| Create News Button | CTA | Restricted to Content Managers and Executives |

**User Interactions:**
- Browse company news and announcements.
- Like and comment on news posts.
- Share news to personal feed.

### 2.3 Screen: Opportunity Pool
**Route:** `/social/opportunities`
**Purpose:** Business leads and opportunities shared by employees.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Opportunity List | Card list | Business leads with description, potential value, source |
| Submit Opportunity | Button | Create new business lead |
| Convert to CRM | Action | Convert lead to CRM opportunity |
| Status Filter | Tabs | New, In Review, Converted, Archived |

**User Interactions:**
- Submit business opportunities.
- Browse and engage with opportunities.
- Convert leads to CRM records.

### 2.4 Screen: Ideas Feed
**Route:** `/social/ideas`
**Purpose:** Innovation suggestions and improvement ideas from employees.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Ideas List | Card list | Ideas with description, votes, comments |
| Submit Idea | Button | Create new idea |
| Vote Button | Action | Upvote ideas (separate from like) |
| Status Filter | Tabs | New, Under Review, Approved, Implemented |

**User Interactions:**
- Submit innovation ideas.
- Vote on and comment on ideas.
- Track idea implementation status.

### 2.5 Screen: Post Detail View
**Route:** `/social/posts/{id}`
**Purpose:** Full post view with all comments and engagement details.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Post Header | Card | Author, timestamp, content, images |
| Engagement Bar | Action buttons | Like (with count), Comment (with count), Share (with count) |
| Comment Thread | List | Comments with replies, avatars, timestamps |
| Comment Input | Form | Add new comment |
| Liked By List | Modal | View all users who liked the post |
| Shared By List | Modal | View all users who shared the post |

**User Interactions:**
- View full post content with all images.
- Add comments and replies.
- See who liked and shared the post.

### 2.6 Screen: Profile Page
**Route:** `/social/profile/{user_id}`
**Purpose:** User's social profile showing their activity.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Profile Header | Card | Avatar, name, department, join date |
| Activity Tabs | Tabs | Posts, Comments, Likes, Shares |
| Post History | Card list | User's posts with engagement metrics |

---

## 3. Feature Specifications

### 3.1 Feature: Post Creation and Feed
**Description:** Create posts in multiple feed types with text, images, and rich content.

**User Stories:**
- As an employee, I want to share updates with photos on the news feed, so that my colleagues can see what I'm working on.
- As an employee, I want to post ideas for innovation, so that the company can benefit from my suggestions.

**Acceptance Criteria:**
- Given a user types a post with images
- When they click Post
- Then the post appears in the feed with images displayed in a grid

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Content | Max 5,000 characters | "Post exceeds maximum length" |
| Images | Max 10 images, 5MB each | "Maximum 10 images allowed, 5MB each" |
| Image Types | jpg, png, gif | "Image format not supported" |

**Edge Cases:**
- Posts with no text but images are allowed (photo-only posts).
- Draft auto-save every 30 seconds to prevent data loss.

### 3.2 Feature: Social Engagement (Like, Comment, Share)
**Description:** Full engagement suite with likes, threaded comments, and sharing.

**User Stories:**
- As a user, I want to like a post, so that I can show appreciation without commenting.
- As a user, I want to comment on a post, so that I can engage in discussion.
- As a user, I want to share a post, so that my connections can see it too.

**Acceptance Criteria:**
- Given a post is displayed
- When a user clicks Like
- Then the like count increments and the button highlights
- When they click again
- Then the like is removed and count decrements

**Edge Cases:**
- Deleting the original post preserves comments with "Post deleted" notice for context.
- Shares reference the original post; if original is deleted, shared post shows "Original post removed."

### 3.3 Feature: Birthday Notifications
**Description:** Automatic birthday celebrations displayed on the sidebar.

**User Stories:**
- As an employee, I want to see colleagues' birthdays, so that I can wish them well.
- As HR, I want birthday notifications auto-generated, so that no birthday is missed.

**Acceptance Criteria:**
- Given an employee's birthday is today
- When users view the social feed
- Then the birthday sidebar shows the employee's name and age milestone

**Edge Cases:**
- Birthdays from employee profiles in M01; if profile lacks DOB, birthday is not shown.
- Privacy: employees can opt out of public birthday display.

### 3.4 Feature: Company Announcements and Notifications
**Description:** Auto-generated notifications for company events displayed in the sidebar.

**User Stories:**
- As an employee, I want to see company events like new hires, promotions, and commendations, so that I stay informed about the organization.

**Acceptance Criteria:**
- Given a new employee joins the company
- When the social feed loads
- Then the notifications sidebar shows "{Name} just joined the company"

**Notification Types:**
- New employee joined
- Commendation received
- Promotion to new position
- Marriage announcement
- Resignation notice
- Department transfer

**Edge Cases:**
- Notification visibility can be configured by HR (e.g., hide resignation notices from general feed).

### 3.5 Feature: Opportunity Pool
**Description:** Business leads shared by employees that can be converted to CRM opportunities.

**User Stories:**
- As an employee, I want to share a business lead, so that the sales team can pursue it.
- As a salesperson, I want to convert a social lead to a CRM opportunity, so that I can track it formally.

**Acceptance Criteria:**
- Given an opportunity post in the pool
- When a sales user clicks "Convert to CRM"
- Then a new CRM opportunity is created with the post content as description

**Edge Cases:**
- Duplicate detection prevents creating CRM records from the same lead twice.
- Original poster receives notification when their lead is converted.

### 3.6 Feature: Ideas and Innovation
**Description:** Employee idea submission with voting and implementation tracking.

**User Stories:**
- As an employee, I want to submit improvement ideas, so that the company can innovate.
- As a manager, I want to review and approve ideas, so that valuable suggestions get implemented.

**Acceptance Criteria:**
- Given an employee submits an idea
- When other employees vote on it
- Then the vote count increases and the idea rises in visibility

**Edge Cases:**
- Users can only vote once per idea.
- Idea status updates notify the original submitter.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|  SocialPost    |       | SocialComment  |       |  SocialLike    |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |<----->| id (PK)        |
| user_id (FK)   |       | post_id (FK)   |       | post_id (FK)   |
| post_type      |       | user_id (FK)   |       | user_id (FK)   |
| content        |       | parent_id (FK) |       | created_at     |
| image_urls     |       | content        |       +----------------+
| like_count     |       | created_at     |
| comment_count  |       +----------------+
| share_count    |
| created_at     |       +----------------+
+----------------+       | SocialShare    |
                         +----------------+
                         | id (PK)        |
                         | post_id (FK)   |
                         | user_id (FK)   |
                         | share_comment  |
                         | created_at     |
                         +----------------+
```

### 4.2 Table Schemas

#### Table: social_posts
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | NOT NULL, FK -> users.id | Post author |
| post_type | ENUM('share','idea','news','opportunity') | NOT NULL | Post type/feed |
| content | TEXT | NULL | Post text content |
| image_urls | JSON | NULL | Array of image URLs (max 10) |
| like_count | INT | NOT NULL, DEFAULT 0 | Total likes |
| comment_count | INT | NOT NULL, DEFAULT 0 | Total comments |
| share_count | INT | NOT NULL, DEFAULT 0 | Total shares |
| vote_count | INT | NOT NULL, DEFAULT 0 | Idea votes only |
| status | ENUM('draft','published','hidden','deleted') | NOT NULL, DEFAULT 'published' | Post status |
| is_pinned | BOOLEAN | NOT NULL, DEFAULT FALSE | Pinned to top |
| original_post_id | BIGINT | NULL, FK -> social_posts.id | For shared posts |
| share_comment | TEXT | NULL | Comment added when sharing |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| edited_at | TIMESTAMP | NULL, DEFAULT NULL | Edit timestamp |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_social_posts_user_id ON social_posts(user_id);
CREATE INDEX idx_social_posts_post_type ON social_posts(post_type);
CREATE INDEX idx_social_posts_created_at ON social_posts(created_at DESC);
CREATE INDEX idx_social_posts_status ON social_posts(status);
```

#### Table: social_comments
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| post_id | BIGINT | NOT NULL, FK -> social_posts.id | Target post |
| user_id | BIGINT | NOT NULL, FK -> users.id | Comment author |
| parent_id | BIGINT | NULL, FK -> social_comments.id | Reply-to comment |
| content | TEXT | NOT NULL | Comment text |
| is_deleted | BOOLEAN | NOT NULL, DEFAULT FALSE | Deletion flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_social_comments_post_id ON social_comments(post_id);
CREATE INDEX idx_social_comments_user_id ON social_comments(user_id);
CREATE INDEX idx_social_comments_parent_id ON social_comments(parent_id);
CREATE INDEX idx_social_comments_created_at ON social_comments(created_at);
```

#### Table: social_likes
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| post_id | BIGINT | NOT NULL, FK -> social_posts.id | Target post |
| user_id | BIGINT | NOT NULL, FK -> users.id | Liking user |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Like timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_social_likes_post_user ON social_likes(post_id, user_id);
CREATE INDEX idx_social_likes_post_id ON social_likes(post_id);
CREATE INDEX idx_social_likes_user_id ON social_likes(user_id);
```

#### Table: social_shares
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| post_id | BIGINT | NOT NULL, FK -> social_posts.id | Original post |
| user_id | BIGINT | NOT NULL, FK -> users.id | Sharing user |
| share_comment | TEXT | NULL | Comment added with share |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Share timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_social_shares_post_user ON social_shares(post_id, user_id);
CREATE INDEX idx_social_shares_post_id ON social_shares(post_id);
```

#### Table: social_albums
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Album name |
| user_id | BIGINT | NOT NULL, FK -> users.id | Album owner |
| cover_url | VARCHAR(500) | NULL | Cover image URL |
| photo_count | INT | NOT NULL, DEFAULT 0 | Photo count |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |

**Indexes:**
```sql
CREATE INDEX idx_social_albums_user_id ON social_albums(user_id);
```

#### Table: social_events
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| title | VARCHAR(300) | NOT NULL | Event title |
| description | TEXT | NULL | Event description |
| event_date | DATE | NOT NULL | Event date |
| event_type | ENUM('birthday','holiday','team_building','conference','other') | NOT NULL | Event type |
| image_url | VARCHAR(500) | NULL | Event image |
| created_by | BIGINT | NOT NULL, FK -> users.id | Creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |

**Indexes:**
```sql
CREATE INDEX idx_social_events_event_date ON social_events(event_date);
CREATE INDEX idx_social_events_type ON social_events(event_type);
```

#### Table: birthdays
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | NOT NULL, UNIQUE, FK -> users.id | Employee |
| birth_date | DATE | NOT NULL | Date of birth (month/day used annually) |
| is_public | BOOLEAN | NOT NULL, DEFAULT TRUE | Show publicly |
| last_celebrated | DATE | NULL, DEFAULT NULL | Last celebration date |

**Indexes:**
```sql
CREATE INDEX idx_birthdays_birth_date ON birthdays(birth_date);
```

#### Table: announcements
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| title | VARCHAR(300) | NOT NULL | Announcement title |
| content | TEXT | NOT NULL | Announcement content |
| announcement_type | ENUM('new_hire','commendation','promotion','marriage','resignation','transfer','general') | NOT NULL | Type |
| subject_user_id | BIGINT | NULL, FK -> users.id | Subject of announcement |
| created_by | BIGINT | NOT NULL, FK -> users.id | Creator (usually HR) |
| is_published | BOOLEAN | NOT NULL, DEFAULT FALSE | Published status |
| published_at | TIMESTAMP | NULL, DEFAULT NULL | Publication timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |

**Indexes:**
```sql
CREATE INDEX idx_announcements_type ON announcements(announcement_type);
CREATE INDEX idx_announcements_published ON announcements(is_published, published_at DESC);
CREATE INDEX idx_announcements_subject ON announcements(subject_user_id);
```

#### Table: ideas
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| post_id | BIGINT | NOT NULL, UNIQUE, FK -> social_posts.id | Linked social post |
| title | VARCHAR(300) | NOT NULL | Idea title |
| category | VARCHAR(100) | NULL | Idea category |
| status | ENUM('new','under_review','approved','implemented','rejected') | NOT NULL, DEFAULT 'new' | Idea status |
| review_notes | TEXT | NULL | Reviewer notes |
| reviewed_by | BIGINT | NULL, FK -> users.id | Reviewer |
| reviewed_at | TIMESTAMP | NULL, DEFAULT NULL | Review timestamp |

**Indexes:**
```sql
CREATE INDEX idx_ideas_status ON ideas(status);
```

#### Table: opportunity_pool
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| post_id | BIGINT | NOT NULL, UNIQUE, FK -> social_posts.id | Linked social post |
| estimated_value | DECIMAL(15,2) | NULL | Estimated deal value |
| source_details | TEXT | NULL | Additional source information |
| status | ENUM('new','in_review','converted','archived') | NOT NULL, DEFAULT 'new' | Status |
| crm_opportunity_id | BIGINT | NULL, FK -> crm_opportunities.id | Converted CRM record |
| converted_by | BIGINT | NULL, FK -> users.id | Conversion user |
| converted_at | TIMESTAMP | NULL, DEFAULT NULL | Conversion timestamp |

**Indexes:**
```sql
CREATE INDEX idx_opportunity_pool_status ON opportunity_pool(status);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| social_posts | All social posts | 50,000 |
| social_comments | Post comments | 200,000 |
| social_likes | Post likes | 300,000 |
| social_shares | Post shares | 50,000 |
| social_albums | Photo albums | 2,000 |
| social_events | Company events | 1,000 |
| birthdays | Employee birthdays | 1,253 |
| announcements | Company announcements | 5,000 |
| ideas | Innovation ideas | 5,000 |
| opportunity_pool | Business leads | 3,000 |

---

## 5. API Specifications

### 5.1 Post Endpoints

#### GET /api/v1/social/posts
**Description:** List posts with filtering by feed type

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page (default: 20) |
| post_type | string | No | share, idea, news, opportunity |
| user_id | bigint | No | Filter by author |
| sort | string | No | created_at, like_count, comment_count |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "user": {"id": 10, "name": "Dang Thuy Anh", "avatar_url": "...", "department": "Marketing"},
      "post_type": "share",
      "content": "Teambuilding with Misa in the last days of autumn is wonderful...",
      "image_urls": ["https://.../img1.jpg", "https://.../img2.jpg"],
      "like_count": 17,
      "comment_count": 34,
      "share_count": 5,
      "has_liked": false,
      "created_at": "2024-04-14T10:00:00Z"
    }
  ],
  "meta": {"total": 500, "page": 1, "limit": 20}
}
```

**Permission:** `social:post:read`

#### POST /api/v1/social/posts
**Description:** Create a new post

**Request Body:**
```json
{
  "post_type": "share",
  "content": "Great team building event today!",
  "image_ids": [101, 102, 103]
}
```

**Response (201 Created):**
```json
{
  "id": 501,
  "post_type": "share",
  "created_at": "2024-04-14T11:00:00Z"
}
```

**Permission:** `social:post:create`

#### POST /api/v1/social/posts/{id}/like
**Description:** Toggle like on a post

**Response (200 OK):**
```json
{
  "has_liked": true,
  "like_count": 18
}
```

**Permission:** `social:post:like`

#### POST /api/v1/social/posts/{id}/share
**Description:** Share a post

**Request Body:**
```json
{
  "share_comment": "Great memories!"
}
```

**Permission:** `social:post:share`

### 5.2 Comment Endpoints

#### GET /api/v1/social/posts/{post_id}/comments
**Description:** List comments on a post

**Permission:** `social:comment:read`

#### POST /api/v1/social/posts/{post_id}/comments
**Description:** Add a comment

**Request Body:**
```json
{
  "content": "Looks amazing!",
  "parent_id": null
}
```

**Permission:** `social:comment:create`

### 5.3 Birthday and Announcement Endpoints

#### GET /api/v1/social/birthdays/today
**Description:** Get today's birthdays

**Response (200 OK):**
```json
{
  "data": [
    {"user_id": 10, "name": "Hoang Trung Kien", "age": 27, "avatar_url": "..."},
    {"user_id": 11, "name": "Nguyen Hanh Nguyen", "age": 23, "avatar_url": "..."},
    {"user_id": 12, "name": "Hoang Hai Nam", "age": 24, "avatar_url": "..."}
  ]
}
```

**Permission:** `social:birthday:read`

#### GET /api/v1/social/announcements/recent
**Description:** Get recent company announcements

**Permission:** `social:announcement:read`

### 5.4 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/social/posts | List posts | social:post:read |
| POST | /api/v1/social/posts | Create post | social:post:create |
| POST | /api/v1/social/posts/{id}/like | Toggle like | social:post:like |
| POST | /api/v1/social/posts/{id}/share | Share post | social:post:share |
| GET | /api/v1/social/posts/{id}/comments | List comments | social:comment:read |
| POST | /api/v1/social/posts/{id}/comments | Add comment | social:comment:create |
| GET | /api/v1/social/birthdays/today | Today's birthdays | social:birthday:read |
| GET | /api/v1/social/announcements/recent | Recent announcements | social:announcement:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Employee | employee | Standard social features |
| Content Manager | content_manager | Can create News posts and announcements |
| Community Moderator | community_moderator | Content moderation, hide inappropriate posts |
| Executive | executive | Company-wide announcements |
| System Admin | sys_admin | Full access |

### Permission Matrix
| Role | Create Share | Create Idea | Create News | Like | Comment | Share | Moderate | Create Announcement |
|------|-------------|------------|------------|------|---------|-------|---------|-------------------|
| Employee | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Content Manager | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Community Moderator | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Executive | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| System Admin | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

### Row-Level Security
- Hidden posts are visible only to moderators and admins.
- Deleted posts show content to comment authors but hide original content.
- Private birthday opt-out respected across all views.

---

## 7. State Machine / Workflows

### 7.1 Post Status Transitions

```
[DRAFT] ──publish──→ [PUBLISHED] ──hide──→ [HIDDEN]
                                              │
   ┌──────────────────────────────────────────┘
   │
   └──delete──→ [DELETED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | PUBLISHED | publish | Author | Post appears in feed, notifications sent |
| PUBLISHED | HIDDEN | hide | Moderator | Post hidden from feed, author notified |
| PUBLISHED | DELETED | delete | Author (30min) or Moderator | Content hidden, comments preserved |

### 7.2 Idea Status Transitions

```
[NEW] ──review──→ [UNDER_REVIEW] ──approve──→ [APPROVED] ──implement──→ [IMPLEMENTED]
                          │
                          └──reject──→ [REJECTED]
```

### 7.3 Opportunity Status Transitions

```
[NEW] ──review──→ [IN_REVIEW] ──convert──→ [CONVERTED]
                        │
                        └──archive──→ [ARCHIVED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Social Engagement Summary
**Type:** Real-time
**Purpose:** Track overall social platform engagement and activity levels.

**Dimensions:**
- Feed type (Share, News, Ideas, Opportunity)
- Time period
- Department

**Metrics:**
- Total posts created
- Total likes (17 on sample post)
- Total comments (34 on sample post)
- Total shares (5 on sample post)
- Active users count

**Filters:**
- Date range
- Department
- Post type

**Chart Type:** Bar chart, summary cards

### 8.2 Report: Birthday and Events Calendar
**Type:** Real-time
**Purpose:** Monthly birthday and event calendar view.

**Metrics:**
- Birthdays this month
- Upcoming events
- Anniversary dates

**Chart Type:** Calendar view

### 8.3 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Social Engagement Summary | Real-time | On-demand | Bar chart, cards |
| Top Posts Report | Real-time | On-demand | Table |
| Birthday Calendar | Real-time | Monthly | Calendar |
| Idea Implementation Rate | Real-time | On-demand | Gauge chart |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Image Processing (Sharp/Pillow) | Thumbnail generation, image optimization | Local | N/A |
| Push Notification (FCM) | Mobile notifications for likes, comments | POST | API Key |
| M01 User Service | Resolve user profiles, DOB for birthdays | REST | JWT |
| M03 Org Service | Department info for posts | REST | JWT |
| M32 CRM Service | Convert opportunity to CRM record | REST | JWT |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| social.post.created | {post_id, user_id, post_type} | M20 Chat, M39 Dashboard |
| social.post.liked | {post_id, user_id, liker_id} | M01 Notification Service |
| social.comment.created | {post_id, comment_id, user_id} | M01 Notification Service |
| social.opportunity.converted | {opportunity_id, crm_id} | M32 CRM |
| social.idea.approved | {idea_id, post_id} | M01 Notification Service |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| employee.onboarded | M01 Auth Service | Create birthday record, post welcome announcement |
| employee.promoted | M18 Labor Relations | Create promotion announcement |
| employee.resigned | M18 Labor Relations | Create resignation notification |
| employee.commended | M18 Labor Relations | Create commendation announcement |

---

## 10. Technical Notes

### Performance
- Expected data volume: 50,000+ posts, 200,000+ comments, 300,000+ likes
- Query complexity: Medium (feed aggregation with engagement counts)
- Expected response time: <300ms for feed loading with pagination
- Image serving via CDN with lazy loading

### Caching Strategy
- Feed posts cached with 1-minute TTL (Redis)
- Engagement counts (likes, comments, shares) cached with 30-second TTL
- Birthday list cached daily at midnight
- Announcement list cached with 5-minute TTL
- Cache invalidation on new post, like, comment, or share

### File Attachments
- Supported types: jpg, png, gif
- Max size: 5MB per image, 10 images per post
- Storage: S3-compatible object storage with CDN
- Automatic image optimization (resize, compress, WebP conversion)

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Time display: Relative ("30 minutes ago") for recent, absolute for older

### Mobile Considerations
- Infinite scroll feed optimized for mobile
- Image lazy loading and progressive JPEG loading
- Touch-friendly like/comment/share buttons
- Push notifications for likes, comments, shares, mentions
- Offline draft saving
- Mobile app icon visible in marketplace (image_06, image_07)

### Security
- Image upload virus scanning
- Content moderation with configurable keyword filters
- Report functionality for inappropriate content
- Rate limiting: 30 posts per hour per user, 60 comments per hour
- PII protection: birthday opt-out, announcement visibility controls

### News Feed Algorithm
- Default chronological ordering
- Pinned posts always appear first
- Future enhancement: engagement-weighted ordering (more liked posts appear higher)
