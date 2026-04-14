# M21 - Document Management (Tai lieu)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_18.png, assets/image_57.png
**Dependencies:** M01 (User & Authentication), M02 (Roles & Permissions), M03 (Company & Organization)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Document Management module provides a hierarchical, category-based system for organizing, storing, versioning, and sharing company documents including policies, procedures, handbooks, QA documents, and standard operating procedures. It serves as the central repository for all organizational knowledge with fine-grained access control, version tracking, commenting, favorites, tagging, and document locking for edit prevention. The module supports PDF, DOCX, DOC, JPG, PNG, and XLSX file formats with full-text search capabilities. Documents are organized in a tree structure with categories such as Management Documents, Process Documents & Regulations, Employee Handbooks, Business Policies, and IT Handbooks, enabling employees to quickly locate and access the information they need.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Document Administrator | Manages document categories, uploads, and permissions | Full |
| Document Author | Creates and edits documents in assigned categories | Create/Edit (own) |
| Document Reviewer | Reviews and approves documents before publication | Read + Comment |
| Regular User | Views and searches published documents | Read-only |
| Department Manager | Manages department-specific document collections | Full (dept-scoped) |
| System Administrator | Configures global settings and storage limits | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication), M02 (Roles & Permissions), M03 (Company & Organization)
- **Used by:** M12 (Payroll), M13 (Time Tracking), M18 (Labor Relations), M22 (Correspondence), M28 (Notes & Knowledge)

### Key Business Rules
1. Documents are organized in a hierarchical category tree; each document belongs to exactly one category.
2. Version numbering follows semantic versioning (1.0, 1.1, 2.0); major versions for significant changes, minor for updates.
3. Only document authors and administrators can upload new versions; previous versions are preserved for audit trail.
4. Document comments are visible to all users with read access to the document.
5. Favorites are user-specific bookmarks for quick document access.
6. Tags are user-created labels for cross-category document discovery; tags are shared across all users.
7. Document locking prevents editing while a document is under review or being updated; only the locker and admins can unlock.
8. Supported file types: PDF, DOCX, DOC, JPG, PNG, XLSX, PPTX with a maximum file size of 50MB.
9. Permission levels per document or category: View, Comment, Edit, Manage (full control including delete).
10. The system tracks 10 documents in the standard view with pagination supporting 10 records per page (img_18).
11. Document metadata includes: name, description, creator, creation date, last editor, last edited date, version number, tags.
12. Soft-deleted documents are retained for 90 days before permanent deletion.

---

## 2. Screen Inventory

### 2.1 Screen: Document Library List
**Route:** `/documents`
**Purpose:** Main document browsing interface with hierarchical category navigation and document listing (img_18).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Breadcrumb | Navigation | All > Process Documents, Regulations > Company Culture |
| Left Sidebar | Tree navigation | Hierarchical category tree with expand/collapse |
| Document Table | Data grid | Name, Description, Creator, Created Date, Last Editor, Last Edited Date |
| Search Bar | Input | Keyword search across document content and metadata |
| Upload Button | CTA | Upload new document to current category |
| Pagination | Footer | Total 10 documents, 10 per page |
| View Toggle | Toggle | List view / Grid view |

**User Interactions:**
- Navigate category tree in left sidebar.
- Click a document to view/download it.
- Upload new documents to the current category.
- Search across all accessible documents.
- Sort by any column.
- Toggle between list and grid view.

**Data Displayed:**
- Documents with metadata: name, description, creator, dates, last editor
- Sample documents: Standard Document (Le Tien Dao), Archive Document (Dang Thai Duong), QA Document (Nguyen Thanh Quyen), Quality Handbook, ISO Draft Document, Customer Complaint Scenarios
- All dated 20/02/2019
- Total count: 10 documents

**Category Tree (from img_18):**
- Favorite documents
- All
- Management documents
- Process Documents, Regulations (expanded)
  - Standard documents
  - Archive documents
  - QA documents
- Sales employee handbook
- Business model documents
- Business policies
- HR handbook
- Employee handbook
- Software dealer list
- Partner contact info
- Market development handbook
- IT handbook
- PR handbook
- Admin block handbook

### 2.2 Screen: Document Detail View
**Route:** `/documents/{id}`
**Purpose:** Full document view with preview, version history, comments, and metadata.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Document Preview | Embedded viewer | PDF/image viewer or download link |
| Document Info | Card | Name, description, category, creator, dates, version |
| Version History | Timeline | All versions with date, uploader, change notes |
| Comments Section | Thread | User comments with timestamps and avatars |
| Tags | Badge list | Clickable tags for related document discovery |
| Action Buttons | Button group | Download, Favorite, Comment, Share, Lock, Edit |
| Related Documents | Card grid | Documents in same category or with shared tags |

**User Interactions:**
- Preview document inline or download.
- View version history and restore previous versions.
- Add comments to the document.
- Add/remove tags.
- Toggle favorite status.
- Lock/unlock document (authors and admins).
- Share document link with colleagues.

### 2.3 Screen: Document Upload Modal
**Route:** Modal from document list
**Purpose:** Upload a new document or new version to a category.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| File Drop Zone | Drag-and-drop | Drag files or click to browse |
| File Preview | Card | File name, type, size, thumbnail |
| Name Input | Text field | Document name (auto-filled from filename) |
| Description | Textarea | Document description |
| Category Selector | Tree dropdown | Target category (defaults to current) |
| Tags Input | Multi-tag input | Add tags for discoverability |
| Version Note | Textarea | Change notes for new versions |
| Permission Settings | Table | Set access levels per role/user |
| Upload Button | CTA | Submit upload |

**User Interactions:**
- Drag-and-drop or browse for files.
- Fill in document metadata.
- Select target category.
- Add tags.
- Set initial permissions.
- Upload file.

### 2.4 Screen: Category Management
**Route:** `/documents/settings/categories`
**Purpose:** Manage the hierarchical category tree structure.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Category Tree | Interactive tree | Drag-and-drop reordering, add/delete |
| Add Category Form | Modal | Name, description, parent category |
| Permission Matrix | Table | Default permissions per category |
| Save Button | CTA | Apply changes |

**User Interactions:**
- Add new categories at any level.
- Reorder categories via drag-and-drop.
- Set default permissions per category.
- Delete empty categories.

### 2.5 Screen: Document Favorites
**Route:** `/documents/favorites`
**Purpose:** Quick access to user's bookmarked documents.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Favorites Grid | Card grid | Document cards with name, category, last accessed |
| Remove Button | Icon | Remove from favorites |
| Empty State | Placeholder | "No favorites yet. Click the star icon on any document." |

### 2.6 Screen: Document Version History
**Route:** `/documents/{id}/versions`
**Purpose:** Complete version audit trail with comparison and restore capabilities.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Version List | Table | Version number, date, uploader, file size, change notes |
| Compare Button | Action | Select two versions to compare |
| Restore Button | Action | Revert to a previous version |
| Download Link | Action | Download specific version |

**User Interactions:**
- View all historical versions.
- Compare two versions side-by-side (for text-based docs).
- Restore a previous version (creates a new version based on old content).
- Download any historical version.

---

## 3. Feature Specifications

### 3.1 Feature: Hierarchical Document Organization
**Description:** Documents organized in a nested category tree with unlimited depth (img_18).

**User Stories:**
- As a document administrator, I want to create nested categories, so that documents are logically organized.
- As a user, I want to browse the category tree, so that I can find documents in the right section.

**Acceptance Criteria:**
- Given an admin is on the category management page
- When they create a subcategory under "Process Documents, Regulations" named "Standard Documents"
- Then the new category appears nested under the parent in the sidebar tree

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Category Name | Required, unique within parent, max 200 chars | "Category name is required and must be unique" |
| Max Depth | No limit on nesting | N/A |

**Edge Cases:**
- Moving a category with documents preserves all document associations.
- Deleting a category with documents requires reassignment or bulk deletion confirmation.

### 3.2 Feature: Document Versioning
**Description:** Automatic version tracking with full history and restore capabilities.

**User Stories:**
- As a document author, I want to upload a new version, so that the document stays current while history is preserved.
- As a user, I want to compare two versions, so that I can see what changed.

**Acceptance Criteria:**
- Given a document exists at version 1.0
- When the author uploads a new version
- Then the document becomes version 1.1 with the old version preserved in history

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| File Type | pdf, docx, doc, jpg, png, xlsx, pptx | "File type not supported" |
| File Size | Max 50MB | "File exceeds maximum size of 50MB" |

**Edge Cases:**
- Concurrent version uploads are serialized; second upload waits for first to complete.
- Restoring a version creates a new version number, not an overwrite.

### 3.3 Feature: Document Permissions
**Description:** Fine-grained access control at document and category level.

**User Stories:**
- As an admin, I want to set different permission levels per category, so that sensitive documents are protected.
- As a user, I want to see only documents I have access to, so that I'm not confused by restricted content.

**Acceptance Criteria:**
- Given a category has "View" permission for HR department only
- When a non-HR user browses documents
- Then they do not see documents in that category

**Edge Cases:**
- Document-level permissions override category-level permissions (more restrictive wins).
- Inherited permissions from parent category apply unless explicitly overridden.

### 3.4 Feature: Document Comments
**Description:** Threaded commenting on documents for collaboration and feedback.

**User Stories:**
- As a reviewer, I want to leave comments on a document, so that I can provide feedback.
- As an author, I want to see all comments on my document, so that I can address feedback.

**Acceptance Criteria:**
- Given a user has read access to a document
- When they type a comment and submit
- Then the comment appears in the comments section with their name and timestamp

**Edge Cases:**
- Comments cannot be edited after 15 minutes; they can be deleted by the author or admin.
- Deleted comments show as "Comment deleted" to maintain thread context.

### 3.5 Feature: Document Locking
**Description:** Lock documents to prevent concurrent editing during review processes.

**User Stories:**
- As an author, I want to lock a document while I'm updating it, so that no one else makes conflicting changes.
- As a reviewer, I want to see if a document is locked, so that I know why I can't edit it.

**Acceptance Criteria:**
- Given a user locks a document
- When another user attempts to edit it
- Then they see a message indicating the document is locked and by whom

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Lock Reason | Optional, max 500 chars | N/A |

**Edge Cases:**
- Auto-unlock after 7 days if locker doesn't release (configurable).
- Admins can force-unlock any document.

### 3.6 Feature: Favorites and Tags
**Description:** Personal bookmarking and community-driven tagging for document discovery.

**User Stories:**
- As a user, I want to favorite frequently accessed documents, so that I can find them quickly.
- As a user, I want to tag documents with relevant keywords, so that others can find them through tag browsing.

**Acceptance Criteria:**
- Given a user clicks the favorite star on a document
- When they navigate to their Favorites page
- Then the document appears in their favorites list

**Edge Cases:**
- Tags are deduplicated; creating an existing tag links to it rather than creating a duplicate.
- Tag suggestions appear as user types based on existing tags.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------------+       +----------------------+       +----------------------+
|     Document         |       |  DocumentCategory    |       |  DocumentVersion     |
+----------------------+       +----------------------+       +----------------------+
| id (PK)              |<----->| id (PK)              |<----->| id (PK)              |
| name                 |       | name                 |       | document_id (FK)     |
| description          |       | parent_id (FK)       |       | version_number       |
| category_id (FK)     |       | sort_order           |       | file_url             |
| creator_id (FK)      |       | created_at           |       | file_size            |
| current_version_id   |       +----------------------+       | uploaded_by (FK)     |
| is_locked            |                                       | change_notes         |
| lock_reason          |                                       | created_at           |
| created_at           |       +----------------------+       +----------------------+
| updated_at           |       | DocumentPermission   |
+----------------------+       +----------------------+
                               | id (PK)              |
                               | document_id (FK)     |
                               | user_id (FK)         |
                               | permission_level     |
                               | created_at           |
                               +----------------------+
```

### 4.2 Table Schemas

#### Table: documents
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(500) | NOT NULL | Document name |
| description | TEXT | NULL | Document description |
| category_id | BIGINT | NOT NULL, FK -> document_categories.id | Parent category |
| creator_id | BIGINT | NOT NULL, FK -> users.id | Document creator |
| current_version_id | BIGINT | NULL, FK -> document_versions.id | Latest version |
| is_locked | BOOLEAN | NOT NULL, DEFAULT FALSE | Lock status |
| lock_reason | VARCHAR(500) | NULL | Reason for lock |
| locked_by | BIGINT | NULL, FK -> users.id | User who locked |
| locked_at | TIMESTAMP | NULL, DEFAULT NULL | Lock timestamp |
| view_count | INT | NOT NULL, DEFAULT 0 | Total views |
| download_count | INT | NOT NULL, DEFAULT 0 | Total downloads |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_documents_category_id ON documents(category_id);
CREATE INDEX idx_documents_creator_id ON documents(creator_id);
CREATE INDEX idx_documents_name ON documents(name);
CREATE INDEX idx_documents_created_at ON documents(created_at);
CREATE INDEX idx_documents_is_locked ON documents(is_locked);
```

#### Table: document_categories
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Category name |
| description | TEXT | NULL | Category description |
| parent_id | BIGINT | NULL, FK -> document_categories.id | Parent category (NULL for root) |
| sort_order | INT | NOT NULL, DEFAULT 0 | Display order among siblings |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_document_categories_parent_id ON document_categories(parent_id);
CREATE INDEX idx_document_categories_sort_order ON document_categories(parent_id, sort_order);
```

#### Table: document_versions
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| document_id | BIGINT | NOT NULL, FK -> documents.id | Parent document |
| version_number | VARCHAR(20) | NOT NULL | Version (e.g., 1.0, 1.1, 2.0) |
| file_url | VARCHAR(500) | NOT NULL | File storage URL |
| file_type | VARCHAR(20) | NOT NULL | MIME type |
| file_size | BIGINT | NOT NULL | File size in bytes |
| uploaded_by | BIGINT | NOT NULL, FK -> users.id | Uploader |
| change_notes | TEXT | NULL | Description of changes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Upload timestamp |

**Indexes:**
```sql
CREATE INDEX idx_document_versions_document_id ON document_versions(document_id);
CREATE INDEX idx_document_versions_version_number ON document_versions(document_id, version_number);
CREATE UNIQUE INDEX idx_document_versions_doc_ver ON document_versions(document_id, version_number);
```

#### Table: document_permissions
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| document_id | BIGINT | NOT NULL, FK -> documents.id | Target document |
| user_id | BIGINT | NULL, FK -> users.id | Specific user (NULL for role-based) |
| role_id | BIGINT | NULL, FK -> roles.id | Role-based permission |
| department_id | BIGINT | NULL, FK -> departments.id | Department-based |
| permission_level | ENUM('view','comment','edit','manage') | NOT NULL | Access level |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_document_permissions_document_id ON document_permissions(document_id);
CREATE INDEX idx_document_permissions_user_id ON document_permissions(user_id);
CREATE INDEX idx_document_permissions_role_id ON document_permissions(role_id);
```

#### Table: document_comments
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| document_id | BIGINT | NOT NULL, FK -> documents.id | Target document |
| user_id | BIGINT | NOT NULL, FK -> users.id | Comment author |
| content | TEXT | NOT NULL | Comment text |
| parent_id | BIGINT | NULL, FK -> document_comments.id | Reply-to comment |
| is_deleted | BOOLEAN | NOT NULL, DEFAULT FALSE | Deletion flag |
| edited_at | TIMESTAMP | NULL, DEFAULT NULL | Edit timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_document_comments_document_id ON document_comments(document_id);
CREATE INDEX idx_document_comments_user_id ON document_comments(user_id);
CREATE INDEX idx_document_comments_created_at ON document_comments(created_at);
```

#### Table: document_favorites
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | NOT NULL, FK -> users.id | User |
| document_id | BIGINT | NOT NULL, FK -> documents.id | Favorited document |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Favorite timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_document_favorites_user_doc ON document_favorites(user_id, document_id);
CREATE INDEX idx_document_favorites_user_id ON document_favorites(user_id);
```

#### Table: document_tags
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL, UNIQUE | Tag name |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Tag creation time |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_document_tags_name ON document_tags(name);
```

#### Table: document_document_tag
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| document_id | BIGINT | NOT NULL, FK -> documents.id | Document |
| tag_id | BIGINT | NOT NULL, FK -> document_tags.id | Tag |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_doc_tag_doc_tag ON document_document_tag(document_id, tag_id);
CREATE INDEX idx_doc_tag_tag_id ON document_document_tag(tag_id);
```

#### Table: document_locks
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| document_id | BIGINT | NOT NULL, FK -> documents.id | Locked document |
| user_id | BIGINT | NOT NULL, FK -> users.id | Locker |
| reason | VARCHAR(500) | NULL | Lock reason |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Lock time |
| released_at | TIMESTAMP | NULL, DEFAULT NULL | Unlock time |

**Indexes:**
```sql
CREATE INDEX idx_document_locks_document_id ON document_locks(document_id);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| documents | Document records | 5,000 |
| document_categories | Category tree | 200 |
| document_versions | Version history | 20,000 |
| document_permissions | Access control | 25,000 |
| document_comments | User comments | 50,000 |
| document_favorites | User bookmarks | 20,000 |
| document_tags | Tag definitions | 1,000 |
| document_document_tag | Document-tag mapping | 20,000 |
| document_locks | Lock history | 5,000 |

---

## 5. API Specifications

### 5.1 Document Endpoints

#### GET /api/v1/documents
**Description:** List documents with filtering and pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| limit | integer | No | Items per page (default: 10, max: 100) |
| category_id | bigint | No | Filter by category |
| q | string | No | Full-text search |
| tag | string | No | Filter by tag |
| creator_id | bigint | No | Filter by creator |
| sort | string | No | Sort field: name, created_at, updated_at |
| order | string | No | asc/desc |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "name": "Standard Document",
      "description": "Company operation information",
      "category": {"id": 5, "name": "Standard documents", "path": "Process Documents > Standard documents"},
      "creator": {"id": 10, "name": "Le Tien Dao"},
      "last_editor": {"id": 10, "name": "Le Tien Dao"},
      "current_version": "1.0",
      "created_at": "2019-02-20T08:00:00Z",
      "updated_at": "2019-02-20T08:00:00Z",
      "is_locked": false,
      "view_count": 150
    }
  ],
  "meta": {
    "total": 10,
    "page": 1,
    "limit": 10
  }
}
```

**Permission:** `document:read`

#### POST /api/v1/documents
**Description:** Upload a new document

**Request Body:** (multipart/form-data)
```
file: [binary]
name: "Quality Handbook v2"
description: "Updated quality procedures"
category_id: 5
tags: ["quality", "procedures", "ISO"]
```

**Response (201 Created):**
```json
{
  "id": 11,
  "name": "Quality Handbook v2",
  "category": {"id": 5, "name": "Standard documents"},
  "current_version": "1.0",
  "created_at": "2024-04-14T09:00:00Z"
}
```

**Permission:** `document:create`

#### GET /api/v1/documents/{id}
**Description:** Get document detail with versions and comments

**Permission:** `document:read`

#### PUT /api/v1/documents/{id}
**Description:** Update document metadata

**Permission:** `document:edit`

#### DELETE /api/v1/documents/{id}
**Description:** Soft delete a document

**Permission:** `document:delete`

#### POST /api/v1/documents/{id}/versions
**Description:** Upload a new version

**Permission:** `document:edit`

### 5.2 Category Endpoints

#### GET /api/v1/document-categories
**Description:** Get full category tree

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "name": "Process Documents, Regulations",
      "children": [
        {"id": 2, "name": "Standard documents", "children": []},
        {"id": 3, "name": "Archive documents", "children": []},
        {"id": 4, "name": "QA documents", "children": []}
      ]
    }
  ]
}
```

**Permission:** `document:category:read`

#### POST /api/v1/document-categories
**Description:** Create a new category

**Permission:** `document:category:write`

### 5.3 Favorite Endpoints

#### POST /api/v1/documents/{id}/favorite
**Description:** Toggle document favorite status

**Response (200 OK):**
```json
{
  "is_favorite": true
}
```

**Permission:** `document:favorite`

### 5.4 Lock Endpoints

#### POST /api/v1/documents/{id}/lock
**Description:** Lock a document

**Request Body:**
```json
{
  "reason": "Updating content for Q2 review"
}
```

**Permission:** `document:lock`

#### DELETE /api/v1/documents/{id}/lock
**Description:** Unlock a document

**Permission:** `document:lock`

### 5.5 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/documents | List documents | document:read |
| POST | /api/v1/documents | Upload document | document:create |
| GET | /api/v1/documents/{id} | Get document | document:read |
| PUT | /api/v1/documents/{id} | Update document | document:edit |
| DELETE | /api/v1/documents/{id} | Delete document | document:delete |
| POST | /api/v1/documents/{id}/versions | New version | document:edit |
| GET | /api/v1/document-categories | Category tree | document:category:read |
| POST | /api/v1/document-categories | Create category | document:category:write |
| POST | /api/v1/documents/{id}/favorite | Toggle favorite | document:favorite |
| POST | /api/v1/documents/{id}/lock | Lock document | document:lock |
| DELETE | /api/v1/documents/{id}/lock | Unlock document | document:lock |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Document Admin | doc_admin | Full document and category management |
| Document Author | doc_author | Create and edit own documents |
| Document Reviewer | doc_reviewer | Read and comment on documents |
| Document Viewer | doc_viewer | Read-only access |
| System Admin | sys_admin | Full system access |

### Permission Matrix
| Role | Create Category | Manage Categories | Upload Document | Edit Document | Delete Document | Comment | Manage Permissions | Lock/Unlock |
|------|----------------|-------------------|----------------|--------------|----------------|---------|-------------------|------------|
| Document Admin | ✅ | ✅ | ✅ | ✅ (all) | ✅ | ✅ | ✅ | ✅ |
| Document Author | ❌ | ❌ | ✅ | ✅ (own) | ❌ | ✅ | ❌ | ✅ (own) |
| Document Reviewer | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| Document Viewer | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| System Admin | ✅ | ✅ | ✅ | ✅ (all) | ✅ | ✅ | ✅ | ✅ |

### Row-Level Security
- Document-level permissions override category-level defaults.
- Users only see documents in categories they have at least "view" permission for.
- Category inheritance: child categories inherit parent permissions unless explicitly overridden.
- Deleted documents are visible only to admins during the 90-day retention period.

---

## 7. State Machine / Workflows

### 7.1 Document Lifecycle

```
[DRAFT] ──publish──→ [PUBLISHED] ──archive──→ [ARCHIVED]
   │                      │
   └──delete──────────────┘
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | PUBLISHED | publish | Author or Admin | Notify subscribers, update search index |
| PUBLISHED | ARCHIVED | archive | Admin | Remove from active listings, preserve in archive |
| DRAFT | DELETED | delete | Author or Admin | Schedule for permanent deletion (90 days) |

### 7.2 Document Lock State

```
[UNLOCKED] ──lock──→ [LOCKED] ──unlock──→ [UNLOCKED]
                        │
                        └──auto-unlock (7 days)──→ [UNLOCKED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Document Usage Statistics
**Type:** Real-time
**Purpose:** Track document engagement and usage patterns.

**Dimensions:**
- Category
- Time period
- Document type

**Metrics:**
- Total documents per category
- View counts
- Download counts
- Comment counts
- Version counts

**Filters:**
- Date range
- Category selection
- Creator

**Chart Type:** Bar chart, table

### 8.2 Report: Document Activity Log
**Type:** Real-time
**Purpose:** Audit trail of all document operations.

**Metrics:**
- Documents created
- Documents updated
- Documents deleted
- Comments added

**Chart Type:** Timeline table

### 8.3 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Document Usage Statistics | Real-time | On-demand | Bar chart, table |
| Document Activity Log | Real-time | On-demand | Table |
| Version History Report | Real-time | On-demand | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| File Storage (S3) | Document file storage | REST | IAM Role |
| Full-Text Search (Elasticsearch) | Document content indexing | REST | API Key |
| PDF Viewer Service | Inline PDF preview | REST | API Key |
| M01 User Service | Resolve user profiles | REST | JWT |
| M02 Permission Service | Check access permissions | REST | JWT |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| document.created | {document_id, category_id, creator_id} | M28 Knowledge, M22 Correspondence |
| document.versioned | {document_id, version_number, uploader_id} | M19 Workflow |
| document.commented | {document_id, comment_id, user_id} | M20 Chat |
| document.locked | {document_id, locked_by, reason} | M19 Workflow |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| employee.departed | M01 Auth Service | Reassign document ownership to manager |
| department.restructured | M03 Org Service | Update category permissions |

---

## 10. Technical Notes

### Performance
- Expected data volume: 5,000+ documents, 20,000+ versions, total storage 500GB+
- Query complexity: Low for single document, Medium for category tree traversal
- Expected response time: <200ms for list views, <500ms for full-text search
- File downloads served via CDN for performance

### Caching Strategy
- Category tree cached with 1-hour TTL (Redis)
- Document metadata cached with 10-minute TTL
- Full-text search index updated within 30 seconds of document upload
- Cache invalidation on document create/update/delete or category change

### File Attachments
- Supported types: pdf, docx, doc, jpg, png, xlsx, pptx
- Max size: 50MB per file
- Storage: S3-compatible object storage with CDN distribution
- Thumbnail generation for images and first page of PDFs

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Document metadata in user's preferred language where available

### Mobile Considerations
- Document list and detail views optimized for mobile screens
- PDF inline rendering on mobile devices
- Offline access to favorited documents (downloaded locally)
- Touch-friendly category tree navigation
- Mobile app icon visible in app grid (image_05, image_06)

### Security
- Files encrypted at rest (AES-256) and in transit (TLS 1.3)
- Signed URLs with expiration for document downloads
- Document access logged for audit compliance
- Virus scanning on all uploaded files
- Content-Disposition headers prevent inline execution of uploaded files

### Full-Text Search
- Elasticsearch index for document content, metadata, and comments
- Vietnamese language analyzer with stemming
- Search result highlighting
- Fuzzy matching for typo tolerance
- Search suggestions based on popular queries
