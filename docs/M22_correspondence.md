# M22 - Correspondence (Van thu)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_18.png, assets/image_60.png
**Dependencies:** M01 (User & Authentication), M03 (Company & Organization), M19 (Workflow), M21 (Document Management), M29 (Electronic Signature)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Correspondence module manages the complete lifecycle of incoming and outgoing official documents, letters, and administrative correspondence within the organization. It serves as the central registry for all formal document exchanges, tracking tracking numbers, document flows, signatory authorities, and archival status. The module handles incoming documents from external parties (government agencies, partners, customers), outgoing documents (official letters, responses, reports), and internal correspondence requiring formal tracking. Each document is assigned a unique tracking number, routed through approval workflows, signed by authorized signatories, and archived according to retention policies. The module integrates with the Operations Dashboard (img_60) showing correspondence statistics and with the Document Management module (img_18) for document storage and retrieval.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Correspondence Officer | Registry clerk managing document intake and dispatch | Full |
| Signatory | Authorized signers (Director, Deputy Director) | Sign + approve |
| Department Manager | Receives and processes documents for their department | Partial (dept-scoped) |
| Regular User | Views documents assigned to them | Read-only |
| Archivist | Manages document archival and retention schedules | Partial |
| System Administrator | Configures document types, tracking rules, and settings | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication), M03 (Company & Organization)
- **Used by:** M19 (Workflow), M21 (Document Management), M29 (Electronic Signature), M39 (Operations Dashboard)

### Key Business Rules
1. Every incoming and outgoing document must be assigned a unique tracking number following the format: `{type-prefix}/{year}/{sequence}` (e.g., `IN/2024/0001`, `OUT/2024/0001`).
2. Incoming documents must be registered within 24 hours of receipt with metadata: sender, date received, subject, document type, urgency level, and assigned department.
3. Outgoing documents require approval from the designated signatory authority before dispatch.
4. Signatory authority is role-based: Director can sign all documents, Deputy Director can sign within delegated scope, Department Managers can sign departmental correspondence.
5. Document urgency levels: Normal (5 business day SLA), Urgent (2 business days), Emergency (same day).
6. Document types include: Official Letter, Decision, Report, Contract, Invoice, Notice, Request, Response, and custom types configured by admin.
7. All documents follow a configurable workflow: Receive/Register → Route → Process → Approve → Sign → Dispatch/Archive.
8. Documents are archived after completion with configurable retention periods (1 year, 3 years, 5 years, permanent).
9. The operations dashboard shows 3 new documents requiring attention (img_60).
10. Document attachments support PDF, DOCX, DOC, JPG, PNG with max 50MB per file.
11. Cross-referencing: outgoing documents can reference incoming documents (reply tracking).
12. Document flow tracking records every handoff with timestamp, sender, receiver, and action taken.

---

## 2. Screen Inventory

### 2.1 Screen: Incoming Document Registry
**Route:** `/correspondence/incoming`
**Purpose:** Register and track all incoming official documents.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| New Document Button | CTA | Register new incoming document |
| Incoming Table | Data grid | Tracking number, date, sender, subject, type, urgency, status, assigned dept |
| Filter Bar | Controls | Filter by date range, type, urgency, status, department |
| Search | Input | Search by tracking number, subject, sender |
| Summary Cards | Metrics | New today, In process, Completed, Overdue |

**User Interactions:**
- Register new incoming documents with full metadata.
- View and filter incoming document list.
- Assign documents to departments for processing.
- Update document status as it progresses through workflow.

**Data Displayed:**
- New documents count: 3 (from img_60)
- Document tracking numbers, subjects, senders
- Urgency indicators (color-coded)
- Processing status

### 2.2 Screen: Outgoing Document Dispatch
**Route:** `/correspondence/outgoing`
**Purpose:** Create, approve, and dispatch outgoing official documents.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| New Outgoing Button | CTA | Draft new outgoing document |
| Outgoing Table | Data grid | Tracking number, date, recipient, subject, type, signatory, status |
| Approval Status | Badge | Draft, Pending Approval, Approved, Signed, Dispatched |
| Signatory Selector | Dropdown | Select authorized signer |
| Reference Link | Input | Link to related incoming document |

**User Interactions:**
- Draft outgoing documents.
- Submit for signatory approval.
- Track approval and signing status.
- Dispatch approved documents.
- Reference incoming documents being replied to.

### 2.3 Screen: Document Flow Tracker
**Route:** `/correspondence/flows/{id}`
**Purpose:** Visual tracking of a document's journey through the organization.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Flow Timeline | Vertical timeline | Each handoff with timestamp, from, to, action |
| Current Status | Badge | Current position in workflow |
| Document Preview | Card | Document metadata and attachments |
| Next Action | Card | Who needs to act next and by when |

**User Interactions:**
- View complete document movement history.
- See current position and pending actions.
- Access document attachments.

### 2.4 Screen: Signatory Authority Management
**Route:** `/correspondence/settings/signatories`
**Purpose:** Configure who is authorized to sign which types of documents.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Authority Table | Data grid | User, position, document types, delegation scope, validity period |
| Delegation Form | Modal | Temporary delegation when signatory is unavailable |
| Add Signatory | Button | Add new authorized signer |

**User Interactions:**
- View all authorized signatories.
- Configure signing scope per user.
- Set up temporary delegations.
- Revoke signing authority.

### 2.5 Screen: Document Archive
**Route:** `/correspondence/archive`
**Purpose:** Browse and manage archived documents.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Archive Browser | Tree/Grid | Documents organized by year and type |
| Retention Badge | Indicator | Retention period and expiry date |
| Restore Button | Action | Restore archived document to active |
| Permanent Delete | Action | Permanently delete expired documents |

**User Interactions:**
- Browse archived documents by year and type.
- View retention periods.
- Restore documents when needed.
- Purge expired documents per retention policy.

### 2.6 Screen: Correspondence Dashboard
**Route:** `/correspondence/overview`
**Purpose:** Summary dashboard showing correspondence statistics (img_60).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Summary Cards | Metric cards | New documents (3), In process, Completed this month, Overdue |
| Recent Incoming | List | Latest incoming documents with status |
| Recent Outgoing | List | Latest outgoing documents with status |
| Urgency Breakdown | Pie chart | Normal vs Urgent vs Emergency |
| Department Workload | Bar chart | Documents processed per department |

**Data Displayed:**
- 3 new documents (from img_60)
- Incoming/outgoing volume trends
- Processing time averages
- Department workload distribution

---

## 3. Feature Specifications

### 3.1 Feature: Incoming Document Registration
**Description:** Register incoming documents with unique tracking numbers and metadata.

**User Stories:**
- As a correspondence officer, I want to register an incoming document with sender, subject, and type, so that it enters the tracking system.
- As a department manager, I want to see documents assigned to my department, so that I can process them promptly.

**Acceptance Criteria:**
- Given a correspondence officer fills the incoming document form
- When they submit
- Then a unique tracking number is generated (e.g., IN/2024/0001) and the document is assigned to the specified department

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Sender | Required, max 200 chars | "Sender is required" |
| Subject | Required, max 500 chars | "Subject is required" |
| Date Received | Required, cannot be future date | "Date received must be today or earlier" |
| Document Type | Required, from predefined list | "Document type is required" |
| Attachment | Optional, max 50MB | "File exceeds maximum size" |

**Edge Cases:**
- If the same document is received from multiple channels, the system detects duplicates by sender + date + subject similarity.
- Tracking numbers are sequential per year; new year resets the sequence.

### 3.2 Feature: Outgoing Document Approval and Signing
**Description:** Draft, route for approval, and sign outgoing documents with signatory authority enforcement.

**User Stories:**
- As a staff member, I want to draft an outgoing document and submit it for signing, so that it can be officially dispatched.
- As a signatory, I want to review and sign documents within my authority, so that they are officially approved.

**Acceptance Criteria:**
- Given a draft outgoing document is submitted for signing
- When the signatory reviews and signs it
- Then the document status changes to "Signed" and is ready for dispatch

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Recipient | Required | "Recipient is required" |
| Signatory | Must have authority for this document type | "Selected signatory lacks authority for this document type" |
| Subject | Required | "Subject is required" |

**Edge Cases:**
- If a signatory is unavailable, temporary delegation allows another authorized person to sign.
- Documents signed electronically (M29) are equivalent to physical signatures.

### 3.3 Feature: Document Flow Tracking
**Description:** Complete audit trail of document movement through the organization.

**User Stories:**
- As a user, I want to see the complete history of a document's journey, so that I know where it has been and who handled it.

**Acceptance Criteria:**
- Given a document has been processed through multiple handoffs
- When a user views the flow tracker
- Then they see a timeline with each handoff's timestamp, sender, receiver, and action

**Edge Cases:**
- If a document is returned for revision, the flow shows the backward movement.
- Parallel processing (multiple departments processing simultaneously) is shown with branching.

### 3.4 Feature: Document Type Management
**Description:** Configure and manage document types with specific workflows and requirements.

**User Stories:**
- As an admin, I want to define custom document types, so that the system supports our specific correspondence needs.

**Acceptance Criteria:**
- Given an admin creates a new document type "Internal Memo"
- When users create correspondence
- Then "Internal Memo" appears as an available type option

**Edge Cases:**
- Deleting a document type that has existing documents is prevented; it must be archived instead.

### 3.5 Feature: Retention and Archival
**Description:** Automatic archival based on configurable retention schedules.

**User Stories:**
- As an archivist, I want documents to be auto-archived after processing, so that the active registry stays manageable.
- As a compliance officer, I want retention periods enforced, so that documents are kept for the required duration.

**Acceptance Criteria:**
- Given a document is completed and the auto-archive period is 30 days
- When 30 days pass
- Then the document is moved to the archive with the appropriate retention label

**Edge Cases:**
- Documents under legal hold cannot be archived or deleted until the hold is released.
- Retention period expiry triggers notification 30 days before permanent deletion.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+--------------------+       +--------------------+       +--------------------+
|   Correspondence   |       | IncomingDocument   |       | OutgoingDocument   |
+--------------------+       +--------------------+       +--------------------+
| id (PK)            |<----->| id (PK)            |       | id (PK)            |
| tracking_number    |       | correspondence(FK) |       | correspondence(FK) |
| doc_type_id (FK)   |       | sender             |       | recipient          |
| urgency_level      |       | date_received      |       | date_dispatched    |
| subject            |       | reference_number   |       | signatory_id (FK)  |
| status             |       | assigned_dept(FK)  |       | signed_at          |
| created_by (FK)    |       | status             |       | status             |
| created_at         |       | processed_by       |       | dispatched_by      |
+--------------------+       +--------------------+       +--------------------+
         |
         |
+--------------------+       +--------------------+
|   DocumentType     |       |   DocumentFlow     |
+--------------------+       +--------------------+
| id (PK)            |       | id (PK)            |
| name               |       | correspondence(FK) |
| prefix             |       | from_user_id (FK)  |
| retention_years    |       | to_user_id (FK)    |
| workflow_config    |       | action             |
| created_at         |       | timestamp          |
+--------------------+       | notes              |
                             +--------------------+
```

### 4.2 Table Schemas

#### Table: correspondences
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| tracking_number | VARCHAR(30) | NOT NULL, UNIQUE | Tracking number (IN/2024/0001) |
| doc_type_id | BIGINT | NOT NULL, FK -> document_types.id | Document type |
| direction | ENUM('incoming','outgoing') | NOT NULL | Document direction |
| urgency_level | ENUM('normal','urgent','emergency') | NOT NULL, DEFAULT 'normal' | Urgency |
| subject | VARCHAR(500) | NOT NULL | Document subject |
| content | TEXT | NULL | Document body/summary |
| status | ENUM('new','registered','in_process','approved','signed','dispatched','archived') | NOT NULL, DEFAULT 'new' | Processing status |
| created_by | BIGINT | NOT NULL, FK -> users.id | Creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_correspondences_tracking ON correspondences(tracking_number);
CREATE INDEX idx_correspondences_direction ON correspondences(direction);
CREATE INDEX idx_correspondences_status ON correspondences(status);
CREATE INDEX idx_correspondences_doc_type ON correspondences(doc_type_id);
CREATE INDEX idx_correspondences_created_at ON correspondences(created_at);
CREATE INDEX idx_correspondences_urgency ON correspondences(urgency_level);
```

#### Table: incoming_documents
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| correspondence_id | BIGINT | NOT NULL, UNIQUE, FK -> correspondences.id | Parent correspondence |
| sender | VARCHAR(300) | NOT NULL | Sender name/organization |
| sender_address | VARCHAR(500) | NULL | Sender address |
| date_received | DATE | NOT NULL | Date document was received |
| reference_number | VARCHAR(100) | NULL | Sender's reference number |
| assigned_department_id | BIGINT | NULL, FK -> departments.id | Processing department |
| processed_by | BIGINT | NULL, FK -> users.id | Assigned processor |
| deadline | DATE | NULL | Processing deadline |
| processed_at | TIMESTAMP | NULL, DEFAULT NULL | Processing completion |
| reply_to_outgoing_id | BIGINT | NULL, FK -> outgoing_documents.id | Linked reply |

**Indexes:**
```sql
CREATE INDEX idx_incoming_dept ON incoming_documents(assigned_department_id);
CREATE INDEX idx_incoming_processed_by ON incoming_documents(processed_by);
CREATE INDEX idx_incoming_date_received ON incoming_documents(date_received);
```

#### Table: outgoing_documents
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| correspondence_id | BIGINT | NOT NULL, UNIQUE, FK -> correspondences.id | Parent correspondence |
| recipient | VARCHAR(300) | NOT NULL | Recipient name/organization |
| recipient_address | VARCHAR(500) | NULL | Recipient address |
| signatory_id | BIGINT | NULL, FK -> users.id | Signing authority |
| signed_at | TIMESTAMP | NULL, DEFAULT NULL | Signing timestamp |
| dispatched_by | BIGINT | NULL, FK -> users.id | Dispatching user |
| date_dispatched | DATE | NULL, DEFAULT NULL | Dispatch date |
| dispatch_method | ENUM('email','postal','hand_delivery','fax') | NULL | Dispatch method |
| references_incoming_id | BIGINT | NULL, FK -> incoming_documents.id | Incoming document being replied to |

**Indexes:**
```sql
CREATE INDEX idx_outgoing_signatory ON outgoing_documents(signatory_id);
CREATE INDEX idx_outgoing_dispatched ON outgoing_documents(date_dispatched);
```

#### Table: document_types
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL, UNIQUE | Type name |
| prefix | VARCHAR(10) | NOT NULL, UNIQUE | Tracking number prefix (e.g., OL for Official Letter) |
| retention_years | INT | NOT NULL, DEFAULT 3 | Archive retention period |
| workflow_config | JSON | NULL | Default workflow for this type |
| requires_signatory | BOOLEAN | NOT NULL, DEFAULT TRUE | Whether signing is required |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_document_types_name ON document_types(name);
CREATE UNIQUE INDEX idx_document_types_prefix ON document_types(prefix);
```

#### Table: document_flows
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| correspondence_id | BIGINT | NOT NULL, FK -> correspondences.id | Parent correspondence |
| from_user_id | BIGINT | NOT NULL, FK -> users.id | Transfer from user |
| to_user_id | BIGINT | NOT NULL, FK -> users.id | Transfer to user |
| action | ENUM('receive','assign','process','approve','sign','dispatch','return','archive') | NOT NULL | Action performed |
| notes | TEXT | NULL | Action notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Action timestamp |

**Indexes:**
```sql
CREATE INDEX idx_document_flows_correspondence ON document_flows(correspondence_id);
CREATE INDEX idx_document_flows_to_user ON document_flows(to_user_id);
CREATE INDEX idx_document_flows_created_at ON document_flows(created_at);
```

#### Table: tracking_numbers
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| year | INT | NOT NULL | Year |
| direction | ENUM('incoming','outgoing') | NOT NULL | Direction |
| doc_type_id | BIGINT | NOT NULL, FK -> document_types.id | Document type |
| last_sequence | INT | NOT NULL, DEFAULT 0 | Last used sequence number |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_tracking_numbers_year_type ON tracking_numbers(year, direction, doc_type_id);
```

#### Table: signatory_authorities
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | NOT NULL, FK -> users.id | Authorized user |
| position | VARCHAR(200) | NOT NULL | Position title |
| document_type_ids | JSON | NULL | Types this user can sign (NULL = all) |
| delegation_scope | ENUM('all','department','specific') | NOT NULL, DEFAULT 'all' | Signing scope |
| valid_from | DATE | NOT NULL | Authority start date |
| valid_to | DATE | NULL, DEFAULT NULL | Authority end date |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_signatory_authorities_user ON signatory_authorities(user_id);
CREATE INDEX idx_signatory_authorities_active ON signatory_authorities(is_active, valid_from, valid_to);
```

#### Table: document_archives
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| correspondence_id | BIGINT | NOT NULL, UNIQUE, FK -> correspondences.id | Archived correspondence |
| archived_by | BIGINT | NOT NULL, FK -> users.id | Archiving user |
| archived_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Archive timestamp |
| retention_until | DATE | NOT NULL | Retention expiry date |
| retention_period_years | INT | NOT NULL | Retention period in years |
| is_permanent | BOOLEAN | NOT NULL, DEFAULT FALSE | Permanent retention flag |
| is_legal_hold | BOOLEAN | NOT NULL, DEFAULT FALSE | Legal hold flag |

**Indexes:**
```sql
CREATE INDEX idx_document_archives_retention_until ON document_archives(retention_until);
CREATE INDEX idx_document_archives_is_legal_hold ON document_archives(is_legal_hold);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| correspondences | Master correspondence records | 20,000 |
| incoming_documents | Incoming document details | 10,000 |
| outgoing_documents | Outgoing document details | 10,000 |
| document_types | Document type definitions | 20 |
| document_flows | Document movement audit trail | 100,000 |
| tracking_numbers | Sequence tracking per year/type | 100 |
| signatory_authorities | Signing authority configuration | 50 |
| document_archives | Archived document records | 15,000 |

---

## 5. API Specifications

### 5.1 Correspondence Endpoints

#### GET /api/v1/correspondences
**Description:** List all correspondence with filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page (default: 20) |
| direction | string | No | incoming, outgoing |
| status | string | No | Filter by status |
| urgency_level | string | No | normal, urgent, emergency |
| doc_type_id | bigint | No | Filter by document type |
| department_id | bigint | No | Filter by department |
| date_from | date | No | Start date |
| date_to | date | No | End date |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "tracking_number": "IN/2024/0001",
      "direction": "incoming",
      "doc_type": {"id": 1, "name": "Official Letter"},
      "urgency_level": "urgent",
      "subject": "Tax audit notification",
      "status": "in_process",
      "created_at": "2024-04-14T08:00:00Z"
    }
  ],
  "meta": {"total": 45, "page": 1, "limit": 20}
}
```

**Permission:** `correspondence:read`

#### POST /api/v1/correspondences/incoming
**Description:** Register a new incoming document

**Request Body:**
```json
{
  "sender": "Tax Authority - Hanoi Branch",
  "sender_address": "123 Tran Hung Dao, Hanoi",
  "date_received": "2024-04-14",
  "subject": "Tax audit notification for FY2023",
  "doc_type_id": 1,
  "urgency_level": "urgent",
  "assigned_department_id": 7,
  "reference_number": "TA-2024-0456"
}
```

**Response (201 Created):**
```json
{
  "id": 46,
  "tracking_number": "IN/2024/0046",
  "status": "registered",
  "created_at": "2024-04-14T09:00:00Z"
}
```

**Permission:** `correspondence:incoming:create`

#### POST /api/v1/correspondences/outgoing
**Description:** Create a new outgoing document

**Request Body:**
```json
{
  "recipient": "Tax Authority - Hanoi Branch",
  "subject": "Response to tax audit notification",
  "doc_type_id": 1,
  "urgency_level": "urgent",
  "signatory_id": 5,
  "references_incoming_id": 46,
  "content": "We acknowledge receipt of..."
}
```

**Response (201 Created):**
```json
{
  "id": 47,
  "tracking_number": "OUT/2024/0047",
  "status": "new",
  "created_at": "2024-04-14T10:00:00Z"
}
```

**Permission:** `correspondence:outgoing:create`

#### GET /api/v1/correspondences/{id}
**Description:** Get correspondence detail with flow history

**Permission:** `correspondence:read`

#### PUT /api/v1/correspondences/{id}/status
**Description:** Update correspondence status

**Permission:** `correspondence:update`

### 5.2 Flow Endpoints

#### GET /api/v1/correspondences/{id}/flow
**Description:** Get document flow history

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "from_user": {"id": 10, "name": "Correspondence Officer"},
      "to_user": {"id": 15, "name": "Accounting Manager"},
      "action": "assign",
      "notes": "Please review and respond",
      "created_at": "2024-04-14T08:30:00Z"
    },
    {
      "id": 2,
      "from_user": {"id": 15, "name": "Accounting Manager"},
      "to_user": {"id": 16, "name": "Chief Accountant"},
      "action": "process",
      "notes": "Drafted response",
      "created_at": "2024-04-14T14:00:00Z"
    }
  ]
}
```

**Permission:** `correspondence:flow:read`

### 5.3 Signatory Endpoints

#### GET /api/v1/signatory-authorities
**Description:** List all authorized signatories

**Permission:** `correspondence:signatory:read`

#### POST /api/v1/correspondences/{id}/sign
**Description:** Sign a document

**Request Body:**
```json
{
  "signatory_id": 5,
  "signature_method": "electronic"
}
```

**Permission:** `correspondence:sign`

### 5.4 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/correspondences | List correspondence | correspondence:read |
| POST | /api/v1/correspondences/incoming | Register incoming | correspondence:incoming:create |
| POST | /api/v1/correspondences/outgoing | Create outgoing | correspondence:outgoing:create |
| GET | /api/v1/correspondences/{id} | Get detail | correspondence:read |
| PUT | /api/v1/correspondences/{id}/status | Update status | correspondence:update |
| GET | /api/v1/correspondences/{id}/flow | Flow history | correspondence:flow:read |
| GET | /api/v1/signatory-authorities | List signatories | correspondence:signatory:read |
| POST | /api/v1/correspondences/{id}/sign | Sign document | correspondence:sign |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Correspondence Officer | corr_officer | Full registry management |
| Signatory | signatory | Authorized to sign documents |
| Department Manager | dept_manager | Process documents for department |
| Document Viewer | doc_viewer | Read-only access |
| Archivist | archivist | Manage archival and retention |
| System Admin | sys_admin | Full system access |

### Permission Matrix
| Role | Register Incoming | Create Outgoing | Sign Documents | Process | View All | Archive | Manage Types |
|------|------------------|----------------|---------------|---------|---------|---------|-------------|
| Correspondence Officer | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ |
| Signatory | ❌ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ |
| Department Manager | ❌ | ✅ | ❌ | ✅ | Partial | ❌ | ❌ |
| Document Viewer | ❌ | ❌ | ❌ | ❌ | Partial | ❌ | ❌ |
| Archivist | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ |
| System Admin | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

### Row-Level Security
- Users can only view documents in their department scope unless they have global read access.
- Signatories can only sign documents within their configured authority scope.
- Processed documents are visible to the assigned processor and their manager.

---

## 7. State Machine / Workflows

### 7.1 Incoming Document Status

```
[NEW] ──register──→ [REGISTERED] ──assign──→ [IN_PROCESS] ──complete──→ [COMPLETED] ──archive──→ [ARCHIVED]
                                                                                                      │
                                                                                                      └──purge──→ [DELETED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| NEW | REGISTERED | register | Correspondence Officer | Generate tracking number, notify assigned dept |
| REGISTERED | IN_PROCESS | assign | Correspondence Officer | Assign to department/processor |
| IN_PROCESS | COMPLETED | complete | Processor | Mark as processed, create outgoing if reply needed |
| COMPLETED | ARCHIVED | archive | System (auto) | Move to archive with retention period |
| ARCHIVED | DELETED | purge | System (auto) | Permanent deletion after retention expiry |

### 7.2 Outgoing Document Status

```
[DRAFT] ──submit──→ [PENDING_APPROVAL] ──approve──→ [APPROVED] ──sign──→ [SIGNED] ──dispatch──→ [DISPATCHED]
     │                    │                                                                     │
     └──delete────────────┘                                                                     └──archive──→ [ARCHIVED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Correspondence Volume Report
**Type:** Real-time
**Purpose:** Track incoming and outgoing document volumes over time.

**Dimensions:**
- Direction (incoming/outgoing)
- Document type
- Department
- Time period

**Metrics:**
- New documents today: 3
- Total processed this month
- Average processing time (days)
- Overdue count

**Filters:**
- Date range
- Direction
- Document type
- Department

**Chart Type:** Bar chart (volume over time), Summary cards

### 8.2 Report: Processing Time Analysis
**Type:** Real-time
**Purpose:** Measure SLA compliance by urgency level and department.

**Metrics:**
- Average processing time by urgency level
- SLA compliance rate (%)
- Documents exceeding deadline
- Department processing speed comparison

**Chart Type:** Bar chart, table

### 8.3 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Correspondence Volume | Real-time | On-demand | Bar chart, cards |
| Processing Time Analysis | Real-time | On-demand | Bar chart, table |
| Signatory Activity | Real-time | On-demand | Table |
| Archive Retention Status | Scheduled | Weekly | Table, alerts |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Email Service (SMTP) | Send notifications for new/assigned documents | POST | API Key |
| M19 Workflow Service | Route documents through approval workflows | REST | JWT |
| M29 E-Signature | Electronic signing of outgoing documents | REST | OAuth2 |
| M21 Document Storage | Store scanned copies of physical documents | REST | JWT |
| M39 Operations Dashboard | Feed correspondence statistics | REST | JWT |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| correspondence.incoming.registered | {tracking_number, sender, subject, department_id} | M19 Workflow, M39 Dashboard |
| correspondence.outgoing.dispatched | {tracking_number, recipient, dispatch_method} | M21 Document Management |
| correspondence.signed | {correspondence_id, signatory_id, signed_at} | M29 E-Signature |
| correspondence.archived | {correspondence_id, retention_until} | M21 Document Management |
| correspondence.overdue | {tracking_number, assigned_to, deadline} | M06 Task Management |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| employee.departed | M01 Auth Service | Reassign pending documents to manager |
| workflow.completed | M19 Workflow | Update correspondence status to completed |
| department.restructured | M03 Org Service | Update department assignments for pending documents |

---

## 10. Technical Notes

### Performance
- Expected data volume: 20,000+ correspondence records per year
- Query complexity: Low for single document, Medium for flow history with joins
- Expected response time: <200ms for list views, <300ms for flow history
- Tracking number generation must be atomic to prevent duplicates

### Caching Strategy
- Document type definitions cached with 1-hour TTL
- Signatory authority list cached with 15-minute TTL
- Tracking number sequence cached per year/type with Redis atomic increment
- Cache invalidation on type change or signatory authority update

### File Attachments
- Supported types: pdf, docx, doc, jpg, png
- Max size: 50MB per file
- Storage: S3-compatible object storage with CDN
- Scanned physical documents stored with OCR text for searchability

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Tracking number format: `{PREFIX}/{YEAR}/{SEQUENCE}` (e.g., IN/2024/0001)
- Vietnamese document type names with English translations

### Mobile Considerations
- Mobile-optimized registry view for correspondence officers on-the-go
- Push notifications for new document assignments and urgent documents
- Signature capture on mobile for physical document signing
- Photo upload for scanning physical incoming documents
- Document preview optimized for mobile screens

### Security
- Document attachments encrypted at rest (AES-256)
- Flow audit log is append-only
- Signatory actions require re-authentication for sensitive documents
- Access to correspondence logged for compliance
- Legal hold prevents archival/deletion regardless of retention schedule

### Compliance
- Vietnam government correspondence regulations compliance
- Document retention periods aligned with national archive requirements
- Audit trail suitable for regulatory inspections
- Export capability for government reporting formats
