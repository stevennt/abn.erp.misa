# M27 - Photo Library (Kho anh)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_24.png, assets/image_72.png
**Dependencies:** M01 (User & Authentication), M23 (Social Network)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Photo Library module provides a comprehensive photo storage, organization, and social engagement platform for the organization. It enables users to upload, store, and browse photos in albums, view photos in a full-screen viewer with navigation, and engage with photos through likes, comments, and shares. The module supports social features similar to the Social Network module but focused specifically on photo content, creating a company photo gallery for team building events, office activities, product photos, and memories. With mobile app support (img_72) and a rich full-screen viewer (img_24) showing engagement metrics (17 likes, 34 comments, 5 shares on sample photos), the module serves as both a photo archive and a social engagement platform. Photos can be tagged, organized into albums, and shared across the organization with privacy controls.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Photo Viewer | Any employee who browses and engages with photos | Read + engage |
| Photo Uploader | Users who upload and manage photos | Create + edit (own) |
| Album Manager | Creates and manages photo albums | Full album management |
| System Administrator | Configures storage limits and global settings | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication)
- **Used by:** M23 (Social Network), M39 (Operations Dashboard)

### Key Business Rules
1. Photos can be uploaded individually or in batches (up to 50 photos per upload session).
2. Each photo has metadata: title, description, uploader, upload date, album assignment, tags.
3. Photos are organized into albums for structured browsing; a photo can belong to multiple albums.
4. Full-screen photo viewer with previous/next navigation arrows for browsing within an album or photo set (img_24).
5. Engagement tracking per photo: likes (one per user), comments (with replies), and shares.
6. Sample photos show engagement: 17 likes, 34 comments, 5 shares (img_24).
7. Photo tags are user-applied labels for discoverability; tags are shared across all users.
8. Privacy settings: Public (visible to all), Department (visible to uploader's department), Private (visible only to uploader and shared users).
9. Photo metadata includes EXIF data (camera, date taken, location) when available.
10. Supported upload formats: JPG, PNG, GIF, WEBP with max 20MB per photo.
11. Photos can be shared to the Social Network feed (M23) for broader visibility.
12. Mobile app support with photo upload from camera roll and in-app photo viewer (img_72).

---

## 2. Screen Inventory

### 2.1 Screen: Photo Library Grid
**Route:** `/photos`
**Purpose:** Main photo browsing interface with album navigation (img_24).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Upload Button | CTA | Upload new photos |
| Album Tabs | Tab bar | All, My Albums, Shared, Team Building, Events, custom albums |
| Photo Grid | Masonry grid | Photos in varying sizes, thumbnail display |
| Search Bar | Input | Search by title, description, tags |
| Sort Options | Dropdown | Sort by: newest, most liked, most commented |
| View Toggle | Toggle | Grid view / List view |

**User Interactions:**
- Browse photos in grid view.
- Filter by album.
- Search by metadata.
- Click a photo to open full-screen viewer.
- Upload new photos.

### 2.2 Screen: Full-Screen Photo Viewer
**Route:** `/photos/{id}/view`
**Purpose:** Full-screen photo display with navigation and engagement (img_24).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Full-Screen Image | Full viewport | Photo displayed at maximum size |
| Navigation Arrows | Left/Right buttons | Previous/Next photo in album |
| Photo Details Panel | Side panel | Uploader, date, description, tags, album |
| Engagement Bar | Action buttons | Like (17), Comment (34), Share (5) |
| Close Button | Icon | Exit full-screen view |
| Zoom Controls | Buttons | Zoom in/out, fit to screen |

**User Interactions:**
- Navigate between photos with arrows.
- Zoom in/out for detail viewing.
- Like, comment on, and share the photo.
- View photo metadata and details.
- Close to return to grid view.

**Data Displayed:**
- Sample engagement: 17 likes, 34 comments, 5 shares
- Uploader information and upload date
- Photo description and tags
- Album membership

### 2.3 Screen: Photo Upload Modal
**Route:** Modal from photo library
**Purpose:** Upload photos with metadata assignment.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| File Drop Zone | Drag-and-drop | Drop photos or click to browse (max 50) |
| Photo Preview Grid | Grid | Uploaded photos with thumbnails |
| Title Input | Text field | Per-photo title (auto-filled from filename) |
| Description | Textarea | Per-photo description |
| Album Selector | Multi-select | Assign to albums |
| Tags Input | Multi-tag | Add tags |
| Privacy Selector | Dropdown | Public, Department, Private |
| Progress Bar | Indicator | Upload progress for batch uploads |
| Upload Button | CTA | Complete upload |

**User Interactions:**
- Drag-and-drop or browse for photos.
- Fill in metadata for each photo or apply bulk settings.
- Assign to albums and add tags.
- Set privacy level.
- Monitor upload progress.

### 2.4 Screen: Album Management
**Route:** `/photos/albums`
**Purpose:** Create and manage photo albums.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Album Grid | Card grid | Album cover, name, photo count, creator, date |
| Create Album | Button | Create new album |
| Album Detail | Card | Album name, description, photo grid, settings |
| Add Photos | Multi-select | Add existing photos to album |
| Remove Photos | Action | Remove photos from album |

**User Interactions:**
- Create new albums with cover photos.
- Add and remove photos from albums.
- Edit album metadata.
- Delete albums.

### 2.5 Screen: Photo Detail View
**Route:** `/photos/{id}`
**Purpose:** Photo information with all engagement data.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Photo Display | Large image | Photo with zoom capability |
| Metadata | Card | Title, description, uploader, date, EXIF data |
| Engagement Bar | Actions | Like, Comment, Share with counts |
| Comments Section | Thread | User comments with replies |
| Liked By List | Modal | Users who liked the photo |
| Tags | Badge list | Photo tags |
| Album Badges | Badge list | Albums containing this photo |

### 2.6 Screen: Mobile Photo Library
**Route:** Mobile app view (img_72)
**Purpose:** Mobile-optimized photo browsing and upload.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Photo Grid | Responsive grid | Thumbnail grid optimized for mobile |
| Camera Button | FAB | Quick upload from camera |
| Full-Screen Viewer | Swipeable | Swipe left/right to navigate photos |
| Engagement Actions | Bottom bar | Like, comment, share buttons |

**User Interactions:**
- Browse photos with swipe navigation.
- Upload photos directly from camera.
- Like, comment, and share on mobile.

---

## 3. Feature Specifications

### 3.1 Feature: Photo Upload and Batch Processing
**Description:** Upload individual photos or batches of up to 50 photos with metadata.

**User Stories:**
- As a user, I want to upload multiple photos at once, so that I can quickly add event photos.
- As a user, I want to assign albums and tags during upload, so that photos are organized immediately.

**Acceptance Criteria:**
- Given a user drops 20 photos on the upload zone
- When the upload completes
- Then all 20 photos appear in the library with their assigned metadata

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| File Type | jpg, png, gif, webp | "Photo format not supported" |
| File Size | Max 20MB per photo | "Photo exceeds maximum size of 20MB" |
| Batch Size | Max 50 photos per upload | "Maximum 50 photos per upload session" |

**Edge Cases:**
- Failed individual uploads in a batch don't affect successful ones.
- EXIF orientation is auto-corrected during upload.

### 3.2 Feature: Full-Screen Photo Viewer
**Description:** Immersive full-screen photo viewing with navigation (img_24).

**User Stories:**
- As a user, I want to view photos in full-screen, so that I can see all the details.
- As a user, I want to navigate between photos with arrows, so that I can browse an album efficiently.

**Acceptance Criteria:**
- Given a user clicks a photo in the grid
- When the full-screen viewer opens
- Then the photo fills the viewport with navigation arrows visible

**Edge Cases:**
- High-resolution photos are served at an optimized resolution based on viewport size.
- Keyboard navigation (left/right arrows) supported on desktop.

### 3.3 Feature: Photo Engagement (Like, Comment, Share)
**Description:** Social engagement features for photos.

**User Stories:**
- As a user, I want to like a photo, so that I can show appreciation.
- As a user, I want to comment on a photo, so that I can engage with the content.
- As a user, I want to share a photo, so that others can see it.

**Acceptance Criteria:**
- Given a photo is displayed in the viewer
- When a user clicks Like
- Then the like count increments and the button highlights

**Engagement Metrics:**
- Likes: 17 (sample)
- Comments: 34 (sample)
- Shares: 5 (sample)

**Edge Cases:**
- Shares can include an optional comment.
- Shared photos link back to the original in the library.

### 3.4 Feature: Photo Tagging
**Description:** Tag photos for discoverability and organization.

**User Stories:**
- As a user, I want to tag photos with relevant keywords, so that others can find them through tag browsing.
- As a user, I want to tag people in photos, so that they are notified and can find photos of themselves.

**Acceptance Criteria:**
- Given a user adds tags to a photo
- When another user browses by tag
- Then the tagged photo appears in the tag filter results

**Edge Cases:**
- Tags are deduplicated across the system.
- Tag suggestions appear as user types.

### 3.5 Feature: Album Organization
**Description:** Organize photos into albums for structured browsing.

**User Stories:**
- As a user, I want to create an album for our team building event, so that all event photos are grouped together.

**Acceptance Criteria:**
- Given a user creates an album "Team Building 2024"
- When they add photos to it
- Then browsing the album shows only those photos

**Edge Cases:**
- A photo can belong to multiple albums.
- Deleting an album doesn't delete its photos.

### 3.6 Feature: Photo Privacy Controls
**Description:** Control who can view each photo.

**User Stories:**
- As a user, I want to mark some photos as private, so that only specific people can see them.

**Acceptance Criteria:**
- Given a photo is marked Private
- When a non-authorized user browses the library
- Then the photo does not appear in their view

**Privacy Levels:**
- Public: Visible to all employees
- Department: Visible to uploader's department
- Private: Visible only to uploader and explicitly shared users

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|     Photo      |       |   PhotoAlbum   |       | PhotoComment   |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |<----->| id (PK)        |
| title          |       | name           |       | photo_id (FK)  |
| description    |       | description    |       | user_id (FK)   |
| file_url       |       | cover_photo_id |       | content        |
| thumbnail_url  |       | creator_id(FK) |       | parent_id (FK) |
| uploader(FK)   |       | created_at     |       | created_at     |
| like_count     |       +----------------+       +----------------+
| comment_count  |
| share_count    |       +----------------+
| privacy_level  |       |PhotoAlbumPhoto |
| created_at     |       +----------------+
+----------------+       | photo_id (FK)|
                         | album_id(FK) |
                         +----------------+
```

### 4.2 Table Schemas

#### Table: photos
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| title | VARCHAR(300) | NOT NULL | Photo title |
| description | TEXT | NULL | Photo description |
| file_url | VARCHAR(500) | NOT NULL | Full-size photo URL |
| thumbnail_url | VARCHAR(500) | NOT NULL | Thumbnail URL |
| medium_url | VARCHAR(500) | NULL | Medium-size URL (for viewer) |
| uploader_id | BIGINT | NOT NULL, FK -> users.id | Photo uploader |
| file_type | VARCHAR(20) | NOT NULL | MIME type |
| file_size | BIGINT | NOT NULL | File size in bytes |
| width | INT | NULL | Image width in pixels |
| height | INT | NULL | Image height in pixels |
| like_count | INT | NOT NULL, DEFAULT 0 | Total likes |
| comment_count | INT | NOT NULL, DEFAULT 0 | Total comments |
| share_count | INT | NOT NULL, DEFAULT 0 | Total shares |
| view_count | INT | NOT NULL, DEFAULT 0 | Total views |
| privacy_level | ENUM('public','department','private') | NOT NULL, DEFAULT 'public' | Privacy level |
| exif_data | JSON | NULL | EXIF metadata (camera, date, location) |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Upload timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_photos_uploader_id ON photos(uploader_id);
CREATE INDEX idx_photos_like_count ON photos(like_count DESC);
CREATE INDEX idx_photos_comment_count ON photos(comment_count DESC);
CREATE INDEX idx_photos_created_at ON photos(created_at DESC);
CREATE INDEX idx_photos_privacy ON photos(privacy_level);
CREATE INDEX idx_photos_file_type ON photos(file_type);
```

#### Table: photo_albums
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Album name |
| description | TEXT | NULL | Album description |
| cover_photo_id | BIGINT | NULL, FK -> photos.id | Cover photo |
| creator_id | BIGINT | NOT NULL, FK -> users.id | Album creator |
| photo_count | INT | NOT NULL, DEFAULT 0 | Photo count |
| is_public | BOOLEAN | NOT NULL, DEFAULT TRUE | Album visibility |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_photo_albums_creator ON photo_albums(creator_id);
CREATE INDEX idx_photo_albums_created ON photo_albums(created_at DESC);
```

#### Table: photo_album_photos
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| photo_id | BIGINT | NOT NULL, FK -> photos.id | Photo |
| album_id | BIGINT | NOT NULL, FK -> photo_albums.id | Album |
| sort_order | INT | NOT NULL, DEFAULT 0 | Display order in album |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_pap_photo_album ON photo_album_photos(photo_id, album_id);
CREATE INDEX idx_pap_album_id ON photo_album_photos(album_id);
```

#### Table: photo_comments
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| photo_id | BIGINT | NOT NULL, FK -> photos.id | Target photo |
| user_id | BIGINT | NOT NULL, FK -> users.id | Comment author |
| parent_id | BIGINT | NULL, FK -> photo_comments.id | Reply-to comment |
| content | TEXT | NOT NULL | Comment text |
| is_deleted | BOOLEAN | NOT NULL, DEFAULT FALSE | Deletion flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_photo_comments_photo_id ON photo_comments(photo_id);
CREATE INDEX idx_photo_comments_user_id ON photo_comments(user_id);
CREATE INDEX idx_photo_comments_parent_id ON photo_comments(parent_id);
```

#### Table: photo_likes
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| photo_id | BIGINT | NOT NULL, FK -> photos.id | Target photo |
| user_id | BIGINT | NOT NULL, FK -> users.id | Liking user |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Like timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_photo_likes_photo_user ON photo_likes(photo_id, user_id);
CREATE INDEX idx_photo_likes_photo_id ON photo_likes(photo_id);
```

#### Table: photo_shares
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| photo_id | BIGINT | NOT NULL, FK -> photos.id | Target photo |
| user_id | BIGINT | NOT NULL, FK -> users.id | Sharing user |
| share_comment | TEXT | NULL | Comment with share |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Share timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_photo_shares_photo_user ON photo_shares(photo_id, user_id);
```

#### Table: photo_tags
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL, UNIQUE | Tag name |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Tag creation time |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_photo_tags_name ON photo_tags(name);
```

#### Table: photo_photo_tag
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| photo_id | BIGINT | NOT NULL, FK -> photos.id | Photo |
| tag_id | BIGINT | NOT NULL, FK -> photo_tags.id | Tag |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_ppt_photo_tag ON photo_photo_tag(photo_id, tag_id);
```

#### Table: photo_metadata
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| photo_id | BIGINT | NOT NULL, UNIQUE, FK -> photos.id | Target photo |
| camera_make | VARCHAR(100) | NULL | Camera manufacturer |
| camera_model | VARCHAR(100) | NULL | Camera model |
| date_taken | TIMESTAMP | NULL | When photo was taken |
| gps_latitude | DECIMAL(10,8) | NULL | GPS latitude |
| gps_longitude | DECIMAL(11,8) | NULL | GPS longitude |
| aperture | VARCHAR(20) | NULL | Aperture setting |
| shutter_speed | VARCHAR(20) | NULL | Shutter speed |
| iso | INT | NULL | ISO setting |
| focal_length | VARCHAR(20) | NULL | Focal length |

**Indexes:**
```sql
CREATE INDEX idx_photo_metadata_date_taken ON photo_metadata(date_taken);
```

#### Table: photo_privacy
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| photo_id | BIGINT | NOT NULL, FK -> photos.id | Target photo |
| user_id | BIGINT | NOT NULL, FK -> users.id | Granted user |
| granted_by | BIGINT | NOT NULL, FK -> users.id | Granting user |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Grant timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_photo_privacy_photo_user ON photo_privacy(photo_id, user_id);
CREATE INDEX idx_photo_privacy_user_id ON photo_privacy(user_id);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| photos | Photo records | 50,000 |
| photo_albums | Album definitions | 2,000 |
| photo_album_photos | Album-photo mapping | 150,000 |
| photo_comments | Photo comments | 100,000 |
| photo_likes | Photo likes | 200,000 |
| photo_shares | Photo shares | 30,000 |
| photo_tags | Tag definitions | 2,000 |
| photo_photo_tag | Photo-tag mapping | 100,000 |
| photo_metadata | EXIF data | 50,000 |
| photo_privacy | Private access grants | 5,000 |

---

## 5. API Specifications

### 5.1 Photo Endpoints

#### GET /api/v1/photos
**Description:** List photos with filtering and pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page (default: 30) |
| album_id | bigint | No | Filter by album |
| tag | string | No | Filter by tag |
| q | string | No | Search query |
| sort | string | No | created_at, like_count, comment_count |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "title": "Team Building Beach Photo",
      "thumbnail_url": "https://.../thumb1.jpg",
      "uploader": {"id": 10, "name": "Dang Thuy Anh"},
      "like_count": 17,
      "comment_count": 34,
      "share_count": 5,
      "created_at": "2024-04-14T10:00:00Z"
    }
  ],
  "meta": {"total": 5000, "page": 1, "limit": 30}
}
```

**Permission:** `photo:read`

#### POST /api/v1/photos
**Description:** Upload photos (multipart/form-data)

**Permission:** `photo:create`

#### GET /api/v1/photos/{id}
**Description:** Get photo detail with comments and metadata

**Permission:** `photo:read`

#### POST /api/v1/photos/{id}/like
**Description:** Toggle like

**Response (200 OK):**
```json
{
  "has_liked": true,
  "like_count": 18
}
```

**Permission:** `photo:like`

### 5.2 Album Endpoints

#### GET /api/v1/photo-albums
**Description:** List albums

**Permission:** `photo:album:read`

#### POST /api/v1/photo-albums
**Description:** Create album

**Permission:** `photo:album:create`

#### POST /api/v1/photo-albums/{id}/photos
**Description:** Add photos to album

**Permission:** `photo:album:update`

### 5.3 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/photos | List photos | photo:read |
| POST | /api/v1/photos | Upload photos | photo:create |
| GET | /api/v1/photos/{id} | Get photo | photo:read |
| PUT | /api/v1/photos/{id} | Update photo | photo:update |
| DELETE | /api/v1/photos/{id} | Delete photo | photo:delete |
| POST | /api/v1/photos/{id}/like | Toggle like | photo:like |
| POST | /api/v1/photos/{id}/share | Share photo | photo:share |
| GET | /api/v1/photo-albums | List albums | photo:album:read |
| POST | /api/v1/photo-albums | Create album | photo:album:create |
| POST | /api/v1/photo-albums/{id}/photos | Add photos | photo:album:update |

---

## 6. Permissions Matrix

### Roles
| Role | View Photos | Upload Photos | Edit Photos | Delete Photos | Manage Albums |
|------|-----------|--------------|------------|--------------|--------------|
| Employee | ✅ (public) | ✅ | ✅ (own) | ✅ (own) | ✅ |
| Album Manager | ✅ | ✅ | ✅ (album) | ✅ (album) | ✅ |
| System Admin | ✅ | ✅ | ✅ (all) | ✅ (all) | ✅ |

### Row-Level Security
- Private photos visible only to uploader and explicitly granted users.
- Department-scoped photos visible only to members of the uploader's department.

---

## 7. State Machine / Workflows

### 7.1 Photo Processing Status

```
[UPLOADING] ──process──→ [PROCESSING] ──complete──→ [AVAILABLE]
                                                        │
                                                        └──delete──→ [DELETED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| UPLOADING | PROCESSING | upload complete | System | Generate thumbnails, extract EXIF |
| PROCESSING | AVAILABLE | processing done | System | Photo appears in library |
| PROCESSING | ERROR | processing failed | System | Notify uploader |
| AVAILABLE | DELETED | delete | Owner or Admin | Hide from listings |

---

## 8. Reports & Analytics

### 8.1 Report: Photo Engagement Report
**Type:** Real-time
**Purpose:** Track photo engagement metrics.

**Metrics:**
- Total photos uploaded
- Total likes (17 on sample)
- Total comments (34 on sample)
- Total shares (5 on sample)
- Most viewed photos
- Most active albums

**Chart Type:** Bar chart, table

### 8.2 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Photo Engagement | Real-time | On-demand | Bar chart, table |
| Album Usage | Real-time | On-demand | Pie chart |
| Upload Trends | Real-time | On-demand | Line chart |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Image Processing (Sharp) | Thumbnail generation, optimization | Local | N/A |
| CDN (CloudFront) | Serve photo files | REST | IAM Role |
| M23 Social Network | Share photos to social feed | REST | JWT |
| M01 User Service | Resolve user profiles | REST | JWT |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| photo.uploaded | {photo_id, album_id, uploader_id} | M23 Social Network |
| photo.liked | {photo_id, user_id, liker_id} | M01 Notification |
| photo.commented | {photo_id, comment_id, user_id} | M01 Notification |
| photo.shared | {photo_id, user_id} | M23 Social Network |

---

## 10. Technical Notes

### Performance
- Expected data volume: 50,000+ photos, total storage 1TB+
- Query complexity: Low for list, Medium for album aggregation
- Expected response time: <200ms for grid view, <100ms for single photo
- Photo serving via CDN with lazy loading and progressive JPEG

### Caching Strategy
- Photo thumbnails cached at CDN edge with 1-hour TTL
- Photo metadata cached with 10-minute TTL (Redis)
- Engagement counts cached with 30-second TTL, batched flush to DB
- Cache invalidation on photo upload, like, comment, or delete

### Image Processing
- Three sizes generated: thumbnail (200px), medium (800px), full (original)
- Automatic EXIF orientation correction
- WebP conversion for supported browsers with JPEG fallback
- Image optimization (compression) without visible quality loss
- Faces detection for smart cropping in mobile view

### Mobile Considerations
- Swipe navigation between photos in full-screen viewer
- Camera integration for direct photo upload
- Touch-friendly like/comment/share buttons
- Offline caching of recently viewed photos
- Responsive grid layout with adaptive column count
- Mobile app icon visible in marketplace (image_06, image_07, image_72)

### Storage
- Photos stored in S3 with lifecycle policy (standard → infrequent access after 90 days)
- Thumbnails stored separately for faster CDN serving
- Backup policy: daily incremental, weekly full backup
- Storage cost optimization: deduplication by content hash
