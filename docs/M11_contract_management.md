# M11 Contract Management Module Specification

## 1. Module Overview

### 1.1 Purpose

The Contract Management Module (M11) is the comprehensive contract lifecycle management system within the ABN ERP platform. The core entity "Hop dong" (Contract) covers the complete contract lifecycle from template creation, drafting, negotiation, approval, execution, payment tracking, renewal, and expiration. The module manages contract parties, terms, payment schedules, renewals, alerts, types, and statuses, providing centralized contract management across the organization.

The module serves as the central repository for all organizational contracts including sales contracts, purchase contracts, service agreements, lease agreements, employment contracts, and partnership agreements. It ensures contract compliance through payment tracking, renewal alerts, term monitoring, and automated notifications for key contract events.

### 1.2 Personas

| Persona | Role | Key Responsibilities | Primary Screens | Access Level |
|---------|------|---------------------|-----------------|--------------|
| Contract Manager | Department leadership | Contract policy, approval, reporting | Dashboard, Reports, Settings | Full |
| Contract Officer | Contract administration | Create/update contracts, track lifecycle, manage parties | Contract Registry, Parties | Read/Write |
| Legal Counsel | Legal review | Review contract terms, ensure compliance | Contract Review, Templates | Read/Write |
| Finance Officer | Payment tracking | Track contract payments, billing schedules | Contract Payment | Read/Write |
| Sales Representative | Sales contracts | Create and manage sales contracts | Contract Registry | Read/Write (own) |
| Purchasing Officer | Purchase contracts | Create and manage purchase contracts | Contract Registry | Read/Write (own) |
| Department Manager | Contract oversight | Review contracts in department | Contract List, Reports | Read-only |
| Director | Strategic oversight | High-value contract approval, portfolio view | Reports, Dashboard | Read-only |

### 1.3 Dependencies

| Dependency | Type | Direction | Description |
|-----------|------|-----------|-------------|
| M04 Accounting | Downstream | Outbound | Contract payments -> GL entries |
| M07 Purchasing | Upstream/Downstream | Bidirectional | Purchase contracts -> POs |
| M08 Sales | Upstream/Downstream | Bidirectional | Sales contracts -> Sale orders |
| M05 Employee Mgmt | Peer | Inbound | Employee contracts |
| M10 Asset Mgmt | Peer | Bidirectional | Lease contracts -> Assets |
| M06 Task Mgmt | Peer | Inbound | Contract milestone tasks |
| E-Signature Provider | External | Outbound | Digital signature integration |
| Document Management | External | Bidirectional | Contract document storage |

### 1.4 Business Rules

1. **Contract Numbering**: Contracts must have unique sequential numbers with format HD-{type_code}-{year}-{sequential} (e.g., HD-SAL-2026-0001).
2. **Template Requirement**: All contracts must be based on an approved template; custom contracts require legal review.
3. **Party Verification**: All contract parties must be verified (customer, supplier, employee, partner) before contract execution.
4. **Approval Workflow**: Contracts require approval based on value thresholds: < 100M (dept manager), 100M-500M (contract manager), > 500M (director).
5. **Payment Tracking**: Contract payment schedules must be defined at contract creation; payments tracked against schedule.
6. **Renewal Alerts**: System generates renewal alerts at 90, 60, 30, and 7 days before contract expiry.
7. **Term Enforcement**: Contract terms (price, quantity, delivery) are referenced by dependent modules (Sales, Purchasing).
8. **Amendment Tracking**: Contract amendments create new versions with change history; original terms preserved.
9. **Termination Clauses**: Early termination must follow contract-defined termination clauses with notice period.
10. **Document Attachment**: Signed contract documents must be attached and stored securely.
11. **Status Lifecycle**: Contracts follow defined status flow: Draft -> Under Review -> Approved -> Active -> Expired/Terminated.
12. **Compliance Check**: All contracts must pass compliance check (legal, financial, operational) before activation.

---

## 2. Screen Inventory

### 2.1 Screen Routes and Purposes

| Route | Purpose | Personas | Priority |
|-------|---------|----------|----------|
| `/contracts/dashboard` | Contract dashboard with KPIs and alerts | All personas | Critical |
| `/contracts/registry` | Complete contract registry with search and filters | Contract Officer | Critical |
| `/contracts/registry/{id}` | Contract detail with full lifecycle | Contract Officer, Legal Counsel | High |
| `/contracts/templates` | Contract template management | Legal Counsel, Contract Manager | High |
| `/contracts/parties` | Contract party management | Contract Officer | High |
| `/contracts/payments` | Contract payment tracking | Finance Officer | High |
| `/contracts/renewals` | Contract renewal management | Contract Officer, Contract Manager | High |
| `/contracts/alerts` | Contract alerts and notifications | All personas | High |
| `/contracts/types` | Contract type configuration | Contract Manager, Admin | Medium |
| `/contracts/reports` | Contract reports: status, compliance, value | Contract Manager, Director | High |
| `/contracts/settings` | Contract settings: approval thresholds, alert rules | Contract Manager | Medium |

### 2.2 UI Components by Screen

#### /contracts/dashboard

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Total Contracts KPI | Metric Card | Total active contracts | Contract WHERE status='active' |
| Expiring This Month KPI | Metric Card | Contracts expiring within 30 days | Contract WHERE expiry in 30 days |
| Pending Approval KPI | Metric Card | Contracts awaiting approval | Contract WHERE status='under_review' |
| Overdue Payments KPI | Metric Card | Overdue contract payments | ContractPayment WHERE overdue |
| Contract Value KPI | Metric Card | Total contract portfolio value | Contract sum |
| Status Breakdown Chart | Pie Chart | Contracts by status | Contract status aggregation |
| Type Breakdown Chart | Bar Chart | Contracts by type | Contract type aggregation |
| Expiry Timeline Chart | Timeline | Contracts expiring over next 12 months | Contract expiry dates |
| Alert Panel | List | Active contract alerts | ContractAlert |
| Quick Actions Bar | Action Bar | New contract, new template, renewal | N/A |

#### /contracts/registry

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Search Bar | Input | Search by contract number, title, party | Contract search |
| Status Filter | Multi-select | Filter by contract status | Contract status |
| Type Filter | Multi-select | Filter by contract type | Contract type |
| Party Filter | Autocomplete | Filter by contract party | ContractParty |
| Date Range Filter | Date Picker | Filter by contract date range | Contract dates |
| Contract Grid | Data Table | Contract list with key columns | Contract registry |
| Status Badge | Badge | Contract status indicator | Contract.status |
| Expiry Warning | Badge | Expiring soon warning | Contract expiry |
| Quick View | Hover Panel | Quick contract details | Contract data |
| Export Button | Action Button | Export to Excel | Contract data |

#### /contracts/registry/{id} (Contract Detail)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Contract Header | Info Panel | Contract number, title, type, status | Contract master |
| Basic Info Form | Form Section | Title, type, parties, dates, value | Contract master |
| Parties Panel | Data Table | All contract parties with roles | ContractParty |
| Terms Section | Form/List | Contract terms and conditions | ContractTerm |
| Payment Schedule | Data Table | Payment milestones and amounts | ContractPayment |
| Payment Progress | Progress Bar | Payment completion percentage | ContractPayment aggregation |
| Renewal Info | Form Section | Renewal terms, auto-renewal flag | ContractRenewal |
| Document List | File Grid | Attached contract documents | Contract documents |
| Lifecycle Timeline | Timeline | Complete contract history | ContractMovement |
| Amendment History | Data Table | Contract amendments and versions | Contract versions |

### 2.3 User Interactions

1. **Contract Creation**: Contract Officer selects template -> fills contract details (title, parties, dates, value, terms) -> attaches draft document -> submits for review -> legal counsel reviews terms -> contract manager approves -> contract activated.
2. **Payment Tracking**: Finance Officer views contract payment schedule -> records payment received/made against milestone -> system updates payment progress -> alerts generated for overdue payments.
3. **Renewal Process**: System generates renewal alert at 90 days -> Contract Officer reviews contract terms -> negotiates renewal terms with party -> creates renewal contract or amendment -> updates expiry date -> alert cleared.
4. **Contract Amendment**: Contract Officer creates amendment -> specifies changed terms (price, quantity, dates) -> follows approval workflow -> amendment activated -> new version created -> history preserved.

### 2.4 Data Displayed

- Contract Registry: HD number, title, type, parties, start date, end date, value, status, payment progress, renewal flag
- Contract Detail: Full contract with parties, terms, payment schedule, documents, amendment history, lifecycle timeline
- Payment Schedule: Milestone name, due date, amount, paid amount, outstanding, status, payment date
- Alerts: Contract title, alert type (expiring, renewal, payment overdue, approval pending), due date, severity

---

## 3. Feature Specifications

### 3.1 Contract Lifecycle Management

**Description**: Create, review, approve, execute, and manage contracts through their complete lifecycle with template-based drafting and version control.

**User Stories**:
- As a Contract Officer, I want to create contracts from templates so that contracts are consistent and compliant.
- As a Legal Counsel, I want to review contract terms before approval so that legal risks are minimized.
- As a Contract Manager, I want to approve contracts within my authority so that the approval process is efficient.

**Acceptance Criteria**:
1. Contract creation from template with: title, type, parties, start date, end date, value, currency, terms.
2. Auto-generated contract number (HD-SAL-2026-0001).
3. Multi-step approval workflow based on value thresholds.
4. Version control for contract amendments.
5. Document attachment for signed contracts.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-001 | Title | Required, 3-300 chars | "Contract title is required (3-300 characters)" |
| VR-002 | Type | Required | "Contract type is required" |
| VR-003 | Parties | At least two parties | "At least two parties required" |
| VR-004 | Start Date | Required, cannot be before today (for new) | "Start date is required" |
| VR-005 | End Date | Must be after start date | "End date must be after start date" |
| VR-006 | Value | Must be > 0 | "Contract value must be positive" |
| VR-007 | Template | Required unless custom approved | "Template required or custom approval" |
| VR-008 | Terms | At least one term | "Contract terms are required" |

**Edge Cases**:
- Contract extension: Extend end date without creating new contract.
- Contract suspension: Temporarily suspend contract with resume option.
- Early termination: Terminate before end date with notice period compliance.
- Multi-party contracts: More than two parties (e.g., tri-party agreements).

### 3.2 Template Management

**Description**: Create and manage contract templates with predefined terms, clauses, and approval workflows.

**User Stories**:
- As a Legal Counsel, I want to create contract templates with standard terms so that contract drafting is efficient.
- As a Contract Officer, I want to select from approved templates so that contracts are compliant.
- As a Contract Manager, I want to approve new templates so that template quality is maintained.

**Acceptance Criteria**:
1. Template creation with: name, type, standard terms, required fields, approval workflow.
2. Template variables for dynamic content (party name, dates, amounts).
3. Template versioning with active/inactive status.
4. Clause library for reusable contract clauses.
5. Template usage tracking.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-010 | Template Name | Required, unique | "Template name is required and must be unique" |
| VR-011 | Type | Required | "Contract type is required" |
| VR-012 | Terms | At least one term defined | "Template must have terms" |
| VR-013 | Variables | All required variables defined | "Required variables not defined" |
| VR-014 | Approval | Active template must have approval workflow | "Approval workflow required" |

**Edge Cases**:
- Template retirement: Mark template inactive, existing contracts unaffected.
- Template update: Update creates new version, existing contracts on old version.
- Template customization: Allow field-level customization with approval.

### 3.3 Payment Schedule Tracking

**Description**: Define and track contract payment schedules with milestones, amounts, due dates, and payment status.

**User Stories**:
- As a Contract Officer, I want to define payment schedules at contract creation so that payment expectations are clear.
- As a Finance Officer, I want to record payments against milestones so that payment tracking is accurate.
- As a Contract Manager, I want to see payment progress so that I can assess contract financial health.

**Acceptance Criteria**:
1. Payment schedule definition with: milestone name, due date, amount, conditions.
2. Payment recording with: amount, date, method, reference, notes.
3. Partial payment support for milestones.
4. Overdue payment detection and alerting.
5. Payment progress visualization (percentage, amount).

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-020 | Milestones | At least one milestone | "Define at least one payment milestone" |
| VR-021 | Total Amount | Sum of milestones = contract value | "Milestone amounts must equal contract value" |
| VR-022 | Payment Amount | Cannot exceed milestone amount | "Payment exceeds milestone amount" |
| VR-023 | Payment Date | Cannot be before milestone due date (warning only) | "Payment before due date" |
| VR-024 | Reference | Required for payment record | "Payment reference is required" |

**Edge Cases**:
- Advance payment: Payment before milestone due date.
- Milestone adjustment: Change milestone amounts with contract amendment.
- Payment dispute: Flag payment as disputed, track resolution.
- Currency fluctuation: Multi-currency contracts with exchange rate handling.

### 3.4 Renewal Management

**Description**: Track contract expirations, manage renewals, and handle auto-renewal clauses.

**User Stories**:
- As a Contract Officer, I want renewal alerts so that contracts are renewed before expiry.
- As a Contract Manager, I want to see upcoming expirations so that I can plan renewals.
- As a Contract Officer, I want to create renewal contracts from existing contracts so that renewal is efficient.

**Acceptance Criteria**:
1. Renewal alerts at configurable intervals (90, 60, 30, 7 days).
2. Auto-renewal flag for contracts with automatic renewal clauses.
3. Renewal creation from existing contract with updated dates and terms.
4. Renewal negotiation tracking.
5. Expiry handling: expired contracts marked, no new transactions allowed.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-030 | Auto-Renewal | Valid auto-renewal terms | "Auto-renewal terms required" |
| VR-031 | Renewal Date | Before expiry date | "Renewal must be before expiry" |
| VR-032 | New Terms | Updated terms for renewal | "Renewal terms are required" |
| VR-033 | Approval | Renewal requires same approval as new | "Renewal approval required" |

**Edge Cases**:
- Auto-renewal triggered: System auto-creates renewal with notification.
- Renewal rejected: Party declines renewal, contract expires.
- Renewal with changes: Significant term changes require full approval workflow.
- Post-expiry renewal: Renew contract after expiry with gap documentation.

### 3.5 Contract Party Management

**Description**: Manage contract parties with role assignment, contact information, and party verification.

**User Stories**:
- As a Contract Officer, I want to add multiple parties to a contract so that all stakeholders are documented.
- As a Legal Counsel, I want to verify party information so that contracts are legally sound.
- As a Contract Manager, I want to see party history so that I can assess party relationships.

**Acceptance Criteria**:
1. Party types: Customer, Supplier, Partner, Employee, Vendor, Contractor.
2. Party information: name, tax code, address, contact person, phone, email.
3. Party role in contract: Buyer, Seller, Guarantor, Witness, etc.
4. Party verification status: Pending, Verified, Rejected.
5. Party history across contracts.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-040 | Party Name | Required | "Party name is required" |
| VR-041 | Tax Code | Required for organizations | "Tax code required for organizations" |
| VR-042 | Role | Required, valid role | "Valid party role is required" |
| VR-043 | Verification | Must be verified for active contract | "Party must be verified" |
| VR-044 | Contact | At least one contact method | "Contact information required" |

**Edge Cases**:
- Party name change: Update across all contracts with history.
- Party merger: Merge party records, preserve contract history.
- Party bankruptcy: Flag party, review affected contracts.

### 3.6 Contract Alerts and Notifications

**Description**: Automated alert system for contract events including expiry, renewal, payment overdue, approval pending, and compliance deadlines.

**User Stories**:
- As a Contract Officer, I want alerts for contract expirations so that I can plan renewals.
- As a Finance Officer, I want alerts for overdue payments so that I can follow up.
- As a Contract Manager, I want a consolidated alert dashboard so that I can prioritize actions.

**Acceptance Criteria**:
1. Alert types: Expiry (90/60/30/7 days), Renewal due, Payment overdue, Approval pending, Compliance deadline, Amendment due.
2. Configurable alert timing per alert type.
3. Alert severity levels: Info, Warning, Critical.
4. Alert acknowledgment and resolution tracking.
5. Escalation for unacknowledged critical alerts.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-050 | Alert Type | Valid type | "Valid alert type is required" |
| VR-051 | Due Date | Required | "Due date is required" |
| VR-052 | Severity | Valid severity | "Valid severity is required" |
| VR-053 | Contract | Required | "Contract reference required" |

**Edge Cases**:
- Alert suppression: Temporarily suppress alerts during negotiation.
- Bulk acknowledgment: Acknowledge multiple alerts at once.
- Alert delegation: Delegate alert response to another user.
- Recurring alerts: Payment overdue alerts repeat daily until resolved.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+---------------------+       +---------------------+       +---------------------+
|    ContractType     |       |     Contract        |       |   ContractStatus    |
+---------------------+       +---------------------+       +---------------------+
| PK type_id          |<----->| PK contract_id      |       | PK status_id        |
| type_name           |       | contract_number     |       | status_name         |
| type_code           |       | FK type_id          |       | status_code         |
| default_terms       |       | FK status_id        |       | is_active           |
| approval_threshold  |       | title               |       | is_terminal         |
| is_active           |       | FK template_id      |       +---------------------+
+---------------------+       | start_date          |
                              | end_date            |       +---------------------+
                              | contract_value      |       | ContractTemplate    |
                              | currency            |       +---------------------+
                              | FK party_a_id       |       | PK template_id      |
                              | FK party_b_id       |       | template_name       |
                              +---------------------+       | FK type_id          |
                                    |                       | terms               |
                    +-------------------------------+       | approval_workflow   |
                    |    ContractParty             |       | is_active           |
                    +-------------------------------+       | version             |
                    | PK party_id                  |       +---------------------+
                    | FK contract_id               |
                    | FK entity_id                 |       +---------------------+
                    | party_role                   |       |   ContractTerm      |
                    | party_type                   |       +---------------------+
                    | tax_code                     |       | PK term_id          |
                    | contact_person               |       | FK contract_id      |
                    | phone                        |       | term_name           |
                    | email                        |       | term_value          |
                    | address                      |       | term_order          |
                    | verification_status          |       +---------------------+
                    +-------------------------------+

                    +---------------------+       +---------------------+
                    | ContractPayment     |       | ContractRenewal     |
                    +---------------------+       +---------------------+
                    | PK payment_id       |       | PK renewal_id       |
                    | FK contract_id      |       | FK contract_id      |
                    | milestone_name      |       | FK renewed_contract |
                    | due_date            |       | renewal_date        |
                    | amount              |       | new_start_date      |
                    | paid_amount         |       | new_end_date        |
                    | payment_date        |       | new_value           |
                    | payment_method      |       | status              |
                    | status              |       | FK approved_by      |
                    | reference           |       +---------------------+
                    +---------------------+

                    +---------------------+       +---------------------+
                    |   ContractAlert     |       |  ContractMovement   |
                    +---------------------+       +---------------------+
                    | PK alert_id         |       | PK movement_id      |
                    | FK contract_id      |       | FK contract_id      |
                    | alert_type          |       | movement_type       |
                    | due_date            |       | movement_date       |
                    | severity            |       | from_status         |
                    | status              |       | to_status           |
                    | acknowledged_by     |       | notes               |
                    | acknowledged_at     |       | FK created_by       |
                    | resolved_at         |       +---------------------+
                    +---------------------+
```

### 4.2 Table Schemas

#### Contract

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| contract_id | BIGINT | PK, SERIAL | Unique contract ID |
| contract_number | VARCHAR(30) | NOT NULL, UNIQUE | Auto-generated (HD-SAL-2026-0001) |
| title | NVARCHAR(300) | NOT NULL | Contract title |
| FK type_id | BIGINT | NOT NULL, FK REFERENCES ContractType | Contract type |
| FK status_id | BIGINT | NOT NULL, FK REFERENCES ContractStatus | Current status |
| FK template_id | BIGINT | FK REFERENCES ContractTemplate | Source template |
| description | TEXT | NULL | Contract description |
| start_date | DATE | NOT NULL | Contract start date |
| end_date | DATE | NOT NULL | Contract end date |
| contract_value | DECIMAL(18,2) | NOT NULL | Total contract value |
| currency | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | Contract currency |
| auto_renewal | BOOLEAN | NOT NULL, DEFAULT FALSE | Auto-renewal flag |
| renewal_notice_days | INT | DEFAULT 30 | Days before expiry to notify |
| FK party_a_id | BIGINT | NOT NULL, FK REFERENCES ContractParty | First party |
| FK party_b_id | BIGINT | NOT NULL, FK REFERENCES ContractParty | Second party |
| FK approved_by | BIGINT | NULL, FK REFERENCES User | Approver |
| approved_date | TIMESTAMP | NULL | Approval timestamp |
| signed_date | DATE | NULL | Date contract was signed |
| termination_date | DATE | NULL | Termination date (if terminated) |
| termination_reason | TEXT | NULL | Termination reason |
| notes | TEXT | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### ContractType

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| type_id | BIGINT | PK, SERIAL | Unique type ID |
| type_name | NVARCHAR(100) | NOT NULL | Type name |
| type_code | VARCHAR(10) | NOT NULL, UNIQUE | Type code (SAL, PUR, SRV, LSE, EMP, PRT) |
| default_terms | JSONB | NULL | Default terms for this type |
| approval_threshold | DECIMAL(18,2) | DEFAULT 0 | Default approval threshold |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

#### ContractStatus

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| status_id | BIGINT | PK, SERIAL | Unique status ID |
| status_name | NVARCHAR(50) | NOT NULL | Status name |
| status_code | VARCHAR(20) | NOT NULL, UNIQUE | Status code |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| is_terminal | BOOLEAN | NOT NULL, DEFAULT FALSE | Terminal status |
| display_order | INT | NOT NULL | Display order |
| color | VARCHAR(7) | NOT NULL | Display color |

#### ContractTemplate

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| template_id | BIGINT | PK, SERIAL | Unique template ID |
| template_name | NVARCHAR(100) | NOT NULL | Template name |
| FK type_id | BIGINT | NOT NULL, FK REFERENCES ContractType | Contract type |
| terms | JSONB | NOT NULL | Template terms and clauses |
| approval_workflow | JSONB | NULL | Approval workflow definition |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| version | INT | NOT NULL, DEFAULT 1 | Template version |
| created_by | BIGINT | FK REFERENCES User | Creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### ContractTerm

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| term_id | BIGINT | PK, SERIAL | Unique term ID |
| FK contract_id | BIGINT | NOT NULL, FK REFERENCES Contract | Contract |
| term_name | NVARCHAR(100) | NOT NULL | Term name (e.g., Price, Quantity) |
| term_value | TEXT | NOT NULL | Term value |
| term_order | INT | NOT NULL | Display order |
| is_mandatory | BOOLEAN | NOT NULL, DEFAULT FALSE | Mandatory term flag |

#### ContractParty

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| party_id | BIGINT | PK, SERIAL | Unique party ID |
| FK contract_id | BIGINT | NOT NULL, FK REFERENCES Contract | Contract |
| FK entity_id | BIGINT | NOT NULL | Linked entity (Customer, Supplier, Employee ID) |
| party_role | VARCHAR(30) | NOT NULL | buyer, seller, guarantor, witness, partner |
| party_type | VARCHAR(20) | NOT NULL | customer, supplier, employee, partner, vendor |
| party_name | NVARCHAR(200) | NOT NULL | Party name |
| tax_code | VARCHAR(20) | NULL | Tax code |
| contact_person | NVARCHAR(100) | NULL | Contact person |
| phone | VARCHAR(20) | NULL | Phone |
| email | VARCHAR(100) | NULL | Email |
| address | NVARCHAR(300) | NULL | Address |
| verification_status | VARCHAR(20) | DEFAULT 'pending' | pending, verified, rejected |
| notes | NVARCHAR(300) | NULL | Party notes |

#### ContractPayment

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| payment_id | BIGINT | PK, SERIAL | Unique payment ID |
| FK contract_id | BIGINT | NOT NULL, FK REFERENCES Contract | Contract |
| milestone_name | NVARCHAR(100) | NOT NULL | Payment milestone name |
| milestone_order | INT | NOT NULL | Milestone order |
| due_date | DATE | NOT NULL | Payment due date |
| amount | DECIMAL(18,2) | NOT NULL | Milestone amount |
| paid_amount | DECIMAL(18,2) | DEFAULT 0 | Amount paid to date |
| payment_date | DATE | NULL | Actual payment date |
| payment_method | VARCHAR(20) | NULL | cash, transfer, credit, letter_of_credit |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, partial, paid, overdue, disputed |
| reference | VARCHAR(50) | NULL | Payment reference (bank transfer ref) |
| notes | NVARCHAR(300) | NULL | Payment notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### ContractRenewal

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| renewal_id | BIGINT | PK, SERIAL | Unique renewal ID |
| FK contract_id | BIGINT | NOT NULL, FK REFERENCES Contract | Original contract |
| FK renewed_contract_id | BIGINT | NULL, FK REFERENCES Contract | Renewed contract |
| renewal_date | DATE | NOT NULL | Renewal date |
| new_start_date | DATE | NOT NULL | New contract start date |
| new_end_date | DATE | NOT NULL | New contract end date |
| new_value | DECIMAL(18,2) | NOT NULL | New contract value |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, approved, rejected, expired |
| FK approved_by | BIGINT | NULL, FK REFERENCES User | Approver |
| approved_date | TIMESTAMP | NULL | Approval timestamp |
| terms_changes | JSONB | NULL | Changed terms from original |
| notes | TEXT | NULL | Renewal notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### ContractAlert

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| alert_id | BIGINT | PK, SERIAL | Unique alert ID |
| FK contract_id | BIGINT | NOT NULL, FK REFERENCES Contract | Contract |
| alert_type | VARCHAR(30) | NOT NULL | expiry, renewal, payment_overdue, approval_pending, compliance, amendment |
| due_date | DATE | NOT NULL | Alert due date |
| severity | VARCHAR(20) | NOT NULL, DEFAULT 'warning' | info, warning, critical |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, acknowledged, resolved, expired |
| FK acknowledged_by | BIGINT | NULL, FK REFERENCES User | Acknowledging user |
| acknowledged_at | TIMESTAMP | NULL | Acknowledgment timestamp |
| resolved_at | TIMESTAMP | NULL | Resolution timestamp |
| resolved_by | BIGINT | NULL, FK REFERENCES User | Resolving user |
| notes | TEXT | NULL | Alert notes |

#### ContractMovement

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| movement_id | BIGINT | PK, SERIAL | Unique movement ID |
| FK contract_id | BIGINT | NOT NULL, FK REFERENCES Contract | Contract |
| movement_type | VARCHAR(30) | NOT NULL | status_change, payment, amendment, renewal, termination |
| movement_date | TIMESTAMP | NOT NULL, DEFAULT NOW() | Movement timestamp |
| from_status | VARCHAR(20) | NULL | Previous status |
| to_status | VARCHAR(20) | NULL | New status |
| description | TEXT | NULL | Movement description |
| notes | TEXT | NULL | Movement notes |
| FK created_by | BIGINT | FK REFERENCES User | User who recorded |

### 4.3 Indexes

```sql
-- Contract indexes
CREATE INDEX idx_contract_number ON Contract(contract_number);
CREATE INDEX idx_contract_type ON Contract(type_id);
CREATE INDEX idx_contract_status ON Contract(status_id);
CREATE INDEX idx_contract_dates ON Contract(start_date, end_date);
CREATE INDEX idx_contract_value ON Contract(contract_value);
CREATE INDEX idx_contract_party_a ON Contract(party_a_id);
CREATE INDEX idx_contract_party_b ON Contract(party_b_id);
CREATE INDEX idx_contract_expiry ON Contract(end_date) WHERE status_id IN (active_status_id);
CREATE INDEX idx_contract_template ON Contract(template_id);

-- ContractParty indexes
CREATE INDEX idx_cp_contract ON ContractParty(contract_id);
CREATE INDEX idx_cp_entity ON ContractParty(entity_id);
CREATE INDEX idx_cp_role ON ContractParty(party_role);
CREATE INDEX idx_cp_type ON ContractParty(party_type);
CREATE INDEX idx_cp_verification ON ContractParty(verification_status);

-- ContractPayment indexes
CREATE INDEX idx_cpay_contract ON ContractPayment(contract_id);
CREATE INDEX idx_cpay_due ON ContractPayment(due_date);
CREATE INDEX idx_cpay_status ON ContractPayment(status);
CREATE INDEX idx_cpay_overdue ON ContractPayment(due_date, status) WHERE status = 'pending' AND due_date < CURRENT_DATE;
CREATE INDEX idx_cpay_milestone ON ContractPayment(contract_id, milestone_order);

-- ContractRenewal indexes
CREATE INDEX idx_renewal_contract ON ContractRenewal(contract_id);
CREATE INDEX idx_renewal_status ON ContractRenewal(status);
CREATE INDEX idx_renewal_date ON ContractRenewal(renewal_date);

-- ContractAlert indexes
CREATE INDEX idx_alert_contract ON ContractAlert(contract_id);
CREATE INDEX idx_alert_type ON ContractAlert(alert_type);
CREATE INDEX idx_alert_due ON ContractAlert(due_date);
CREATE INDEX idx_alert_status ON ContractAlert(status) WHERE status = 'pending';
CREATE INDEX idx_alert_severity ON ContractAlert(severity) WHERE severity = 'critical';

-- ContractMovement indexes
CREATE INDEX idx_cmov_contract ON ContractMovement(contract_id);
CREATE INDEX idx_cmov_type ON ContractMovement(movement_type);
CREATE INDEX idx_cmov_date ON ContractMovement(movement_date);

-- ContractTemplate indexes
CREATE INDEX idx_template_type ON ContractTemplate(type_id);
CREATE INDEX idx_template_active ON ContractTemplate(is_active) WHERE is_active = TRUE;

-- ContractTerm indexes
CREATE INDEX idx_cterm_contract ON ContractTerm(contract_id);
CREATE INDEX idx_cterm_order ON ContractTerm(contract_id, term_order);
```

### 4.4 Summary of Tables

| Table | Primary Key | Foreign Keys | Purpose | Estimated Rows |
|-------|-------------|--------------|---------|----------------|
| Contract | contract_id | ContractType, ContractStatus, ContractTemplate, User | Contract registry | 2,000 |
| ContractType | type_id | - | Contract type definitions | 10 |
| ContractStatus | status_id | - | Contract status definitions | 8 |
| ContractTemplate | template_id | ContractType, User | Contract templates | 30 |
| ContractTerm | term_id | Contract | Contract terms | 20,000 |
| ContractParty | party_id | Contract | Contract party records | 4,000 |
| ContractPayment | payment_id | Contract | Payment milestones | 10,000 |
| ContractRenewal | renewal_id | Contract (x2), User | Renewal records | 500 |
| ContractAlert | alert_id | Contract, User | Contract alerts | 10,000 |
| ContractMovement | movement_id | Contract, User | Contract lifecycle history | 15,000 |

---

## 5. API Specifications

### 5.1 Contract API

#### POST /api/v1/contracts

Create a new contract.

**Request Body**:
```json
{
  "title": "Hop dong cung cap phan mem ERP",
  "type_id": 1,
  "template_id": 5,
  "start_date": "2026-05-01",
  "end_date": "2027-05-01",
  "contract_value": 500000000,
  "currency": "VND",
  "auto_renewal": false,
  "renewal_notice_days": 60,
  "parties": [
    {
      "entity_id": 1001,
      "party_role": "seller",
      "party_type": "supplier",
      "party_name": "Cong ty ABN",
      "tax_code": "0123456789",
      "contact_person": "Nguyen Van A",
      "phone": "+84912345678",
      "email": "contact@abn.com.vn"
    },
    {
      "entity_id": 2001,
      "party_role": "buyer",
      "party_type": "customer",
      "party_name": "Cong ty XYZ",
      "tax_code": "0987654321",
      "contact_person": "Tran Thi B",
      "phone": "+84987654321",
      "email": "tranb@xyz.com.vn"
    }
  ],
  "terms": [
    {"term_name": "Pham vi cung cap", "term_value": "He thong ERP toan bo doanh nghiep", "term_order": 1},
    {"term_name": "Phuong thuc thanh toan", "term_value": "Chuyen khoan, 3 dot", "term_order": 2},
    {"term_name": "Thoi gian bao hanh", "term_value": "12 thang", "term_order": 3}
  ],
  "payment_schedule": [
    {"milestone_name": "Dot 1 - Ky hop dong", "milestone_order": 1, "due_date": "2026-05-01", "amount": 150000000},
    {"milestone_name": "Dot 2 - Nghiem thu", "milestone_order": 2, "due_date": "2026-08-01", "amount": 250000000},
    {"milestone_name": "Dot 3 - Quyet toan", "milestone_order": 3, "due_date": "2026-11-01", "amount": 100000000}
  ]
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "contract_id": 201,
    "contract_number": "HD-SAL-2026-0201",
    "title": "Hop dong cung cap phan mem ERP",
    "type_id": 1,
    "type_name": "Hop dong ban hang",
    "status_id": 1,
    "status_name": "Draft",
    "start_date": "2026-05-01",
    "end_date": "2027-05-01",
    "contract_value": 500000000,
    "currency": "VND",
    "parties": [
      {"party_id": 401, "party_role": "seller", "party_name": "Cong ty ABN"},
      {"party_id": 402, "party_role": "buyer", "party_name": "Cong ty XYZ"}
    ],
    "payment_schedule": [
      {"payment_id": 1001, "milestone_name": "Dot 1 - Ky hop dong", "due_date": "2026-05-01", "amount": 150000000, "status": "pending"},
      {"payment_id": 1002, "milestone_name": "Dot 2 - Nghiem thu", "due_date": "2026-08-01", "amount": 250000000, "status": "pending"},
      {"payment_id": 1003, "milestone_name": "Dot 3 - Quyet toan", "due_date": "2026-11-01", "amount": 100000000, "status": "pending"}
    ],
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

#### GET /api/v1/contracts

List contracts with filtering.

**Query Parameters**:
- `type_id` (int): Filter by type
- `status_id` (int): Filter by status
- `party_id` (int): Filter by party
- `value_min` (decimal): Minimum value
- `value_max` (decimal): Maximum value
- `expiry_within` (int): Days until expiry
- `search` (string): Search by number, title, party
- `page` (int): Page number
- `per_page` (int): Items per page

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "contracts": [
      {
        "contract_id": 201,
        "contract_number": "HD-SAL-2026-0201",
        "title": "Hop dong cung cap phan mem ERP",
        "type_name": "Hop dong ban hang",
        "status_name": "Active",
        "status_color": "#10B981",
        "party_a_name": "Cong ty ABN",
        "party_b_name": "Cong ty XYZ",
        "start_date": "2026-05-01",
        "end_date": "2027-05-01",
        "contract_value": 500000000,
        "payment_progress": 30,
        "days_to_expiry": 382,
        "auto_renewal": false
      }
    ],
    "summary": {
      "total_contracts": 245,
      "active": 180,
      "expiring_30_days": 12,
      "pending_approval": 8,
      "overdue_payments": 5,
      "total_value": 25000000000
    },
    "pagination": {
      "page": 1,
      "per_page": 20,
      "total_pages": 13,
      "total_count": 245
    }
  }
}
```

### 5.2 Payment API

#### POST /api/v1/contracts/{contract_id}/payments/{payment_id}/record

Record a contract payment.

**Request Body**:
```json
{
  "paid_amount": 150000000,
  "payment_date": "2026-05-01",
  "payment_method": "transfer",
  "reference": "UB20260501001",
  "notes": "Thanh toan dot 1 theo hop dong"
}
```

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "payment_id": 1001,
    "contract_number": "HD-SAL-2026-0201",
    "milestone_name": "Dot 1 - Ky hop dong",
    "amount": 150000000,
    "paid_amount": 150000000,
    "outstanding": 0,
    "payment_date": "2026-05-01",
    "payment_method": "transfer",
    "reference": "UB20260501001",
    "status": "paid",
    "contract_payment_progress": 30
  }
}
```

### 5.3 Renewal API

#### POST /api/v1/contracts/{contract_id}/renewals

Create a contract renewal.

**Request Body**:
```json
{
  "renewal_date": "2027-04-01",
  "new_start_date": "2027-05-02",
  "new_end_date": "2028-05-01",
  "new_value": 550000000,
  "terms_changes": {
    "price_increase_pct": 10,
    "additional_terms": "Extended support included"
  },
  "notes": "Renewal with 10% price adjustment"
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "renewal_id": 51,
    "contract_id": 201,
    "contract_number": "HD-SAL-2026-0201",
    "renewal_date": "2027-04-01",
    "new_start_date": "2027-05-02",
    "new_end_date": "2028-05-01",
    "new_value": 550000000,
    "status": "pending",
    "terms_changes": {
      "price_increase_pct": 10,
      "additional_terms": "Extended support included"
    },
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

### 5.4 Dashboard API

#### GET /api/v1/contracts/dashboard

Get contract dashboard summary.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "summary": {
      "total_contracts": 245,
      "active": 180,
      "draft": 15,
      "under_review": 8,
      "expired": 30,
      "terminated": 12
    },
    "expiring": {
      "within_7_days": 3,
      "within_30_days": 12,
      "within_90_days": 25
    },
    "pending_approvals": 8,
    "overdue_payments": 5,
    "total_value": {
      "active_contracts": 25000000000,
      "total_receivable": 8000000000,
      "total_payable": 3000000000
    },
    "alerts": {
      "critical": 5,
      "warning": 12,
      "info": 8
    },
    "type_breakdown": [
      {"type_name": "Hop dong ban hang", "count": 80, "value": 12000000000},
      {"type_name": "Hop dong mua hang", "count": 60, "value": 8000000000},
      {"type_name": "Hop dong dich vu", "count": 40, "value": 5000000000}
    ]
  }
}
```

### 5.5 Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Contract validation failed",
    "details": [
      {
        "field": "payment_schedule",
        "message": "Total milestone amounts (450,000,000) do not equal contract value (500,000,000)"
      }
    ],
    "request_id": "req-ctr123"
  }
}
```

### 5.6 API Summary Table

| Endpoint | Method | Auth Required | Rate Limit | Description |
|----------|--------|---------------|------------|-------------|
| /api/v1/contracts | POST | Yes | 30/min | Create contract |
| /api/v1/contracts | GET | Yes | 200/min | List contracts |
| /api/v1/contracts/{id} | GET | Yes | 200/min | Get contract detail |
| /api/v1/contracts/{id} | PUT | Yes | 30/min | Update contract |
| /api/v1/contracts/{id}/submit | POST | Yes | 20/min | Submit for review |
| /api/v1/contracts/{id}/approve | POST | Yes | 20/min | Approve contract |
| /api/v1/contracts/{id}/payments | GET | Yes | 200/min | List payment milestones |
| /api/v1/contracts/{id}/payments/{pid}/record | POST | Yes | 50/min | Record payment |
| /api/v1/contracts/{id}/renewals | POST | Yes | 20/min | Create renewal |
| /api/v1/contracts/{id}/renewals | GET | Yes | 200/min | List renewals |
| /api/v1/contracts/{id}/parties | GET | Yes | 200/min | List parties |
| /api/v1/contracts/{id}/movements | GET | Yes | 200/min | Lifecycle history |
| /api/v1/contracts/templates | POST | Yes | 10/min | Create template |
| /api/v1/contracts/templates | GET | Yes | 200/min | List templates |
| /api/v1/contracts/alerts | GET | Yes | 100/min | List contract alerts |
| /api/v1/contracts/dashboard | GET | Yes | 100/min | Dashboard summary |
| /api/v1/contracts/reports | GET | Yes | 30/min | Contract reports |
| /api/v1/contracts/types | GET | Yes | 200/min | List contract types |

---

## 6. Permissions Matrix

### 6.1 Role Definitions

| Role | Code | Description | Department |
|------|------|-------------|------------|
| Contract Manager | CTR_MGR | Full contract management | Legal/Contracts |
| Contract Officer | CTR_OFFICER | Contract administration | Legal/Contracts |
| Legal Counsel | LEGAL | Legal review | Legal |
| Finance Officer | FIN_OFFICER | Payment tracking | Finance |
| Sales Representative | SALES_REP | Sales contracts | Sales |
| Purchasing Officer | PURCH_OFFICER | Purchase contracts | Purchasing |
| Department Manager | DEPT_MGR | Contract oversight | Various |
| Director | DIRECTOR | Strategic oversight | Executive |

### 6.2 CRUD Permissions

| Resource | CTR_MGR | CTR_OFFICER | LEGAL | FIN_OFFICER | SALES_REP | PURCH_OFFICER | DEPT_MGR | DIRECTOR |
|----------|---------|-------------|-------|-------------|-----------|---------------|----------|----------|
| Contract | CRUD | CRUD | R + Review | R | CRUD (own) | CRUD (own) | R (dept) | R |
| ContractTemplate | CRUD | R | CRUD | R | R | R | R | R |
| ContractParty | CRUD | CRUD | R | R | CRUD (own) | CRUD (own) | R | R |
| ContractPayment | R | R | R | CRUD | R | R | R | R |
| ContractRenewal | CRUD | CRUD | R | R | R | R | R | R |
| ContractAlert | CRUD | CRUD | R | R | R | R | R | R |

### 6.3 Row-Level Security

| Resource | Rule | Description |
|----------|------|-------------|
| Contract | Sales/Purchasing reps see only own contracts | `created_by = user.id OR user.role IN ('CTR_MGR', 'CTR_OFFICER', 'LEGAL', 'DIRECTOR')` |
| ContractPayment | Finance officers see all payments | No restriction for finance roles |
| ContractAlert | Users see alerts for contracts they manage | `contract_id IN (user.managed_contracts) OR user.role IN ('CTR_MGR', 'DIRECTOR')` |

### 6.4 Approval Thresholds

| Contract Value | Required Approver | Escalation |
|----------------|-------------------|------------|
| < 100M VND | Department Manager | 24h -> Contract Manager |
| 100M - 500M VND | Contract Manager | 48h -> Director |
| > 500M VND | Director | No escalation |

---

## 7. State Machine

### 7.1 Contract State Diagram

```
                    +---------+
                    |  DRAFT  |
                    +----+----+
                         |
                   (Submit)
                         v
                  +-------------+
           +----->| UNDER REVIEW|<----+
           |      +------+------+     |
           |             |            |
      +----+----+  +----+----+        |
      |         |  |         |        |
      v         v  v         v        |
   +-------+ +------+ +-----------+   |
   |APPROVED| |REJECTED| NEEDS CHANGE|--+
   +---+---+ +------+ +-----------+
       |
       v (Sign)
   +---------+
   |  ACTIVE |
   +----+----+
        |
   +----+----+
   |         |
   v         v
+--------+ +-----------+
|EXPIRED | | TERMINATED|
+--------+ +-----------+
```

### 7.2 State Transition Tables

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| DRAFT | UNDER REVIEW | Submit | Contract Officer | All fields complete |
| UNDER REVIEW | APPROVED | Approve | Approver (per threshold) | Review complete |
| UNDER REVIEW | REJECTED | Reject | Approver | Reason required |
| UNDER REVIEW | DRAFT | Request Changes | Approver | Changes needed |
| REJECTED | DRAFT | Revise | Contract Officer | Address rejection |
| APPROVED | ACTIVE | Sign | Both parties | Contract signed |
| ACTIVE | EXPIRED | End date reached | System | Automatic |
| ACTIVE | TERMINATED | Early termination | Contract Manager | Per termination clause |
| EXPIRED | ACTIVE | Renewed | Contract Officer | Renewal contract signed |

### 7.3 Approval Workflow

```
Contract Officer creates contract -> Draft
     |
     v
Check value for routing
     /      |         \
  <100M   100-500M    >500M
    |        |          |
    v        v          v
  Dept    Contract    Director
  Manager  Manager    Approval
  Approval  Approval    |
    |        |          v
    +---+----+----- Approved
        |             |
        v             v
    Signed         Signed
        |             |
        v             v
      ACTIVE        ACTIVE
```

---

## 8. Reports

### 8.1 Report Inventory

| Report | Description | Dimensions | Metrics | Filters | Chart Type |
|--------|-------------|------------|---------|---------|------------|
| Contract Register | Complete contract listing | Type, status, party | Count, total value, avg value | Type, status | Table |
| Contract Expiry | Upcoming contract expirations | Type, month | Expiring count, total value | Date range | Timeline |
| Payment Compliance | Payment tracking compliance | Contract, milestone | On-time %, overdue count, total overdue | Period, type | Bar chart |
| Contract Value | Contract portfolio value analysis | Type, party, period | Total value, by type, by party | Period | Stacked bar |
| Renewal Rate | Contract renewal analysis | Type, period | Renewal rate, avg renewal value change | Period, type | Line chart |
| Approval Efficiency | Approval process metrics | Approver, level, period | Avg approval time, rejection rate | Period, approver | Box plot |
| Contract Type Distribution | Portfolio composition | Type | Count, value, avg duration | Current | Pie chart |
| Party Analysis | Contract party analysis | Party | Contract count, total value, compliance rate | Period, party | Scatter plot |

### 8.2 Report Summary Table

| Report ID | Report Name | Schedule | Auto-Generate | Distribution | Retention |
|-----------|-------------|----------|---------------|--------------|-----------|
| RPT-CTR-001 | Contract Register | Monthly | Yes | Contract Manager, Director | 10 years |
| RPT-CTR-002 | Contract Expiry | Weekly | Yes | Contract Officer, Manager | 2 years |
| RPT-CTR-003 | Payment Compliance | Monthly | Yes | Finance Officer, Manager | 5 years |
| RPT-CTR-004 | Contract Value | Quarterly | Yes | Director, Finance Manager | 10 years |
| RPT-CTR-005 | Renewal Rate | Quarterly | Yes | Contract Manager | 5 years |
| RPT-CTR-006 | Approval Efficiency | Monthly | Yes | Contract Manager | 2 years |
| RPT-CTR-007 | Type Distribution | Monthly | Yes | Contract Manager, Director | 5 years |
| RPT-CTR-008 | Party Analysis | Quarterly | Yes | Contract Manager, Director | 5 years |

---

## 9. Integration Points

### 9.1 External API Integrations

| Integration | Provider | Protocol | Direction | Frequency | Data Exchanged |
|------------|----------|----------|-----------|-----------|----------------|
| E-Signature | VNPT, Viettel CA | HTTPS/API | Outbound | On signing | Digital signature requests, confirmations |
| Document Storage | Cloud storage (S3) | HTTPS/API | Bidirectional | On demand | Contract document upload/download |
| Email Notifications | SMTP/SendGrid | SMTP/API | Outbound | Real-time | Contract notifications |
| Party Verification | Tax Authority | HTTPS/API | Outbound | On contract creation | Tax code verification |

### 9.2 Internal Module Integrations

| Source Module | Target | Data Flow | Trigger | Frequency |
|---------------|--------|-----------|---------|-----------|
| M11 Contract Mgmt | M04 Accounting | Contract payments -> GL | Payment recorded | Real-time |
| M11 Contract Mgmt | M07 Purchasing | Purchase contract -> PO terms | Contract active | Real-time |
| M11 Contract Mgmt | M08 Sales | Sales contract -> Price list | Contract active | Real-time |
| M11 Contract Mgmt | M10 Asset Mgmt | Lease contract -> Asset tracking | Contract active | Real-time |
| M07 Purchasing | M11 Contract Mgmt | PO -> Contract reference | PO created | Real-time |
| M08 Sales | M11 Contract Mgmt | Sale -> Contract reference | Sale created | Real-time |
| M04 Accounting | M11 Contract Mgmt | Payment confirmation -> Contract | Payment posted | Real-time |

### 9.3 Webhook Events

| Event | Trigger | Payload | Subscribers |
|-------|---------|---------|-------------|
| contract.created | New contract created | contract_id, number, title, type, value | Parties, Finance |
| contract.approved | Contract approved | contract_id, approver, approval_date | Parties, Finance |
| contract.activated | Contract signed and activated | contract_id, signed_date, start_date | All modules |
| contract.expiring | Contract expiring soon | contract_id, end_date, days_remaining | Contract Officer, Manager |
| contract.payment_due | Payment milestone due | contract_id, payment_id, amount, due_date | Finance, Parties |
| contract.payment_overdue | Payment overdue | contract_id, payment_id, amount, days_overdue | Finance, Manager |
| contract.renewed | Contract renewed | contract_id, renewal_id, new_end_date | Contract Officer |
| contract.terminated | Contract terminated | contract_id, reason, termination_date | All parties, Finance |

---

## 10. Technical Notes

### 10.1 Performance Requirements

| Metric | Target | Measurement | Notes |
|--------|--------|-------------|-------|
| Contract Search | < 500ms | P95 latency | With multiple filters |
| Contract Detail Load | < 800ms | P95 latency | Full detail with payments |
| Contract List (paginated) | < 1s | P95 latency | 20 items per page |
| Payment Record | < 300ms | P95 latency | Single payment recording |
| Dashboard Load | < 1.5s | P95 latency | All KPIs and charts |
| Report Generation | < 5s | P95 latency | Standard reports |
| Concurrent Users | 100+ | Simultaneous | Read and write operations |

### 10.2 Caching Strategy

| Cache Type | Data | TTL | Invalidation |
|------------|------|-----|--------------|
| Redis | Contract list (recent) | 5 minutes | On contract update |
| Redis | Dashboard summary | 5 minutes | On any contract change |
| Redis | Active alerts | 2 minutes | On alert change |
| Redis | Template catalog | 1 hour | On template change |
| Redis | Type/status catalogs | 1 hour | On catalog change |
| Application | Approval threshold config | 1 day | On config change |
| Browser | Contract documents | 1 hour | On document update |

### 10.3 Key Files and Components

| File/Component | Path | Purpose |
|----------------|------|---------|
| Contract Service | `src/modules/contracts/services/contract.service.ts` | Contract CRUD, lifecycle |
| Template Service | `src/modules/contracts/services/template.service.ts` | Template management |
| Payment Service | `src/modules/contracts/services/payment.service.ts` | Payment tracking |
| Renewal Service | `src/modules/contracts/services/renewal.service.ts` | Renewal management |
| Party Service | `src/modules/contracts/services/party.service.ts` | Party management |
| Alert Service | `src/modules/contracts/services/alert.service.ts` | Alert generation |
| Approval Service | `src/modules/contracts/services/approval.service.ts` | Approval workflow |
| Report Generator | `src/modules/contracts/services/report-generator.service.ts` | Contract reports |
| E-Signature Service | `src/modules/contracts/services/esignature.service.ts` | Digital signatures |
| Document Service | `src/modules/contracts/services/document.service.ts` | Contract document storage |

### 10.4 Localization

| Aspect | Configuration | Notes |
|--------|---------------|-------|
| Language | Vietnamese (primary), English | All labels, messages, templates |
| Currency | VND (base), multi-currency support | Exchange rate handling |
| Date Format | DD/MM/YYYY | Vietnamese standard |
| Number Format | 1.234.567.890 (dot separator) | Vietnamese standard |
| Contract Number Format | HD-{type_code}-{year}-{sequential} | e.g., HD-SAL-2026-0001 |
| Legal Compliance | Vietnamese contract law | Per Civil Code 2015 |
| E-Signature | Vietnamese e-signature law | Per Law on E-Transactions |

### 10.5 Mobile Support

| Feature | Mobile Support | Notes |
|---------|----------------|-------|
| Contract Search | Responsive | Search and view |
| Contract Detail | Responsive | Full detail view |
| Payment Recording | Responsive | Record payments |
| Approval | Responsive | Approve/reject on the go |
| Alert Viewing | Responsive | View and acknowledge |
| Document Viewing | Responsive | View contract documents |
| E-Signature | Mobile SDK | Sign contracts on mobile |
| Push Notifications | Push | Expiry, payment, approval alerts |

### 10.6 Security Considerations

| Aspect | Implementation |
|--------|----------------|
| Contract Confidentiality | RBAC with row-level security |
| Document Security | Encrypted storage, signed URLs |
| E-Signature Integrity | Tamper-evident digital signatures |
| Payment Data Protection | Encrypted payment references |
| Audit Trail | Complete lifecycle history |
| Approval Integrity | Immutable approval records |
| Data Retention | Retain contracts 10 years post-expiry |
| Party Data Privacy | PII encrypted at rest |
| Version Control | Immutable contract versions |
| Access Logging | Complete access audit trail |

### 10.7 Database Considerations

- All monetary values stored as DECIMAL(18,2)
- Soft deletes not used for contracts; terminated/expired status preserved
- Partition ContractPayment table by year
- Archive expired/terminated contracts after 10 years
- Materialized views for dashboard metrics
- Full-text search on contract title and description
- Unique constraints on contract_number, template_name
- ContractMovement append-only for complete audit trail
- JSONB storage for terms and approval workflows

---

*Document Version: 1.0*
*Last Updated: 2026-04-14*
*Author: ERP Design Team*
*Review Status: Draft*
