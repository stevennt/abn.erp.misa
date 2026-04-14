# M28 - Notes & Knowledge (Kien thuc)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_18.png, assets/image_72.png
**Dependencies:** M01 (User & Authentication), M21 (Document Management)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Notes & Knowledge module provides a comprehensive knowledge management platform for creating, organizing, and sharing company knowledge through articles, how-to guides, best practices, and reference materials. It serves as the organization's internal wiki/encyclopedia, enabling employees to document procedures, share expertise, and create a searchable knowledge base organized by categories and tags. The module supports collaborative knowledge creation with multiple authors per article, version tracking, user ratings for quality feedback, and threaded comments for discussion. Knowledge articles are categorized hierarchically (similar to the Document Management module in img_18) and tagged for cross-category discovery. The mobile app integration (img_72) ensures knowledge is accessible on-the-go, and the rating system helps identify the most valuable content. The module complements the Document Management module by focusing on living knowledge articles rather than static documents.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Knowledge Contributor | Employees who create and edit articles | Create + edit (own) |
| Knowledge Editor | Users who can edit any article | Create + edit (all) |
| Category Manager | Manages knowledge categories | Full category management |
| Knowledge Consumer | Any employee who reads and rates articles | Read + rate + comment |
| System Administrator | Configures global settings | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication)
- **Used by:** M21 (Document Management), M26 (Video Library), M31 (aiMarketing)

### Key Business Rules
1. Knowledge articles are organized in a hierarchical category tree for structured browsing.
2. Each article has a title, content (rich text), category assignment, tags, author(s), and version number.
3. Multiple authors can be assigned to a single article for collaborative knowledge creation.
4. Articles support versioning; each edit creates a new version with the previous version preserved.
5. User ratings (1-5 stars) provide quality feedback; average rating is displayed on article cards.
6. Comments support threaded discussion on articles for clarification and additional input.
7. Tags are user-created labels for cross-category article discovery.
8. Articles can be marked as draft (work in progress) or published (ready for consumption).
9. Published articles are searchable across the entire knowledge base.
10. Article metadata includes: title, description, category, tags, authors, version, rating, view count, creation date, last edited date.
11. Articles can reference related articles for cross-linking knowledge.
12. The mobile app (img_72) provides knowledge browsing and article reading on mobile devices.

---

## 2. Screen Inventory

### 2.1 Screen: Knowledge Base Home
**Route:** `/knowledge`
**Purpose:** Main knowledge browsing interface with category navigation (img_18).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Search Bar | Full-width | "Enter keyword to search" |
| Category Sidebar | Tree navigation | Hierarchical category tree with expand/collapse |
| Article List | Data grid | Title, description, creator, created date, last editor, last edited date |
| Breadcrumb | Navigation | All > Process Documents, Regulations > Company Culture |
| Create Article | CTA | Create new knowledge article |
| Pagination | Footer | Total count, records per page |
| View Toggle | Toggle | List view / Card view |

**User Interactions:**
- Browse articles by category.
- Search across all knowledge articles.
- Click an article to read it.
- Create new articles.
- Navigate category tree.

**Data Displayed:**
- Articles with metadata: title, description, creator, dates
- Total count: 10 documents (similar to img_18 pattern)
- Category hierarchy for navigation

### 2.2 Screen: Article Detail View
**Route:** `/knowledge/articles/{id}`
**Purpose:** Full article reading with engagement features.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Article Header | Card | Title, authors, category, version, rating |
| Article Content | Rich text display | Formatted article body |
| Rating Widget | Star input | Rate this article (1-5 stars) |
| Rating Summary | Display | Average rating, total ratings count |
| Comments Section | Thread | User comments with replies |
| Table of Contents | Sidebar | Auto-generated from article headings |
| Related Articles | Card grid | Articles in same category or with shared tags |
| Version History | Link | View all versions |
| Action Buttons | Button group | Edit, Share, Print, Export PDF |

**User Interactions:**
- Read the full article.
- Rate the article with stars.
- Add comments and replies.
- Navigate using table of contents.
- View related articles.
- Edit article (if authorized).
- View version history.
- Export to PDF.

### 2.3 Screen: Article Editor
**Route:** `/knowledge/articles/new` or `/knowledge/articles/{id}/edit`
**Purpose:** Rich text editor for creating and editing knowledge articles.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Title Input | Text field | Article title |
| Rich Text Editor | WYSIWYG | Full formatting toolbar (bold, italic, lists, tables, images, code blocks, links) |
| Category Selector | Tree dropdown | Assign to category |
| Tags Input | Multi-tag | Add tags |
| Co-Authors | Multi-select | Add additional authors |
| Version Note | Textarea | Change notes for new versions |
| Preview | Button | Preview article as reader |
| Save Draft | Button | Save as draft |
| Publish | Button | Publish article |

**User Interactions:**
- Write article content with rich formatting.
- Insert images, tables, and code blocks.
- Assign category and tags.
- Add co-authors.
- Preview before publishing.
- Save as draft or publish.

### 2.4 Screen: Category Management
**Route:** `/knowledge/settings/categories`
**Purpose:** Manage the knowledge category tree.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Category Tree | Interactive tree | Drag-and-drop reordering, add/delete |
| Add Category Form | Modal | Name, description, parent category |
| Permission Settings | Table | Default permissions per category |
| Save Button | CTA | Apply changes |

### 2.5 Screen: Knowledge Rating Dashboard
**Route:** `/knowledge/analytics`
**Purpose:** Analytics on knowledge article quality and engagement.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Summary Cards | Metrics | Total articles, average rating, total views, total comments |
| Top Rated Articles | List | Highest-rated articles with scores |
| Most Viewed | List | Articles with highest view counts |
| Rating Distribution | Bar chart | Distribution of 1-5 star ratings |
| Category Performance | Table | Average rating per category |

### 2.6 Screen: Mobile Knowledge View
**Route:** Mobile app (img_72)
**Purpose:** Mobile-optimized knowledge browsing and reading.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Article List | Card list | Article cards with title, category, rating |
| Search | Input | Full-text search |
| Article Reader | Full-screen | Scrollable article view |
| Rating | Star widget | Tap to rate |

---

## 3. Feature Specifications

### 3.1 Feature: Knowledge Article Creation
**Description:** Rich text editor for creating knowledge articles with formatting, images, and structure.

**User Stories:**
- As a knowledge contributor, I want to write a how-to guide with step-by-step instructions and screenshots, so that others can follow the procedure.
- As a knowledge contributor, I want to save my article as a draft before publishing, so that I can review it first.

**Acceptance Criteria:**
- Given a user opens the article editor
- When they write content with headings, lists, and images
- Then the article renders correctly in the preview

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Title | Required, max 500 chars | "Article title is required" |
| Content | Required, min 50 chars | "Article content is too short" |
| Category | Required | "Please assign a category" |

**Edge Cases:**
- Auto-save every 30 seconds to prevent data loss.
- Concurrent editing detected with optimistic locking; last writer wins with notification.

### 3.2 Feature: Article Versioning
**Description:** Automatic version tracking for knowledge articles.

**User Stories:**
- As an editor, I want to update an article and have the old version preserved, so that changes are traceable.
- As a reader, I want to see the version history, so that I can understand how the article has evolved.

**Acceptance Criteria:**
- Given an article exists at version 1.0
- When an editor makes changes and saves
- Then the article becomes version 1.1 with the old version preserved

**Edge Cases:**
- Version numbering: major.minor (major for significant changes, minor for updates).
- Restoring a previous version creates a new version based on the old content.

### 3.3 Feature: Article Rating System
**Description:** 5-star rating system for article quality feedback.

**User Stories:**
- As a reader, I want to rate an article, so that I can provide feedback on its quality.
- As a contributor, I want to see the average rating of my articles, so that I know which content is most valued.

**Acceptance Criteria:**
- Given an article is displayed
- When a user clicks the 4th star
- Then the article receives a 4-star rating from that user

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Rating | 1-5 stars | "Rating must be between 1 and 5 stars" |

**Edge Cases:**
- Users can only rate once per article; re-rating updates their previous rating.
- Average rating calculated as weighted mean with display to 1 decimal place.

### 3.4 Feature: Multi-Author Support
**Description:** Multiple authors can be assigned to a single knowledge article.

**User Stories:**
- As a team lead, I want to add my team members as co-authors, so that credit is shared for collaborative work.

**Acceptance Criteria:**
- Given an article has a primary author
- When they add co-authors
- Then all authors are displayed on the article header

**Edge Cases:**
- Any assigned author can edit the article.
- Primary author has additional permissions (delete, change authors).

### 3.5 Feature: Knowledge Comments
**Description:** Threaded discussion on knowledge articles.

**User Stories:**
- As a reader, I want to ask questions in the comments, so that I can get clarification.
- As an author, I want to see and respond to comments, so that I can help readers.

**Acceptance Criteria:**
- Given a user reads an article
- When they add a comment
- Then the comment appears below the article for others to see and reply to

**Edge Cases:**
- Comments support one level of nesting (comment + reply).
- Authors can pin helpful comments.

### 3.6 Feature: Related Articles
**Description:** Automatic suggestion of related articles for cross-referencing.

**User Stories:**
- As a reader, I want to see related articles, so that I can explore connected topics.

**Acceptance Criteria:**
- Given an article is displayed
- When the related articles section loads
- Then articles in the same category or with shared tags are suggested

**Edge Cases:**
- Related articles algorithm: same category (primary), shared tags (secondary), same author (tertiary).
- Maximum 6 related articles displayed.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------------+       +----------------------+       +----------------------+
| KnowledgeArticle     |       | KnowledgeCategory    |       | KnowledgeAuthor    |
+----------------------+       +----------------------+       +----------------------+
| id (PK)              |<----->| id (PK)              |<----->| id (PK)              |
| title                |       | name                 |       | article_id (FK)      |
| content              |       | parent_id (FK)       |       | user_id (FK)         |
| category_id (FK)     |       | sort_order           |       | is_primary           |
| version_number       |       | created_at           |       | created_at           |
| average_rating       |       +----------------------+       +----------------------+
| view_count           |
| created_at           |       +----------------------+
+----------------------+       | KnowledgeComment   |
                               +----------------------+
+----------------------+       | id (PK)              |
| KnowledgeVersion     |       | article_id (FK)      |
+----------------------+       | user_id (FK)         |
| id (PK)              |       | content              |
| article_id (FK)      |       | parent_id (FK)       |
| version_number       |       | created_at           |
| content              |       +----------------------+
| change_notes         |
| created_by (FK)      |       +----------------------+
| created_at           |       | KnowledgeRating    |
+----------------------+       +----------------------+
                               | id (PK)              |
+----------------------+       | article_id (FK)      |
| KnowledgeTag         |       | user_id (FK)         |
+----------------------+       | rating (1-5)         |
| id (PK)              |       | created_at           |
| name                 |       +----------------------+
+----------------------+
```

### 4.2 Table Schemas

#### Table: knowledge_articles
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| title | VARCHAR(500) | NOT NULL | Article title |
| summary | TEXT | NULL | Article summary/description |
| category_id | BIGINT | NOT NULL, FK -> knowledge_categories.id | Category |
| current_version_id | BIGINT | NULL, FK -> knowledge_versions.id | Latest version |
| version_number | VARCHAR(20) | NOT NULL, DEFAULT '1.0' | Current version |
| average_rating | DECIMAL(3,2) | NULL, DEFAULT NULL | Average rating (1.00-5.00) |
| rating_count | INT | NOT NULL, DEFAULT 0 | Total ratings |
| view_count | INT | NOT NULL, DEFAULT 0 | Total views |
| comment_count | INT | NOT NULL, DEFAULT 0 | Total comments |
| status | ENUM('draft','published','archived') | NOT NULL, DEFAULT 'draft' | Article status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| published_at | TIMESTAMP | NULL, DEFAULT NULL | Publication timestamp |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_knowledge_articles_category ON knowledge_articles(category_id);
CREATE INDEX idx_knowledge_articles_status ON knowledge_articles(status);
CREATE INDEX idx_knowledge_articles_rating ON knowledge_articles(average_rating DESC);
CREATE INDEX idx_knowledge_articles_views ON knowledge_articles(view_count DESC);
CREATE INDEX idx_knowledge_articles_created ON knowledge_articles(created_at DESC);
```

#### Table: knowledge_versions
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| article_id | BIGINT | NOT NULL, FK -> knowledge_articles.id | Parent article |
| version_number | VARCHAR(20) | NOT NULL | Version number |
| content | LONGTEXT | NOT NULL | Article content (HTML) |
| change_notes | TEXT | NULL | Description of changes |
| created_by | BIGINT | NOT NULL, FK -> users.id | Version creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Version timestamp |

**Indexes:**
```sql
CREATE INDEX idx_knowledge_versions_article ON knowledge_versions(article_id);
CREATE UNIQUE INDEX idx_knowledge_versions_art_ver ON knowledge_versions(article_id, version_number);
```

#### Table: knowledge_categories
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Category name |
| description | TEXT | NULL | Category description |
| parent_id | BIGINT | NULL, FK -> knowledge_categories.id | Parent category |
| sort_order | INT | NOT NULL, DEFAULT 0 | Display order |
| article_count | INT | NOT NULL, DEFAULT 0 | Article count |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_knowledge_categories_parent ON knowledge_categories(parent_id);
CREATE INDEX idx_knowledge_categories_sort ON knowledge_categories(parent_id, sort_order);
```

#### Table: knowledge_authors
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| article_id | BIGINT | NOT NULL, FK -> knowledge_articles.id | Article |
| user_id | BIGINT | NOT NULL, FK -> users.id | Author |
| is_primary | BOOLEAN | NOT NULL, DEFAULT FALSE | Primary author flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Assignment timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_knowledge_authors_art_user ON knowledge_authors(article_id, user_id);
CREATE INDEX idx_knowledge_authors_user ON knowledge_authors(user_id);
```

#### Table: knowledge_comments
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| article_id | BIGINT | NOT NULL, FK -> knowledge_articles.id | Article |
| user_id | BIGINT | NOT NULL, FK -> users.id | Comment author |
| parent_id | BIGINT | NULL, FK -> knowledge_comments.id | Reply-to |
| content | TEXT | NOT NULL | Comment text |
| is_pinned | BOOLEAN | NOT NULL, DEFAULT FALSE | Pinned comment |
| is_deleted | BOOLEAN | NOT NULL, DEFAULT FALSE | Deletion flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_knowledge_comments_article ON knowledge_comments(article_id);
CREATE INDEX idx_knowledge_comments_user ON knowledge_comments(user_id);
CREATE INDEX idx_knowledge_comments_parent ON knowledge_comments(parent_id);
```

#### Table: knowledge_ratings
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| article_id | BIGINT | NOT NULL, FK -> knowledge_articles.id | Article |
| user_id | BIGINT | NOT NULL, FK -> users.id | Rating user |
| rating | TINYINT | NOT NULL, CHECK (1-5) | Rating value |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Rating timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Re-rating timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_knowledge_ratings_art_user ON knowledge_ratings(article_id, user_id);
CREATE INDEX idx_knowledge_ratings_article ON knowledge_ratings(article_id);
```

#### Table: knowledge_tags
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL, UNIQUE | Tag name |
| usage_count | INT | NOT NULL, DEFAULT 0 | How many articles use this tag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Tag creation time |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_knowledge_tags_name ON knowledge_tags(name);
```

#### Table: knowledge_article_tag
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| article_id | BIGINT | NOT NULL, FK -> knowledge_articles.id | Article |
| tag_id | BIGINT | NOT NULL, FK -> knowledge_tags.id | Tag |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_kat_article_tag ON knowledge_article_tag(article_id, tag_id);
CREATE INDEX idx_kat_tag_id ON knowledge_article_tag(tag_id);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| knowledge_articles | Article records | 10,000 |
| knowledge_versions | Version history | 40,000 |
| knowledge_categories | Category tree | 200 |
| knowledge_authors | Article-author mapping | 15,000 |
| knowledge_comments | Article comments | 50,000 |
| knowledge_ratings | User ratings | 80,000 |
| knowledge_tags | Tag definitions | 2,000 |
| knowledge_article_tag | Article-tag mapping | 30,000 |

---

## 5. API Specifications

### 5.1 Knowledge Article Endpoints

#### GET /api/v1/knowledge/articles
**Description:** List articles with filtering and pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page (default: 20) |
| category_id | bigint | No | Filter by category |
| tag | string | No | Filter by tag |
| q | string | No | Full-text search |
| sort | string | No | created_at, average_rating, view_count |
| status | string | No | draft, published, archived |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "title": "Standard Document - Company Operation Information",
      "summary": "Information about company operations",
      "category": {"id": 5, "name": "Standard documents"},
      "authors": [{"id": 10, "name": "Le Tien Dao", "is_primary": true}],
      "version_number": "1.0",
      "average_rating": 4.5,
      "rating_count": 12,
      "view_count": 150,
      "comment_count": 5,
      "status": "published",
      "created_at": "2019-02-20T08:00:00Z",
      "updated_at": "2019-02-20T08:00:00Z"
    }
  ],
  "meta": {"total": 10, "page": 1, "limit": 10}
}
```

**Permission:** `knowledge:article:read`

#### POST /api/v1/knowledge/articles
**Description:** Create a new article

**Request Body:**
```json
{
  "title": "How to Submit Expense Reports",
  "content": "<h1>Step 1: ...</h1><p>...</p>",
  "category_id": 5,
  "tags": ["expense", "finance", "procedure"],
  "co_author_ids": [5, 8]
}
```

**Response (201 Created):**
```json
{
  "id": 11,
  "title": "How to Submit Expense Reports",
  "status": "draft",
  "version_number": "1.0",
  "created_at": "2024-04-14T09:00:00Z"
}
```

**Permission:** `knowledge:article:create`

#### POST /api/v1/knowledge/articles/{id}/rate
**Description:** Rate an article

**Request Body:**
```json
{
  "rating": 4
}
```

**Response (200 OK):**
```json
{
  "average_rating": 4.3,
  "rating_count": 13
}
```

**Permission:** `knowledge:article:rate`

### 5.2 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/knowledge/articles | List articles | knowledge:article:read |
| POST | /api/v1/knowledge/articles | Create article | knowledge:article:create |
| GET | /api/v1/knowledge/articles/{id} | Get article | knowledge:article:read |
| PUT | /api/v1/knowledge/articles/{id} | Update article | knowledge:article:update |
| DELETE | /api/v1/knowledge/articles/{id} | Delete article | knowledge:article:delete |
| POST | /api/v1/knowledge/articles/{id}/rate | Rate article | knowledge:article:rate |
| GET | /api/v1/knowledge/articles/{id}/comments | List comments | knowledge:comment:read |
| POST | /api/v1/knowledge/articles/{id}/comments | Add comment | knowledge:comment:create |

---

## 6. Permissions Matrix

### Roles
| Role | View Articles | Create Articles | Edit Articles | Delete Articles | Manage Categories | Rate | Comment |
|------|-------------|----------------|--------------|----------------|------------------|------|---------|
| Knowledge Consumer | ✅ (published) | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| Knowledge Contributor | ✅ | ✅ | ✅ (own) | ❌ | ❌ | ✅ | ✅ |
| Knowledge Editor | ✅ | ✅ | ✅ (all) | ❌ | ❌ | ✅ | ✅ |
| Category Manager | ✅ | ✅ | ✅ (all) | ❌ | ✅ | ✅ | ✅ |
| System Admin | ✅ | ✅ | ✅ (all) | ✅ | ✅ | ✅ | ✅ |

### Row-Level Security
- Draft articles visible only to authors and editors.
- Published articles visible to all employees.
- Archived articles visible to readers but not in default listings.

---

## 7. State Machine / Workflows

### 7.1 Article Status Transitions

```
[DRAFT] ──publish──→ [PUBLISHED] ──archive──→ [ARCHIVED]
   │
   └──delete──→ [DELETED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | PUBLISHED | publish | Author or Editor | Article appears in public listings, notifications sent |
| PUBLISHED | ARCHIVED | archive | Editor or Admin | Article removed from active listings, preserved for reference |
| DRAFT | DELETED | delete | Author or Admin | Permanent deletion |

---

## 8. Reports & Analytics

### 8.1 Report: Knowledge Quality Report
**Type:** Real-time
**Purpose:** Track article quality through ratings and engagement.

**Metrics:**
- Total articles published
- Average rating across all articles
- Top-rated articles
- Articles needing improvement (rating < 3)
- Total views and comments

**Chart Type:** Bar chart, table

### 8.2 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Knowledge Quality Report | Real-time | On-demand | Bar chart, table |
| Category Performance | Real-time | On-demand | Table |
| Author Contribution Report | Real-time | On-demand | Table |
| View Trend Analysis | Real-time | On-demand | Line chart |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Full-Text Search (Elasticsearch) | Article content indexing | REST | API Key |
| PDF Export Service | Generate PDF from article | REST | API Key |
| M01 User Service | Resolve author profiles | REST | JWT |
| M21 Document Management | Link related documents | REST | JWT |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| knowledge.article.published | {article_id, title, category_id, author_ids} | M21 Document Management, M39 Dashboard |
| knowledge.article.rated | {article_id, user_id, rating} | M39 Dashboard |
| knowledge.article.commented | {article_id, comment_id, user_id} | M01 Notification |

---

## 10. Technical Notes

### Performance
- Expected data volume: 10,000+ articles, 40,000+ versions
- Query complexity: Low for single article, Medium for full-text search
- Expected response time: <200ms for list views, <100ms for single article
- Article content stored as HTML; rendering is client-side

### Caching Strategy
- Published article list cached with 5-minute TTL
- Individual article content cached with 10-minute TTL
- Rating averages cached with 1-minute TTL
- Full-text search index updated within 30 seconds of publish
- Cache invalidation on article publish/update/delete

### Rich Text Content
- Content stored as sanitized HTML
- XSS protection via HTML sanitization (whitelist approach)
- Image uploads stored in S3 with CDN
- Code blocks with syntax highlighting support
- Table support for structured data

### Mobile Considerations
- Article content responsive for mobile screens
- Touch-friendly rating widget
- Offline reading for favorited articles
- Search with voice input support
- Mobile app icon visible in app grid (image_05, image_72)

### Full-Text Search
- Elasticsearch index for article titles, summaries, and content
- Vietnamese language analyzer with stemming
- Search result highlighting
- Category and tag filters combined with full-text search
- Fuzzy matching for typo tolerance

### Version Control
- Diff generation between versions for change visualization
- Version notes required for major version bumps
- Restore creates new version rather than overwrite
- Version comparison view side-by-side
