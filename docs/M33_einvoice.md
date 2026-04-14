# M33 - E-Invoice (Hoa don Dien tu)

**Category:** Digital Office
**Priority:** P3
**Reference Images:** assets/image_28.png
**Dependencies:** M04 (Accounting), M08 (Sales), M07 (Purchasing), M35 (Digital Signature)
**Generated:** April 14, 2026

---

## 1. Module Overview

The E-Invoice module provides a complete electronic invoicing solution compliant with Vietnamese tax regulations (Circular 78/2021/TT-BTC and Circular 39/2014/TT-BTC). It enables businesses to create, issue, manage, and store electronic invoices with embedded QR codes for verification, replacing traditional paper invoices entirely. The module integrates seamlessly with the Accounting module (M04) for automatic journal entry generation, the Sales module (M08) for invoice creation from sales orders, and the Purchasing module (M07) for vendor invoice processing.

The e-invoice dashboard (image_28) presents a comprehensive overview of invoice statistics, including a pie chart showing invoice distribution by type (VAT invoice, direct invoice, credit note, debit note), a timeline chart showing invoice issuance trends, a tax period declaration table mapping invoices to tax reporting periods, and a mobile app preview showing the MISA e-invoice mobile application which has been used 1,091 times total with 27 uses this month. Each invoice includes a unique QR code that can be scanned by tax authorities and customers to verify authenticity. The mobile application enables field sales representatives and remote workers to issue invoices directly from their smartphones, with offline capability that syncs when connectivity is restored.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| Finance Manager | Oversees all invoicing operations, tax reporting, and compliance | Full access to all invoice data and configuration |
| Accountant | Creates, issues, and manages invoices; prepares tax declarations | Full access to invoice creation and management |
| Sales Representative | Views invoices related to their sales orders, sends to customers | Read access to assigned invoices, send capability |
| Purchasing Staff | Processes incoming vendor invoices | Read/write on purchase invoices only |
| Tax Consultant | Reviews tax declarations and invoice compliance | Read access to all invoice and tax data |
| System Administrator | Manages invoice templates, tax provider configuration | Full system access, read-only invoice data |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M04 - Accounting | Invoices generate journal entries; tax data flows to tax declarations |
| M08 - Sales | Sales orders convert to invoices; customer data shared |
| M07 - Purchasing | Vendor invoices processed and matched to purchase orders |
| M35 - Digital Signature | Invoices are digitally signed before issuance |
| M01 - Authentication | User access control and role-based permissions |
| M41 - E-Banking | Payment status sync for paid/unpaid invoice tracking |

### Key Business Rules

1. All invoices must comply with Vietnamese e-invoice regulations: sequential invoice numbers, unique invoice codes, mandatory fields (seller info, buyer info, items, taxes, totals).
2. Invoices must be digitally signed by an authorized certificate before they can be issued to customers.
3. Once an invoice is issued, it cannot be deleted; corrections must be made through credit notes or adjustment invoices.
4. VAT invoices must include the seller's tax code, buyer's tax code, item descriptions, quantities, unit prices, VAT rates, and total amounts.
5. Invoice numbers are assigned sequentially per invoice code series; gaps in sequences must be reported to tax authorities.
6. Tax periods are calculated monthly; all invoices issued within a calendar month belong to that month's tax declaration.
7. Mobile-issued invoices must sync within 24 hours of issuance; offline invoices are stored locally and uploaded automatically.
8. QR codes on invoices must encode: invoice code, invoice number, seller tax code, total amount, and digital signature hash.
9. Batch invoicing allows multiple invoices to be created and issued simultaneously from a set of sales orders.
10. Invoice templates are configurable per company and must be approved by the finance manager before use.

---

## 2. Screen Inventory

### 2.1 Screen: E-Invoice Dashboard

**Route:** `/einvoice/dashboard` | **Purpose:** Display comprehensive e-invoice statistics, issuance trends, and tax period status.

| UI Component | Description |
|--------------|-------------|
| Invoice Count Cards | Total invoices issued this period by type (VAT, Direct, Credit Note, Debit Note) |
| Pie Chart | Invoice distribution by type with percentages and absolute counts |
| Timeline Chart | Monthly invoice issuance trend over last 12 months |
| Tax Period Table | Declaration table mapping periods (month/year) to invoice counts, taxable amounts, and VAT amounts |
| Mobile Usage Stats | Total mobile app uses: 1,091; This month: 27 |
| QR Code Preview | Sample QR code with decoded information display |
| Quick Actions | New Invoice, New Batch, Import Invoices, Tax Declaration |

**User Interactions:**
- Click on pie chart segments to filter invoices by type
- Click tax period row to view invoices for that period
- View mobile usage statistics and trends
- Click quick actions to navigate to creation forms

**Data Displayed:**
- Invoice counts by type
- Issuance trends over time
- Tax period declarations with totals
- Mobile application usage metrics

### 2.2 Screen: Invoice List

**Route:** `/einvoice/invoices` | **Purpose:** Browse, search, and manage all electronic invoices.

| UI Component | Description |
|--------------|-------------|
| Invoice List Table | Columns: Invoice Code, Number, Date, Buyer, Total, Status, Type, QR Code |
| Advanced Filters | Filter by date range, type, status, buyer, amount range |
| Bulk Actions | Issue batch, export, print, send email, download PDF |
| Status Badges | Draft, Issued, Cancelled, Adjusted, Replaced |
| QR Code Column | Clickable QR code thumbnail for each invoice |

**User Interactions:**
- Search invoices by code, number, buyer name, or tax code
- Filter by multiple criteria simultaneously
- Select multiple invoices for batch operations
- Click QR code to view full encoded data
- Click invoice row to open detail view

**Data Displayed:**
- Invoice code and number
- Issue date and buyer information
- Total amount and VAT amount
- Current status and type

### 2.3 Screen: Invoice Detail

**Route:** `/einvoice/invoices/{id}` | **Purpose:** View and edit a single invoice with full details and QR code.

| UI Component | Description |
|--------------|-------------|
| Invoice Header | Invoice code, number, date, status badge, template name |
| Seller Information | Company name, tax code, address, phone, email |
| Buyer Information | Company name, tax code, address, phone, email, payment method |
| Line Items Table | Item name, unit, quantity, unit price, total, VAT rate, VAT amount |
| Totals Section | Subtotal, total VAT, grand total, amount in words |
| QR Code Display | Large QR code with verification link |
| Digital Signature Info | Certificate details, signing timestamp, signature hash |
| Action Bar | Issue, Cancel, Adjust, Print, Send Email, Download PDF |

**User Interactions:**
- Edit invoice details while in draft status
- Issue invoice with digital signature
- Cancel invoice with reason (requires manager approval)
- Print invoice in standard format
- Send invoice PDF via email to buyer
- Download invoice as PDF

**Data Displayed:**
- Complete invoice data (seller, buyer, items, totals)
- QR code with scannable verification data
- Digital signature certificate and timestamp
- Invoice history and audit trail

### 2.4 Screen: Tax Period Declaration

**Route:** `/einvoice/tax-periods` | **Purpose:** Manage tax period declarations with invoice aggregation and VAT calculation.

| UI Component | Description |
|--------------|-------------|
| Period List Table | Columns: Period (MM/YYYY), Output Invoices, Output VAT, Input Invoices, Input VAT, Net VAT, Status |
| Period Detail View | Expanded view showing all invoices in the period with line-by-line breakdown |
| VAT Calculation | Automatic calculation of output VAT, input VAT, and net VAT payable |
| Submission Status | Declaration submission status to tax authority |
| Export Button | Export declaration form in tax authority format |

**User Interactions:**
- Select period to view detailed invoice breakdown
- Verify VAT calculations before submission
- Submit declaration to tax authority portal
- Export declaration form for offline filing

**Data Displayed:**
- Period-level invoice and VAT totals
- Output vs. input VAT comparison
- Net VAT payable or refundable
- Submission status and timestamps

### 2.5 Screen: Invoice Template Management

**Route:** `/einvoice/templates` | **Purpose:** Configure and manage invoice templates per company and invoice type.

| UI Component | Description |
|--------------|-------------|
| Template List | Template name, type, company, status, fields count |
| Template Editor | Visual editor with drag-drop field placement |
| Field Configuration | Mandatory fields, optional fields, custom fields |
| Preview Mode | Live preview of template with sample data |
| Approval Status | Template approval workflow status |

**User Interactions:**
- Create new templates based on invoice type
- Configure mandatory and optional fields
- Preview templates with sample data
- Submit templates for finance manager approval

**Data Displayed:**
- Template configuration details
- Field definitions and validation rules
- Preview rendering with company branding

### 2.6 Screen: Batch Invoice Processing

**Route:** `/einvoice/batches` | **Purpose:** Create and issue multiple invoices simultaneously from sales orders or import.

| UI Component | Description |
|--------------|-------------|
| Batch List | Batch name, source, invoice count, status, created date |
| Order Selection | Select sales orders to convert to invoices |
| Import File Upload | Upload CSV/Excel file with invoice data |
| Preview Table | Preview all invoices before batch creation |
| Bulk Issue | Issue all invoices in batch with digital signature |

**User Interactions:**
- Select multiple sales orders for batch conversion
- Upload and validate import file
- Preview generated invoices before creation
- Issue entire batch at once

**Data Displayed:**
- Batch summary (count, total value)
- Individual invoice previews
- Validation errors and warnings
- Processing status

### 2.7 Screen: Mobile Invoice Preview

**Route:** `/einvoice/mobile` | **Purpose:** Preview and test the mobile invoice issuance experience.

| UI Component | Description |
|--------------|-------------|
| Mobile Mockup | Phone-sized preview of mobile app interface |
| Invoice Form | Simplified form for mobile invoice creation |
| QR Code Scanner | Simulated QR code scanner for verification |
| Offline Mode Indicator | Shows sync status for offline invoices |
| Usage Statistics | 1,091 total uses, 27 this month |

**User Interactions:**
- Test mobile invoice creation workflow
- Simulate offline mode and sync
- Scan QR codes for verification testing
- View mobile usage analytics

**Data Displayed:**
- Mobile app usage trends
- Sync status for offline invoices
- QR code verification results

---

## 3. Feature Specifications

### 3.1 Feature: Invoice Creation and Issuance
**Description:** Create electronic invoices with QR codes, digital signatures, and compliance with Vietnamese tax regulations.

**User Stories:**
- As an Accountant, I want to create VAT invoices from sales orders, so that I can bill customers compliantly.
- As a Finance Manager, I want invoices to be digitally signed before issuance, so that they are legally valid.

**Acceptance Criteria:**
- Given a sales order, when the user creates an invoice, then all order data flows into the invoice with correct VAT calculations.
- Given an invoice is in draft status, when the user clicks Issue, then the invoice is digitally signed and the QR code is generated.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Buyer tax code | Valid 10/13 digit format | Tax code must be 10 or 13 digits |
| Invoice date | Cannot be in the future | Invoice date cannot be in the future |
| Line items | At least one item required | Invoice must have at least one line item |
| Total amount | Must be positive | Total amount must be greater than zero |

**Edge Cases:**
- Invoice creation during offline mobile mode stores data locally and syncs when online
- Sequential number assignment is atomic to prevent gaps or duplicates
- If digital signing fails, the invoice remains in draft with an error message

### 3.2 Feature: QR Code Generation and Verification
**Description:** Generate unique QR codes for each invoice encoding verification data for tax authority and customer verification.

**User Stories:**
- As a Tax Authority, I want to scan QR codes on invoices, so that I can verify authenticity.
- As a Customer, I want to verify my invoice using the QR code, so that I can ensure it is legitimate.

**Acceptance Criteria:**
- Given an invoice is issued, when the QR code is generated, then it encodes invoice code, number, seller tax code, total amount, and signature hash.
- Given a QR code is scanned, when the verification service processes it, then the invoice details and validity status are returned.

**Edge Cases:**
- QR codes remain valid even after invoice cancellation (showing cancelled status)
- QR code data is immutable after invoice issuance
- Offline QR codes are generated locally and verified against stored data

### 3.3 Feature: Tax Period Declaration
**Description:** Aggregate invoices by tax period and generate VAT declarations for submission to tax authorities.

**User Stories:**
- As an Accountant, I want to view all invoices in a tax period, so that I can prepare accurate tax declarations.
- As a Tax Consultant, I want automatic VAT calculations, so that I can verify declaration accuracy.

**Acceptance Criteria:**
- Given a tax period (month/year), when the user opens the declaration, then all invoices for that period are aggregated with VAT totals.
- Given the declaration is prepared, when the user submits, then the data is formatted for tax authority portal submission.

**Edge Cases:**
- Invoices spanning multiple periods are allocated based on issue date
- Adjusted invoices in a later period correct the original period's declaration
- Late submissions trigger penalty warnings

### 3.4 Feature: Invoice Template Configuration
**Description:** Design and configure invoice templates with mandatory fields, optional fields, and company branding.

**User Stories:**
- As a Finance Manager, I want to configure invoice templates per company, so that invoices reflect company branding.
- As a System Administrator, I want to ensure mandatory fields are always present, so that invoices remain compliant.

**Acceptance Criteria:**
- Given a template is created, when mandatory fields are configured, then all invoices using this template must include these fields.
- Given a template is submitted for approval, when the finance manager approves, then the template becomes available for use.

**Edge Cases:**
- Templates cannot be deleted if they have been used on any invoice
- Template changes only affect new invoices, not existing ones
- Multiple templates can be active simultaneously for different invoice types

### 3.5 Feature: Batch Invoice Processing
**Description:** Create and issue multiple invoices simultaneously from sales orders or file import.

**User Stories:**
- As an Accountant, I want to convert multiple sales orders to invoices at once, so that I save time on repetitive data entry.
- As a Finance Manager, I want to import invoices from external systems, so that I can consolidate all invoices in one place.

**Acceptance Criteria:**
- Given a set of sales orders, when the user creates a batch, then one invoice is generated per order with correct data mapping.
- Given an import file, when it is validated and processed, then invoices are created for each valid row.

**Edge Cases:**
- Batch processing with 1,000+ invoices is handled asynchronously with progress tracking
- Import file errors are reported per-row with line numbers
- Partial batch failures are reported with success/failure counts

### 3.6 Feature: Mobile Invoice Issuance
**Description:** Issue invoices from the mobile application with offline capability and automatic sync.

**User Stories:**
- As a Sales Representative, I want to issue invoices from my phone, so that I can bill customers immediately after delivery.
- As an Accountant, I want mobile invoices to sync automatically, so that the central database is always up to date.

**Acceptance Criteria:**
- Given the mobile app has no connectivity, when the user creates an invoice, then it is stored locally and flagged for sync.
- Given connectivity is restored, when the sync runs, then all offline invoices are uploaded and issued.

**Edge Cases:**
- Offline invoice numbering is reserved and confirmed during sync
- Conflicts (duplicate numbers during sync) are resolved by reassigning numbers
- Mobile invoices require the same digital signing as desktop invoices

### 3.7 Feature: Invoice Cancellation and Adjustment
**Description:** Cancel issued invoices with approval workflow and create adjustment invoices for corrections.

**User Stories:**
- As an Accountant, I want to cancel an incorrectly issued invoice, so that I can correct the error.
- As a Finance Manager, I want to approve invoice cancellations, so that all cancellations are documented.

**Acceptance Criteria:**
- Given an invoice needs cancellation, when the user submits a cancellation request, then it requires manager approval.
- Given a cancellation is approved, when processed, then the original invoice is marked as cancelled and a credit note is generated.

**Edge Cases:**
- Cancelled invoices retain their sequential numbers for audit purposes
- Cancellation reasons are mandatory and stored for reporting
- Partial adjustments create adjustment invoices for the difference only

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|   EInvoice     |       |  InvoiceItem   |       |  TaxPeriod     |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |       | id (PK)        |
| invoice_code   |       | invoice_id(FK) |       | period_month   |
| invoice_number |       | item_name      |       | period_year    |
| invoice_date   |       | quantity       |       | output_count   |
| seller_id(FK)  |       | unit_price     |       | output_vat     |
| buyer_id(FK)   |       | vat_rate       |       | input_count    |
| template_id(FK)|       | vat_amount     |       | input_vat      |
| status         |       | total          |       | net_vat        |
| type           |       +----------------+       | status         |
| qr_code_data   |                                +----------------+
| signed_at      |                                        |
| signed_by(FK)  |                                        v
+----------------+       +----------------+       +----------------+
        |                | InvoiceStatus  |       | TaxDeclaration |
        v                +----------------+       +----------------+
+----------------+       +----------------+       | id (PK)        |
|InvoiceTemplate |       |  InvoiceType   |       | period_id(FK)  |
+----------------+       +----------------+       | submitted_at   |
| id (PK)        |       | id (PK)        |       | status         |
| name           |       | name           |       | file_url       |
| company_id(FK) |       | code           |       | created_at     |
| fields_json    |       | description    |       +----------------+
| is_approved    |       +----------------+
| created_at     |
+----------------+              |
        |                       v
        v                +----------------+
+----------------+       |  InvoiceBatch  |
|  InvoiceStatus |       +----------------+
+----------------+       | id (PK)        |
| id (PK)        |       | name           |
| code           |       | source_type    |
| name           |       | invoice_count  |
| description    |       | total_value    |
+----------------+       | status         |
                         | created_at     |
                         +----------------+
        |
        v
+----------------+       +----------------+
|    QRCode      |       |  InvoiceAudit  |
+----------------+       +----------------+
| id (PK)        |       | id (PK)        |
| invoice_id(FK) |       | invoice_id(FK) |
| encoded_data   |       | action         |
| verification_url|      | actor_id(FK)   |
| created_at     |       | timestamp      |
+----------------+       +----------------+
```

### 4.2 Table Schemas

#### Table: e_invoice
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| invoice_code | VARCHAR(20) | NOT NULL | Invoice code series |
| invoice_number | VARCHAR(20) | NOT NULL | Sequential invoice number |
| invoice_date | DATE | NOT NULL | Invoice issue date |
| seller_company_id | BIGINT | FK -> company.id | Seller company |
| buyer_name | VARCHAR(200) | NOT NULL | Buyer company name |
| buyer_tax_code | VARCHAR(20) | NULL | Buyer tax code |
| buyer_address | TEXT | NULL | Buyer address |
| buyer_payment_method | VARCHAR(50) | DEFAULT 'transfer' | Payment method |
| template_id | BIGINT | FK -> invoice_template.id | Invoice template |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status: draft, issued, cancelled, adjusted, replaced |
| type | VARCHAR(30) | NOT NULL | Type: vat, direct, credit_note, debit_note |
| subtotal | DECIMAL(18,2) | NOT NULL | Pre-tax subtotal |
| total_vat | DECIMAL(18,2) | NOT NULL | Total VAT amount |
| grand_total | DECIMAL(18,2) | NOT NULL | Grand total |
| amount_in_words | TEXT | NULL | Amount spelled out |
| qr_code_data | TEXT | NULL | Encoded QR code data |
| signature_hash | VARCHAR(255) | NULL | Digital signature hash |
| signed_at | TIMESTAMP | NULL | Signing timestamp |
| signed_by | BIGINT | FK -> users.id | Signing certificate owner |
| tax_period_id | BIGINT | FK -> tax_period.id | Associated tax period |
| notes | TEXT | NULL | Invoice notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| updated_by | BIGINT | FK -> users.id | Last updater |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_einvoice_code_number ON e_invoice(invoice_code, invoice_number);
CREATE INDEX idx_einvoice_date ON e_invoice(invoice_date);
CREATE INDEX idx_einvoice_status ON e_invoice(status);
CREATE INDEX idx_einvoice_type ON e_invoice(type);
CREATE INDEX idx_einvoice_buyer_tax ON e_invoice(buyer_tax_code);
CREATE INDEX idx_einvoice_tax_period ON e_invoice(tax_period_id);
CREATE INDEX idx_einvoice_seller ON e_invoice(seller_company_id);
```

#### Table: invoice_item
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| invoice_id | BIGINT | FK -> e_invoice.id, NOT NULL | Parent invoice |
| item_name | VARCHAR(300) | NOT NULL | Item/service description |
| unit | VARCHAR(20) | DEFAULT 'unit' | Unit of measure |
| quantity | DECIMAL(10,2) | NOT NULL | Quantity |
| unit_price | DECIMAL(18,2) | NOT NULL | Unit price |
| vat_rate | DECIMAL(5,2) | DEFAULT 0 | VAT percentage |
| vat_amount | DECIMAL(18,2) | NOT NULL | VAT amount |
| total | DECIMAL(18,2) | NOT NULL | Line total |
| sort_order | INT | DEFAULT 0 | Display order |

**Indexes:**
```sql
CREATE INDEX idx_invoice_item_invoice ON invoice_item(invoice_id);
```

#### Table: tax_period
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| period_month | INT | NOT NULL | Period month (1-12) |
| period_year | INT | NOT NULL | Period year |
| output_invoice_count | INT | DEFAULT 0 | Output invoice count |
| output_vat_total | DECIMAL(18,2) | DEFAULT 0 | Output VAT total |
| input_invoice_count | INT | DEFAULT 0 | Input invoice count |
| input_vat_total | DECIMAL(18,2) | DEFAULT 0 | Input VAT total |
| net_vat | DECIMAL(18,2) | DEFAULT 0 | Net VAT payable |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'open' | Status: open, declared, submitted, closed |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_tax_period_unique ON tax_period(period_month, period_year);
CREATE INDEX idx_tax_period_status ON tax_period(status);
```

#### Table: tax_declaration
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| tax_period_id | BIGINT | FK -> tax_period.id, NOT NULL | Associated tax period |
| submitted_at | TIMESTAMP | NULL | Submission timestamp |
| submission_reference | VARCHAR(100) | NULL | Tax authority reference number |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'draft' | Status: draft, submitted, accepted, rejected |
| file_url | VARCHAR(500) | NULL | Exported declaration file URL |
| notes | TEXT | NULL | Declaration notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |

**Indexes:**
```sql
CREATE INDEX idx_tax_declaration_period ON tax_declaration(tax_period_id);
CREATE INDEX idx_tax_declaration_status ON tax_declaration(status);
```

#### Table: invoice_template
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Template name |
| company_id | BIGINT | FK -> company.id | Associated company |
| invoice_type | VARCHAR(30) | NOT NULL | Applicable invoice type |
| fields_json | JSON | NOT NULL | Field configuration |
| layout_json | JSON | NOT NULL | Visual layout configuration |
| is_approved | TINYINT(1) | DEFAULT 0 | Approval status |
| approved_by | BIGINT | FK -> users.id | Approving manager |
| approved_at | TIMESTAMP | NULL | Approval timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_invoice_template_company ON invoice_template(company_id);
CREATE INDEX idx_invoice_template_type ON invoice_template(invoice_type);
CREATE INDEX idx_invoice_template_approved ON invoice_template(is_approved);
```

#### Table: invoice_status
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| code | VARCHAR(30) | NOT NULL, UNIQUE | Status code |
| name | VARCHAR(100) | NOT NULL | Status display name |
| description | TEXT | NULL | Status description |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_invoice_status_code ON invoice_status(code);
```

#### Table: invoice_type
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL | Type name |
| code | VARCHAR(30) | NOT NULL, UNIQUE | Type code |
| description | TEXT | NULL | Type description |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_invoice_type_code ON invoice_type(code);
```

#### Table: invoice_batch
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Batch name |
| source_type | VARCHAR(30) | NOT NULL | Source: sales_orders, import |
| source_reference | VARCHAR(200) | NULL | Source reference |
| invoice_count | INT | DEFAULT 0 | Number of invoices |
| total_value | DECIMAL(18,2) | DEFAULT 0 | Total batch value |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'pending' | Status: pending, processing, completed, failed |
| error_log | TEXT | NULL | Error details if failed |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| created_by | BIGINT | FK -> users.id | Creator |

**Indexes:**
```sql
CREATE INDEX idx_invoice_batch_status ON invoice_batch(status);
CREATE INDEX idx_invoice_batch_source ON invoice_batch(source_type);
```

#### Table: qr_code
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| invoice_id | BIGINT | FK -> e_invoice.id, NOT NULL | Associated invoice |
| encoded_data | TEXT | NOT NULL | Encoded QR data string |
| verification_url | VARCHAR(500) | NULL | Verification URL |
| image_url | VARCHAR(500) | NULL | QR code image URL |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_qr_code_invoice ON qr_code(invoice_id);
```

#### Table: invoice_audit
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| invoice_id | BIGINT | FK -> e_invoice.id, NOT NULL | Associated invoice |
| action | VARCHAR(50) | NOT NULL | Action: created, updated, issued, cancelled, adjusted |
| old_values_json | JSON | NULL | Previous values |
| new_values_json | JSON | NULL | New values |
| actor_id | BIGINT | FK -> users.id | User who performed action |
| timestamp | TIMESTAMP | NOT NULL, DEFAULT NOW() | Action timestamp |
| ip_address | VARCHAR(45) | NULL | Actor IP address |

**Indexes:**
```sql
CREATE INDEX idx_invoice_audit_invoice ON invoice_audit(invoice_id);
CREATE INDEX idx_invoice_audit_action ON invoice_audit(action);
CREATE INDEX idx_invoice_audit_timestamp ON invoice_audit(timestamp);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| e_invoice | Electronic invoice records | 500,000 |
| invoice_item | Invoice line items | 2,500,000 |
| tax_period | Tax period records | 100 |
| tax_declaration | Tax declaration submissions | 50 |
| invoice_template | Invoice templates | 50 |
| invoice_status | Status definitions | 10 |
| invoice_type | Type definitions | 10 |
| invoice_batch | Batch processing records | 5,000 |
| qr_code | QR code data | 500,000 |
| invoice_audit | Audit trail | 2,000,000 |

---

## 5. API Specifications

### 5.1 Invoice Endpoints

#### GET /api/v1/einvoice/invoices
**Description:** List invoices with filtering and pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| limit | integer | No | Items per page (default: 20, max: 100) |
| date_from | string | No | Filter by invoice date from |
| date_to | string | No | Filter by invoice date to |
| type | string | No | Filter by invoice type |
| status | string | No | Filter by status |
| buyer_tax_code | string | No | Filter by buyer tax code |
| sort | string | No | Sort field |
| order | string | No | asc/desc |

**Permission:** `einvoice:invoice:read`

#### POST /api/v1/einvoice/invoices
**Description:** Create a new invoice

**Permission:** `einvoice:invoice:create`

#### POST /api/v1/einvoice/invoices/{id}/issue
**Description:** Issue an invoice with digital signature

**Response (200 OK):**
```json
{
  "status": "issued",
  "invoice_number": "00001234",
  "qr_code_url": "/api/v1/einvoice/invoices/123/qr-code",
  "issued_at": "2026-04-14T10:30:00Z"
}
```

**Permission:** `einvoice:invoice:issue`

#### GET /api/v1/einvoice/invoices/{id}/qr-code
**Description:** Get QR code image for an invoice

**Permission:** `einvoice:invoice:read`

#### POST /api/v1/einvoice/invoices/{id}/cancel
**Description:** Request invoice cancellation

**Permission:** `einvoice:invoice:cancel`

### 5.2 Tax Period Endpoints

#### GET /api/v1/einvoice/tax-periods
**Description:** List tax periods

**Permission:** `einvoice:tax-period:read`

#### GET /api/v1/einvoice/tax-periods/{id}/invoices
**Description:** Get all invoices in a tax period

**Permission:** `einvoice:tax-period:read`

### 5.3 Tax Declaration Endpoints

#### POST /api/v1/einvoice/tax-declarations
**Description:** Create tax declaration for a period

**Permission:** `einvoice:tax-declaration:create`

#### POST /api/v1/einvoice/tax-declarations/{id}/submit
**Description:** Submit declaration to tax authority

**Permission:** `einvoice:tax-declaration:submit`

### 5.4 Batch Invoice Endpoints

#### POST /api/v1/einvoice/batches
**Description:** Create batch of invoices from sales orders

**Request Body:**
```json
{
  "name": "April 2026 Batch 1",
  "source_type": "sales_orders",
  "order_ids": [1, 2, 3, 4, 5]
}
```

**Permission:** `einvoice:batch:create`

### 5.5 Template Endpoints

#### GET /api/v1/einvoice/templates
**Description:** List invoice templates

**Permission:** `einvoice:template:read`

#### POST /api/v1/einvoice/templates/{id}/approve
**Description:** Approve invoice template

**Permission:** `einvoice:template:approve`

### 5.6 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/einvoice/invoices | List invoices | einvoice:invoice:read |
| POST | /api/v1/einvoice/invoices | Create invoice | einvoice:invoice:create |
| POST | /api/v1/einvoice/invoices/{id}/issue | Issue invoice | einvoice:invoice:issue |
| GET | /api/v1/einvoice/invoices/{id}/qr-code | Get QR code | einvoice:invoice:read |
| POST | /api/v1/einvoice/invoices/{id}/cancel | Cancel invoice | einvoice:invoice:cancel |
| GET | /api/v1/einvoice/tax-periods | List tax periods | einvoice:tax-period:read |
| GET | /api/v1/einvoice/tax-periods/{id}/invoices | Period invoices | einvoice:tax-period:read |
| POST | /api/v1/einvoice/tax-declarations | Create declaration | einvoice:tax-declaration:create |
| POST | /api/v1/einvoice/tax-declarations/{id}/submit | Submit declaration | einvoice:tax-declaration:submit |
| POST | /api/v1/einvoice/batches | Create batch | einvoice:batch:create |
| GET | /api/v1/einvoice/templates | List templates | einvoice:template:read |
| POST | /api/v1/einvoice/templates/{id}/approve | Approve template | einvoice:template:approve |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Finance Manager | finance_manager | Full access to all invoice operations and approvals |
| Accountant | accountant | Create, issue, and manage invoices |
| Sales Representative | sales_rep | View assigned invoices, send to customers |
| Purchasing Staff | purchasing_staff | Process vendor invoices |
| Tax Consultant | tax_consultant | Read access for tax compliance review |
| System Administrator | system_admin | Manage templates and configuration |

### Permission Matrix
| Role | Create | Read | Issue | Cancel | Approve Template | Submit Tax | Export |
|------|--------|------|-------|--------|-----------------|-----------|--------|
| Finance Manager | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Accountant | Yes | Yes | Yes | Request | No | Yes | Yes |
| Sales Representative | No | Assigned | No | No | No | No | No |
| Purchasing Staff | Yes (vendor) | Vendor only | Yes | Request | No | No | Yes |
| Tax Consultant | No | Yes | No | No | No | No | Yes |
| System Administrator | No | Yes | No | No | Yes | No | Yes |

### Row-Level Security
- Sales Representatives can only view invoices linked to their sales orders.
- Purchasing Staff can only access invoices of type "purchase" linked to their purchase orders.
- Accountants have access to all invoices within their assigned company.
- Finance Managers have unrestricted access across all companies.

---

## 7. State Machine / Workflows

### 7.1 Invoice Status Transitions

```
[DRAFT] ──issue──→ [ISSUED] ──pay──→ [PAID]
  │                  │
  └──cancel──→ [CANCELLED]←──adjust──→ [ADJUSTED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | ISSUED | issue | Accountant | Generate QR code, apply digital signature, assign number |
| ISSUED | PAID | pay | System | Update payment status from banking |
| ISSUED | CANCELLED | cancel | Finance Manager | Generate credit note, notify buyer |
| ISSUED | ADJUSTED | adjust | Accountant | Create adjustment invoice |

### 7.2 Tax Declaration Workflow

```
[OPEN] ──prepare──→ [DECLARED] ──submit──→ [SUBMITTED] ──accept──→ [CLOSED]
                                              │
                                              └──reject──→ [DECLARED]
```

### 7.3 Invoice Cancellation Approval

```
[REQUESTED] ──approve──→ [APPROVED] ──process──→ [CANCELLED]
     │                       │
     └──reject──→ [REJECTED] └──error──→ [FAILED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Invoice Issuance Summary
**Type:** Real-time
**Purpose:** Track invoice issuance volume and value by type, period, and company

**Metrics:**
- Invoice count by type
- Total value by type
- Monthly issuance trend

**Chart Type:** Pie chart, Line chart

### 8.2 Report: Tax Period Report
**Type:** Real-time
**Purpose:** Aggregate invoice data by tax period for declaration preparation

**Metrics:**
- Output invoice count and VAT
- Input invoice count and VAT
- Net VAT payable

**Chart Type:** Table, Bar chart

### 8.3 Report: Mobile Usage Report
**Type:** Real-time
**Purpose:** Monitor mobile app adoption and usage patterns

**Metrics:**
- Total uses, monthly uses
- Invoices issued via mobile
- Offline sync success rate

**Chart Type:** Line chart, Table

### 8.4 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Invoice Issuance Summary | Real-time | Monthly | Pie Chart, CSV |
| Tax Period Report | Real-time | Monthly | Table, Excel |
| Mobile Usage Report | Real-time | Monthly | Line Chart |
| Cancellation Report | Real-time | Quarterly | Table |
| Template Usage | Real-time | On-demand | Table |
| Batch Processing Report | Real-time | On-demand | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| General Department of Taxation API | Tax declaration submission | REST | Certificate-based |
| MISA Accounting API | Journal entry sync | REST | OAuth 2.0 |
| MISA Sales API | Sales order to invoice conversion | REST | OAuth 2.0 |
| MISA ESign API (M35) | Digital signature application | REST | Certificate-based |
| QR Verification API | QR code verification service | REST | API Key |
| Email Provider API | Invoice email delivery | REST | API Key |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| invoice.issued | {invoice_id, code, number, total, buyer_tax_code} | Accounting (M04), Sales (M08) |
| invoice.cancelled | {invoice_id, reason, credit_note_id} | Accounting (M04) |
| tax_declaration.submitted | {declaration_id, period, status} | Finance team |
| batch.completed | {batch_id, success_count, failed_count} | Batch creator |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| order.completed | Sales (M08) | Trigger invoice creation |
| payment.received | E-Banking (M41) | Update invoice payment status |
| journal.posted | Accounting (M04) | Link invoice to accounting entries |

---

## 10. Technical Notes

### Performance
- Expected data volume: 500,000+ invoices, 2,500,000+ line items
- Query complexity: Medium (aggregations for tax periods, detail retrieval for invoices)
- Expected response time: List views < 1s, Detail views < 500ms, Batch processing < 30s per 100 invoices
- QR code generation: < 100ms per invoice
- Digital signing: < 200ms per invoice

### Caching Strategy
- Redis cache for tax period aggregations and invoice type reference data
- TTL: 15 minutes for tax period summaries, 1 hour for template data
- QR code images cached at CDN with 24-hour TTL
- Cache invalidation on invoice status change or tax period recalculation

### File Attachments
- Supported types: PDF (invoice exports), XML (tax declaration files)
- Max size: 10 MB per file
- Storage: S3-compatible object storage
- Invoice PDFs generated on-demand with watermark for cancelled invoices

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Currency: VND with Vietnamese number formatting
- Amount in words: Vietnamese language spelling for legal compliance

### Mobile Considerations
- Mobile app supports full invoice creation with offline mode
- Offline invoices stored locally with encrypted storage
- Auto-sync when connectivity is restored (within 24 hours)
- QR code scanning capability for invoice verification
- Push notifications for sync status and issuance confirmation

### Security
- Digital signatures use certificate-based encryption compliant with Vietnamese regulations
- Invoice data is encrypted at rest using AES-256
- Sequential number assignment uses database transactions to prevent gaps
- Audit trail captures all invoice changes with before/after values
- API rate limiting: 100 requests per minute for read, 20 for write

### Compliance
- Invoice format complies with Circular 78/2021/TT-BTC and Circular 39/2014/TT-BTC
- QR code encoding follows General Department of Taxation specifications
- Tax period alignment follows calendar month boundaries
- Cancellation procedures require documented approval trail
- Data retention: invoices must be retained for minimum 10 years per Vietnamese law
