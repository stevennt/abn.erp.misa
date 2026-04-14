# M26 - Video Library (Kho phim)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_23.png
**Dependencies:** M01 (User & Authentication)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Video Library module provides a centralized platform for storing, organizing, and sharing video content within the organization. It enables users to add video links (from YouTube, Vimeo, internal hosting, or direct uploads), manage video metadata (title, thumbnail, description), categorize videos, and track engagement through views, comments, and likes. The module serves as the corporate video repository for training materials, meeting recordings, product demos, company events, and educational content. With 150 views tracked on sample videos and a clean interface for video management, the module ensures that valuable video content is easily discoverable and accessible to all employees. Videos can be organized into categories for structured browsing, and the engagement tracking helps identify the most valuable content.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Video Viewer | Any employee who watches videos | Read + engage |
| Video Contributor | Users who add and manage videos | Create + edit (own) |
| Category Manager | Manages video categories | Full category management |
| System Administrator | Configures global settings and storage | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication)
- **Used by:** M28 (Notes & Knowledge), M31 (aiMarketing), M38 (OneAI Platform)

### Key Business Rules
1. Videos can be added via external link (YouTube, Vimeo, etc.) or direct upload to the platform.
2. Each video requires: link/source URL, title, thumbnail image, and description.
3. Videos are organized into user-managed categories for structured browsing.
4. View count is incremented each time a user plays a video (deduplicated per session).
5. Comments support threading with one level of replies.
6. Likes are limited to one per user per video.
7. Video sources can be tracked (YouTube, Vimeo, Internal Upload, Google Drive, etc.).
8. Videos can be marked as private (visible only to creator and shared users) or public (visible to all employees).
9. Sample videos show 150 views, demonstrating engagement tracking capability.
10. Thumbnail images can be auto-generated from video or manually uploaded.
11. Video playback supports embedded player for external sources and native player for uploads.
12. Videos are soft-deletable; deleted videos are hidden but preserved for 90 days.

---

## 2. Screen Inventory

### 2.1 Screen: Video Library List
**Route:** `/videos`
**Purpose:** Main video browsing interface with category filtering (img_23).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Add Video Button | CTA | Add new video link or upload |
| Category Tabs | Tab bar | All, Training, Events, Product Demos, Meetings, custom categories |
| Video Grid | Card grid | Thumbnail, title, date, view count, duration |
| Search Bar | Input | Search by title or description |
| Sort Options | Dropdown | Sort by: newest, most viewed, most liked |
| View Toggle | Toggle | Grid view / List view |

**User Interactions:**
- Browse videos by category.
- Search videos by title or description.
- Sort by date, views, or likes.
- Click a video to view details and play.
- Add new videos via link or upload.

**Data Displayed:**
- Video cards with thumbnail, title, date, view count
- Sample: 150 views on featured video
- Category filtering

### 2.2 Screen: Add Video Modal
**Route:** Modal from video list
**Purpose:** Add a new video via link or upload (img_23).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Source Type Selector | Radio buttons | Link (YouTube, Vimeo, etc.) or Upload |
| Video Link Input | URL input | Paste video URL (for link source) |
| Title Input | Text field | Video title (required) |
| Thumbnail Upload | Image uploader | Upload or auto-generate thumbnail |
| Description | Textarea | Video description |
| Category Selector | Multi-select | Assign to categories |
| Privacy Toggle | Toggle | Public or Private |
| Source Tags | Tag input | Source label (YouTube, Vimeo, etc.) |
| Submit Button | CTA | Add video |

**User Interactions:**
- Choose source type (link or upload).
- Enter video link or upload file.
- Fill in title, thumbnail, and description.
- Assign categories.
- Set privacy level.
- Submit to add video.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Title | Required, max 300 chars | "Video title is required" |
| Video Link | Valid URL (for link source) | "Please enter a valid video URL" |
| Upload File | mp4, webm, max 500MB | "File type/size not supported" |
| Thumbnail | jpg, png, max 5MB | "Thumbnail format/size not supported" |

### 2.3 Screen: Video Detail and Player
**Route:** `/videos/{id}`
**Purpose:** Full video player with metadata, comments, and related videos.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Video Player | Embedded player | Embedded (external) or native (upload) player |
| Video Info | Card | Title, description, source, uploader, date, category |
| Engagement Bar | Action buttons | Like (with count), View count (150), Share |
| Comments Section | Thread | User comments with replies |
| Related Videos | Card grid | Videos in same category or by same uploader |
| Edit/Delete | Buttons | For video owner and admins |

**User Interactions:**
- Play video with embedded or native player.
- Like and comment on video.
- Share video link with colleagues.
- View related videos.
- Edit or delete video (owner/admin).

### 2.4 Screen: Video Category Management
**Route:** `/videos/settings/categories`
**Purpose:** Manage video categories.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Category List | Table | Category name, video count, actions |
| Add Category | Form | Name, description, icon |
| Edit/Delete | Actions | Modify or remove categories |

### 2.5 Screen: My Videos
**Route:** `/videos/my-videos`
**Purpose:** List of videos added by the current user.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| My Video List | Card grid | User's uploaded/linked videos |
| Stats | Summary | Total videos, total views, total likes |
| Edit/Delete | Actions | Manage own videos |

---

## 3. Feature Specifications

### 3.1 Feature: Video Link Addition
**Description:** Add videos from external sources (YouTube, Vimeo, etc.) by pasting a URL.

**User Stories:**
- As a user, I want to add a YouTube video by pasting its link, so that I can share training content without uploading files.
- As a user, I want the system to auto-generate a thumbnail from the video, so that I don't need to create one manually.

**Acceptance Criteria:**
- Given a user pastes a YouTube URL
- When they submit the form
- Then the video is added with metadata fetched from the source (title, thumbnail, duration)

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| URL | Must be valid YouTube/Vimeo/external video URL | "Please enter a supported video URL" |
| Supported Sources | YouTube, Vimeo, Google Drive direct link | "Video source not supported" |

**Edge Cases:**
- If the external video is deleted or made private, the system shows an error on playback.
- Auto-fetching metadata may fail for some sources; manual input is fallback.

### 3.2 Feature: Video Upload
**Description:** Direct upload of video files to the platform.

**User Stories:**
- As a user, I want to upload an MP4 recording of a meeting, so that it's stored within our platform.

**Acceptance Criteria:**
- Given a user uploads an MP4 file
- When the upload completes
- Then the video is processed, a thumbnail is generated, and it appears in the library

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| File Type | mp4, webm | "Video format not supported" |
| File Size | Max 500MB | "File exceeds maximum size of 500MB" |
| Duration | Max 4 hours | "Video exceeds maximum duration of 4 hours" |

**Edge Cases:**
- Large uploads use resumable upload protocol to handle connection interruptions.
- Video transcoding occurs asynchronously; video is available after processing completes.

### 3.3 Feature: View Tracking
**Description:** Track and display video view counts.

**User Stories:**
- As a content creator, I want to see how many times my video has been viewed, so that I know its impact.

**Acceptance Criteria:**
- Given a user plays a video
- When the video starts playing
- Then the view count increments (deduplicated per user per 24 hours)

**Edge Cases:**
- Views are deduplicated: one view per user per video per 24-hour period.
- Bot/crawler views are excluded using user agent detection.

### 3.4 Feature: Video Categories
**Description:** Organize videos into categories for structured browsing.

**User Stories:**
- As a category manager, I want to create categories like "Training" and "Product Demos", so that users can find videos easily.

**Acceptance Criteria:**
- Given a manager creates a "Training" category
- When videos are assigned to it
- Then filtering by "Training" shows only those videos

**Edge Cases:**
- Videos can belong to multiple categories.
- Deleting a category unassigns videos but doesn't delete them.

### 3.5 Feature: Video Comments and Likes
**Description:** Engagement features for video content.

**User Stories:**
- As a viewer, I want to comment on a video, so that I can discuss the content.
- As a viewer, I want to like a video, so that I can show appreciation.

**Acceptance Criteria:**
- Given a video is playing
- When a user clicks Like
- Then the like count increments for that video

**Edge Cases:**
- Comments can be deleted by the author or admin.
- Deleted comments show as "Comment deleted" to preserve thread.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|     Video      |       |  VideoCategory |       | VideoComment   |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |<----->| id (PK)        |
| title          |       | name           |       | video_id (FK)  |
| source_url     |       | description    |       | user_id (FK)   |
| thumbnail_url  |       | created_at     |       | content        |
| description    |       +----------------+       | parent_id (FK) |
| source_type    |                                | created_at     |
| view_count     |       +----------------+       +----------------+
| like_count     |       |VideoCategory |
| uploader(FK)   |       |   Mapping    |
| created_at     |       +----------------+
+----------------+       | video_id (FK)|
                         | category(FK) |
                         +----------------+
```

### 4.2 Table Schemas

#### Table: videos
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| title | VARCHAR(300) | NOT NULL | Video title |
| source_url | VARCHAR(1000) | NOT NULL | Video URL or storage path |
| source_type | ENUM('youtube','vimeo','upload','gdrive','other') | NOT NULL | Video source |
| thumbnail_url | VARCHAR(500) | NULL | Thumbnail image URL |
| description | TEXT | NULL | Video description |
| duration_seconds | INT | NULL | Video duration in seconds |
| file_size | BIGINT | NULL | File size in bytes (for uploads) |
| uploader_id | BIGINT | NOT NULL, FK -> users.id | Video uploader |
| view_count | INT | NOT NULL, DEFAULT 0 | Total views |
| like_count | INT | NOT NULL, DEFAULT 0 | Total likes |
| comment_count | INT | NOT NULL, DEFAULT 0 | Total comments |
| is_public | BOOLEAN | NOT NULL, DEFAULT TRUE | Visibility |
| status | ENUM('processing','available','error','deleted') | NOT NULL, DEFAULT 'processing' | Video status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_videos_uploader_id ON videos(uploader_id);
CREATE INDEX idx_videos_source_type ON videos(source_type);
CREATE INDEX idx_videos_view_count ON videos(view_count DESC);
CREATE INDEX idx_videos_like_count ON videos(like_count DESC);
CREATE INDEX idx_videos_created_at ON videos(created_at DESC);
CREATE INDEX idx_videos_is_public ON videos(is_public, status);
```

#### Table: video_categories
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL, UNIQUE | Category name |
| description | TEXT | NULL | Category description |
| icon | VARCHAR(50) | NULL | Icon identifier |
| video_count | INT | NOT NULL, DEFAULT 0 | Video count |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_video_categories_name ON video_categories(name);
```

#### Table: video_video_category
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| video_id | BIGINT | NOT NULL, FK -> videos.id | Video |
| category_id | BIGINT | NOT NULL, FK -> video_categories.id | Category |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_vvc_video_category ON video_video_category(video_id, category_id);
CREATE INDEX idx_vvc_category_id ON video_video_category(category_id);
```

#### Table: video_comments
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| video_id | BIGINT | NOT NULL, FK -> videos.id | Target video |
| user_id | BIGINT | NOT NULL, FK -> users.id | Comment author |
| parent_id | BIGINT | NULL, FK -> video_comments.id | Reply-to comment |
| content | TEXT | NOT NULL | Comment text |
| is_deleted | BOOLEAN | NOT NULL, DEFAULT FALSE | Deletion flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_video_comments_video_id ON video_comments(video_id);
CREATE INDEX idx_video_comments_user_id ON video_comments(user_id);
CREATE INDEX idx_video_comments_parent_id ON video_comments(parent_id);
CREATE INDEX idx_video_comments_created_at ON video_comments(created_at);
```

#### Table: video_likes
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| video_id | BIGINT | NOT NULL, FK -> videos.id | Target video |
| user_id | BIGINT | NOT NULL, FK -> users.id | Liking user |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Like timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_video_likes_video_user ON video_likes(video_id, user_id);
CREATE INDEX idx_video_likes_video_id ON video_likes(video_id);
```

#### Table: video_views
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| video_id | BIGINT | NOT NULL, FK -> videos.id | Target video |
| user_id | BIGINT | NOT NULL, FK -> users.id | Viewing user |
| viewed_at | DATE | NOT NULL | View date (for dedup) |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | View timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_video_views_video_user_date ON video_views(video_id, user_id, viewed_at);
CREATE INDEX idx_video_views_video_id ON video_views(video_id);
```

#### Table: video_playlists
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Playlist name |
| description | TEXT | NULL | Playlist description |
| user_id | BIGINT | NOT NULL, FK -> users.id | Playlist owner |
| is_public | BOOLEAN | NOT NULL, DEFAULT FALSE | Visibility |
| video_count | INT | NOT NULL, DEFAULT 0 | Video count |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_video_playlists_user_id ON video_playlists(user_id);
```

#### Table: video_playlist_videos
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| playlist_id | BIGINT | NOT NULL, FK -> video_playlists.id | Playlist |
| video_id | BIGINT | NOT NULL, FK -> videos.id | Video |
| sort_order | INT | NOT NULL, DEFAULT 0 | Display order |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_vpv_playlist_video ON video_playlist_videos(playlist_id, video_id);
```

#### Table: video_sources
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL, UNIQUE | Source name (YouTube, Vimeo, etc.) |
| embed_template | VARCHAR(500) | NULL | URL template for embedding |
| api_endpoint | VARCHAR(500) | NULL | API endpoint for metadata |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_video_sources_name ON video_sources(name);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| videos | Video records | 5,000 |
| video_categories | Category definitions | 50 |
| video_video_category | Video-category mapping | 10,000 |
| video_comments | Video comments | 30,000 |
| video_likes | Video likes | 50,000 |
| video_views | View tracking (deduplicated) | 200,000 |
| video_playlists | User playlists | 2,000 |
| video_playlist_videos | Playlist-video mapping | 10,000 |
| video_sources | Supported video sources | 10 |

---

## 5. API Specifications

### 5.1 Video Endpoints

#### GET /api/v1/videos
**Description:** List videos with filtering and pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page (default: 20) |
| category_id | bigint | No | Filter by category |
| source_type | string | No | Filter by source |
| q | string | No | Search query |
| sort | string | No | created_at, view_count, like_count |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "title": "Employee Onboarding Training",
      "source_type": "youtube",
      "source_url": "https://youtube.com/watch?v=...",
      "thumbnail_url": "https://...",
      "description": "Complete onboarding guide for new employees",
      "duration_seconds": 1800,
      "view_count": 150,
      "like_count": 23,
      "comment_count": 8,
      "uploader": {"id": 10, "name": "HR Department"},
      "created_at": "2024-04-14T08:00:00Z"
    }
  ],
  "meta": {"total": 200, "page": 1, "limit": 20}
}
```

**Permission:** `video:read`

#### POST /api/v1/videos
**Description:** Add a new video (link or upload)

**Request Body (link):**
```json
{
  "title": "Q3 Company Meeting Recording",
  "source_url": "https://youtube.com/watch?v=abc123",
  "source_type": "youtube",
  "description": "Recording of Q3 all-hands meeting",
  "category_ids": [1, 3]
}
```

**Response (201 Created):**
```json
{
  "id": 201,
  "title": "Q3 Company Meeting Recording",
  "source_type": "youtube",
  "view_count": 0,
  "created_at": "2024-04-14T09:00:00Z"
}
```

**Permission:** `video:create`

#### POST /api/v1/videos/{id}/view
**Description:** Record a view

**Response (200 OK):**
```json
{
  "view_count": 151
}
```

**Permission:** `video:view`

#### POST /api/v1/videos/{id}/like
**Description:** Toggle like

**Response (200 OK):**
```json
{
  "has_liked": true,
  "like_count": 24
}
```

**Permission:** `video:like`

### 5.2 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/videos | List videos | video:read |
| POST | /api/v1/videos | Add video | video:create |
| GET | /api/v1/videos/{id} | Get video detail | video:read |
| PUT | /api/v1/videos/{id} | Update video | video:update |
| DELETE | /api/v1/videos/{id} | Delete video | video:delete |
| POST | /api/v1/videos/{id}/view | Record view | video:view |
| POST | /api/v1/videos/{id}/like | Toggle like | video:like |
| GET | /api/v1/videos/{id}/comments | List comments | video:comment:read |
| POST | /api/v1/videos/{id}/comments | Add comment | video:comment:create |

---

## 6. Permissions Matrix

### Roles
| Role | View Videos | Add Videos | Edit Videos | Delete Videos | Manage Categories |
|------|-----------|-----------|------------|--------------|------------------|
| Employee | ✅ (public) | ✅ | ✅ (own) | ✅ (own) | ❌ |
| Video Contributor | ✅ | ✅ | ✅ (own) | ✅ (own) | ❌ |
| Category Manager | ✅ | ✅ | ✅ (all) | ✅ (all) | ✅ |
| System Admin | ✅ | ✅ | ✅ (all) | ✅ (all) | ✅ |

### Row-Level Security
- Private videos visible only to uploader and explicitly shared users.
- Deleted videos hidden from listings but accessible to admin during retention period.

---

## 7. State Machine / Workflows

### 7.1 Video Status Transitions

```
[PROCESSING] ──complete──→ [AVAILABLE] ──delete──→ [DELETED]
     │
     └──error──→ [ERROR]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| PROCESSING | AVAILABLE | processing complete | System | Video becomes playable, appears in listings |
| PROCESSING | ERROR | processing failed | System | Notify uploader, show error |
| AVAILABLE | DELETED | delete | Owner or Admin | Hide from listings, start retention timer |

---

## 8. Reports & Analytics

### 8.1 Report: Video Engagement Report
**Type:** Real-time
**Purpose:** Track video view and engagement metrics.

**Metrics:**
- Total views: 150 (sample)
- Total likes
- Total comments
- Most viewed videos
- Average watch time

**Chart Type:** Bar chart, table

### 8.2 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Video Engagement | Real-time | On-demand | Bar chart, table |
| Category Usage | Real-time | On-demand | Pie chart |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| YouTube Data API | Fetch video metadata | REST | API Key |
| Vimeo API | Fetch video metadata | REST | OAuth2 |
| Video Processing (FFmpeg) | Transcode uploaded videos | Local | N/A |
| CDN (CloudFront) | Serve video files | REST | IAM Role |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| video.created | {video_id, title, uploader_id} | M28 Knowledge, M39 Dashboard |
| video.viewed | {video_id, user_id, view_count} | M39 Dashboard |

---

## 10. Technical Notes

### Performance
- Expected data volume: 5,000+ videos, 200,000+ views
- Query complexity: Low for list, Medium for engagement analytics
- Expected response time: <200ms for list views
- Video streaming via CDN for optimal playback performance

### Caching Strategy
- Video metadata cached with 10-minute TTL
- View counts cached with 1-minute TTL, flushed to DB in batches
- Category listings cached with 1-hour TTL
- Cache invalidation on video create/update/delete

### File Storage
- Uploaded videos: S3 storage with CloudFront CDN
- Thumbnails: S3 with auto-generation from video frame at 10% mark
- Max upload: 500MB, 4 hours duration
- External videos: stored as URLs, embedded via iframe

### Mobile Considerations
- Responsive video player for mobile screens
- Adaptive bitrate streaming for variable network conditions
- Offline download capability for uploaded videos
- Mobile app icon visible in marketplace (image_06, image_07)
