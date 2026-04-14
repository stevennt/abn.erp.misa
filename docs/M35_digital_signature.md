# M35 - Digital Signature (Chu ky so)

**Category:** Digital Office
**Priority:** P3
**Reference Images:** assets/image_26.png
**Dependencies:** M33 (E-Invoice), M01 (Authentication), M40 (Group Management)
**Generated:** April 14, 2026

---

## 1. Module Overview

The Digital Signature module (MISA ESign) provides certificate-based electronic signature capabilities for legally binding document signing across the ABN ERP platform. It manages the complete lifecycle of digital certificates -- from issuance and configuration through active use, renewal, and revocation -- enabling users to sign e-invoices, contracts, approval documents, and official correspondence with cryptographic security that meets Vietnamese regulatory requirements (Law on Electronic Transactions 2005, Decree 130/2018/ND-CP).

The MISA ESign dashboard (image_26) displays a certificate tree visualization showing the hierarchical relationship between certificate authorities, organizational certificates, and individual user certificates. The owner information panel shows certificate holder details including organization name, tax code, and authorized representative information. The certificate ID panel displays the unique certificate serial number issued by the certificate authority. The validity section shows the certificate issue date, expiration date, and remaining validity period with visual indicator. The support contact information (090 488 5833) is prominently displayed for certificate-related assistance. The module integrates with the E-Invoice module (M33) for invoice signing, the Contract Management module (M11) for contract execution, and the Workflow module (M19) for approval document signing.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| System Administrator | Manages certificate enrollment, configuration, and provider integration | Full access to certificate management and configuration |
| Certificate Owner | Uses their certificate for document signing | Read/write own certificate, sign capability |
| Finance Manager | Oversees certificate usage for financial documents | Read access to all certificates, signing capability |
| Legal Officer | Reviews certificate compliance and validity | Read-only access to all certificate data |
| Support Staff | Handles certificate issues and renewal requests | Read access, support ticket creation |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M33 - E-Invoice | E-invoices require digital signature before issuance |
| M11 - Contract Management | Contracts are digitally signed using certificates |
| M19 - Workflow | Approval documents signed with digital certificates |
| M01 - Authentication | User identity linked to certificate ownership |
| M40 - Group Management | Parent company certificates used for subsidiary document signing |

### Key Business Rules

1. Each digital certificate is bound to a specific individual or organization and cannot be transferred to another party.
2. Certificates have a fixed validity period (typically 1-3 years) and must be renewed before expiration; expired certificates cannot be used for signing.
3. A certificate can be revoked by the owner or the certificate authority in case of compromise, employee departure, or organizational change.
4. Documents signed with a certificate retain their legal validity even after the certificate is revoked or expired, as the signature was valid at the time of signing.
5. Certificate private keys must be stored in secure hardware tokens (USB tokens) or secure cloud HSM; they must never be exported in plain text.
6. Each signing operation creates an immutable audit trail including document hash, certificate serial number, timestamp, and signer identity.
7. Organizational certificates follow a hierarchy: root certificate (company level) -> department certificates -> individual user certificates.
8. Certificate usage is restricted to authorized document types as configured per certificate; a certificate designated for e-invoice signing cannot be used for contracts unless explicitly permitted.
9. Certificate renewal must be initiated at least 30 days before expiration to ensure continuity of signing capability.
10. All certificate operations comply with Vietnamese Law on Electronic Transactions and Decree 130/2018/ND-CP.

---

## 2. Screen Inventory

### 2.1 Screen: Certificate Dashboard

**Route:** `/digital-signature/dashboard` | **Purpose:** Display certificate overview, validity status, and usage statistics.

| UI Component | Description |
|--------------|-------------|
| Certificate Count | Total active, expiring soon, expired, revoked certificates |
| Certificate Tree | Hierarchical visualization of CA -> Org -> Individual certificates |
| Owner Info Panel | Certificate holder name, organization, tax code, authorized representative |
| Certificate ID Panel | Serial number, issuer name, certificate type |
| Validity Section | Issue date, expiration date, days remaining, visual progress bar |
| Support Contact | Phone: 090 488 5833, email, business hours |
| Usage Statistics | Documents signed this month, total documents signed |

**User Interactions:**
- Click certificate tree nodes to view certificate details
- View validity countdown and expiration warnings
- Click support contact to initiate support request
- View usage statistics and signing history

**Data Displayed:**
- Certificate hierarchy and relationships
- Owner and identification information
- Validity period and remaining days
- Support contact details
- Signing activity metrics

### 2.2 Screen: Certificate List

**Route:** `/digital-signature/certificates` | **Purpose:** Browse, search, and manage all digital certificates.

| UI Component | Description |
|--------------|-------------|
| Certificate List Table | Columns: Serial Number, Owner, Type, Status, Expiry Date, Issuer |
| Status Filters | Active, Expiring Soon (30 days), Expired, Revoked |
| Type Filters | Organization, Individual, Server |
| Search | Search by serial number, owner name, organization |

**User Interactions:**
- Search certificates by serial number or owner
- Filter by status and type
- Click certificate to view full details
- Initiate renewal or revocation actions

**Data Displayed:**
- Certificate identification (serial, owner, type)
- Status and expiration information
- Issuing certificate authority

### 2.3 Screen: Certificate Detail

**Route:** `/digital-signature/certificates/{id}` | **Purpose:** View and manage a single certificate with full details.

| UI Component | Description |
|--------------|-------------|
| Certificate Header | Serial number, type badge, status indicator |
| Owner Information | Organization name, tax code, address, authorized representative |
| Certificate Details | Issuer, issue date, expiry date, algorithm, key length |
| Validity Timeline | Visual timeline showing issue date, current date, expiry date |
| Usage Log | Recent signing operations with document references |
| Action Bar | Renew, Revoke, Download Certificate, View Public Key |

**User Interactions:**
- View complete certificate information
- Initiate renewal process (30+ days before expiry)
- Revoke certificate with reason
- Download certificate file (public key only)
- View public key details

**Data Displayed:**
- Full certificate chain information
- Owner identification data
- Validity period with visual timeline
- Signing usage history
- Technical details (algorithm, key length)

### 2.4 Screen: Certificate Enrollment

**Route:** `/digital-signature/enroll` | **Purpose:** Request new digital certificate from certificate authority.

| UI Component | Description |
|--------------|-------------|
| Enrollment Form | Organization info, owner info, certificate type, validity period |
| Document Upload | Required supporting documents (business license, ID, authorization letter) |
| Provider Selection | Select certificate authority provider |
| Payment Information | Certificate issuance fee |
| Status Tracker | Application progress: submitted, under review, approved, issued |

**User Interactions:**
- Complete enrollment form with required information
- Upload supporting documentation
- Select certificate authority and validity period
- Track application status
- Receive notification when certificate is issued

**Data Displayed:**
- Form fields and validation status
- Application progress tracker
- Provider options and pricing

### 2.5 Screen: Certificate Revocation

**Route:** `/digital-signature/revocations` | **Purpose:** Request certificate revocation and track revocation status.

| UI Component | Description |
|--------------|-------------|
| Revocation Request Form | Certificate selection, reason category, detailed reason |
| Revocation History | List of past revocations with status |
| Reason Categories | Compromise, cessation of operation, affiliation changed, superseded |

**User Interactions:**
- Select certificate to revoke
- Choose revocation reason category
- Provide detailed explanation
- Submit revocation request
- Track revocation processing status

**Data Displayed:**
- Revocation request details
- Processing status and timeline
- Revocation certificate (CRL) reference

### 2.6 Screen: Signing Operations

**Route:** `/digital-signature/signing` | **Purpose:** Sign documents and view signing history.

| UI Component | Description |
|--------------|-------------|
| Document Upload | Upload document for signing |
| Certificate Selector | Choose certificate for signing |
| Signing Confirmation | Review document hash and certificate info before signing |
| Signing History | List of past signing operations with document references |
| Signature Verification | Upload signed document to verify signature validity |

**User Interactions:**
- Upload document and select certificate
- Review and confirm signing operation
- View signing history with document links
- Verify signatures on received documents

**Data Displayed:**
- Document information and hash
- Certificate used for signing
- Signing timestamp and location
- Signature verification result

---

## 3. Feature Specifications

### 3.1 Feature: Certificate Management
**Description:** Manage the complete lifecycle of digital certificates including enrollment, activation, renewal, and revocation.

**User Stories:**
- As a System Administrator, I want to enroll new certificates from approved certificate authorities, so that users can sign documents.
- As a Certificate Owner, I want to renew my certificate before expiration, so that my signing capability is not interrupted.

**Acceptance Criteria:**
- Given a certificate enrollment request, when all required documents are uploaded and fees are paid, then the request is submitted to the certificate authority.
- Given an active certificate approaching expiration, when the user initiates renewal 30+ days before expiry, then a new certificate application is started.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Organization tax code | Valid 10/13 digit format | Tax code must be 10 or 13 digits |
| Owner ID | Valid national ID or passport | Please enter a valid identification number |
| Validity period | 1, 2, or 3 years | Validity period must be 1, 2, or 3 years |
| Supporting documents | All required documents uploaded | Please upload all required supporting documents |

**Edge Cases:**
- Expired certificates cannot be renewed; a new enrollment is required
- Revoked certificates cannot be renewed or reactivated
- Certificate authority processing delays may extend enrollment timeline

### 3.2 Feature: Document Signing
**Description:** Sign documents digitally using certificate-based cryptographic signatures.

**User Stories:**
- As a Finance Manager, I want to sign e-invoices digitally, so that they are legally valid and compliant.
- As a Legal Officer, I want to sign contracts digitally, so that they are binding and tamper-evident.

**Acceptance Criteria:**
- Given a document is uploaded for signing, when the user selects their certificate and confirms, then the document is signed and a signature record is created.
- Given a signed document, when the signature is verified, then the document integrity and signer identity are confirmed.

**Edge Cases:**
- Signing with an expired or revoked certificate is rejected with clear error message
- Large documents (>50MB) are processed asynchronously with progress notification
- Network interruption during signing operation is handled gracefully with rollback

### 3.3 Feature: Signature Verification
**Description:** Verify digital signatures on received documents to confirm authenticity and integrity.

**User Stories:**
- As a Customer, I want to verify the signature on an e-invoice, so that I can confirm it is authentic.
- As a Legal Officer, I want to verify signed contracts, so that I can ensure document integrity.

**Acceptance Criteria:**
- Given a signed document is uploaded for verification, when the verification process runs, then the signature validity, certificate status, and document integrity are reported.
- Given a document with a revoked certificate signature, when verified, then the result indicates the certificate was revoked but the signature was valid at signing time.

**Edge Cases:**
- Documents with expired certificates still show valid signature if signed during validity period
- Tampered documents fail integrity check with specific error details
- Unknown certificate authorities trigger a warning but do not invalidate the signature

### 3.4 Feature: Certificate Revocation
**Description:** Revoke certificates when they are compromised, no longer needed, or owner affiliation changes.

**User Stories:**
- As a System Administrator, I want to revoke a certificate when an employee leaves, so that unauthorized signing is prevented.
- As a Certificate Owner, I want to revoke my certificate if the private key is compromised, so that misuse is prevented.

**Acceptance Criteria:**
- Given a revocation request is submitted, when the reason is validated, then the certificate is added to the Certificate Revocation List (CRL) and the CA is notified.
- Given a certificate is revoked, when a signing operation is attempted, then it is rejected with a revoked certificate error.

**Edge Cases:**
- Revocation is irreversible; a new certificate must be enrolled
- Documents signed before revocation remain legally valid
- Revocation propagation to all verification systems may take up to 24 hours

### 3.5 Feature: Certificate Usage Tracking
**Description:** Track and report on certificate usage including signing operations, document types, and frequency.

**User Stories:**
- As a System Administrator, I want to see how each certificate is being used, so that I can detect unauthorized usage.
- As an Auditor, I want to review signing history, so that I can verify compliance.

**Acceptance Criteria:**
- Given a signing operation occurs, when the operation completes, then the event is logged with certificate, document hash, timestamp, and signer.
- Given a user views certificate usage, when the report is generated, then all signing operations are listed chronologically.

**Edge Cases:**
- Signing logs are immutable and cannot be modified or deleted
- High-volume signing operations are batched for logging efficiency
- Usage reports can be filtered by certificate, date range, and document type

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+-------------------+       +-------------------+       +-------------------+
| DigitalCertificate|       | CertificateOwner  |       | CertificateProvider|
+-------------------+       +-------------------+       +-------------------+
| id (PK)           |<----->| id (PK)           |       | id (PK)           |
| serial_number     |       | certificate_id(FK)|       | name              |
| type              |       | organization_name |       | country           |
| status            |       | tax_code          |       | website           |
| issuer_id(FK)     |       | representative    |       | contact_phone     |
| issue_date        |       | contact_email     |       | is_active         |
| expiry_date       |       +-------------------+       +-------------------+
| algorithm         |
| key_length        |                |
+-------------------+                |
        |                            v
        v                   +-------------------+
+-------------------+       | CertificateValidity|
|CertificateRevocation|     +-------------------+
+-------------------+       | id (PK)           |
| id (PK)           |       | certificate_id(FK)|
| certificate_id(FK)|       | issue_date        |
| reason            |       | expiry_date       |
| revocation_date   |       | days_remaining    |
| crl_number        |       | status            |
| created_at        |       | created_at        |
+-------------------+       +-------------------+
        |
        v
+-------------------+       +-------------------+
| CertificateUsage  |       | SigningOperation  |
+-------------------+       +-------------------+
| id (PK)           |       | id (PK)           |
| certificate_id(FK)|       | certificate_id(FK)|
| document_type     |       | document_hash     |
| document_id       |       | document_type     |
| signed_at         |       | signed_at         |
| signer_id(FK)     |       | signer_id(FK)     |
+-------------------+       | signature_data    |
                            +-------------------+
```

### 4.2 Table Schemas

#### Table: digital_certificate
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| serial_number | VARCHAR(100) | NOT NULL, UNIQUE | Certificate serial number |
| type | VARCHAR(30) | NOT NULL | Type: organization, individual, server |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'pending' | Status: pending, active, expired, revoked |
| provider_id | BIGINT | FK -> certificate_provider.id | Issuing provider |
| owner_id | BIGINT | FK -> certificate_owner.id | Certificate owner |
| issue_date | DATE | NOT NULL | Certificate issue date |
| expiry_date | DATE | NOT NULL | Certificate expiration date |
| algorithm | VARCHAR(30) | DEFAULT 'RSA' | Signing algorithm |
| key_length | INT | DEFAULT 2048 | Key length in bits |
| certificate_file_url | VARCHAR(500) | NULL | Certificate file URL |
| public_key | TEXT | NULL | Public key data |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_cert_serial ON digital_certificate(serial_number);
CREATE INDEX idx_cert_status ON digital_certificate(status);
CREATE INDEX idx_cert_type ON digital_certificate(type);
CREATE INDEX idx_cert_owner ON digital_certificate(owner_id);
CREATE INDEX idx_cert_expiry ON digital_certificate(expiry_date);
CREATE INDEX idx_cert_provider ON digital_certificate(provider_id);
```

#### Table: certificate_owner
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| certificate_id | BIGINT | FK -> digital_certificate.id, NOT NULL | Associated certificate |
| organization_name | VARCHAR(300) | NOT NULL | Organization name |
| tax_code | VARCHAR(20) | NULL | Organization tax code |
| address | TEXT | NULL | Organization address |
| representative_name | VARCHAR(200) | NOT NULL | Authorized representative |
| representative_id | VARCHAR(50) | NULL | Representative ID number |
| contact_email | VARCHAR(200) | NULL | Contact email |
| contact_phone | VARCHAR(30) | NULL | Support phone: 090 488 5833 |
| user_id | BIGINT | FK -> users.id | Linked system user |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_cert_owner_cert ON certificate_owner(certificate_id);
CREATE INDEX idx_cert_owner_user ON certificate_owner(user_id);
CREATE INDEX idx_cert_owner_tax ON certificate_owner(tax_code);
```

#### Table: certificate_provider
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Provider name |
| country | VARCHAR(50) | DEFAULT 'Vietnam' | Country |
| website | VARCHAR(255) | NULL | Provider website |
| contact_phone | VARCHAR(30) | NULL | Contact phone |
| contact_email | VARCHAR(200) | NULL | Contact email |
| api_config_json | JSON | NULL | API integration settings |
| is_active | TINYINT(1) | DEFAULT 1 | Active flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_cert_provider_active ON certificate_provider(is_active);
```

#### Table: certificate_validity
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| certificate_id | BIGINT | FK -> digital_certificate.id, NOT NULL | Associated certificate |
| issue_date | DATE | NOT NULL | Issue date |
| expiry_date | DATE | NOT NULL | Expiration date |
| days_remaining | INT | NULL | Calculated remaining days |
| status | VARCHAR(30) | NOT NULL | Status: valid, expiring_soon, expired |
| notification_sent | TINYINT(1) | DEFAULT 0 | Expiry notification sent flag |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_cert_validity_cert ON certificate_validity(certificate_id);
CREATE INDEX idx_cert_validity_status ON certificate_validity(status);
CREATE INDEX idx_cert_validity_remaining ON certificate_validity(days_remaining);
```

#### Table: certificate_revocation
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| certificate_id | BIGINT | FK -> digital_certificate.id, NOT NULL | Revoked certificate |
| reason | VARCHAR(50) | NOT NULL | Reason: compromise, cessation, affiliation_changed, superseded |
| reason_detail | TEXT | NULL | Detailed explanation |
| revocation_date | TIMESTAMP | NOT NULL, DEFAULT NOW() | Revocation timestamp |
| crl_number | VARCHAR(100) | NULL | Certificate Revocation List number |
| requested_by | BIGINT | FK -> users.id | Revocation requester |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_cert_revocation_cert ON certificate_revocation(certificate_id);
CREATE INDEX idx_cert_revocation_reason ON certificate_revocation(reason);
CREATE INDEX idx_cert_revocation_date ON certificate_revocation(revocation_date);
```

#### Table: certificate_usage
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| certificate_id | BIGINT | FK -> digital_certificate.id, NOT NULL | Used certificate |
| document_type | VARCHAR(50) | NOT NULL | Document type: einvoice, contract, approval, other |
| document_id | BIGINT | NULL | Referenced document ID |
| document_hash | VARCHAR(255) | NULL | Document hash |
| signed_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Signing timestamp |
| signer_id | BIGINT | FK -> users.id | User who signed |
| ip_address | VARCHAR(45) | NULL | Signer IP address |

**Indexes:**
```sql
CREATE INDEX idx_cert_usage_cert ON certificate_usage(certificate_id);
CREATE INDEX idx_cert_usage_type ON certificate_usage(document_type);
CREATE INDEX idx_cert_usage_date ON certificate_usage(signed_at);
CREATE INDEX idx_cert_usage_signer ON certificate_usage(signer_id);
```

#### Table: signing_operation
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| certificate_id | BIGINT | FK -> digital_certificate.id, NOT NULL | Signing certificate |
| document_hash | VARCHAR(255) | NOT NULL | SHA-256 hash of signed document |
| document_type | VARCHAR(50) | NOT NULL | Type of document signed |
| document_reference | VARCHAR(200) | NULL | Document reference number |
| signature_data | TEXT | NOT NULL | Encoded signature data |
| signed_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Signing timestamp |
| signer_id | BIGINT | FK -> users.id | Signing user |
| signer_name | VARCHAR(200) | NULL | Signer display name |
| location | VARCHAR(200) | NULL | Signing location |
| reason | VARCHAR(200) | NULL | Signing reason |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_signing_op_cert ON signing_operation(certificate_id);
CREATE INDEX idx_signing_op_date ON signing_operation(signed_at);
CREATE INDEX idx_signing_op_signer ON signing_operation(signer_id);
CREATE INDEX idx_signing_op_doc_type ON signing_operation(document_type);
CREATE INDEX idx_signing_op_hash ON signing_operation(document_hash);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| digital_certificate | Certificate records | 1,000 |
| certificate_owner | Certificate owner information | 1,000 |
| certificate_provider | CA provider configurations | 10 |
| certificate_validity | Validity tracking | 2,000 |
| certificate_revocation | Revocation records | 200 |
| certificate_usage | Usage tracking | 500,000 |
| signing_operation | Detailed signing audit log | 500,000 |

---

## 5. API Specifications

### 5.1 Certificate Endpoints

#### GET /api/v1/digital-signature/certificates
**Description:** List certificates with filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page |
| status | string | No | Filter by status |
| type | string | No | Filter by type |
| owner | string | No | Filter by owner name |

**Permission:** `dsig:certificate:read`

#### POST /api/v1/digital-signature/certificates/enroll
**Description:** Enroll new certificate

**Permission:** `dsig:certificate:enroll`

#### GET /api/v1/digital-signature/certificates/{id}
**Description:** Get certificate details

**Permission:** `dsig:certificate:read`

#### POST /api/v1/digital-signature/certificates/{id}/renew
**Description:** Renew certificate

**Permission:** `dsig:certificate:renew`

#### POST /api/v1/digital-signature/certificates/{id}/revoke
**Description:** Revoke certificate

**Request Body:**
```json
{
  "reason": "compromise",
  "reason_detail": "Private key potentially compromised"
}
```

**Permission:** `dsig:certificate:revoke`

### 5.2 Signing Endpoints

#### POST /api/v1/digital-signature/sign
**Description:** Sign a document

**Request Body:**
```json
{
  "certificate_id": 1,
  "document_hash": "sha256:abc123...",
  "document_type": "einvoice",
  "document_reference": "INV-2026-0001",
  "reason": "Invoice issuance"
}
```

**Response (200 OK):**
```json
{
  "signing_id": 5001,
  "signature_data": "base64_encoded_signature...",
  "signed_at": "2026-04-14T10:30:00Z",
  "certificate_serial": "ABC123456"
}
```

**Permission:** `dsig:sign:execute`

#### POST /api/v1/digital-signature/verify
**Description:** Verify a document signature

**Permission:** `dsig:verify:execute`

### 5.3 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/digital-signature/certificates | List certificates | dsig:certificate:read |
| POST | /api/v1/digital-signature/certificates/enroll | Enroll certificate | dsig:certificate:enroll |
| GET | /api/v1/digital-signature/certificates/{id} | Get certificate | dsig:certificate:read |
| POST | /api/v1/digital-signature/certificates/{id}/renew | Renew certificate | dsig:certificate:renew |
| POST | /api/v1/digital-signature/certificates/{id}/revoke | Revoke certificate | dsig:certificate:revoke |
| POST | /api/v1/digital-signature/sign | Sign document | dsig:sign:execute |
| POST | /api/v1/digital-signature/verify | Verify signature | dsig:verify:execute |
| GET | /api/v1/digital-signature/signing-operations | List signing ops | dsig:certificate:read |
| GET | /api/v1/digital-signature/providers | List providers | dsig:provider:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| System Administrator | system_admin | Full certificate management and configuration |
| Certificate Owner | cert_owner | Use own certificate for signing |
| Finance Manager | finance_manager | Sign financial documents |
| Legal Officer | legal_officer | Read access for compliance review |
| Auditor | auditor | Read access to all signing operations |

### Permission Matrix
| Role | Enroll | Read | Renew | Revoke | Sign | Verify | View Logs |
|------|--------|------|-------|--------|------|--------|-----------|
| System Administrator | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Certificate Owner | No | Own only | No | Own only | Yes | Yes | Own only |
| Finance Manager | No | Yes | No | No | Yes | Yes | Yes |
| Legal Officer | No | Yes | No | No | No | Yes | Yes |
| Auditor | No | Yes | No | No | No | Yes | Yes |

### Row-Level Security
- Certificate Owners can only view and use their own certificates.
- Finance Managers and Auditors have read access to all certificates but can only sign with certificates assigned to them.
- System Administrators have full access across all certificates and organizations.

---

## 7. State Machine / Workflows

### 7.1 Certificate Status Transitions

```
[PENDING] ──issue──→ [ACTIVE] ──expire──→ [EXPIRED]
   │                    │
   │                    └──revoke──→ [REVOKED]
   │
   └──reject──→ [REJECTED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| PENDING | ACTIVE | issue | Certificate Authority | Certificate becomes usable for signing |
| PENDING | REJECTED | reject | Certificate Authority | Enrollment denied |
| ACTIVE | EXPIRED | expire | System | Certificate can no longer sign |
| ACTIVE | REVOKED | revoke | Owner/Admin | Certificate added to CRL |

### 7.2 Signing Operation Workflow

```
[UPLOAD] ──hash──→ [REVIEW] ──confirm──→ [SIGN] ──complete──→ [SIGNED]
                        │
                        └──cancel──→ [CANCELLED]
```

### 7.3 Revocation Workflow

```
[REQUESTED] ──validate──→ [APPROVED] ──revoke──→ [REVOKED]
     │                         │
     └──reject──→ [REJECTED]   └──notify──→ [CRL_UPDATED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Certificate Inventory
**Type:** Real-time
**Purpose:** List all certificates with status, validity, and ownership

**Metrics:**
- Certificate count by status
- Expiring soon count
- Revoked count

**Chart Type:** Table, Donut chart

### 8.2 Report: Signing Activity
**Type:** Real-time
**Purpose:** Track signing operations by certificate, document type, and user

**Metrics:**
- Signing count by period
- Document type breakdown
- Top signing users

**Chart Type:** Bar chart, Table

### 8.3 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Certificate Inventory | Real-time | Monthly | Table, CSV |
| Signing Activity | Real-time | Weekly | Bar Chart, PDF |
| Revocation Report | Real-time | Quarterly | Table |
| Expiry Forecast | Real-time | Monthly | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| CA Provider API (VNPT, Viettel, BKAV) | Certificate enrollment and management | REST | Certificate-based |
| CRL Distribution Point | Certificate Revocation List download | HTTP/LDAP | None (public) |
| OCSP Responder | Online certificate status verification | HTTP/OCSP | None (public) |
| Timestamp Authority | Trusted timestamp for signatures | REST | API Key |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| certificate.issued | {certificate_id, serial_number, expiry_date} | Certificate owner, E-Invoice (M33) |
| certificate.expiring_soon | {certificate_id, days_remaining} | Certificate owner, System admin |
| certificate.revoked | {certificate_id, reason, crl_number} | All dependent modules |
| document.signed | {signing_id, document_type, document_reference} | Workflow (M19), Contracts (M11) |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| invoice.ready_for_signing | E-Invoice (M33) | Prompt user to sign invoice |
| contract.ready_for_signing | Contract Management (M11) | Prompt user to sign contract |
| user.departed | HR (M05) | Trigger certificate revocation review |

---

## 10. Technical Notes

### Performance
- Expected data volume: 1,000+ certificates, 500,000+ signing operations
- Query complexity: Low (direct lookups for certificates, chronological for signing logs)
- Expected response time: Certificate list < 500ms, Signing operation < 2 seconds, Verification < 3 seconds
- CRL download and processing: < 30 seconds for full list

### Caching Strategy
- Redis cache for certificate status and validity data
- TTL: 1 hour for certificate details, 15 minutes for CRL data
- OCSP responses cached for 1 hour (per OCSP max-age)
- Cache invalidation on certificate status change or CRL update

### File Attachments
- Certificate files (X.509 format) stored securely with access control
- Signed documents stored with signature data appended
- Max certificate file size: 100 KB
- Storage: Encrypted S3-compatible object storage

### Localization
- Supported languages: Vietnamese (primary), English
- Certificate owner information supports Vietnamese characters and diacritics
- Date format: DD/MM/YYYY
- Support phone: 090 488 5833 (Vietnamese format)

### Mobile Considerations
- Mobile app supports document viewing and signing notification
- Certificate management requires web browser for full functionality
- Push notifications for certificate expiry warnings (30, 14, 7, 1 day)
- Mobile signing operations supported via USB token or cloud HSM

### Security
- Private keys are never stored in the database; only public key and certificate data
- Hardware security module (HSM) recommended for enterprise deployments
- Signing operations require re-authentication (password or biometric)
- All certificate data transmitted over TLS 1.2+
- Audit trail is immutable; signing logs cannot be modified or deleted
- Certificate revocation uses standard CRL and OCSP protocols

### Compliance
- Complies with Vietnamese Law on Electronic Transactions (2005)
- Complies with Decree 130/2018/ND-CP on electronic signatures
- Certificate providers must be licensed by Ministry of Information and Communications
- Signing operations meet evidentiary standards for Vietnamese courts
- Data retention: signing logs retained for minimum 10 years
