# M25 - Contact Directory (Danh ba)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_03.png, assets/image_04.png, assets/image_05.png
**Dependencies:** M01 (User & Authentication), M03 (Company & Organization)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Contact Directory module provides a comprehensive employee and external contact management system for viewing, searching, and communicating with colleagues and business contacts. It serves as the organization's phonebook, enabling employees to quickly find contact information, view colleague profiles, initiate communication (chat, email, call), and organize contacts into groups and favorites. The module is prominently featured in the platform onboarding guide (img_03, img_04) as step 5 of the onboarding process: "Xem, tìm kiếm liên hệ và trao đổi với đồng nghiệp" (View, search contacts and communicate with colleagues). It also appears as a primary app icon on the desktop dashboard (img_05) with a purple phone icon, indicating its importance as a core communication tool within the ERP ecosystem.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Employee | Any staff member who needs to find and contact colleagues | Read + communicate |
| HR Administrator | Manages employee contact data accuracy | Full (employee data) |
| Department Manager | Views department contact structure | Read + group management |
| System Administrator | Configures global settings and data sync | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication), M03 (Company & Organization)
- **Used by:** M20 (Internal Chat), M24 (Meeting Room Booking), M19 (Workflow)

### Key Business Rules
1. The contact directory automatically includes all active employees from the employee database (M01/M05) with their contact information.
2. External contacts can be added manually by any employee with name, phone, email, company, and notes.
3. Contact search supports multiple fields: name, phone number, email, department, position, and tags.
4. Contact groups are user-created collections for organizing frequently contacted people (e.g., "Project Team", "Management").
5. Tags are user-applied labels for cross-group contact categorization.
6. Favorite contacts are pinned to the top of the user's contact list for quick access.
7. Contact history tracks all interactions (calls, messages, meetings) with a contact for context.
8. Notes can be added to any contact for personal reference (private to the note creator).
9. Employee contact information is synchronized from the HR module; external contacts are user-managed.
10. Privacy: personal phone numbers and home addresses are visible only to HR and the employee themselves.
11. Communication actions (chat, email, call) launch the respective integrated modules.
12. Contact cards display: name, avatar, department, position, phone, email, location, online status.

---

## 2. Screen Inventory

### 2.1 Screen: Contact List View
**Route:** `/contacts`
**Purpose:** Main contact browsing and search interface (img_05).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Search Bar | Input | Search by name, phone, email, department |
| Filter Tabs | Tabs | All, Employees, External, Favorites, Groups |
| Contact List | Data grid | Avatar, name, department, position, phone, email, status |
| Group Sidebar | Navigation | User-created contact groups |
| Add Contact Button | CTA | Add new external contact |
| View Toggle | Toggle | List view / Card view |
| Alphabet Index | Sidebar | Quick jump by first letter |

**User Interactions:**
- Search contacts by multiple criteria.
- Filter by contact type (employee/external).
- Click a contact to view their detail card.
- Add external contacts manually.
- Organize contacts into groups.
- Toggle between list and card view.

**Data Displayed:**
- All active employees with contact information
- External contacts added by the user
- Online status indicators
- Favorite markers

### 2.2 Screen: Contact Detail Card
**Route:** `/contacts/{id}`
**Purpose:** Full contact information with communication actions.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Contact Header | Card | Avatar, name, department, position, online status |
| Contact Info | List | Phone, email, location, timezone |
| Action Buttons | Button group | Chat, Email, Call, Video Call, Create Task |
| Notes Section | Textarea | Private notes about this contact |
| Interaction History | Timeline | Recent communications and interactions |
| Shared Groups | Badge list | Groups containing this contact |
| Tags | Badge list | Applied tags |

**User Interactions:**
- View all contact information.
- Initiate communication (chat, email, call).
- Add private notes.
- View interaction history.
- Add/remove tags.
- Add to or remove from groups.

### 2.3 Screen: Contact Group Management
**Route:** `/contacts/groups`
**Purpose:** Create and manage contact groups.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Group List | Card list | Group name, member count, last activity |
| Create Group | Button | Create new group |
| Group Detail | Card | Group name, description, member list |
| Add Members | Multi-select | Search and add contacts to group |
| Remove Members | Action | Remove contacts from group |

**User Interactions:**
- Create new contact groups.
- Add and remove group members.
- View group member list.
- Delete groups.

### 2.4 Screen: Contact Search and Filter
**Route:** `/contacts/search`
**Purpose:** Advanced search with multiple filter criteria.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Search Input | Full-width | Keyword search |
| Department Filter | Multi-select | Filter by department |
| Position Filter | Multi-select | Filter by position/title |
| Location Filter | Multi-select | Filter by office location |
| Status Filter | Toggle | Online, Offline, All |
| Results List | Card list | Matching contacts with highlighted matches |

**User Interactions:**
- Search by keyword across all fields.
- Apply multiple filters simultaneously.
- View results with match highlighting.

### 2.5 Screen: Onboarding Contact Discovery
**Route:** Part of onboarding flow (img_03, img_04)
**Purpose:** Introduce new users to the contact directory during onboarding.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Onboarding Card | Modal | "Danh bạ - Xem, tìm kiếm liên hệ và trao đổi với đồng nghiệp" |
| Discover Button | CTA | "Khám phá ngay" (Explore now) |
| Preview | Screenshot | Contact directory interface preview |

### 2.6 Screen: External Contact Form
**Route:** Modal from contact list
**Purpose:** Add a new external contact.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Name Input | Text field | Full name (required) |
| Company | Text field | Company/organization |
| Position | Text field | Job title |
| Phone | Text field | Phone number |
| Email | Text field | Email address |
| Address | Textarea | Physical address |
| Tags Input | Multi-tag | Add tags |
| Notes | Textarea | Additional notes |
| Save Button | CTA | Save contact |

**User Interactions:**
- Fill in external contact details.
- Add tags for categorization.
- Save to add to contact directory.

---

## 3. Feature Specifications

### 3.1 Feature: Employee Contact Synchronization
**Description:** Automatic synchronization of employee contact data from the HR module.

**User Stories:**
- As a new employee, I want my contact information to appear in the directory automatically, so that colleagues can reach me.
- As a user, I want employee contact details to stay up-to-date, so that I always have correct information.

**Acceptance Criteria:**
- Given a new employee is added to the HR system
- When the contact directory sync runs
- Then the employee appears in the contact list with their information

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Sync Frequency | Every 15 minutes | N/A |
| Data Source | M01 Employee database | N/A |

**Edge Cases:**
- If an employee's department changes, the contact directory updates immediately.
- Deactivated employees are removed from the active directory but preserved in history.

### 3.2 Feature: Contact Search
**Description:** Multi-field search across contact names, phone numbers, emails, and departments.

**User Stories:**
- As a user, I want to search for a colleague by typing part of their name, so that I can find them quickly.
- As a user, I want to search by phone number, so that I can identify an unknown caller.

**Acceptance Criteria:**
- Given a user types "Trương" in the search bar
- When the search executes
- Then all contacts with "Trương" in their name appear in results

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Search Query | Min 2 characters | "Enter at least 2 characters to search" |

**Edge Cases:**
- Vietnamese diacritic-insensitive search (searching "Truong" also finds "Trương").
- Search results limited to top 50 matches for performance.

### 3.3 Feature: Contact Groups
**Description:** User-created groups for organizing contacts.

**User Stories:**
- As a project manager, I want to create a group for my project team, so that I can quickly message everyone.
- As a user, I want to organize my external contacts into groups, so that I can manage them efficiently.

**Acceptance Criteria:**
- Given a user creates a group "Project Alpha"
- When they add members
- Then the group appears in their sidebar with the member count

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Group Name | Required, unique per user, max 100 chars | "Group name is required and must be unique" |
| Max Members | 500 per group | "Group exceeds maximum member count" |

**Edge Cases:**
- A contact can belong to multiple groups simultaneously.
- Deleting a group does not delete its member contacts.

### 3.4 Feature: Contact Notes
**Description:** Private notes attached to contacts for personal reference.

**User Stories:**
- As a user, I want to add notes to a contact, so that I can remember important details about our interactions.

**Acceptance Criteria:**
- Given a user adds a note to a contact
- When they view that contact later
- Then the note is displayed in the contact detail view

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Note Content | Max 2,000 characters | "Note exceeds maximum length" |

**Edge Cases:**
- Notes are private to the creator; other users cannot see them.
- Notes persist even if the contact is removed from the directory.

### 3.5 Feature: Favorite Contacts
**Description:** Pin frequently contacted people for quick access.

**User Stories:**
- As a user, I want to favorite my manager and key teammates, so that I can reach them with one click.

**Acceptance Criteria:**
- Given a user favorites a contact
- When they view their contact list with "Favorites" filter
- Then the favorited contacts appear at the top

**Edge Cases:**
- Maximum 50 favorite contacts per user.
- Favoriting an employee who later leaves the company removes them from favorites.

### 3.6 Feature: Contact History
**Description:** Track interactions with contacts for context.

**User Stories:**
- As a user, I want to see my recent interactions with a contact, so that I have context before reaching out.

**Acceptance Criteria:**
- Given a user chatted with a contact yesterday
- When they view the contact detail
- Then the chat interaction appears in the history timeline

**Interaction Types Tracked:**
- Chat messages sent
- Emails sent/received
- Calls made
- Meetings attended together
- Tasks assigned

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|    Contact     |       |  ContactGroup  |       | ContactGroup   |
+----------------+       +----------------+       |   Member       |
| id (PK)        |<----->| id (PK)        |<----->+----------------+
| user_id (FK)   |       | name           |       | id (PK)        |
| contact_type   |       | user_id (FK)   |       | group_id (FK)  |
| name           |       | description    |       | contact_id(FK) |
| phone          |       | created_at     |       +----------------+
| email          |       +----------------+
| company        |
| position       |
| avatar_url     |
| is_employee    |
| employee_id    |
| created_at     |
+----------------+
```

### 4.2 Table Schemas

#### Table: contacts
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| owner_id | BIGINT | NULL, FK -> users.id | Owner (NULL for employee contacts) |
| contact_type | ENUM('employee','external') | NOT NULL | Contact type |
| name | VARCHAR(200) | NOT NULL | Full name |
| phone | VARCHAR(50) | NULL | Phone number |
| email | VARCHAR(200) | NULL | Email address |
| company | VARCHAR(200) | NULL | Company/organization |
| position | VARCHAR(200) | NULL | Job title |
| department | VARCHAR(200) | NULL | Department (for employees) |
| location | VARCHAR(300) | NULL | Office location |
| avatar_url | VARCHAR(500) | NULL | Avatar image URL |
| is_employee | BOOLEAN | NOT NULL, DEFAULT FALSE | Employee flag |
| employee_id | BIGINT | NULL, FK -> employees.id | Linked employee record |
| user_id | BIGINT | NULL, FK -> users.id | Linked user account |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_contacts_owner_id ON contacts(owner_id);
CREATE INDEX idx_contacts_name ON contacts(name);
CREATE INDEX idx_contacts_phone ON contacts(phone);
CREATE INDEX idx_contacts_email ON contacts(email);
CREATE INDEX idx_contacts_department ON contacts(department);
CREATE INDEX idx_contacts_type ON contacts(contact_type);
CREATE INDEX idx_contacts_is_active ON contacts(is_active);
```

#### Table: contact_groups
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL | Group name |
| description | TEXT | NULL | Group description |
| user_id | BIGINT | NOT NULL, FK -> users.id | Group owner |
| member_count | INT | NOT NULL, DEFAULT 0 | Member count |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_contact_groups_user_id ON contact_groups(user_id);
CREATE UNIQUE INDEX idx_contact_groups_user_name ON contact_groups(user_id, name);
```

#### Table: contact_group_members
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| group_id | BIGINT | NOT NULL, FK -> contact_groups.id | Group |
| contact_id | BIGINT | NOT NULL, FK -> contacts.id | Contact |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Added timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_cgm_group_contact ON contact_group_members(group_id, contact_id);
CREATE INDEX idx_cgm_contact_id ON contact_group_members(contact_id);
```

#### Table: contact_tags
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL | Tag name |
| user_id | BIGINT | NOT NULL, FK -> users.id | Tag owner |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_contact_tags_user_name ON contact_tags(user_id, name);
```

#### Table: contact_contact_tag
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| contact_id | BIGINT | NOT NULL, FK -> contacts.id | Contact |
| tag_id | BIGINT | NOT NULL, FK -> contact_tags.id | Tag |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_cct_contact_tag ON contact_contact_tag(contact_id, tag_id);
```

#### Table: contact_notes
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| contact_id | BIGINT | NOT NULL, FK -> contacts.id | Target contact |
| user_id | BIGINT | NOT NULL, FK -> users.id | Note author |
| content | TEXT | NOT NULL | Note content |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_contact_notes_contact_id ON contact_notes(contact_id);
CREATE INDEX idx_contact_notes_user_id ON contact_notes(user_id);
```

#### Table: contact_favorites
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | NOT NULL, FK -> users.id | User |
| contact_id | BIGINT | NOT NULL, FK -> contacts.id | Favorited contact |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Favorite timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_contact_favorites_user_contact ON contact_favorites(user_id, contact_id);
CREATE INDEX idx_contact_favorites_user_id ON contact_favorites(user_id);
```

#### Table: contact_history
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| contact_id | BIGINT | NOT NULL, FK -> contacts.id | Target contact |
| user_id | BIGINT | NOT NULL, FK -> users.id | Acting user |
| interaction_type | ENUM('chat','email','call','video_call','meeting','task') | NOT NULL | Interaction type |
| summary | VARCHAR(500) | NULL | Interaction summary |
| reference_id | BIGINT | NULL | Reference to source record |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Interaction timestamp |

**Indexes:**
```sql
CREATE INDEX idx_contact_history_contact_id ON contact_history(contact_id);
CREATE INDEX idx_contact_history_user_id ON contact_history(user_id);
CREATE INDEX idx_contact_history_created_at ON contact_history(created_at DESC);
CREATE INDEX idx_contact_history_type ON contact_history(interaction_type);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| contacts | Contact records (employee + external) | 10,000 |
| contact_groups | User-created groups | 5,000 |
| contact_group_members | Group membership | 50,000 |
| contact_tags | User-defined tags | 20,000 |
| contact_contact_tag | Contact-tag mapping | 50,000 |
| contact_notes | Private notes | 30,000 |
| contact_favorites | User favorites | 30,000 |
| contact_history | Interaction history | 200,000 |

---

## 5. API Specifications

### 5.1 Contact Endpoints

#### GET /api/v1/contacts
**Description:** List contacts with search and filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page (default: 20) |
| q | string | No | Search query |
| contact_type | string | No | employee, external |
| department | string | No | Filter by department |
| is_favorite | boolean | No | Filter favorites |
| group_id | bigint | No | Filter by group |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "name": "Trương Thanh Sơn",
      "contact_type": "employee",
      "department": "Sales",
      "position": "Sales Staff",
      "phone": "+84 123 456 789",
      "email": "son.tt@company.com",
      "location": "Hanoi Office",
      "avatar_url": "...",
      "is_online": true,
      "is_favorite": false
    }
  ],
  "meta": {"total": 1253, "page": 1, "limit": 20}
}
```

**Permission:** `contact:read`

#### POST /api/v1/contacts
**Description:** Add a new external contact

**Request Body:**
```json
{
  "name": "Nguyen Van Partner",
  "company": "Partner Corp",
  "position": "Director",
  "phone": "+84 987 654 321",
  "email": "partner@partnercorp.com",
  "tags": ["partner", "vendor"]
}
```

**Response (201 Created):**
```json
{
  "id": 1254,
  "name": "Nguyen Van Partner",
  "contact_type": "external",
  "created_at": "2024-04-14T09:00:00Z"
}
```

**Permission:** `contact:create`

#### GET /api/v1/contacts/{id}
**Description:** Get contact detail with notes and history

**Permission:** `contact:read`

### 5.2 Group Endpoints

#### GET /api/v1/contact-groups
**Description:** List user's contact groups

**Permission:** `contact:group:read`

#### POST /api/v1/contact-groups
**Description:** Create a new contact group

**Request Body:**
```json
{
  "name": "Project Alpha Team",
  "description": "Core team for Project Alpha",
  "member_ids": [1, 5, 8, 12]
}
```

**Permission:** `contact:group:create`

#### POST /api/v1/contact-groups/{id}/members
**Description:** Add members to a group

**Permission:** `contact:group:update`

### 5.3 Favorite Endpoints

#### POST /api/v1/contacts/{id}/favorite
**Description:** Toggle favorite status

**Response (200 OK):**
```json
{
  "is_favorite": true
}
```

**Permission:** `contact:favorite`

### 5.4 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/contacts | List contacts | contact:read |
| POST | /api/v1/contacts | Add external contact | contact:create |
| GET | /api/v1/contacts/{id} | Get contact detail | contact:read |
| GET | /api/v1/contact-groups | List groups | contact:group:read |
| POST | /api/v1/contact-groups | Create group | contact:group:create |
| POST | /api/v1/contact-groups/{id}/members | Add members | contact:group:update |
| POST | /api/v1/contacts/{id}/favorite | Toggle favorite | contact:favorite |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Employee | employee | View and search all contacts |
| HR Administrator | hr_admin | Manage employee contact data |
| System Admin | sys_admin | Full system access |

### Permission Matrix
| Role | View Employees | View External | Create External | Edit External | Delete External | Manage Groups | Export Contacts |
|------|--------------|--------------|----------------|--------------|----------------|--------------|----------------|
| Employee | ✅ | ✅ (own) | ✅ | ✅ (own) | ✅ (own) | ✅ | ❌ |
| HR Administrator | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| System Admin | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

### Row-Level Security
- Employee contacts are visible to all active employees.
- External contacts are visible only to their creator (owner).
- Personal contact information (personal phone, home address) visible only to HR and the employee.

---

## 7. State Machine / Workflows

### 7.1 Contact Status Transitions

```
[ACTIVE] ──deactivate──→ [INACTIVE]
   │
   └──delete──→ [DELETED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| ACTIVE | INACTIVE | deactivate | System (employee left) | Remove from active directory |
| ACTIVE | DELETED | delete | Owner (external only) | Remove permanently |

### 7.2 Employee Sync Status

```
[SYNCED] ──update──→ [PENDING_SYNC] ──sync-complete──→ [SYNCED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Contact Usage Statistics
**Type:** Real-time
**Purpose:** Track contact directory usage patterns.

**Metrics:**
- Total contacts in directory
- Employee vs external ratio
- Most contacted people
- Groups created

**Chart Type:** Summary cards, pie chart

### 8.2 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Contact Usage Statistics | Real-time | On-demand | Cards, pie chart |
| Contact Activity Report | Real-time | On-demand | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| M01 Employee Service | Sync employee contact data | REST | JWT |
| M20 Chat Service | Launch chat from contact card | REST | JWT |
| Email Service (SMTP) | Launch email compose | POST | API Key |
| M39 Dashboard | Feed contact usage stats | REST | JWT |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| contact.created | {contact_id, contact_type, owner_id} | M20 Chat |
| contact.interaction | {contact_id, interaction_type, user_id} | M01 Notification |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| employee.created | M01 Auth Service | Create employee contact record |
| employee.updated | M05 Employee Management | Update contact information |
| employee.departed | M01 Auth Service | Deactivate contact record |

---

## 10. Technical Notes

### Performance
- Expected data volume: 10,000+ contacts (1,253 employees + external)
- Query complexity: Low for single contact, Medium for multi-field search
- Expected response time: <100ms for single contact, <300ms for search
- Search uses trigram index for fuzzy matching and diacritic-insensitive Vietnamese search

### Caching Strategy
- Employee contact list cached with 15-minute TTL
- Individual contact cards cached with 5-minute TTL
- Online status cached with 30-second TTL
- Search results cached with 1-minute TTL per query
- Cache invalidation on contact create/update/delete or employee sync

### Localization
- Supported languages: Vietnamese (primary), English
- Phone number format: Vietnamese (+84) and international
- Name display: Vietnamese order (Middle Last First) with English option
- Diacritic-insensitive search for Vietnamese names

### Mobile Considerations
- Contact cards optimized for mobile with tap-to-call and tap-to-email
- Offline access to favorite contacts
- Quick search with voice input support
- Avatar display optimized for low-bandwidth
- Contact app icon on desktop dashboard (image_05)

### Search
- Trigram-based fuzzy search for name matching
- Vietnamese diacritic normalization (Truong = Trương)
- Phone number normalization for search (strips spaces, dashes)
- Email domain search support
- Department and position full-text search

### Privacy
- Employee personal data (personal phone, home address) restricted to HR
- External contacts visible only to creator
- Contact notes are private to the author
- Interaction history visible only to the acting user
