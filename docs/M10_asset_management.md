# M10 Asset Management Module Specification

## 1. Module Overview

### 1.1 Purpose

The Asset Management Module (M10) is the comprehensive fixed asset tracking and lifecycle management system within the ABN ERP platform. It handles the complete asset lifecycle from acquisition through allocation, transfer, repair, maintenance, loss, destruction, disposal, and final disposal. The module provides real-time asset visibility, depreciation tracking, inventory management, and reminder/escalation systems.

Key metrics: 165 total assets tracked with status distribution: 60 in use, 30 not used, 24 in maintenance, 10 in repair, 10 damaged, 5 broken, 5 lost, 5 under insurance, 5 scheduled for destruction, 5 for disposal, 3 pending transfer. Reminder system shows: Today 5 reminders, Yesterday 3 reminders, This Week 2 reminders, This Month 4 reminders, Older 2 reminders. The module covers asset overview, asset registry, allocation and recovery, transfer management, repair and maintenance, loss and destruction and disposal, inventory counting, reports, and settings.

### 1.2 Personas

| Persona | Role | Key Responsibilities | Primary Screens | Access Level |
|---------|------|---------------------|-----------------|--------------|
| Asset Manager | Department leadership | Asset policy, approval, reporting | Dashboard, Reports, Settings | Full |
| Asset Officer | Asset administration | Create/update assets, track lifecycle, manage inventory | Asset Registry, Inventory | Read/Write |
| Maintenance Technician | Asset maintenance | Perform maintenance, record repairs | Repair, Maintenance | Read/Write |
| Department User | Asset user | Use assigned assets, report issues | My Assets, Issue Report | Read |
| Finance Officer | Depreciation tracking | Asset depreciation, valuation, disposal accounting | Depreciation, Reports | Read/Write |
| Inventory Counter | Asset counting | Conduct physical asset counts | Asset Inventory | Read/Write |
| Insurance Officer | Insurance management | Track insurance coverage, file claims | Insurance | Read/Write |
| Director | Strategic oversight | Asset investment analysis, utilization | Reports, Dashboard | Read-only |

### 1.3 Dependencies

| Dependency | Type | Direction | Description |
|-----------|------|-----------|-------------|
| M04 Accounting | Downstream | Outbound | Depreciation entries, asset acquisition, disposal |
| M07 Purchasing | Upstream | Inbound | Asset purchase orders -> Asset creation |
| M09 Inventory/Warehouse | Peer | Inbound | Asset storage location |
| M05 Employee Mgmt | Peer | Inbound | Asset assignment to employees |
| M06 Task Mgmt | Peer | Inbound | Maintenance tasks, repair requests |
| M11 Contract Mgmt | Peer | Inbound | Asset insurance contracts |
| Barcode/QR System | External | Inbound | Asset identification tags |
| Insurance Provider | External | Outbound | Insurance claims processing |

### 1.4 Business Rules

1. **Asset Registration**: Every asset must be registered with unique ID, description, category, acquisition cost, acquisition date, and assigned location.
2. **Asset ID Format**: Asset IDs follow format AS-{year}-{sequential} (e.g., AS-2026-0001).
3. **Depreciation Method**: Straight-line depreciation by default; declining balance available for specific categories.
4. **Asset Status Tracking**: Assets tracked through statuses: New, In Use, Not Used, Maintenance, Repair, Damaged, Broken, Lost, Insurance, Destroyed, Disposal, Transfer.
5. **Reminder System**: Automated reminders for maintenance schedules, warranty expirations, insurance renewals, and inventory counts.
6. **Transfer Approval**: Asset transfers between departments or locations require approval from both source and destination managers.
7. **Maintenance Schedule**: Preventive maintenance schedules defined per asset category; overdue maintenance triggers alerts.
8. **Disposal Approval**: Asset disposal requires manager approval and documentation of disposal method (sale, scrap, donation).
9. **Insurance Coverage**: Assets can be covered by insurance policies; coverage amounts tracked separately from book value.
10. **Physical Inventory**: Annual full asset count required; cycle counts quarterly for high-value assets.
11. **Loss Reporting**: Lost assets must be reported with investigation; write-off requires director approval.
12. **Depreciation Posting**: Monthly depreciation automatically posted to expense accounts in M04 Accounting.

---

## 2. Screen Inventory

### 2.1 Screen Routes and Purposes

| Route | Purpose | Personas | Priority |
|-------|---------|----------|----------|
| `/assets/dashboard` | Asset dashboard with status summary and reminders | All personas | Critical |
| `/assets/overview` | Asset overview with key metrics | Asset Manager, Director | High |
| `/assets/registry` | Complete asset registry with search and filters | Asset Officer | Critical |
| `/assets/registry/{id}` | Asset detail with full lifecycle history | Asset Officer, Department User | High |
| `/assets/allocation-recovery` | Asset allocation to users/departments and recovery | Asset Officer | High |
| `/assets/transfers` | Asset transfer management | Asset Officer, Asset Manager | High |
| `/assets/repair-maintenance` | Repair and maintenance tracking | Maintenance Technician | High |
| `/assets/loss-destroy-disposal` | Loss reporting, destruction, disposal | Asset Officer, Asset Manager | High |
| `/assets/inventory` | Asset inventory counting | Inventory Counter | High |
| `/assets/reports` | Asset reports: depreciation, utilization, valuation | Asset Manager, Finance, Director | High |
| `/assets/settings` | Asset settings: categories, depreciation methods, reminder rules | Asset Manager | Medium |

### 2.2 UI Components by Screen

#### /assets/dashboard

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Total Assets KPI | Metric Card | Total 165 assets | Asset count |
| In Use KPI | Metric Card | 60 assets in use | Asset WHERE status='in_use' |
| Not Used KPI | Metric Card | 30 assets not used | Asset WHERE status='not_used' |
| Maintenance KPI | Metric Card | 24 in maintenance | Asset WHERE status='maintenance' |
| Repair KPI | Metric Card | 10 in repair | Asset WHERE status='repair' |
| Damaged KPI | Metric Card | 10 damaged | Asset WHERE status='damaged' |
| Lost KPI | Metric Card | 5 lost | Asset WHERE status='lost' |
| Disposal KPI | Metric Card | 5 disposal + 5 destroyed | Asset WHERE status IN (disposal, destroyed) |
| Status Breakdown Chart | Pie Chart | Asset distribution by status | Asset status aggregation |
| Reminder Panel | List | Today 5, Yesterday 3, Week 2, Month 4, Older 2 | Reminder system |
| Asset Value Chart | Bar Chart | Total asset value by category | Asset valuation |
| Quick Actions Bar | Action Bar | New asset, new transfer, new maintenance | N/A |

#### /assets/registry

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Search Bar | Input | Search by asset ID, name, serial number | Asset search |
| Status Filter | Multi-select | Filter by asset status | Asset status |
| Category Filter | Multi-select | Filter by asset category | Asset category |
| Department Filter | Dropdown | Filter by assigned department | Department |
| Asset Grid | Data Table | Asset list with key columns | Asset registry |
| Status Badge | Badge | Asset status indicator | Asset.status |
| Quick View | Hover Panel | Quick asset details on hover | Asset data |
| Bulk Actions | Dropdown | Mass update, export, transfer selected | Asset selection |
| Export Button | Action Button | Export to Excel | Asset data |

#### /assets/registry/{id} (Asset Detail)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Asset Header | Info Panel | Asset ID, name, category, status | Asset master |
| Basic Info Form | Form Section | Description, serial number, model, manufacturer | Asset master |
| Acquisition Info | Form Section | Acquisition date, cost, supplier, PO reference | Asset master |
| Location Info | Form Section | Current location, department, assigned user | AssetAssignment |
| Depreciation Widget | Summary | Acquisition cost, accumulated depreciation, net book value | AssetDepreciation |
| Lifecycle Timeline | Timeline | Complete asset history: acquisition, transfers, maintenance, repairs | AssetMovement |
| Maintenance History | Data Table | Maintenance records with dates and costs | AssetMaintenance |
| Documents Tab | File List | Asset documents, warranties, insurance | AssetDocument |
| Photos Tab | Image Grid | Asset photos | AssetImage |

### 2.3 User Interactions

1. **Asset Registration**: Asset Officer creates new asset -> enters details (name, category, serial number, acquisition cost, date) -> assigns to location and department -> system generates asset ID (AS-2026-0001) -> starts depreciation schedule.
2. **Asset Transfer**: Asset Officer creates transfer -> selects asset, source, destination -> source manager approves -> destination manager approves -> asset location updated -> transfer recorded.
3. **Maintenance Scheduling**: System generates maintenance reminder based on schedule -> maintenance technician creates maintenance record -> performs maintenance -> records completion date and cost -> reminder cleared.
4. **Asset Inventory Count**: Inventory Counter starts count session -> scans asset barcodes -> records found/not found -> system calculates variances -> Asset Officer investigates discrepancies -> adjustments approved.

### 2.4 Data Displayed

- Asset Registry: ID (AS-2026-0001), Name, Category, Serial Number, Status, Location, Department, Assigned User, Acquisition Date, Cost, Net Book Value
- Status Summary: Total 165, In Use 60, Not Used 30, Maintenance 24, Repair 10, Damaged 10, Broken 5, Lost 5, Insurance 5, Destroyed 5, Disposal 5, Transfer 3
- Reminders: Today 5, Yesterday 3, This Week 2, This Month 4, Older 2
- Depreciation: Original Cost, Accumulated Depreciation, Net Book Value, Monthly Depreciation, Remaining Life

---

## 3. Feature Specifications

### 3.1 Asset Registry and Lifecycle Tracking

**Description**: Register and track assets through their complete lifecycle with detailed information, status management, and history.

**User Stories**:
- As an Asset Officer, I want to register new assets with complete details so that all assets are tracked from acquisition.
- As an Asset Manager, I want to see asset lifecycle history so that I can understand asset utilization patterns.
- As a Department User, I want to see assets assigned to me so that I know my responsibilities.

**Acceptance Criteria**:
1. Asset registration with: name, category, serial number, model, manufacturer, acquisition cost, acquisition date, supplier, warranty info.
2. Auto-generated asset ID in format AS-{year}-{sequential}.
3. Asset status tracking through: New, In Use, Not Used, Maintenance, Repair, Damaged, Broken, Lost, Insurance, Destroyed, Disposal, Transfer.
4. Complete lifecycle history with timestamps for all status changes.
5. Asset photos and documents attachment.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-001 | Asset Name | Required, 3-200 chars | "Asset name is required (3-200 characters)" |
| VR-002 | Category | Required | "Asset category is required" |
| VR-003 | Acquisition Cost | Must be > 0 | "Acquisition cost must be positive" |
| VR-004 | Acquisition Date | Required, cannot be future | "Acquisition date is required" |
| VR-005 | Serial Number | Unique | "Serial number already registered" |
| VR-006 | Status | Valid status value | "Invalid asset status" |
| VR-007 | Location | Required for in-use assets | "Location required for active assets" |

**Edge Cases**:
- Asset split: Parent asset split into multiple child assets (rare, for composite assets).
- Asset merge: Multiple assets merged into one (rare).
- Asset revaluation: Revalue asset above original cost with proper accounting treatment.

### 3.2 Depreciation Management

**Description**: Calculate and track asset depreciation with multiple methods, monthly posting to GL, and depreciation reports.

**User Stories**:
- As a Finance Officer, I want the system to calculate depreciation automatically so that financial records are accurate.
- As a Finance Officer, I want monthly depreciation posted to GL so that expenses are recorded timely.
- As an Asset Manager, I want to see depreciation schedules so that I can plan asset replacements.

**Acceptance Criteria**:
1. Depreciation methods: Straight-line, Declining balance, Units of production.
2. Useful life defined per asset category.
3. Monthly depreciation calculation and accumulation.
4. Depreciation posting to GL with correct expense accounts.
5. Depreciation report showing original cost, accumulated depreciation, net book value.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-010 | Useful Life | Must be > 0 | "Useful life must be positive" |
| VR-011 | Salvage Value | Must be >= 0, < cost | "Salvage value must be less than cost" |
| VR-012 | Method | Valid depreciation method | "Valid depreciation method is required" |
| VR-013 | Start Date | Must be after acquisition | "Depreciation start must be after acquisition" |
| VR-014 | Monthly Post | Cannot post for closed period | "Fiscal period is closed" |

**Edge Cases**:
- Impairment: Asset value impaired below book value.
- Depreciation catch-up: Asset registered mid-year, catch-up depreciation for prior months.
- Fully depreciated: Asset continues in use after full depreciation.

### 3.3 Asset Allocation and Recovery

**Description**: Allocate assets to users, departments, or locations and recover them when no longer needed.

**User Stories**:
- As an Asset Officer, I want to allocate assets to employees so that responsibility is tracked.
- As a Department Manager, I want to see assets allocated to my department so that I know my resources.
- As an Asset Officer, I want to recover assets from users so that assets can be reassigned.

**Acceptance Criteria**:
1. Allocation with: asset, assignee (user/department), allocation date, expected return date, notes.
2. Recovery with: asset, recovery date, condition assessment, notes.
3. Allocation history preserved for audit.
4. Multiple assets can be allocated in batch.
5. Overdue allocation alerts for assets not returned on expected date.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-020 | Asset | Required, active, available | "Asset is not available for allocation" |
| VR-021 | Assignee | Required, active user/department | "Valid assignee is required" |
| VR-022 | Allocation Date | Required, cannot be future | "Allocation date is required" |
| VR-023 | Recovery | Asset must be allocated | "Asset is not currently allocated" |
| VR-024 | Condition | Required on recovery | "Condition assessment required" |

**Edge Cases**:
- Lost during allocation: Asset lost while allocated, report and write-off process.
- Damaged during allocation: Damage assessment and repair/replacement decision.
- Transfer during allocation: Asset transferred to new department while allocated.

### 3.4 Transfer Management

**Description**: Transfer assets between departments, locations, or warehouses with approval workflow.

**User Stories**:
- As an Asset Officer, I want to create transfer requests so that assets can be moved between locations.
- As a Department Manager, I want to approve incoming transfers so that I know what assets are arriving.
- As an Asset Manager, I want to track transfer history so that I can audit asset movements.

**Acceptance Criteria**:
1. Transfer creation with: assets, source, destination, transfer date, reason.
2. Two-step approval: source approves release, destination accepts receipt.
3. Transfer-in-transit tracking.
4. Transfer completion updates asset location and department.
5. Transfer history preserved.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-030 | Assets | At least one asset | "Select at least one asset" |
| VR-031 | Source != Destination | Must differ | "Source and destination must differ" |
| VR-032 | Asset Status | Must be transferable | "Asset status does not allow transfer" |
| VR-033 | Both Approvals | Required for completion | "Both parties must approve" |

**Edge Cases**:
- Partial transfer: Some assets transferred, others cancelled.
- Transfer rejection: Destination rejects transfer, assets returned to source.
- Transfer with allocation: Allocated asset transferred with current assignee.

### 3.5 Repair and Maintenance

**Description**: Track asset maintenance (preventive) and repair (corrective) with scheduling, cost tracking, and completion records.

**User Stories**:
- As a Maintenance Technician, I want to schedule preventive maintenance so that assets remain in good condition.
- As a Department User, I want to report asset issues so that repairs can be scheduled.
- As an Asset Manager, I want to track maintenance costs so that I can assess total cost of ownership.

**Acceptance Criteria**:
1. Maintenance types: Preventive (scheduled), Corrective (repair), Emergency.
2. Maintenance scheduling with frequency per asset category.
3. Issue reporting with description, priority, and photos.
4. Maintenance/repair record with: asset, type, description, cost, start/end dates, technician.
5. Maintenance history per asset.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-040 | Asset | Required, active | "Valid asset is required" |
| VR-041 | Type | Required, valid type | "Valid maintenance type is required" |
| VR-042 | Cost | Must be >= 0 | "Cost cannot be negative" |
| VR-043 | End Date | Must be after start date | "End date must be after start date" |
| VR-044 | Status | Valid status | "Valid status is required" |

**Edge Cases**:
- Repair not economical: Repair cost exceeds asset value, recommend disposal.
- Warranty repair: Track warranty claim and recovery.
- Maintenance overdue: Auto-generate maintenance task when schedule missed.

### 3.6 Loss, Destruction, and Disposal

**Description**: Handle asset loss reporting, destruction scheduling, and disposal processing with proper approvals.

**User Stories**:
- As an Asset Officer, I want to report lost assets so that they are removed from active inventory.
- As an Asset Manager, I want to approve asset disposal so that disposals are authorized.
- As a Finance Officer, I want disposal records so that asset write-offs are properly accounted.

**Acceptance Criteria**:
1. Loss reporting with: asset, discovery date, circumstances, investigation notes.
2. Destruction scheduling with: asset, destruction date, method, authorization.
3. Disposal processing with: asset, disposal method (sale, scrap, donation), proceeds (if any).
4. Approval workflow for all loss/disposal actions.
5. Financial impact calculation and GL posting.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-050 | Asset | Required, not already disposed | "Asset cannot be disposed twice" |
| VR-051 | Reason | Required | "Reason is required" |
| VR-052 | Approval | Required for all disposals | "Approval required for disposal" |
| VR-053 | Investigation | Required for lost assets | "Investigation required for lost assets" |
| VR-054 | Proceeds | Must be >= 0 | "Proceeds cannot be negative" |

**Edge Cases**:
- Insurance claim: Lost/damaged asset covered by insurance, track claim.
- Partial disposal: Only component parts disposed.
- Recovery after loss: Lost asset found, reverse write-off.

### 3.7 Asset Inventory Counting

**Description**: Conduct physical asset counts, record variances, and adjust asset records.

**User Stories**:
- As an Inventory Counter, I want to scan asset barcodes during counts so that counting is fast and accurate.
- As an Asset Officer, I want to see count variances so that I can investigate discrepancies.
- As an Asset Manager, I want to approve count adjustments so that record changes are authorized.

**Acceptance Criteria**:
1. Count session creation with: scope (all assets, category, department, location), date range.
2. Barcode/QR code scanning for asset identification.
3. Found/not found recording with condition assessment.
4. Variance calculation and flagging.
5. Approval workflow: counter records -> officer reviews -> manager approves -> records adjusted.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-060 | Scope | Required | "Count scope is required" |
| VR-061 | Asset Found | Boolean | "Mark asset as found or not found" |
| VR-062 | Condition | Required for found assets | "Condition assessment required" |
| VR-063 | Approval | Required before adjustment | "Count must be approved before adjustment" |

**Edge Cases**:
- Unregistered asset found: Create new asset record with "found" flag.
- Asset at wrong location: Update location without variance.
- Counter discrepancy: Re-count specific assets.

### 3.8 Reminder System

**Description**: Automated reminder system for maintenance schedules, warranty expirations, insurance renewals, and inventory counts.

**User Stories**:
- As an Asset Officer, I want reminders for upcoming maintenance so that I can schedule it in time.
- As an Asset Officer, I want reminders for warranty expirations so that I can plan repairs before warranty ends.
- As an Asset Manager, I want a reminder dashboard so that I can see all upcoming actions.

**Acceptance Criteria**:
1. Reminder types: Maintenance due, Warranty expiring, Insurance expiring, Inventory count due, Transfer pending approval, Disposal pending approval.
2. Reminder timing: Configurable advance notice (7 days, 14 days, 30 days).
3. Reminder dashboard with categorized counts: Today, Yesterday, This Week, This Month, Older.
4. Reminder acknowledgment and resolution tracking.
5. Escalation for unacknowledged reminders.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-070 | Reminder Type | Valid type | "Valid reminder type is required" |
| VR-071 | Due Date | Required | "Due date is required" |
| VR-072 | Advance Notice | Must be > 0 | "Advance notice must be positive" |

**Edge Cases**:
- Recurring reminders: Maintenance reminders repeat after each completion.
- Suppressed reminders: Temporarily suppress reminders during planned downtime.
- Bulk acknowledgment: Acknowledge multiple reminders at once.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+-------------------+       +-------------------+       +-------------------+
|   AssetCategory   |       |     Asset         |       |   AssetStatus     |
+-------------------+       +-------------------+       +-------------------+
| PK category_id    |<----->| PK asset_id       |       | PK status_id      |
| category_name     |       | asset_code        |       | status_name       |
| useful_life_years |       | FK category_id    |       | status_code       |
| depreciation_method|      | asset_name        |       | is_active         |
| is_active         |       | serial_number     |       | is_terminal       |
+-------------------+       | FK status_id      |       +-------------------+
                            | description       |
                            | model             |       +-------------------+
                            | manufacturer      |       |  AssetMovement    |
                            | acquisition_cost  |       +-------------------+
                            | acquisition_date  |       | PK movement_id    |
                            | FK supplier_id    |       | FK asset_id       |
                            | warranty_end      |       | movement_type     |
                            | salvage_value     |       | movement_date     |
                            +-------------------+       | from_location     |
                                    |                   | to_location       |
                    +-------------------------------+   | FK from_dept      |
                    |     AssetDepreciation       |   | FK to_dept        |
                    +-------------------------------+   | FK from_user      |
                    | PK depreciation_id          |   | FK to_user        |
                    | FK asset_id                 |   | notes             |
                    | year                        |   +-------------------+
                    | month                       |
                    | depreciation_amount         |
                    | accumulated_depreciation    |
                    | net_book_value              |
                    +-------------------------------+

+-------------------+       +-------------------+       +-------------------+
| AssetAllocation   |       |  AssetTransfer    |       | AssetRepair       |
+-------------------+       +-------------------+       +-------------------+
| PK allocation_id  |       | PK transfer_id    |       | PK repair_id      |
| FK asset_id       |       | transfer_number   |       | FK asset_id       |
| FK assignee_id    |       | transfer_date     |       | repair_type       |
| FK department_id  |       | FK source_dept    |       | description       |
| allocation_date   |       | FK dest_dept      |       | start_date        |
| expected_return   |       | status            |       | end_date          |
| return_date       |       | FK approved_by    |       | cost              |
| condition         |       +-------------------+       | FK technician     |
| status            |                                   | status            |
+-------------------+                                   +-------------------+

+---------------------+       +---------------------+       +---------------------+
| AssetMaintenance    |       |   AssetDisposal     |       |   AssetInventory    |
+---------------------+       +---------------------+       +---------------------+
| PK maintenance_id   |       | PK disposal_id      |       | PK inventory_id     |
| FK asset_id         |       | FK asset_id         |       | inventory_number    |
| maintenance_type    |       | disposal_date       |       | count_date          |
| scheduled_date      |       | disposal_method     |       | FK scope_type       |
| completed_date      |       | proceeds            |       | FK scope_value      |
| cost                |       | FK approved_by      |       | status              |
| FK technician       |       | notes               |       | FK started_by       |
| status              |       +---------------------+       | FK approved_by      |
+---------------------+                                     +---------------------+

+-------------------+       +-------------------+       +-------------------+
|   AssetReminder   |       | AssetDepreciation |       | AssetImage        |
+-------------------+       +-------------------+       +-------------------+
| PK reminder_id    |       | (see above)       |       | PK image_id       |
| FK asset_id       |                               | FK asset_id       |
| reminder_type     |       +-------------------+       | image_url         |
| due_date          |       | AssetDocument     |       | is_primary        |
| advance_days      |       +-------------------+       | display_order     |
| status            |       | PK document_id    |       +-------------------+
| acknowledged_by   |       | FK asset_id       |
| acknowledged_at   |       | document_type     |
| resolved_at       |       | file_url          |
+-------------------+       | uploaded_at       |
                            +-------------------+
```

### 4.2 Table Schemas

#### Asset

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| asset_id | BIGINT | PK, SERIAL | Unique asset ID |
| asset_code | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated (AS-2026-0001) |
| asset_name | NVARCHAR(200) | NOT NULL | Asset name |
| FK category_id | BIGINT | NOT NULL, FK REFERENCES AssetCategory | Asset category |
| FK status_id | BIGINT | NOT NULL, FK REFERENCES AssetStatus | Current status |
| serial_number | VARCHAR(50) | NULL, UNIQUE | Serial number |
| model | NVARCHAR(100) | NULL | Model |
| manufacturer | NVARCHAR(100) | NULL | Manufacturer |
| description | TEXT | NULL | Asset description |
| acquisition_cost | DECIMAL(18,2) | NOT NULL | Original acquisition cost |
| acquisition_date | DATE | NOT NULL | Acquisition date |
| FK supplier_id | BIGINT | NULL, FK REFERENCES Supplier | Acquisition supplier |
| warranty_end | DATE | NULL | Warranty expiration |
| salvage_value | DECIMAL(18,2) | DEFAULT 0 | Estimated salvage value |
| FK current_location_id | BIGINT | FK REFERENCES Location | Current location |
| FK department_id | BIGINT | FK REFERENCES Department | Current department |
| FK assigned_user_id | BIGINT | NULL, FK REFERENCES User | Assigned user |
| useful_life_months | INT | NOT NULL | Useful life in months |
| depreciation_method | VARCHAR(20) | NOT NULL, DEFAULT 'straight_line' | straight_line, declining_balance |
| monthly_depreciation | DECIMAL(18,2) | CALCULATED | Monthly depreciation amount |
| accumulated_depreciation | DECIMAL(18,2) | DEFAULT 0 | Total accumulated depreciation |
| net_book_value | DECIMAL(18,2) | CALCULATED | acquisition_cost - accumulated_depreciation |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### AssetCategory

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| category_id | BIGINT | PK, SERIAL | Unique category ID |
| category_name | NVARCHAR(100) | NOT NULL | Category name |
| useful_life_years | INT | NOT NULL | Default useful life |
| depreciation_method | VARCHAR(20) | NOT NULL, DEFAULT 'straight_line' | Default depreciation method |
| maintenance_frequency_days | INT | DEFAULT 0 | Preventive maintenance interval |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

#### AssetStatus

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| status_id | BIGINT | PK, SERIAL | Unique status ID |
| status_name | NVARCHAR(50) | NOT NULL | Status name |
| status_code | VARCHAR(20) | NOT NULL, UNIQUE | Status code |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| is_terminal | BOOLEAN | NOT NULL, DEFAULT FALSE | Terminal status (destroyed, disposed) |
| display_order | INT | NOT NULL | Display order |
| color | VARCHAR(7) | NOT NULL | Display color |

#### AssetDepreciation

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| depreciation_id | BIGINT | PK, SERIAL | Unique depreciation ID |
| FK asset_id | BIGINT | NOT NULL, FK REFERENCES Asset | Asset |
| year | INT | NOT NULL | Depreciation year |
| month | INT | NOT NULL | Depreciation month |
| depreciation_amount | DECIMAL(18,2) | NOT NULL | Monthly depreciation |
| accumulated_depreciation | DECIMAL(18,2) | NOT NULL | Running accumulated depreciation |
| net_book_value | DECIMAL(18,2) | NOT NULL | Net book value after depreciation |
| is_posted | BOOLEAN | NOT NULL, DEFAULT FALSE | Posted to GL flag |
| posted_at | TIMESTAMP | NULL | GL posting timestamp |

#### AssetAllocation

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| allocation_id | BIGINT | PK, SERIAL | Unique allocation ID |
| FK asset_id | BIGINT | NOT NULL, FK REFERENCES Asset | Asset |
| FK assignee_id | BIGINT | NOT NULL, FK REFERENCES User | Assigned user |
| FK department_id | BIGINT | NOT NULL, FK REFERENCES Department | Department |
| allocation_date | DATE | NOT NULL | Allocation start date |
| expected_return_date | DATE | NULL | Expected return date |
| return_date | DATE | NULL | Actual return date |
| condition_at_allocation | NVARCHAR(200) | NULL | Condition when allocated |
| condition_at_return | NVARCHAR(200) | NULL | Condition when returned |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active, returned, overdue |
| notes | TEXT | NULL | Allocation notes |

#### AssetTransfer

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| transfer_id | BIGINT | PK, SERIAL | Unique transfer ID |
| transfer_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated number |
| transfer_date | DATE | NOT NULL | Transfer date |
| FK source_department_id | BIGINT | NOT NULL, FK REFERENCES Department | Source department |
| FK dest_department_id | BIGINT | NOT NULL, FK REFERENCES Department | Destination department |
| FK source_location_id | BIGINT | FK REFERENCES Location | Source location |
| FK dest_location_id | BIGINT | FK REFERENCES Location | Destination location |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, source_approved, in_transit, received, completed, cancelled |
| FK approved_by | BIGINT | NULL, FK REFERENCES User | Final approver |
| approved_date | TIMESTAMP | NULL | Approval timestamp |
| notes | TEXT | NULL | Transfer notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### AssetRepair

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| repair_id | BIGINT | PK, SERIAL | Unique repair ID |
| FK asset_id | BIGINT | NOT NULL, FK REFERENCES Asset | Asset |
| repair_type | VARCHAR(20) | NOT NULL | corrective, emergency, warranty |
| description | TEXT | NOT NULL | Repair description |
| reported_date | DATE | NOT NULL | Date issue reported |
| start_date | DATE | NULL | Repair start date |
| end_date | DATE | NULL | Repair completion date |
| cost | DECIMAL(18,2) | DEFAULT 0 | Repair cost |
| FK technician_id | BIGINT | NULL, FK REFERENCES User | Technician |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'reported' | reported, in_progress, completed, cancelled |
| notes | TEXT | NULL | Repair notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### AssetMaintenance

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| maintenance_id | BIGINT | PK, SERIAL | Unique maintenance ID |
| FK asset_id | BIGINT | NOT NULL, FK REFERENCES Asset | Asset |
| maintenance_type | VARCHAR(20) | NOT NULL | preventive, corrective, inspection |
| scheduled_date | DATE | NOT NULL | Scheduled date |
| completed_date | DATE | NULL | Actual completion date |
| cost | DECIMAL(18,2) | DEFAULT 0 | Maintenance cost |
| FK technician_id | BIGINT | NULL, FK REFERENCES User | Technician |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'scheduled' | scheduled, in_progress, completed, skipped |
| checklist | JSONB | NULL | Maintenance checklist |
| notes | TEXT | NULL | Maintenance notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### AssetDisposal

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| disposal_id | BIGINT | PK, SERIAL | Unique disposal ID |
| FK asset_id | BIGINT | NOT NULL, FK REFERENCES Asset | Asset |
| disposal_date | DATE | NOT NULL | Disposal date |
| disposal_method | VARCHAR(20) | NOT NULL | sale, scrap, donation, destruction |
| proceeds | DECIMAL(18,2) | DEFAULT 0 | Sale proceeds (if any) |
| FK approved_by | BIGINT | FK REFERENCES User | Approver |
| approved_date | TIMESTAMP | NOT NULL | Approval timestamp |
| reason | TEXT | NOT NULL | Disposal reason |
| notes | TEXT | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### AssetInventory

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| inventory_id | BIGINT | PK, SERIAL | Unique inventory ID |
| inventory_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated number |
| count_date | DATE | NOT NULL | Count date |
| scope_type | VARCHAR(20) | NOT NULL | all, category, department, location |
| scope_value | JSONB | NULL | Scope filter values |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'in_progress' | in_progress, completed, approved, adjusted |
| FK started_by | BIGINT | FK REFERENCES User | Count initiator |
| FK approved_by | BIGINT | NULL, FK REFERENCES User | Approver |
| approved_date | TIMESTAMP | NULL | Approval date |
| total_assets | INT | DEFAULT 0 | Total assets in scope |
| counted_assets | INT | DEFAULT 0 | Assets counted |
| found_assets | INT | DEFAULT 0 | Assets found |
| not_found_assets | INT | DEFAULT 0 | Assets not found |
| notes | TEXT | NULL | Count notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### AssetReminder

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| reminder_id | BIGINT | PK, SERIAL | Unique reminder ID |
| FK asset_id | BIGINT | NOT NULL, FK REFERENCES Asset | Related asset |
| reminder_type | VARCHAR(30) | NOT NULL | maintenance, warranty, insurance, inventory, approval |
| due_date | DATE | NOT NULL | Reminder due date |
| advance_days | INT | NOT NULL | Days before due to remind |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, acknowledged, resolved, expired |
| FK acknowledged_by | BIGINT | NULL, FK REFERENCES User | Acknowledging user |
| acknowledged_at | TIMESTAMP | NULL | Acknowledgment timestamp |
| resolved_at | TIMESTAMP | NULL | Resolution timestamp |
| resolved_by | BIGINT | NULL, FK REFERENCES User | Resolving user |
| escalation_level | INT | DEFAULT 0 | Escalation level |
| notes | TEXT | NULL | Reminder notes |

#### AssetMovement

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| movement_id | BIGINT | PK, SERIAL | Unique movement ID |
| FK asset_id | BIGINT | NOT NULL, FK REFERENCES Asset | Asset |
| movement_type | VARCHAR(30) | NOT NULL | allocation, recovery, transfer, repair, maintenance, disposal, status_change |
| movement_date | TIMESTAMP | NOT NULL, DEFAULT NOW() | Movement timestamp |
| FK from_department_id | BIGINT | NULL, FK REFERENCES Department | Previous department |
| FK to_department_id | BIGINT | NULL, FK REFERENCES Department | New department |
| FK from_location_id | BIGINT | NULL, FK REFERENCES Location | Previous location |
| FK to_location_id | BIGINT | NULL, FK REFERENCES Location | New location |
| FK from_user_id | BIGINT | NULL, FK REFERENCES User | Previous assignee |
| FK to_user_id | BIGINT | NULL, FK REFERENCES User | New assignee |
| from_status | VARCHAR(20) | NULL | Previous status |
| to_status | VARCHAR(20) | NULL | New status |
| notes | TEXT | NULL | Movement notes |
| FK created_by | BIGINT | FK REFERENCES User | User who recorded |

### 4.3 Indexes

```sql
-- Asset indexes
CREATE INDEX idx_asset_code ON Asset(asset_code);
CREATE INDEX idx_asset_category ON Asset(category_id);
CREATE INDEX idx_asset_status ON Asset(status_id);
CREATE INDEX idx_asset_department ON Asset(department_id);
CREATE INDEX idx_asset_assigned ON Asset(assigned_user_id);
CREATE INDEX idx_asset_location ON Asset(current_location_id);
CREATE INDEX idx_asset_acquisition ON Asset(acquisition_date);

-- AssetDepreciation indexes
CREATE UNIQUE INDEX idx_depreciation_asset_month ON AssetDepreciation(asset_id, year, month);
CREATE INDEX idx_depreciation_asset ON AssetDepreciation(asset_id);
CREATE INDEX idx_depreciation_posted ON AssetDepreciation(is_posted) WHERE is_posted = FALSE;

-- AssetAllocation indexes
CREATE INDEX idx_allocation_asset ON AssetAllocation(asset_id);
CREATE INDEX idx_allocation_assignee ON AssetAllocation(assignee_id);
CREATE INDEX idx_allocation_department ON AssetAllocation(department_id);
CREATE INDEX idx_allocation_status ON AssetAllocation(status);
CREATE INDEX idx_allocation_active ON AssetAllocation(asset_id) WHERE status = 'active';

-- AssetTransfer indexes
CREATE INDEX idx_transfer_number ON AssetTransfer(transfer_number);
CREATE INDEX idx_transfer_source ON AssetTransfer(source_department_id);
CREATE INDEX idx_transfer_dest ON AssetTransfer(dest_department_id);
CREATE INDEX idx_transfer_status ON AssetTransfer(status);

-- AssetRepair indexes
CREATE INDEX idx_repair_asset ON AssetRepair(asset_id);
CREATE INDEX idx_repair_status ON AssetRepair(status);
CREATE INDEX idx_repair_dates ON AssetRepair(start_date, end_date);

-- AssetMaintenance indexes
CREATE INDEX idx_maintenance_asset ON AssetMaintenance(asset_id);
CREATE INDEX idx_maintenance_scheduled ON AssetMaintenance(scheduled_date);
CREATE INDEX idx_maintenance_status ON AssetMaintenance(status);
CREATE INDEX idx_maintenance_overdue ON AssetMaintenance(scheduled_date, status) WHERE status = 'scheduled' AND scheduled_date < CURRENT_DATE;

-- AssetDisposal indexes
CREATE INDEX idx_disposal_asset ON AssetDisposal(asset_id);
CREATE INDEX idx_disposal_date ON AssetDisposal(disposal_date);
CREATE INDEX idx_disposal_method ON AssetDisposal(disposal_method);

-- AssetInventory indexes
CREATE INDEX idx_inventory_number ON AssetInventory(inventory_number);
CREATE INDEX idx_inventory_date ON AssetInventory(count_date);
CREATE INDEX idx_inventory_status ON AssetInventory(status);

-- AssetReminder indexes
CREATE INDEX idx_reminder_asset ON AssetReminder(asset_id);
CREATE INDEX idx_reminder_type ON AssetReminder(reminder_type);
CREATE INDEX idx_reminder_due ON AssetReminder(due_date);
CREATE INDEX idx_reminder_status ON AssetReminder(status) WHERE status = 'pending';
CREATE INDEX idx_reminder_escalation ON AssetReminder(escalation_level) WHERE escalation_level > 0;

-- AssetMovement indexes
CREATE INDEX idx_movement_asset ON AssetMovement(asset_id);
CREATE INDEX idx_movement_type ON AssetMovement(movement_type);
CREATE INDEX idx_movement_date ON AssetMovement(movement_date);
```

### 4.4 Summary of Tables

| Table | Primary Key | Foreign Keys | Purpose | Estimated Rows |
|-------|-------------|--------------|---------|----------------|
| Asset | asset_id | Category, Status, Location, Department, User | Asset registry | 500 |
| AssetCategory | category_id | - | Asset category definitions | 20 |
| AssetStatus | status_id | - | Asset status definitions | 12 |
| AssetDepreciation | depreciation_id | Asset | Monthly depreciation records | 6,000/year |
| AssetAllocation | allocation_id | Asset, User, Department | Asset allocations | 1,000 |
| AssetTransfer | transfer_id | Department, Location, User | Asset transfers | 200/year |
| AssetRepair | repair_id | Asset, User | Asset repairs | 500/year |
| AssetMaintenance | maintenance_id | Asset, User | Asset maintenance | 2,000/year |
| AssetDisposal | disposal_id | Asset, User | Asset disposals | 50/year |
| AssetInventory | inventory_id | User | Asset inventory counts | 4/year |
| AssetReminder | reminder_id | Asset, User | Automated reminders | 5,000 |
| AssetMovement | movement_id | Asset, Department, Location, User | Complete movement history | 10,000/year |
| AssetImage | image_id | Asset | Asset photos | 1,000 |
| AssetDocument | document_id | Asset | Asset documents | 2,000 |

---

## 5. API Specifications

### 5.1 Asset API

#### POST /api/v1/assets

Register a new asset.

**Request Body**:
```json
{
  "asset_name": "May lanh Daikin 2HP",
  "category_id": 3,
  "serial_number": "DLK-2026-0045",
  "model": "FTKC35UVV",
  "manufacturer": "Daikin",
  "description": "May lanh 2HP, 1 chieu",
  "acquisition_cost": 15000000,
  "acquisition_date": "2026-01-15",
  "supplier_id": 205,
  "warranty_end": "2028-01-15",
  "salvage_value": 1500000,
  "current_location_id": 10,
  "department_id": 2,
  "assigned_user_id": 1001,
  "useful_life_months": 60
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "asset_id": 166,
    "asset_code": "AS-2026-0166",
    "asset_name": "May lanh Daikin 2HP",
    "category_id": 3,
    "category_name": "Thiet bi dien lanh",
    "status_id": 1,
    "status_name": "New",
    "serial_number": "DLK-2026-0045",
    "model": "FTKC35UVV",
    "manufacturer": "Daikin",
    "acquisition_cost": 15000000,
    "acquisition_date": "2026-01-15",
    "warranty_end": "2028-01-15",
    "salvage_value": 1500000,
    "department_id": 2,
    "department_name": "Phong Ke toan",
    "assigned_user_id": 1001,
    "assigned_user_name": "Luc Phong Vu",
    "useful_life_months": 60,
    "depreciation_method": "straight_line",
    "monthly_depreciation": 225000,
    "accumulated_depreciation": 675000,
    "net_book_value": 14325000,
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

#### GET /api/v1/assets

List assets with filtering.

**Query Parameters**:
- `status_id` (int): Filter by status
- `category_id` (int): Filter by category
- `department_id` (int): Filter by department
- `search` (string): Search by name, code, serial
- `page` (int): Page number
- `per_page` (int): Items per page

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "assets": [
      {
        "asset_id": 166,
        "asset_code": "AS-2026-0166",
        "asset_name": "May lanh Daikin 2HP",
        "category_name": "Thiet bi dien lanh",
        "status_name": "In Use",
        "status_color": "#10B981",
        "serial_number": "DLK-2026-0045",
        "department_name": "Phong Ke toan",
        "assigned_user_name": "Luc Phong Vu",
        "acquisition_cost": 15000000,
        "net_book_value": 14325000,
        "acquisition_date": "2026-01-15"
      }
    ],
    "status_summary": {
      "total": 165,
      "in_use": 60,
      "not_used": 30,
      "maintenance": 24,
      "repair": 10,
      "damaged": 10,
      "broken": 5,
      "lost": 5,
      "insurance": 5,
      "destroyed": 5,
      "disposal": 5,
      "transfer": 3
    },
    "pagination": {
      "page": 1,
      "per_page": 20,
      "total_pages": 9,
      "total_count": 165
    }
  }
}
```

### 5.2 Depreciation API

#### GET /api/v1/assets/{asset_id}/depreciation

Get depreciation schedule for an asset.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "asset_id": 166,
    "asset_code": "AS-2026-0166",
    "asset_name": "May lanh Daikin 2HP",
    "acquisition_cost": 15000000,
    "salvage_value": 1500000,
    "useful_life_months": 60,
    "depreciation_method": "straight_line",
    "monthly_depreciation": 225000,
    "accumulated_depreciation": 675000,
    "net_book_value": 14325000,
    "depreciation_schedule": [
      {"year": 2026, "month": 1, "amount": 225000, "accumulated": 225000, "nbv": 14775000, "is_posted": true},
      {"year": 2026, "month": 2, "amount": 225000, "accumulated": 450000, "nbv": 14550000, "is_posted": true},
      {"year": 2026, "month": 3, "amount": 225000, "accumulated": 675000, "nbv": 14325000, "is_posted": true},
      {"year": 2026, "month": 4, "amount": 225000, "accumulated": 900000, "nbv": 14100000, "is_posted": false}
    ]
  }
}
```

### 5.3 Transfer API

#### POST /api/v1/assets/transfers

Create an asset transfer.

**Request Body**:
```json
{
  "asset_ids": [166, 167],
  "source_department_id": 2,
  "dest_department_id": 5,
  "source_location_id": 10,
  "dest_location_id": 25,
  "transfer_date": "2026-04-20",
  "notes": "Transfer AC units to new office"
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "transfer_id": 201,
    "transfer_number": "AT-2026-0201",
    "transfer_date": "2026-04-20",
    "source_department_id": 2,
    "source_department_name": "Phong Ke toan",
    "dest_department_id": 5,
    "dest_department_name": "Phong Nhan su",
    "status": "pending",
    "assets": [
      {"asset_id": 166, "asset_code": "AS-2026-0166", "asset_name": "May lanh Daikin 2HP"},
      {"asset_id": 167, "asset_code": "AS-2026-0167", "asset_name": "May lanh Daikin 1.5HP"}
    ],
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

### 5.4 Dashboard API

#### GET /api/v1/assets/dashboard

Get asset dashboard summary.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "status_summary": {
      "total": 165,
      "in_use": 60,
      "not_used": 30,
      "maintenance": 24,
      "repair": 10,
      "damaged": 10,
      "broken": 5,
      "lost": 5,
      "insurance": 5,
      "destroyed": 5,
      "disposal": 5,
      "transfer": 3
    },
    "reminders": {
      "today": 5,
      "yesterday": 3,
      "this_week": 2,
      "this_month": 4,
      "older": 2,
      "total_pending": 16
    },
    "total_value": {
      "acquisition_cost": 2500000000,
      "accumulated_depreciation": 800000000,
      "net_book_value": 1700000000
    },
    "pending_transfers": 3,
    "pending_repairs": 10,
    "overdue_maintenance": 8
  }
}
```

### 5.5 Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Asset validation failed",
    "details": [
      {
        "field": "serial_number",
        "message": "Serial number DLK-2026-0045 already registered for asset AS-2026-0089"
      }
    ],
    "request_id": "req-ast123"
  }
}
```

### 5.6 API Summary Table

| Endpoint | Method | Auth Required | Rate Limit | Description |
|----------|--------|---------------|------------|-------------|
| /api/v1/assets | POST | Yes | 50/min | Register asset |
| /api/v1/assets | GET | Yes | 200/min | List assets |
| /api/v1/assets/{id} | GET | Yes | 200/min | Get asset detail |
| /api/v1/assets/{id} | PUT | Yes | 50/min | Update asset |
| /api/v1/assets/{id}/depreciation | GET | Yes | 100/min | Depreciation schedule |
| /api/v1/assets/{id}/movements | GET | Yes | 200/min | Asset movement history |
| /api/v1/assets/allocations | POST | Yes | 50/min | Allocate asset |
| /api/v1/assets/allocations/{id}/recover | POST | Yes | 30/min | Recover asset |
| /api/v1/assets/transfers | POST | Yes | 30/min | Create transfer |
| /api/v1/assets/transfers/{id}/approve | POST | Yes | 20/min | Approve transfer |
| /api/v1/assets/repairs | POST | Yes | 50/min | Record repair |
| /api/v1/assets/maintenance | POST | Yes | 50/min | Schedule maintenance |
| /api/v1/assets/disposals | POST | Yes | 20/min | Dispose asset |
| /api/v1/assets/inventory | POST | Yes | 20/min | Create inventory count |
| /api/v1/assets/reminders | GET | Yes | 100/min | List reminders |
| /api/v1/assets/dashboard | GET | Yes | 100/min | Dashboard summary |
| /api/v1/assets/reports/valuation | GET | Yes | 30/min | Asset valuation report |
| /api/v1/assets/categories | GET | Yes | 200/min | List categories |

---

## 6. Permissions Matrix

### 6.1 Role Definitions

| Role | Code | Description | Department |
|------|------|-------------|------------|
| Asset Manager | AST_MGR | Full asset management | Asset Management |
| Asset Officer | AST_OFFICER | Asset administration | Asset Management |
| Maintenance Tech | MAINT_TECH | Repair and maintenance | Maintenance |
| Department User | DEPT_USER | Asset user | Various |
| Finance Officer | FIN_OFFICER | Depreciation tracking | Finance |
| Inventory Counter | INV_COUNTER | Asset counting | Asset Management |
| Director | DIRECTOR | Strategic oversight | Executive |

### 6.2 CRUD Permissions

| Resource | AST_MGR | AST_OFFICER | MAINT_TECH | DEPT_USER | FIN_OFFICER | INV_COUNTER | DIRECTOR |
|----------|---------|-------------|------------|-----------|-------------|-------------|----------|
| Asset | CRUD | CRUD | R | R (assigned) | R | R | R |
| AssetAllocation | CRUD | CRUD | R | R (own) | R | R | R |
| AssetTransfer | CRUD + Approve | CRUD | R | R | R | R | R |
| AssetRepair | R | R | CRUD | Create | R | R | R |
| AssetMaintenance | CRUD | CRUD | CRUD | R | R | R | R |
| AssetDisposal | CRUD + Approve | CRUD | R | R | R | R | R |
| AssetInventory | CRUD + Approve | CRUD | R | R | R | CRUD | R |
| AssetDepreciation | R | R | - | - | CRUD | - | R |
| AssetReminder | CRUD | CRUD | R (own) | R (own) | R | R | R |

### 6.3 Row-Level Security

| Resource | Rule | Description |
|----------|------|-------------|
| Asset | Department users see only assigned assets | `assigned_user_id = user.id OR department_id = user.department_id OR user.role IN ('AST_MGR', 'AST_OFFICER', 'DIRECTOR')` |
| AssetAllocation | Users see only own allocations | `assignee_id = user.id OR user.role IN ('AST_MGR', 'AST_OFFICER')` |
| AssetReminder | Users see own reminders | `acknowledged_by = user.id OR user.role IN ('AST_MGR', 'AST_OFFICER')` |

---

## 7. State Machine

### 7.1 Asset Status Diagram

```
                    +------+
                    | NEW  |
                    +--+---+
                       |
                 (Allocate)
                       v
                  +---------+
           +----->| IN USE  |<----+
           |      +----+----+     |
           |           |          |
     +-----+---+ +-----+----+ +---+-----+
     | NOT USED| |MAINTENANCE| | REPAIR  |
     +-----+---+ +-----+----+ +---+-----+
           |           |          |
           +-----+-----+-----+----+
                 |
           +-----+-----+
           |  DAMAGED  |
           +-----+-----+
                 |
           +-----+-----+
           |  BROKEN   |
           +-----+-----+
                 |
      +----------+----------+
      |          |          |
      v          v          v
   +------+  +--------+  +---------+
   | LOST |  |INSURANCE| |DISPOSAL |
   +------+  +--------+  +----+----+
                                |
                           +----+----+
                           |DESTROYED|
                           +---------+
```

### 7.2 State Transition Tables

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| NEW | IN USE | Allocate to user | Asset Officer | Asset available |
| IN USE | NOT USED | Recover from user | Asset Officer | Asset returned |
| IN USE | MAINTENANCE | Schedule maintenance | Maintenance Tech | Maintenance due |
| IN USE | REPAIR | Report issue | Department User | Issue reported |
| MAINTENANCE | IN USE | Maintenance complete | Maintenance Tech | Maintenance done |
| REPAIR | IN USE | Repair complete | Maintenance Tech | Repair successful |
| REPAIR | DAMAGED | Repair not viable | Asset Manager | Assessment |
| DAMAGED | BROKEN | Beyond repair | Asset Manager | Assessment |
| DAMAGED | REPAIR | Repair scheduled | Asset Manager | Repair approved |
| BROKEN | DISPOSAL | Decision to dispose | Asset Manager | Disposal approved |
| IN USE | LOST | Report lost | Asset Officer | Investigation started |
| LOST | DISPOSAL | Write-off approved | Director | Investigation complete |
| LOST | IN USE | Asset found | Asset Officer | Asset recovered |
| ANY | INSURANCE | Insurance claim filed | Insurance Officer | Claim submitted |
| DISPOSAL | DESTROYED | Disposal complete | Asset Officer | Disposal executed |

### 7.3 Approval Workflow

#### Asset Disposal Approval

```
Asset Officer initiates disposal
     |
     v
Assess asset condition and NBV
     /         \
  NBV<10M     NBV>=10M
     |          |
     v          v
  Asset     Director
  Manager   Approval
  Approval    /    \
     |     Approve  Reject
     v       |       |
  Disposed  Disposed Returned
```

---

## 8. Reports

### 8.1 Report Inventory

| Report | Description | Dimensions | Metrics | Filters | Chart Type |
|--------|-------------|------------|---------|---------|------------|
| Asset Register | Complete asset listing | Category, status, department | Count, total cost, NBV | Category, status | Table |
| Depreciation Schedule | Monthly depreciation by asset | Asset, category, month | Monthly depreciation, accumulated, NBV | Period, category | Line chart |
| Asset Utilization | Asset usage analysis | Status, department, category | Utilization %, idle time | Period, department | Pie chart |
| Maintenance Cost | Maintenance spending | Asset, category, period | Maintenance cost, count, avg cost | Period, category | Bar chart |
| Disposal Report | Disposal analysis | Method, period, category | Disposal count, proceeds, loss/gain | Period, method | Table |
| Asset Valuation | Total asset value | Category, department | Acquisition cost, depreciation, NBV | Date, category | Stacked bar |
| Warranty Tracking | Warranty status | Asset, category, expiry | Under warranty, expiring soon, expired | Date range | Timeline |
| Insurance Coverage | Insurance analysis | Asset, policy | Insured value, coverage ratio, premium | Current | Table |

### 8.2 Report Summary Table

| Report ID | Report Name | Schedule | Auto-Generate | Distribution | Retention |
|-----------|-------------|----------|---------------|--------------|-----------|
| RPT-AST-001 | Asset Register | Monthly | Yes | Asset Manager, Finance | 10 years |
| RPT-AST-002 | Depreciation Schedule | Monthly | Yes | Finance, Director | 10 years |
| RPT-AST-003 | Asset Utilization | Quarterly | Yes | Asset Manager, Director | 5 years |
| RPT-AST-004 | Maintenance Cost | Monthly | Yes | Asset Manager, Maintenance | 5 years |
| RPT-AST-005 | Disposal Report | Quarterly | Yes | Asset Manager, Finance | 10 years |
| RPT-AST-006 | Asset Valuation | Monthly | Yes | Finance, Director | 10 years |
| RPT-AST-007 | Warranty Tracking | Monthly | Yes | Asset Officer, Maintenance | 3 years |
| RPT-AST-008 | Insurance Coverage | Quarterly | Yes | Asset Manager, Insurance Officer | 5 years |

---

## 9. Integration Points

### 9.1 External API Integrations

| Integration | Provider | Protocol | Direction | Frequency | Data Exchanged |
|------------|----------|----------|-----------|-----------|----------------|
| Barcode/QR Scanner | Hardware | USB/Bluetooth | Inbound | Real-time | Asset identification |
| Insurance Provider | Insurance company | HTTPS/API | Bidirectional | As needed | Claims, policy data |
| Equipment Vendor | Vendor systems | HTTPS/API | Inbound | On demand | Warranty info, service records |
| GPS Tracking | GPS devices | API | Inbound | Real-time | Asset location tracking |

### 9.2 Internal Module Integrations

| Source Module | Target | Data Flow | Trigger | Frequency |
|---------------|--------|-----------|---------|-----------|
| M10 Asset Mgmt | M04 Accounting | Depreciation -> GL entries | Monthly posting | Monthly |
| M07 Purchasing | M10 Asset Mgmt | Asset purchase -> New asset | PO received | Real-time |
| M09 Inventory | M10 Asset Mgmt | Asset storage location | Location change | Real-time |
| M05 Employee Mgmt | M10 Asset Mgmt | Employee -> Asset assignee | Allocation | Real-time |
| M04 Accounting | M10 Asset Mgmt | Asset acquisition cost | Purchase payment | Real-time |
| M06 Task Mgmt | M10 Asset Mgmt | Maintenance task creation | Maintenance schedule | Real-time |

### 9.3 Webhook Events

| Event | Trigger | Payload | Subscribers |
|-------|---------|---------|-------------|
| asset.registered | New asset registered | asset_id, asset_code, name, category, cost | Finance, Department |
| asset.status_changed | Asset status changed | asset_id, from_status, to_status, date | Asset Manager, Finance |
| asset.transferred | Asset transferred | asset_id, from_dept, to_dept, transfer_id | Source dept, Dest dept |
| maintenance.scheduled | Maintenance scheduled | asset_id, scheduled_date, type, technician | Technician, Asset Officer |
| maintenance.overdue | Maintenance overdue | asset_id, scheduled_date, days_overdue | Asset Manager, Technician |
| asset.disposed | Asset disposed | asset_id, method, proceeds, nbv | Finance, Asset Manager |
| reminder.triggered | Reminder triggered | reminder_id, asset_id, type, due_date | Assigned user, Manager |
| warranty.expiring | Warranty expiring soon | asset_id, warranty_end, days_remaining | Asset Officer, Maintenance |

---

## 10. Technical Notes

### 10.1 Performance Requirements

| Metric | Target | Measurement | Notes |
|--------|--------|-------------|-------|
| Asset Search | < 500ms | P95 latency | With name/code/serial filters |
| Asset Detail Load | < 800ms | P95 latency | Full detail with history |
| Depreciation Calc | < 2s for monthly run | P95 latency | All active assets |
| Inventory Count Save | < 300ms | P95 latency | Single asset scan |
| Report Generation | < 5s | P95 latency | Standard reports |
| Barcode Scan | < 200ms | P95 latency | Scan to display |
| Concurrent Users | 100+ | Simultaneous | Including mobile counting |

### 10.2 Caching Strategy

| Cache Type | Data | TTL | Invalidation |
|------------|------|-----|--------------|
| Redis | Asset catalog | 15 minutes | On asset update |
| Redis | Status/category lists | 1 hour | On catalog change |
| Redis | Dashboard summary | 5 minutes | On any asset change |
| Redis | Reminder counts | 2 minutes | On reminder change |
| Redis | Depreciation (current month) | 1 day | On posting |
| Application | Asset category tree | 1 hour | On category change |
| Browser | Asset images | 1 day | On image update |

### 10.3 Key Files and Components

| File/Component | Path | Purpose |
|----------------|------|---------|
| Asset Service | `src/modules/assets/services/asset.service.ts` | Asset CRUD, lifecycle |
| Depreciation Service | `src/modules/assets/services/depreciation.service.ts` | Depreciation calculation |
| Allocation Service | `src/modules/assets/services/allocation.service.ts` | Asset allocation/recovery |
| Transfer Service | `src/modules/assets/services/transfer.service.ts` | Asset transfers |
| Maintenance Service | `src/modules/assets/services/maintenance.service.ts` | Maintenance scheduling |
| Repair Service | `src/modules/assets/services/repair.service.ts` | Repair tracking |
| Disposal Service | `src/modules/assets/services/disposal.service.ts` | Disposal processing |
| Inventory Service | `src/modules/assets/services/inventory.service.ts` | Asset inventory counting |
| Reminder Service | `src/modules/assets/services/reminder.service.ts` | Reminder generation |
| Report Generator | `src/modules/assets/services/report-generator.service.ts` | Asset reports |

### 10.4 Localization

| Aspect | Configuration | Notes |
|--------|---------------|-------|
| Language | Vietnamese (primary), English | All labels, messages |
| Currency | VND | All monetary values |
| Date Format | DD/MM/YYYY | Vietnamese standard |
| Number Format | 1.234.567.890 (dot separator) | Vietnamese standard |
| Asset ID Format | AS-{year}-{sequential} | e.g., AS-2026-0001 |
| Status Names | Vietnamese | Moi, Dang su dung, Bao tri, Thanh ly |

### 10.5 Mobile Support

| Feature | Mobile Support | Notes |
|---------|----------------|-------|
| Asset Search | Responsive | Search and view |
| Asset Detail | Responsive | Full detail view |
| Inventory Count | Touch-optimized | Barcode/QR scan |
| Transfer Management | Responsive | Approve transfers |
| Issue Reporting | Responsive | Report asset issues |
| Reminder Viewing | Responsive | View and acknowledge |
| Push Notifications | Push | Maintenance due, reminders |
| Barcode Scanning | Camera integration | Scan asset barcodes |

### 10.6 Security Considerations

| Aspect | Implementation |
|--------|----------------|
| Asset Movement Audit | Complete history of all asset movements |
| Disposal Approval | Director approval for high-value disposals |
| Depreciation Integrity | Monthly depreciation locked after posting |
| Access Control | Department-level row security for users |
| Data Integrity | Transaction-based status changes |
| Barcode Security | Unique barcodes, tamper-evident tags |
| Insurance Data | Encrypted insurance policy details |
| Photo Privacy | Asset photos access-controlled |

### 10.7 Database Considerations

- Asset values stored as DECIMAL(18,2)
- Soft deletes for assets (is_deleted flag, only for data errors)
- Terminal status assets (destroyed, disposed) cannot be modified
- Partition AssetMovement table by year
- Archive disposed assets after 10 years
- Materialized views for dashboard metrics
- Unique constraints on asset_code, serial_number
- Depreciation records immutable after posting

---

*Document Version: 1.0*
*Last Updated: 2026-04-14*
*Author: ERP Design Team*
*Review Status: Draft*
