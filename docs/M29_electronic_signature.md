# M29 - Electronic Signature (WeSign)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_57.png, assets/image_64.png
**Dependencies:** M01 (User & Authentication), M19 (Workflow), M21 (Document Management), M22 (Correspondence)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Electronic Signature module (WeSign) provides a digital document signing platform for creating, sending, tracking, and managing electronic signature requests. It enables users to upload documents (PDF, DOCX, DOC, JPG, PNG), place signature fields, send signature requests to multiple signers, and track the signing progress through a visual workflow. The module serves as the central hub for all electronic signature needs including contracts, approvals, agreements, and official documents. The dashboard shows clear status breakdowns: Waiting for my signature (12), Waiting for others (12), Completed (168), Rejected (4). The drag-and-drop upload area supports common document formats, and the mobile app integration allows signing on-the-go. The module integrates with the Workflow module (M19) for approval-based signature requests and with the Correspondence module (M22) for officially signed outgoing documents.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Document Sender | Users who create and send signature requests | Create + track |
| Signer | Users who receive and sign documents | Sign + view |
| Signature Admin | Manages templates, certificates, and settings | Full management |
| Viewer | Users who monitor signing progress | Read-only |
| System Administrator | Configures global settings and integrations | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication), M21 (Document Management)
- **Used by:** M19 (Workflow), M22 (Correspondence), M35 (Digital Signature Service)

### Key Business Rules
1. Signature documents support PDF, DOCX, DOC, JPG, and PNG file formats with max 50MB per file.
2. Each signature request can have multiple signers with a configurable signing order (sequential or parallel).
3. Signature fields (signature, initials, date, text, checkbox) are placed on the document using a drag-and-drop editor.
4. Sequential signing: each signer must sign in order; signer N cannot sign until signer N-1 has completed.
5. Parallel signing: all signers can sign simultaneously without ordering.
6. The dashboard shows status breakdowns: Waiting for my signature (12), Waiting for others (12), Completed (168), Rejected (4).
7. Rejected documents return to the sender with rejection reason; the sender can resolve and resend.
8. Completed documents are archived with a signature certificate containing signing timestamps, IP addresses, and signer identity verification.
9. Signature certificates include: document hash, signer identity, signing timestamp, IP address, certificate ID, and audit log.
10. Email notifications are sent to signers when their turn arrives and reminders for pending signatures.
11. Signature templates can be created for recurring document types (contracts, NDAs, agreements).
12. Mobile app supports document viewing and signature capture (touch or stylus).
13. Legal compliance: electronic signatures are legally binding per Vietnam e-transaction law.

---

## 2. Screen Inventory

### 2.1 Screen: WeSign Dashboard
**Route:** `/esign/dashboard`
**Purpose:** Main dashboard showing signature request status breakdowns (img_57).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Summary Cards | Metric cards | Waiting for my sig (12), Waiting for others (12), Completed (168), Rejected (4) |
| Upload Area | Drag-and-drop zone | "Drag files here or click to browse" - supports pdf/docx/doc/jpg/png |
| Recent Activity | List | Latest signature requests with status |
| Quick Actions | Button group | Send for signature, Create template |
| Mobile App CTA | Banner | "Download mobile app to sign anywhere" |

**User Interactions:**
- View signature request statistics.
- Upload documents for signing.
- Click summary cards to filter by status.
- Access recent activity.

**Data Displayed:**
- Waiting for my signature: 12
- Waiting for others: 12
- Completed: 168
- Rejected: 4
- Supported file types: PDF, DOCX, DOC, JPG, PNG

### 2.2 Screen: Signature Request Creation
**Route:** `/esign/send`
**Purpose:** Create a new signature request with document upload and signer assignment.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Document Upload | Drag-and-drop | Upload document (pdf/docx/doc/jpg/png) |
| Document Preview | Viewer | Preview uploaded document |
| Signer Assignment | Repeater | Add signers with email, name, signing order |
| Signing Order | Toggle | Sequential or Parallel |
| Message | Textarea | Message to signers |
| Deadline | Date picker | Signing deadline |
| CC Recipients | Multi-select | Users to notify (not sign) |
| Send Button | CTA | Send signature request |

**User Interactions:**
- Upload the document to be signed.
- Add signers with their details.
- Set signing order (sequential/parallel).
- Add a message and deadline.
- Send the request.

### 2.3 Screen: Signature Field Editor
**Route:** `/esign/documents/{id}/edit`
**Purpose:** Place signature fields on the document.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Document Viewer | Full-page | Document with field placement overlay |
| Field Palette | Sidebar | Signature, Initials, Date, Text, Checkbox fields |
| Field Properties | Panel | Configure selected field (required, label, assigned signer) |
| Undo/Redo | Buttons | Edit history navigation |
| Save | Button | Save field layout |

**User Interactions:**
- Drag field types onto the document.
- Position and resize fields.
- Assign fields to specific signers.
- Configure field properties.
- Save the field layout.

### 2.4 Screen: Signing Interface
**Route:** `/esign/sign/{request_id}`
**Purpose:** Sign a document when it's your turn.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Document Viewer | Full-page | Document with signature fields highlighted |
| Signature Pad | Drawing area | Draw signature with mouse/touch/stylus |
| Signature Options | Tabs | Draw, Type, Upload signature image |
| Required Fields | Highlighted | Fields requiring action |
| Sign Button | CTA | Apply signature and submit |
| Reject Button | Button | Reject with reason |
| Decline Button | Button | Decline to sign (transfer to another) |

**User Interactions:**
- Review the document.
- Draw, type, or upload signature.
- Fill in required fields (initials, date, text).
- Sign and submit, or reject.

### 2.5 Screen: Signature Request Tracking
**Route:** `/esign/requests/{id}`
**Purpose:** Track the progress of a signature request.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Document Preview | Viewer | The document being signed |
| Signer Timeline | Vertical timeline | Each signer with status (pending, signed, rejected) |
| Audit Log | Table | Complete signing history with timestamps and IPs |
| Action Buttons | Button group | Reminder, Cancel, Download completed |
| Status Badge | Indicator | Current overall status |

**User Interactions:**
- View signing progress per signer.
- Send reminders to pending signers.
- Download completed document.
- View audit log.

### 2.6 Screen: Signature Templates
**Route:** `/esign/templates`
**Purpose:** Create and manage reusable signature templates.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Template List | Card grid | Template name, document type, field count |
| Create Template | Button | Create new template |
| Template Editor | Full-page | Document with pre-configured fields |
| Apply Template | Button | Use template for new requests |

---

## 3. Feature Specifications

### 3.1 Feature: Document Upload and Preview
**Description:** Upload documents for signing with preview and format validation.

**User Stories:**
- As a sender, I want to upload a PDF document for signing, so that I can request signatures.
- As a sender, I want to preview the uploaded document, so that I can verify it's correct before sending.

**Acceptance Criteria:**
- Given a user uploads a PDF file
- When the upload completes
- Then the document is displayed in the preview viewer

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| File Type | pdf, docx, doc, jpg, png | "File type not supported" |
| File Size | Max 50MB | "File exceeds maximum size of 50MB" |
| Document Pages | Max 100 pages | "Document exceeds maximum page count" |

**Edge Cases:**
- DOCX/DOC files are converted to PDF for consistent signing experience.
- Multi-page documents support field placement on any page.

### 3.2 Feature: Signature Field Placement
**Description:** Drag-and-drop signature field editor for placing signing fields on documents.

**User Stories:**
- As a sender, I want to place signature fields on the document, so that signers know where to sign.
- As a sender, I want to assign fields to specific signers, so that each person signs in the right place.

**Acceptance Criteria:**
- Given a document is loaded in the editor
- When the user drags a Signature field onto the document
- Then the field appears at the dropped position

**Field Types:**
- Signature (hand-drawn or uploaded)
- Initials
- Date (auto-filled)
- Text (free-form input)
- Checkbox (agreement confirmation)

**Edge Cases:**
- Fields cannot overlap; overlapping placement is rejected.
- Fields must be within document boundaries.

### 3.3 Feature: Sequential and Parallel Signing
**Description:** Support for ordered (sequential) and unordered (parallel) signing workflows.

**User Stories:**
- As a sender, I want signers to sign in a specific order, so that managers approve before executives.
- As a sender, I want all signers to sign simultaneously, so that the process is faster.

**Acceptance Criteria:**
- Given a sequential signing request with 3 signers
- When signer 1 signs
- Then signer 2 receives a notification that it's their turn

**Edge Cases:**
- If a signer rejects in sequential mode, the process stops and the sender is notified.
- In parallel mode, one rejection doesn't block others from signing.

### 3.4 Feature: Signature Capture
**Description:** Multiple methods for capturing signer's signature.

**User Stories:**
- As a signer, I want to draw my signature with my finger on mobile, so that I can sign on-the-go.
- As a signer, I want to type my name as a signature, so that I can sign quickly.
- As a signer, I want to upload an image of my signature, so that it looks authentic.

**Acceptance Criteria:**
- Given a signer selects "Draw" mode
- When they draw on the signature pad
- Then the signature is captured as an image

**Edge Cases:**
- Signature pad supports touch (mobile), mouse (desktop), and stylus input.
- Typed signatures use a cursive font for handwriting appearance.
- Uploaded signature images are validated for minimum quality.

### 3.5 Feature: Signature Certificate and Audit Log
**Description:** Generate a certificate of completion with full audit trail.

**User Stories:**
- As a sender, I want a certificate proving the document was signed, so that I have legal evidence.
- As an auditor, I want to see the complete signing history, so that I can verify the process.

**Acceptance Criteria:**
- Given all signers have completed signing
- When the request is finalized
- Then a signature certificate is generated with document hash, signer identities, timestamps, and IP addresses

**Certificate Contents:**
- Document hash (SHA-256)
- Signer full name and email
- Signing timestamp (timezone-aware)
- IP address at signing time
- Signature image
- Certificate ID
- Complete audit log

### 3.6 Feature: Dashboard Status Tracking
**Description:** Clear dashboard showing signature request status breakdowns.

**User Stories:**
- As a user, I want to see how many documents are waiting for my signature, so that I can prioritize my signing tasks.
- As a sender, I want to see completed and rejected counts, so that I can track overall progress.

**Acceptance Criteria:**
- Given the dashboard loads
- When the user views the summary cards
- Then they see: Waiting for my signature (12), Waiting for others (12), Completed (168), Rejected (4)

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+--------------------+       +--------------------+       +--------------------+
| SignatureDocument  |       | SignatureRequest   |       |      Signer        |
+--------------------+       +--------------------+       +--------------------+
| id (PK)            |<----->| id (PK)            |<----->| id (PK)            |
| title              |       | document_id (FK)   |       | request_id (FK)    |
| file_url           |       | sender_id (FK)     |       | user_id (FK)       |
| file_type          |       | signing_order      |       | email              |
| page_count         |       | status             |       | signing_order      |
| created_by (FK)    |       | deadline           |       | status             |
| created_at         |       | created_at         |       | signed_at          |
+--------------------+       +--------------------+       | signature_image    |
                                                          +--------------------+
+--------------------+       +--------------------+
| SignatureField     |       | SignatureCertificate
+--------------------+       +--------------------+
| id (PK)            |       | id (PK)            |
| request_id (FK)    |       | request_id (FK)    |
| field_type         |       | document_hash      |
| page_number        |       | certificate_id     |
| x_position         |       | created_at         |
| y_position         |       +--------------------+
| width              |
| height             |       +--------------------+
| assigned_signer_id |       | SignatureLog       |
| field_value        |       +--------------------+
+--------------------+       | id (PK)            |
                             | request_id (FK)    |
+--------------------+       | action             |
| SignatureStatus    |       | actor_id (FK)      |
+--------------------+       | ip_address         |
| id (PK)            |       | timestamp          |
| request_id (FK)    |       +--------------------+
| signer_id (FK)     |
| status             |
| created_at         |
+--------------------+
```

### 4.2 Table Schemas

#### Table: signature_documents
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| title | VARCHAR(300) | NOT NULL | Document title |
| file_url | VARCHAR(500) | NOT NULL | Original file URL |
| pdf_url | VARCHAR(500) | NULL | PDF-converted URL |
| file_type | VARCHAR(20) | NOT NULL | Original file type |
| file_size | BIGINT | NOT NULL | File size in bytes |
| page_count | INT | NOT NULL | Number of pages |
| document_hash | VARCHAR(64) | NULL | SHA-256 hash of document |
| created_by | BIGINT | NOT NULL, FK -> users.id | Uploader |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Upload timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_sig_documents_created_by ON signature_documents(created_by);
CREATE INDEX idx_sig_documents_created_at ON signature_documents(created_at DESC);
CREATE INDEX idx_sig_documents_hash ON signature_documents(document_hash);
```

#### Table: signature_requests
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| code | VARCHAR(30) | NOT NULL, UNIQUE | Request code (SIG-2024-0001) |
| document_id | BIGINT | NOT NULL, FK -> signature_documents.id | Document |
| sender_id | BIGINT | NOT NULL, FK -> users.id | Request sender |
| template_id | BIGINT | NULL, FK -> signature_templates.id | Source template |
| signing_order | ENUM('sequential','parallel') | NOT NULL, DEFAULT 'sequential' | Signing order |
| message | TEXT | NULL | Message to signers |
| deadline | DATE | NULL | Signing deadline |
| status | ENUM('draft','pending','in_progress','completed','rejected','cancelled') | NOT NULL, DEFAULT 'draft' | Request status |
| completed_at | TIMESTAMP | NULL, DEFAULT NULL | Completion timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_sig_requests_code ON signature_requests(code);
CREATE INDEX idx_sig_requests_document ON signature_requests(document_id);
CREATE INDEX idx_sig_requests_sender ON signature_requests(sender_id);
CREATE INDEX idx_sig_requests_status ON signature_requests(status);
CREATE INDEX idx_sig_requests_deadline ON signature_requests(deadline);
```

#### Table: signers
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| request_id | BIGINT | NOT NULL, FK -> signature_requests.id | Signature request |
| user_id | BIGINT | NULL, FK -> users.id | Linked user (NULL if external) |
| email | VARCHAR(200) | NOT NULL | Signer email |
| full_name | VARCHAR(200) | NOT NULL | Signer name |
| signing_order | INT | NOT NULL | Order in sequence |
| status | ENUM('pending','viewed','signed','rejected','declined','skipped') | NOT NULL, DEFAULT 'pending' | Signer status |
| signature_image_url | VARCHAR(500) | NULL | Captured signature image |
| signed_at | TIMESTAMP | NULL, DEFAULT NULL | Signing timestamp |
| rejected_reason | TEXT | NULL | Rejection reason |
| ip_address | VARCHAR(45) | NULL | IP at signing time |
| user_agent | VARCHAR(500) | NULL | Browser info at signing |
| reminder_count | INT | NOT NULL, DEFAULT 0 | Number of reminders sent |
| last_reminder_at | TIMESTAMP | NULL, DEFAULT NULL | Last reminder timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_signers_request_id ON signers(request_id);
CREATE INDEX idx_signers_user_id ON signers(user_id);
CREATE INDEX idx_signers_status ON signers(status);
CREATE INDEX idx_signers_order ON signers(request_id, signing_order);
```

#### Table: signature_fields
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| request_id | BIGINT | NOT NULL, FK -> signature_requests.id | Parent request |
| field_type | ENUM('signature','initials','date','text','checkbox') | NOT NULL | Field type |
| page_number | INT | NOT NULL | Page number (1-based) |
| x_position | DECIMAL(8,4) | NOT NULL | X coordinate (percentage) |
| y_position | DECIMAL(8,4) | NOT NULL | Y coordinate (percentage) |
| width | DECIMAL(8,4) | NOT NULL | Field width (percentage) |
| height | DECIMAL(8,4) | NOT NULL | Field height (percentage) |
| assigned_signer_id | BIGINT | NOT NULL, FK -> signers.id | Assigned signer |
| is_required | BOOLEAN | NOT NULL, DEFAULT TRUE | Required flag |
| field_value | TEXT | NULL | Filled value |
| label | VARCHAR(200) | NULL | Field label |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX sig_fields_request ON signature_fields(request_id);
CREATE INDEX sig_fields_signer ON signature_fields(assigned_signer_id);
```

#### Table: signature_certificates
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| request_id | BIGINT | NOT NULL, UNIQUE, FK -> signature_requests.id | Completed request |
| certificate_id | VARCHAR(50) | NOT NULL, UNIQUE | Certificate number |
| document_hash | VARCHAR(64) | NOT NULL | Document SHA-256 hash |
| certificate_pdf_url | VARCHAR(500) | NULL | Certificate PDF URL |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Certificate generation time |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_sig_certificates_request ON signature_certificates(request_id);
CREATE UNIQUE INDEX idx_sig_certificates_code ON signature_certificates(certificate_id);
```

#### Table: signature_logs
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| request_id | BIGINT | NOT NULL, FK -> signature_requests.id | Parent request |
| signer_id | BIGINT | NULL, FK -> signers.id | Related signer |
| action | ENUM('created','sent','viewed','signed','rejected','declined','reminder','cancelled','completed') | NOT NULL | Action type |
| actor_id | BIGINT | NOT NULL, FK -> users.id | Acting user |
| ip_address | VARCHAR(45) | NULL | IP address |
| user_agent | VARCHAR(500) | NULL | Browser info |
| details | JSON | NULL | Additional details |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Action timestamp |

**Indexes:**
```sql
CREATE INDEX idx_sig_logs_request ON signature_logs(request_id);
CREATE INDEX idx_sig_logs_signer ON signature_logs(signer_id);
CREATE INDEX idx_sig_logs_created_at ON signature_logs(created_at);
```

#### Table: signature_templates
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Template name |
| document_url | VARCHAR(500) | NOT NULL | Template document URL |
| field_definitions | JSON | NOT NULL | Pre-configured field positions |
| signer_roles | JSON | NOT NULL | Signer role definitions |
| created_by | BIGINT | NOT NULL, FK -> users.id | Creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_sig_templates_created_by ON signature_templates(created_by);
```

#### Table: signature_statuses
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| request_id | BIGINT | NOT NULL, FK -> signature_requests.id | Request |
| signer_id | BIGINT | NOT NULL, FK -> signers.id | Signer |
| status | ENUM('pending','viewed','signed','rejected','declined') | NOT NULL, DEFAULT 'pending' | Current status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Status change timestamp |

**Indexes:**
```sql
CREATE INDEX idx_sig_statuses_request ON signature_statuses(request_id);
CREATE INDEX idx_sig_statuses_signer ON signature_statuses(signer_id);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| signature_documents | Uploaded documents | 5,000 |
| signature_requests | Signature request records | 10,000 |
| signers | Signer assignments and results | 30,000 |
| signature_fields | Field placement on documents | 50,000 |
| signature_certificates | Completion certificates | 8,000 |
| signature_logs | Complete audit trail | 200,000 |
| signature_templates | Reusable templates | 200 |
| signature_statuses | Signer status tracking | 30,000 |

---

## 5. API Specifications

### 5.1 Document Endpoints

#### POST /api/v1/esign/documents
**Description:** Upload a document for signing

**Request Body:** (multipart/form-data)
```
file: [binary PDF/DOCX/DOC/JPG/PNG]
title: "Employment Contract"
```

**Response (201 Created):**
```json
{
  "id": 1,
  "title": "Employment Contract",
  "file_type": "pdf",
  "page_count": 5,
  "document_hash": "a1b2c3d4...",
  "created_at": "2024-04-14T08:00:00Z"
}
```

**Permission:** `esign:document:upload`

### 5.2 Request Endpoints

#### POST /api/v1/esign/requests
**Description:** Create a signature request

**Request Body:**
```json
{
  "document_id": 1,
  "signing_order": "sequential",
  "message": "Please review and sign this contract",
  "deadline": "2024-04-21",
  "signers": [
    {"email": "employee@company.com", "full_name": "Nguyen Van A", "signing_order": 1},
    {"email": "hr@company.com", "full_name": "HR Manager", "signing_order": 2}
  ],
  "fields": [
    {"field_type": "signature", "page": 5, "x": 10, "y": 80, "width": 30, "height": 10, "signer_order": 1},
    {"field_type": "date", "page": 5, "x": 50, "y": 80, "width": 15, "height": 10, "signer_order": 1}
  ]
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "code": "SIG-2024-0001",
  "status": "pending",
  "created_at": "2024-04-14T09:00:00Z"
}
```

**Permission:** `esign:request:create`

#### GET /api/v1/esign/requests
**Description:** List signature requests with status filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page |
| status | string | No | pending, in_progress, completed, rejected |
| filter | string | No | waiting_for_me, waiting_for_others, completed, rejected |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "code": "SIG-2024-0001",
      "document": {"title": "Employment Contract", "file_type": "pdf"},
      "sender": {"id": 10, "name": "HR Department"},
      "status": "in_progress",
      "signers": [
        {"name": "Nguyen Van A", "status": "signed", "signed_at": "2024-04-14T10:00:00Z"},
        {"name": "HR Manager", "status": "pending"}
      ],
      "deadline": "2024-04-21",
      "created_at": "2024-04-14T09:00:00Z"
    }
  ],
  "meta": {"total": 196, "page": 1, "limit": 20}
}
```

**Permission:** `esign:request:read`

#### GET /api/v1/esign/dashboard/stats
**Description:** Get dashboard statistics

**Response (200 OK):**
```json
{
  "waiting_for_me": 12,
  "waiting_for_others": 12,
  "completed": 168,
  "rejected": 4,
  "total": 196
}
```

**Permission:** `esign:dashboard:read`

### 5.3 Signing Endpoints

#### POST /api/v1/esign/requests/{id}/sign
**Description:** Sign a document

**Request Body:**
```json
{
  "signer_id": 1,
  "signature_image_base64": "data:image/png;base64,...",
  "field_values": {
    "field_1": "signed",
    "field_2": "2024-04-14"
  }
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "signer_status": "signed",
  "request_status": "completed"
}
```

**Permission:** `esign:sign`

#### POST /api/v1/esign/requests/{id}/reject
**Description:** Reject a signature request

**Request Body:**
```json
{
  "signer_id": 1,
  "reason": "Contract terms need revision"
}
```

**Permission:** `esign:reject`

### 5.4 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| POST | /api/v1/esign/documents | Upload document | esign:document:upload |
| POST | /api/v1/esign/requests | Create request | esign:request:create |
| GET | /api/v1/esign/requests | List requests | esign:request:read |
| GET | /api/v1/esign/requests/{id} | Get request detail | esign:request:read |
| POST | /api/v1/esign/requests/{id}/sign | Sign document | esign:sign |
| POST | /api/v1/esign/requests/{id}/reject | Reject request | esign:reject |
| GET | /api/v1/esign/dashboard/stats | Dashboard stats | esign:dashboard:read |
| GET | /api/v1/esign/requests/{id}/certificate | Get certificate | esign:certificate:read |

---

## 6. Permissions Matrix

### Roles
| Role | Upload Document | Create Request | Sign | Reject | View All | Manage Templates |
|------|---------------|---------------|------|--------|---------|-----------------|
| Employee | ✅ | ✅ | ✅ | ✅ | Own only | ❌ |
| Signature Admin | ✅ | ✅ | ✅ | ✅ | All | ✅ |
| System Admin | ✅ | ✅ | ✅ | ✅ | All | ✅ |

### Row-Level Security
- Users can only view requests they sent or are assigned to sign.
- Signature admins can view all requests across the organization.
- Signers can only access their assigned signing link.

---

## 7. State Machine / Workflows

### 7.1 Request Status Transitions

```
[DRAFT] ──send──→ [PENDING] ──first_sign──→ [IN_PROGRESS] ──all_signed──→ [COMPLETED]
     │                  │                        │
     │                  │                        ├──reject──→ [REJECTED]
     │                  │
     └──delete──────────┘
                                                │
                                                └──cancel──→ [CANCELLED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | PENDING | send | Sender | Notify first signer (sequential) or all (parallel) |
| PENDING | IN_PROGRESS | first signature | First signer | Notify next signer (sequential) |
| IN_PROGRESS | COMPLETED | all signers complete | System | Generate certificate, notify sender |
| IN_PROGRESS | REJECTED | any signer rejects | Signer | Notify sender with rejection reason |
| PENDING | CANCELLED | cancel | Sender | Notify all pending signers |

### 7.2 Signer Status Transitions

```
[PENDING] ──open──→ [VIEWED] ──sign──→ [SIGNED]
   │                    │
   │                    ├──reject──→ [REJECTED]
   │                    │
   │                    └──decline──→ [DECLINED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Signature Activity Report
**Type:** Real-time
**Purpose:** Track signature request volume and completion rates.

**Dimensions:**
- Time period
- Sender
- Document type

**Metrics:**
- Waiting for my signature: 12
- Waiting for others: 12
- Completed: 168
- Rejected: 4
- Average completion time (days)
- Rejection rate (%)

**Filters:**
- Date range
- Sender
- Status

**Chart Type:** Summary cards, bar chart

### 8.2 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Signature Activity | Real-time | On-demand | Cards, bar chart |
| Completion Time Analysis | Real-time | On-demand | Line chart |
| Signer Performance | Real-time | On-demand | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Email Service (SMTP/SendGrid) | Send signing invitations and reminders | POST | API Key |
| Document Conversion (LibreOffice) | Convert DOCX/DOC to PDF | Local | N/A |
| M01 Notification Service | Push notifications for signing tasks | REST | JWT |
| M19 Workflow Service | Trigger signature requests from workflows | REST | JWT |
| M22 Correspondence | Attach signed documents to outgoing correspondence | REST | JWT |
| M35 Digital Signature | Apply cryptographic signature to certificate | REST | OAuth2 |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| esign.request.created | {request_id, document_id, sender_id, signer_count} | M19 Workflow, M22 Correspondence |
| esign.document.signed | {request_id, signer_id, signed_at} | M19 Workflow, M22 Correspondence |
| esign.request.completed | {request_id, certificate_id, all_signers} | M21 Document Management, M22 Correspondence |
| esign.request.rejected | {request_id, signer_id, reason} | M19 Workflow |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| workflow.approved | M19 Workflow | Create signature request for approved document |
| correspondence.approved | M22 Correspondence | Route outgoing document for signing |

---

## 10. Technical Notes

### Performance
- Expected data volume: 10,000+ signature requests, 30,000+ signer records
- Query complexity: Low for single request, Medium for dashboard aggregation
- Expected response time: <200ms for list views, <500ms for certificate generation
- Document conversion (DOCX to PDF) processed asynchronously

### Caching Strategy
- Dashboard statistics cached with 2-minute TTL (Redis)
- Document previews cached with 1-hour TTL
- Certificate PDFs cached permanently (immutable after generation)
- Cache invalidation on request status change or new signature

### File Storage
- Original documents stored in S3 with encryption
- PDF-converted documents stored separately
- Signature images stored as base64 in DB (small size)
- Certificate PDFs stored in S3 with public read (signed URLs)
- Retention: documents kept for 5 years minimum per legal requirements

### Security
- Document hash (SHA-256) ensures document integrity after signing
- All signing actions logged with IP address and user agent
- Signing links are single-use tokens with expiration
- Re-authentication required before signing (confirm identity)
- Certificate of completion is tamper-evident (hash-based)
- Signed documents are immutable; cannot be modified after completion

### Mobile Considerations
- Touch-friendly signature pad with pressure sensitivity
- Signature capture optimized for small screens
- Document zoom and pan for field placement
- Push notifications for signing requests
- Offline viewing of pending documents (read-only)
- Mobile app download CTA on dashboard (img_57)

### Legal Compliance
- Vietnam e-transaction law compliance
- Electronic signatures legally binding for most document types
- Certificate of completion suitable as legal evidence
- Audit trail meets regulatory requirements
- Exclusions: certain document types may require physical signatures
