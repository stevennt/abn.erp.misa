# M03 - Company & Organization Structure (Cau truc Cong ty va To chuc)

**Category:** Foundation  
**Priority:** P0  
**Reference Images:** assets/image_29.png, assets/image_30.png, assets/image_37.png, assets/image_39.png, assets/image_60.png  
**Dependencies:** M01 - User & Authentication, M02 - Roles, Permissions & Access Control  
**Generated:** April 14, 2026

---

## 1. Module Overview

The Company & Organization Structure module defines the hierarchical skeleton of the entire enterprise, modeling everything from single-company setups to complex multi-level group corporations with parent-subsidiary relationships. This module is the authoritative source of truth for "who belongs where" in the organization and is consumed by every other module for data scoping, reporting rollups, and access control boundaries. Image_30 illustrates group-level management with a parent company and its subsidiary companies, showing consolidated data views and inter-company relationships. Image_39 displays the organizational tree visualization used for objective cascading and department hierarchy navigation.

The module supports multiple organizational models: a single company with departments (small businesses), a cooperative structure (image_29) with member-based governance, a corporate group with parent-subsidiary hierarchy (image_30), and a multi-branch enterprise with geographically distributed locations (image_37 shows department breakdown: Hanoi 40%, Da Nang 20%, Ho Chi Minh City 40%, Can Tho 5%). Department structures are visualized as tree charts with parent-child relationships, and each department can be further subdivided into teams, units, or sections. The system administration dashboard (image_60) provides a global view of the organization, showing per-company user counts, department distribution, and structural change history.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| System Administrator | Configures companies, subsidiaries, departments, and locations at the structural level | Full CRUD on all organization objects |
| Group CEO / Director | Views consolidated group-level organization charts and cross-company metrics | Read access to all companies in the group, write on group-level settings |
| Company Director | Manages a single company's departments, locations, and branch structure | Full CRUD within own company, read-only on group-level |
| Department Manager | Views department hierarchy, manages sub-departments and team assignments | CRUD on sub-departments within own department tree, read on sibling departments |
| HR Manager | Assigns employees to departments, manages organizational changes, monitors department metrics | Read/write department assignments, read org structure |
| Employee | Views the organizational chart and department directory | Read-only on org chart, can view department members |
| Auditor | Reviews organizational structure for compliance and reporting | Read-only on all org data, including historical changes |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M01 - User & Authentication | Users are assigned to departments and companies for data scoping |
| M02 - Roles & Permissions | Department scope determines row-level security boundaries |
| M05 - Employee Management | Employee records reference department_id and work_location_id |
| M06 - Task Management | Tasks are assigned to departments and scoped by org hierarchy |
| M16 - Objectives & KPI | Objectives cascade down the org tree from group to department level |
| M39 - Operations Dashboard | Org structure drives dashboard data aggregation and filtering |
| M40 - Group Management | Parent-subsidiary relationships defined here, consumed by group module |

### Key Business Rules

1. Every department must belong to exactly one company; orphan departments (without a company) are not permitted.
2. The department hierarchy is a tree (not a general graph): each department has at most one parent, and cycles are strictly prevented.
3. A subsidiary company is both a child of the parent group company (in the corporate hierarchy) AND a company entity in its own right with its own departments and locations.
4. When a department is deactivated, all active employee assignments to that department are flagged for reassignment within 30 days.
5. The root department of each company (company-level department) cannot be deleted; it serves as the anchor for the department tree.
6. Location assignments determine physical presence; an employee can have only one primary work location but multiple secondary locations.
7. Organizational changes (department creation, renaming, merging, splitting) are logged with before/after state for audit trail reconstruction.
8. Data consolidation across subsidiaries follows the ownership percentage: a 60%-owned subsidiary contributes 60% of its metrics to the parent company's consolidated view.
9. Branch offices are distinct from departments: branches are physical locations that may house multiple departments, while departments are functional units that may span multiple branches.
10. Department codes must be unique within a company but can be reused across different companies in the same group.

---

## 2. Screen Inventory

### 2.1 Screen: Group Management (Parent-Subsidiary Hierarchy)

**Route:** `/admin/group` | **Purpose:** Manage the corporate group structure with parent company and subsidiary relationships, including ownership percentages and consolidated data views.

| UI Component | Description |
|--------------|-------------|
| Page Header | "Quan ly tap doan" (Group Management), "Them cong ty con" (Add Subsidiary) button |
| Group Header Card | Parent company name, logo, total subsidiaries count, consolidated employee count, consolidated revenue |
| Subsidiary Tree | Hierarchical tree view: parent company at root, subsidiaries as child nodes with expansion for each subsidiary's departments |
| Subsidiary Card | For each subsidiary: company name, logo, ownership %, employee count, status (Active/Inactive), industry, registration number |
| Ownership Editor | Modal for editing ownership percentage, acquisition date, voting rights percentage |
| Consolidated Metrics Panel | Rollup of key metrics across all subsidiaries: total employees, total revenue, total departments, total locations |
| Inter-Company Transactions Link | Link to view transactions between group companies (if enabled) |
| Action Menu | Per-subsidiary: Edit, View Detail, Deactivate, View Org Chart, View Departments |

**User Interactions:**
- Click on a subsidiary node to expand and view its department structure
- Edit ownership percentage via inline editor with validation (0-100%)
- Add new subsidiary via modal form with company details
- Drag-and-drop to reorganize subsidiary hierarchy (if group has multi-level subsidiaries)
- Click consolidated metrics to drill down into contributing companies
- Export group structure to PDF

**Data Displayed:**
- Parent company identity and metadata
- Subsidiary list with ownership percentages
- Consolidated employee count and revenue
- Subsidiary status (Active/Inactive/Under Acquisition)
- Industry classification and registration details

### 2.2 Screen: Organizational Tree

**Route:** `/admin/org-chart` | **Purpose:** Visualize the complete organizational hierarchy from group level down to individual departments and teams.

| UI Component | Description |
|--------------|-------------|
| Page Header | "So do to chuc" (Organization Chart), company selector, zoom controls, layout toggle (tree/radial) |
| Org Chart Canvas | Interactive tree visualization: nodes represent companies, departments, and teams; edges represent parent-child relationships |
| Node Detail | On hover/click: name, code, manager name, employee count, location, status badge |
| Filter Panel | Filter by company, status (active/inactive), department category |
| Legend | Color coding: blue = company, green = department, orange = team, gray = inactive |
| Export Options | Export as PNG, PDF, or interactive HTML |
| Search | Search by department name, code, or manager name; matching nodes are highlighted |
| Collapse/Expand | Click nodes to expand or collapse subtrees |

**User Interactions:**
- Zoom in/out and pan across the org chart
- Click a node to view detailed department information
- Drag nodes to reorganize (requires System Admin; triggers approval workflow)
- Search to highlight matching nodes
- Filter to show only specific companies or active departments
- Export chart in multiple formats
- Switch between tree and radial layouts

**Data Displayed:**
- Complete org hierarchy: group > companies > departments > teams
- Manager assignments per node
- Employee counts per node
- Status indicators (active/inactive)
- Location information

### 2.3 Screen: Department Structure

**Route:** `/admin/departments` | **Purpose:** Manage department hierarchy within a selected company, including creation, editing, reorganization, and deactivation.

| UI Component | Description |
|--------------|-------------|
| Page Header | "Cau truc phong ban" (Department Structure), company selector, "Them phong ban" (Add Department) button |
| Department Tree | Hierarchical list with expand/collapse: parent departments with indented children |
| Department Row | Columns: Name, Code, Manager, Employee count, Parent, Status, Actions |
| Add/Edit Modal | Fields: name, code, parent department (dropdown), manager (employee selector), location, description, is_active |
| Department Stats Card | For selected department: total employees, sub-department count, open positions, budget |
| Bulk Actions | Bulk reassign employees, bulk deactivate, bulk change manager |
| Reorganization Mode | Toggle to enable drag-and-drop reordering of departments within the tree |

**User Interactions:**
- Add new department at any level of the hierarchy
- Edit department properties via inline edit or modal
- Reassign department manager via employee search
- Move department to a different parent (drag-and-drop or dropdown)
- Deactivate department (soft delete; employees must be reassigned first)
- View department statistics card
- Bulk reassign employees when merging or closing departments
- Export department list to Excel

**Data Displayed:**
- Department hierarchy with parent-child relationships
- Manager assignments
- Employee counts per department
- Department codes and names
- Status (Active/Inactive)
- Location assignments

### 2.4 Screen: Location & Branch Management

**Route:** `/admin/locations` | **Purpose:** Manage physical locations, branches, and offices where employees work and operations occur.

| UI Component | Description |
|--------------|-------------|
| Page Header | "Dia diem va Chi nhanh" (Locations & Branches), company selector, "Them dia diem" (Add Location) button |
| Location List Table | Columns: Name, Code, Type (Head Office/Branch/Remote/Warehouse), Address, City, Employee count, Departments, Actions |
| Location Detail Panel | Full address, map view, assigned departments list, employee list, photos, contact info |
| Map View | Toggle to geographic map showing all company locations with pins |
| Add/Edit Modal | Fields: name, code, type, address line 1/2/3, city, province, country, postal code, phone, email, manager, capacity |
| Filter Panel | Filter by type, city, company, employee count range |

**User Interactions:**
- Add new location with full address details
- View location on embedded map
- Assign departments to a location
- View employees assigned to each location
- Edit location details
- Filter locations by type and geographic criteria
- Set location capacity and receive alerts when approaching capacity

**Data Displayed:**
- Location name, code, type classification
- Full address (country, province, city, district, street)
- Employee count per location
- Assigned departments
- Geographic coordinates (latitude, longitude)
- Contact information

### 2.5 Screen: Company Directory

**Route:** `/admin/companies` | **Purpose:** View and manage all companies in the system, including their registration details, fiscal settings, and structural metadata.

| UI Component | Description |
|--------------|-------------|
| Page Header | "Danh sach cong ty" (Company Directory), "Them cong ty" (Add Company) button |
| Company List Table | Columns: Name, Code, Type (Parent/Subsidiary/Standalone), Registration No, Tax ID, Status, Employee count, Subsidiary count, Actions |
| Company Detail Modal | Full company profile: legal name, trading name, tax ID, registration details, fiscal year, currency, address, legal representative |
| Company Stats Card | Employee count, department count, location count, active contracts count, monthly revenue |
| Group Affiliation | For subsidiaries: parent company name, ownership %, acquisition date |
| Subsidiary List | For parent companies: list of subsidiaries with ownership percentages |

**User Interactions:**
- Add new company with full legal and fiscal details
- View company profile in detail modal
- Edit company properties
- View subsidiary list and ownership structure
- Filter companies by type and status
- Export company directory to Excel
- View company stats dashboard card

**Data Displayed:**
- Company legal and trading names
- Registration number and tax ID
- Company type classification
- Employee, department, and subsidiary counts
- Fiscal year and currency settings
- Legal representative information

---

## 3. Feature Specifications

### 3.1 Feature: Company CRUD & Registration

**Description:** Full lifecycle management of company entities including legal registration details, fiscal configuration, and structural metadata.

**User Stories:**
- As a System Administrator, I want to register a new company in the system so that it can have its own departments, employees, and operational data.
- As a Group Director, I want to view all companies in the group with their registration details so that I can verify compliance and ownership structure.
- As an Auditor, I want to see the history of company changes so that I can track structural evolution over time.

**Acceptance Criteria:**
- **Given** valid company details including legal name, tax ID, and registration number, **When** a System Admin creates a company, **Then** the company is saved with a unique code, a root department is automatically created, and the change is logged.
- **Given** a tax ID that already exists in the system, **When** a System Admin tries to create a company, **Then** a validation error prevents creation.
- **Given** a company with active employees and departments, **When** a System Admin attempts to delete it, **Then** a confirmation dialog shows the impact and requires explicit acknowledgment; deletion is blocked if active employees remain.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Legal Name | Not null, 2-200 characters | "Ten phap ly cua cong ty là bắt buộc" |
| Unique | Tax ID | Not existing in Companies table | "Mã số thuế đã tồn tại" |
| Unique | Company Code | Not existing in Companies table | "Mã công ty đã tồn tại" |
| Format | Tax ID | 10 or 13 digits, valid checksum | "Mã số thuế không hợp lệ" |
| Required | Fiscal Year Start | Not null, valid month (1-12) | "Tháng bắt đầu năm tài chính là bắt buộc" |
| Valid | Currency | Must be ISO 4217 code | "Mã tiền tệ không hợp lệ" |
| Required | Address | Not null, minimum address line 1 | "Địa chỉ là bắt buộc" |

**Edge Cases:**
- Creating a company without assigning it to a group (system creates it as a standalone company; can be added to a group later).
- Creating a subsidiary before the parent company exists (system requires parent company to exist first; validates FK on creation).
- Company with 500+ employees and 50+ departments being deactivated (system requires all employees to be reassigned or terminated before deactivation; provides bulk reassignment tool).
- Tax ID format change (Vietnam transitioned from 10-digit to 13-digit tax IDs; system accepts both formats with validation logic for each).

### 3.2 Feature: Department Hierarchy Management

**Description:** Create, edit, reorganize, and deactivate departments within a company's organizational tree.

**User Stories:**
- As a System Administrator, I want to create a department hierarchy so that the organizational structure reflects the real company layout.
- As a Department Manager, I want to create sub-departments under my department so that I can organize my team structure.
- As an HR Manager, I want to move a department to a different parent so that I can restructure without losing employee assignments.

**Acceptance Criteria:**
- **Given** a valid parent department and unique department code, **When** a user creates a department, **Then** the department is added to the tree and the org chart is updated.
- **Given** a department with child departments, **When** a user attempts to delete it, **Then** the system prevents deletion and shows the list of child departments that must be moved first.
- **Given** a department reorganization (changing parent), **When** the move is confirmed, **Then** all employees in the department retain their assignments, the org chart updates, and the change is logged.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Department Name | Not null, 2-200 characters | "Tên phòng ban là bắt buộc" |
| Unique | Department Code | Unique within company | "Mã phòng ban đã tồn tại trong công ty" |
| Valid Parent | Parent ID | Must exist in same company or be null (root) | "Phòng ban cha không hợp lệ" |
| No Cycles | Hierarchy | Moving must not create a cycle | "Không thể di chuyển: sẽ tạo chu trình trong cây phòng ban" |
| Required | Company ID | Not null | "Công ty là bắt buộc" |

**Edge Cases:**
- Moving a department with 200 employees and 10 sub-departments to a different parent (system processes asynchronously; shows progress indicator; org chart updates when complete).
- Merging two departments (system provides a merge wizard: select source and target departments, choose which manager to keep, reassign all employees, archive source department).
- Renaming a department that is referenced in workflow approval configurations (system updates references automatically via FK; display name change is cosmetic).
- Department code reuse across companies (allowed; uniqueness is scoped to company, not global).

### 3.3 Feature: Subsidiary Management & Ownership

**Description:** Manage parent-subsidiary relationships with ownership percentages, acquisition dates, and voting rights tracking.

**User Stories:**
- As a Group Director, I want to add a subsidiary with ownership percentage so that the consolidated view reflects the correct ownership stake.
- As a System Administrator, I want to edit ownership percentage when shares are transferred so that the group structure is always accurate.
- As an Auditor, I want to see the history of ownership changes so that I can track how the group structure evolved.

**Acceptance Criteria:**
- **Given** a parent company and a target company, **When** a System Admin creates a subsidiary relationship, **Then** the relationship is saved with ownership percentage, acquisition date, and the target company appears in the parent's subsidiary tree.
- **Given** an ownership percentage change, **When** the change is saved, **Then** consolidated metrics are recalculated and the change is logged in the audit trail.
- **Given** a circular ownership attempt (Company A owns B, B owns C, trying to make C own A), **When** the relationship is created, **Then** the system detects the cycle and prevents the relationship.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Parent Company ID | Not null, must exist | "Công ty mẹ là bắt buộc" |
| Required | Subsidiary Company ID | Not null, must exist | "Công ty con là bắt buộc" |
| Range | Ownership % | 0 < percentage <= 100 | "Tỷ lệ sở hữu phải từ 0 đến 100%" |
| No Self-Reference | Parent != Subsidiary | Must be different companies | "Công ty không thể là công ty con của chính nó" |
| No Cycles | Ownership graph | Must not create circular ownership | "Không thể tạo: sẽ tạo quan hệ sở hữu vòng" |
| Required | Acquisition Date | Not null, valid date | "Ngày mua lại là bắt buộc" |

**Edge Cases:**
- Multi-level subsidiaries (Parent > Sub A > Sub B; system supports unlimited depth in the ownership tree).
- Partial ownership change (e.g., from 60% to 75%; system logs the delta and recalculates consolidated metrics).
- Subsidiary that is also a parent to other subsidiaries (common in multi-level groups; fully supported).
- Joint venture with exactly 50% ownership (system allows; consolidated metrics use 50% weighting).

### 3.4 Feature: Location & Branch Management

**Description:** Manage physical locations where the company operates, including branch offices, warehouses, and remote sites.

**User Stories:**
- As a System Administrator, I want to register a new branch office with full address details so that employees can be assigned to work there.
- As an HR Manager, I want to see which employees work at each location so that I can manage location-based policies.
- As a Department Manager, I want to know which departments are present at each location so that I can coordinate cross-department activities.

**Acceptance Criteria:**
- **Given** valid location details, **When** a user creates a location, **Then** it is saved with a unique code and appears in the location list.
- **Given** a location with assigned employees, **When** a user attempts to delete it, **Then** the system prevents deletion and shows the employee count that must be reassigned first.
- **Given** a location with multiple departments, **When** the location is viewed, **Then** all assigned departments and their employee counts are displayed.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Location Name | Not null, 2-200 characters | "Tên địa điểm là bắt buộc" |
| Unique | Location Code | Unique within company | "Mã địa điểm đã tồn tại" |
| Required | Address Line 1 | Not null | "Địa chỉ dòng 1 là bắt buộc" |
| Required | City | Not null | "Thành phố là bắt buộc" |
| Valid | Location Type | Must be in {head_office, branch, remote, warehouse, retail} | "Loại địa điểm không hợp lệ" |
| Format | Postal Code | Valid format for country | "Mã bưu chính không hợp lệ" |

**Edge Cases:**
- Location spans multiple buildings at the same address (system allows multiple location entries with the same address but different building identifiers).
- Remote location with no fixed address (system allows "Remote" type with a general region instead of specific address).
- Employee assigned to a location that is subsequently deactivated (system flags employee for reassignment; location deactivation blocked until all employees reassigned).

### 3.5 Feature: Organizational Chart Visualization

**Description:** Interactive org chart that visualizes the complete company-department-team hierarchy with search, filter, and export capabilities.

**User Stories:**
- As a Company Director, I want to see the full org chart so that I can understand the company structure at a glance.
- As a new employee, I want to see where my department fits in the organization so that I can understand the reporting structure.
- As an HR Analyst, I want to export the org chart as a PDF so that I can include it in onboarding materials.

**Acceptance Criteria:**
- **Given** a company selection, **When** the org chart loads, **Then** the complete department hierarchy is rendered as an interactive tree within 3 seconds for up to 500 departments.
- **Given** a search query, **When** the user searches, **Then** matching departments are highlighted and the tree auto-expands to show them.
- **Given** a filter by status=active, **When** the filter is applied, **Then** inactive departments are grayed out but still visible in the hierarchy.

**Validation Rules:**

| Rule | Condition | Error Message |
|------|-----------|---------------|
| Max Nodes | <= 1,000 nodes rendered at once | "So do qua lon. Vui long loc theo cong ty hoac phong ban" |
| Render Time | < 5 seconds for 500 departments | N/A (performance requirement) |

**Edge Cases:**
- Org chart with 1,000+ departments (system renders a simplified view with collapsible nodes; full detail loaded on demand).
- Department with no manager assigned (node shows "Unassigned" badge; does not prevent rendering).
- Circular reference in department hierarchy (data integrity constraint prevents this; if somehow present, org chart renders with a warning banner).
- Export to PDF with 500+ departments (system generates a multi-page PDF with page breaks at logical hierarchy levels).

---

## 4. Database Design

### ASCII ERD Diagram

```
+-------------+       1:N        +----------------+       1:N        +----------------+
|   Company   |----------------->|  Department    |----------------->|   Employee     |
+-------------+                  +----------------+                  +----------------+
| id (PK)     |                  | id (PK)        |                  | (in M01/M05)   |
| name        |                  | company_id(FK) |                  | department_id  |
| code        |                  | name           |                  | (FK)           |
| tax_id      |                  | code           |                  +----------------+
| type        |                  | parent_id(FK)  |
| status      |                  | manager_id(FK) |
| fiscal_year |                  | location_id    |
| currency    |                  | is_active      |
| address     |                  +----------------+
+-------------+                         |
      |                                 | 1:N
      | 1:N                             |
      |                                 v
      |                        +----------------+
      |                        |   Location     |
      |                        +----------------+
      |                        | id (PK)        |
      |                        | company_id(FK) |
      +----------------------->| name           |
             1:N               | code           |
                               | type           |
+------------------+          | address        |
|   Subsidiary     |          | city           |
+------------------+          | province       |
| id (PK)          |          | country        |
| parent_id (FK)   |          | lat, lng       |
| subsidiary_id(FK)|          | capacity       |
| ownership_pct    |          | is_active      |
| voting_pct       |          +----------------+
| acquisition_date |
| status           |
+------------------+

+------------------+
|    OrgChart      |
+------------------+
| id (PK)          |
| company_id (FK)  |
| version          |
| snapshot (JSONB) |
| created_at       |
+------------------+

+------------------+
|   OrgChangeLog   |
+------------------+
| id (PK)          |
| entity_type      |
| entity_id        |
| action           |
| before (JSONB)   |
| after (JSONB)    |
| changed_by       |
| changed_at       |
+------------------+
```

### Table Schemas

#### Table: `companies`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique company identifier |
| legal_name | VARCHAR(200) | NOT NULL | Legal registered name |
| trading_name | VARCHAR(200) | NULL | Trading/brand name |
| code | VARCHAR(20) | UNIQUE, NOT NULL, INDEX | Short code for internal reference |
| tax_id | VARCHAR(13) | UNIQUE, NOT NULL, INDEX | Tax identification number |
| registration_number | VARCHAR(50) | NOT NULL | Business registration number |
| registration_date | DATE | NOT NULL | Date of business registration |
| company_type | VARCHAR(20) | NOT NULL, CHECK (company_type IN ('parent', 'subsidiary', 'standalone', 'cooperative')), INDEX | Company classification |
| industry | VARCHAR(100) | NULL | Industry classification |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active', CHECK (status IN ('active', 'inactive', 'dissolved')), INDEX | Company status |
| fiscal_year_start_month | INTEGER | NOT NULL, DEFAULT 1, CHECK (fiscal_year_start_month BETWEEN 1 AND 12) | Month fiscal year begins |
| currency_code | VARCHAR(3) | NOT NULL, DEFAULT 'VND' | ISO 4217 currency code |
| address_line1 | VARCHAR(200) | NOT NULL | Primary address line |
| address_line2 | VARCHAR(200) | NULL | Secondary address line |
| city | VARCHAR(100) | NOT NULL | City/province |
| province | VARCHAR(100) | NOT NULL | Province/state |
| country | VARCHAR(100) | NOT NULL, DEFAULT 'Vietnam' | Country |
| postal_code | VARCHAR(20) | NULL | Postal/ZIP code |
| phone | VARCHAR(20) | NULL | Company phone |
| email | VARCHAR(255) | NULL | Company email |
| website | VARCHAR(255) | NULL | Company website |
| legal_representative | VARCHAR(200) | NULL | Name of legal representative |
| legal_rep_title | VARCHAR(100) | NULL | Title of legal representative |
| employee_count | INTEGER | NOT NULL, DEFAULT 0 | Cached employee count |
| department_count | INTEGER | NOT NULL, DEFAULT 0 | Cached department count |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### Table: `departments`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique department identifier |
| company_id | UUID | FK -> companies.id, NOT NULL, INDEX | Parent company |
| name | VARCHAR(200) | NOT NULL, INDEX | Department name |
| code | VARCHAR(20) | NOT NULL, INDEX | Department code (unique within company) |
| parent_id | UUID | FK -> departments.id, NULL, INDEX | Parent department (null = root) |
| manager_id | UUID | FK -> employees.id, NULL | Department head |
| location_id | UUID | FK -> locations.id, NULL | Primary location |
| category | VARCHAR(50) | NULL | Department category (operations, admin, sales, etc.) |
| description | TEXT | NULL | Department description |
| is_active | BOOLEAN | NOT NULL, DEFAULT true, INDEX | Whether department is active |
| is_root | BOOLEAN | NOT NULL, DEFAULT false | Whether this is the company root department |
| employee_count | INTEGER | NOT NULL, DEFAULT 0 | Cached employee count |
| budget_annual | DECIMAL(18,2) | NULL | Annual budget allocation |
| cost_center_code | VARCHAR(20) | NULL | Accounting cost center code |
| level | INTEGER | NOT NULL, DEFAULT 0 | Depth in hierarchy (0 = root) |
| path | VARCHAR(500) | NOT NULL, DEFAULT '/' | Materialized path for fast tree queries |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |
| UNIQUE(company_id, code) | -- | Composite unique constraint | Department code unique within company |

#### Table: `locations`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique location identifier |
| company_id | UUID | FK -> companies.id, NOT NULL, INDEX | Owning company |
| name | VARCHAR(200) | NOT NULL, INDEX | Location name |
| code | VARCHAR(20) | NOT NULL, INDEX | Location code (unique within company) |
| type | VARCHAR(20) | NOT NULL, CHECK (type IN ('head_office', 'branch', 'remote', 'warehouse', 'retail')), INDEX | Location type classification |
| address_line1 | VARCHAR(200) | NOT NULL | Primary address line |
| address_line2 | VARCHAR(200) | NULL | Secondary address line |
| district | VARCHAR(100) | NULL | District/county |
| city | VARCHAR(100) | NOT NULL, INDEX | City/province |
| province | VARCHAR(100) | NOT NULL | Province/state |
| country | VARCHAR(100) | NOT NULL, DEFAULT 'Vietnam' | Country |
| postal_code | VARCHAR(20) | NULL | Postal/ZIP code |
| latitude | DECIMAL(10,8) | NULL | Geographic latitude |
| longitude | DECIMAL(11,8) | NULL | Geographic longitude |
| phone | VARCHAR(20) | NULL | Location phone number |
| email | VARCHAR(255) | NULL | Location email |
| manager_id | UUID | FK -> employees.id, NULL | Location manager |
| capacity | INTEGER | NULL | Maximum employee capacity |
| employee_count | INTEGER | NOT NULL, DEFAULT 0 | Cached employee count |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Whether location is active |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |
| UNIQUE(company_id, code) | -- | Composite unique constraint | Location code unique within company |

#### Table: `subsidiaries`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique subsidiary relationship identifier |
| parent_company_id | UUID | FK -> companies.id, NOT NULL, INDEX | Parent (holding) company |
| subsidiary_company_id | UUID | FK -> companies.id, NOT NULL, INDEX | Subsidiary company |
| ownership_percentage | DECIMAL(5,2) | NOT NULL, CHECK (ownership_percentage > 0 AND ownership_percentage <= 100) | Ownership stake percentage |
| voting_percentage | DECIMAL(5,2) | NOT NULL, DEFAULT ownership_percentage, CHECK (voting_percentage > 0 AND voting_percentage <= 100) | Voting rights percentage |
| acquisition_date | DATE | NOT NULL | Date of acquisition/investment |
| acquisition_cost | DECIMAL(18,2) | NULL | Cost of acquisition |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active', CHECK (status IN ('active', 'pending', 'divested', 'liquidated')) | Relationship status |
| divestment_date | DATE | NULL | Date of divestment (if applicable) |
| notes | TEXT | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |
| UNIQUE(parent_company_id, subsidiary_company_id) | -- | Composite unique | Prevents duplicate relationships |

#### Table: `org_chart_snapshots`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique snapshot identifier |
| company_id | UUID | FK -> companies.id, NOT NULL, INDEX | Company this snapshot represents |
| version | INTEGER | NOT NULL | Incrementing version number |
| snapshot_data | JSONB | NOT NULL | Full org hierarchy as JSON |
| created_by | UUID | FK -> users.id, NOT NULL | User who triggered the snapshot |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Snapshot creation timestamp |

#### Table: `org_change_logs`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique log entry identifier |
| entity_type | VARCHAR(20) | NOT NULL, CHECK (entity_type IN ('company', 'department', 'location', 'subsidiary')) | Type of entity changed |
| entity_id | UUID | NOT NULL, INDEX | ID of the changed entity |
| action | VARCHAR(20) | NOT NULL, CHECK (action IN ('create', 'update', 'delete', 'move', 'merge', 'rename')) | Change action |
| before_data | JSONB | NULL | Entity state before change |
| after_data | JSONB | NULL | Entity state after change |
| changed_by | UUID | FK -> users.id, NOT NULL | User who made the change |
| changed_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Change timestamp |
| reason | TEXT | NULL | Reason for the change |

### Indexes (SQL)

```sql
-- Companies
CREATE INDEX idx_companies_type ON companies(company_type);
CREATE INDEX idx_companies_status ON companies(status);
CREATE INDEX idx_companies_tax_id ON companies(tax_id);
CREATE INDEX idx_companies_city ON companies(city);

-- Departments
CREATE INDEX idx_departments_company ON departments(company_id);
CREATE INDEX idx_departments_parent ON departments(parent_id);
CREATE INDEX idx_departments_manager ON departments(manager_id);
CREATE INDEX idx_departments_location ON departments(location_id);
CREATE INDEX idx_departments_path ON departments(path);
CREATE INDEX idx_departments_company_code ON departments(company_id, code);
CREATE INDEX idx_departments_active ON departments(company_id, is_active) WHERE is_active = true;

-- Locations
CREATE INDEX idx_locations_company ON locations(company_id);
CREATE INDEX idx_locations_type ON locations(type);
CREATE INDEX idx_locations_city ON locations(city);
CREATE INDEX idx_locations_company_code ON locations(company_id, code);

-- Subsidiaries
CREATE INDEX idx_subsidiaries_parent ON subsidiaries(parent_company_id);
CREATE INDEX idx_subsidiaries_subsidiary ON subsidiaries(subsidiary_company_id);
CREATE INDEX idx_subsidiaries_status ON subsidiaries(status);

-- Org Change Logs
CREATE INDEX idx_org_logs_entity ON org_change_logs(entity_type, entity_id);
CREATE INDEX idx_org_logs_changed_at ON org_change_logs(changed_at DESC);
CREATE INDEX idx_org_logs_changed_by ON org_change_logs(changed_by);
CREATE INDEX idx_org_logs_action ON org_change_logs(action);
```

### All Tables Summary

| Table Name | Purpose | Row Count Estimate | Key Relationships |
|------------|---------|-------------------|-------------------|
| `companies` | Company/entity registration and legal details | 5-50 | 1:N with departments, locations, subsidiaries |
| `departments` | Department hierarchy within companies | 50-500 | FK to companies, self-referencing (parent_id), FK to locations |
| `locations` | Physical offices, branches, and sites | 10-200 | FK to companies |
| `subsidiaries` | Parent-subsidiary ownership relationships | 5-100 | FK to companies (both parent and subsidiary) |
| `org_chart_snapshots` | Historical org chart snapshots | 500-5,000 | FK to companies |
| `org_change_logs` | Audit trail for all structural changes | 10,000-100,000 | Polymorphic reference to companies, departments, locations, subsidiaries |

---

## 5. API Specifications

### Base URL
```
/api/v1/companies
/api/v1/departments
/api/v1/locations
/api/v1/subsidiaries
/api/v1/org-chart
```

### Company Endpoints

#### GET /api/v1/companies
List all companies with optional filtering.

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| per_page | integer | No | Items per page (default: 25, max: 100) |
| company_type | string | No | Filter by type (parent, subsidiary, standalone) |
| status | string | No | Filter by status |
| search | string | No | Search name, code, tax ID |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "c10e8400-e29b-41d4-a716-446655440001",
        "legal_name": "Cong ty co phan MISA",
        "trading_name": "MISA",
        "code": "MISA",
        "tax_id": "01234567890",
        "company_type": "parent",
        "status": "active",
        "industry": "Software",
        "city": "Ha Noi",
        "employee_count": 1253,
        "department_count": 25,
        "subsidiary_count": 5,
        "created_at": "2020-01-15T08:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 25,
      "total_items": 8,
      "total_pages": 1
    }
  }
}
```

#### POST /api/v1/companies
Create a new company.

**Request Body:**
```json
{
  "legal_name": "Cong ty TNHH Cong nghe ABC",
  "trading_name": "ABC Tech",
  "code": "ABC",
  "tax_id": "01098765432",
  "registration_number": "01098765432",
  "registration_date": "2024-01-15",
  "company_type": "subsidiary",
  "industry": "Technology",
  "fiscal_year_start_month": 1,
  "currency_code": "VND",
  "address_line1": "123 Nguyen Trai",
  "city": "Ha Noi",
  "province": "Ha Noi",
  "country": "Vietnam",
  "phone": "+84 24 1234 5678",
  "email": "info@abctech.vn",
  "legal_representative": "Nguyen Van A",
  "legal_rep_title": "Giam doc"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "c20e8400-e29b-41d4-a716-446655440002",
    "legal_name": "Cong ty TNHH Cong nghe ABC",
    "code": "ABC",
    "root_department_id": "d10e8400-e29b-41d4-a716-446655440001",
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

#### GET /api/v1/companies/:id
Get a single company's full details.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "c10e8400-e29b-41d4-a716-446655440001",
    "legal_name": "Cong ty co phan MISA",
    "trading_name": "MISA",
    "code": "MISA",
    "tax_id": "01234567890",
    "registration_number": "01234567890",
    "registration_date": "2020-01-15",
    "company_type": "parent",
    "status": "active",
    "industry": "Software",
    "fiscal_year_start_month": 1,
    "currency_code": "VND",
    "address_line1": "123 Nguyen Trai",
    "city": "Ha Noi",
    "province": "Ha Noi",
    "country": "Vietnam",
    "phone": "+84 24 1234 5678",
    "email": "info@misa.vn",
    "website": "https://misa.vn",
    "legal_representative": "Nguyen Van A",
    "legal_rep_title": "Giam doc",
    "employee_count": 1253,
    "department_count": 25,
    "subsidiaries": [
      {
        "company_id": "c20e8400-e29b-41d4-a716-446655440002",
        "company_name": "ABC Tech",
        "ownership_percentage": 75.00,
        "voting_percentage": 75.00,
        "acquisition_date": "2024-01-15",
        "status": "active"
      }
    ],
    "created_at": "2020-01-15T08:00:00Z",
    "updated_at": "2026-03-20T14:30:00Z"
  }
}
```

#### PUT /api/v1/companies/:id
Update a company's details.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "c10e8400-e29b-41d4-a716-446655440001",
    "updated_at": "2026-04-14T10:00:00Z"
  }
}
```

#### DELETE /api/v1/companies/:id
Deactivate a company (soft delete; blocks if active employees exist).

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Cong ty da duoc vo hieu hoa"
}
```

### Department Endpoints

#### GET /api/v1/departments
List departments with optional filtering.

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| per_page | integer | No | Items per page (default: 50, max: 200) |
| company_id | UUID | No | Filter by company |
| parent_id | UUID | No | Filter by parent (null for root) |
| is_active | boolean | No | Filter by active status |
| search | string | No | Search name, code |
| include_tree | boolean | No | If true, returns full tree structure |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "d10e8400-e29b-41d4-a716-446655440001",
        "company_id": "c10e8400-e29b-41d4-a716-446655440001",
        "company_name": "MISA",
        "name": "Phong Cong nghe",
        "code": "IT",
        "parent_id": null,
        "parent_name": null,
        "manager_id": "e001",
        "manager_name": "Nguyen Van A",
        "location_id": "l001",
        "location_name": "Truong so Ha Noi",
        "category": "operations",
        "is_active": true,
        "is_root": true,
        "employee_count": 150,
        "level": 0,
        "children_count": 5,
        "created_at": "2020-01-15T08:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 50,
      "total_items": 25,
      "total_pages": 1
    }
  }
}
```

#### POST /api/v1/departments
Create a new department.

**Request Body:**
```json
{
  "company_id": "c10e8400-e29b-41d4-a716-446655440001",
  "name": "Phong Phat trien Phan mem",
  "code": "DEV",
  "parent_id": "d10e8400-e29b-41d4-a716-446655440001",
  "manager_id": "e002",
  "location_id": "l001",
  "category": "operations",
  "description": "Responsible for software development"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "d20e8400-e29b-41d4-a716-446655440002",
    "name": "Phong Phat trien Phan mem",
    "code": "DEV",
    "level": 1,
    "path": "/d10e8400-e29b-41d4-a716-446655440001/",
    "created_at": "2026-04-14T11:00:00Z"
  }
}
```

#### GET /api/v1/departments/:id
Get a single department with full details.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "d10e8400-e29b-41d4-a716-446655440001",
    "company_id": "c10e8400-e29b-41d4-a716-446655440001",
    "company_name": "MISA",
    "name": "Phong Cong nghe",
    "code": "IT",
    "parent_id": null,
    "manager_id": "e001",
    "manager_name": "Nguyen Van A",
    "location_id": "l001",
    "location_name": "Truong so Ha Noi",
    "category": "operations",
    "description": "Technology and IT department",
    "is_active": true,
    "is_root": true,
    "employee_count": 150,
    "budget_annual": 5000000000,
    "cost_center_code": "CC-IT-001",
    "level": 0,
    "path": "/",
    "children": [
      {
        "id": "d20e8400-e29b-41d4-a716-446655440002",
        "name": "Phong Phat trien Phan mem",
        "code": "DEV",
        "employee_count": 80
      }
    ],
    "created_at": "2020-01-15T08:00:00Z"
  }
}
```

#### PUT /api/v1/departments/:id
Update a department.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "d10e8400-e29b-41d4-a716-446655440001",
    "updated_at": "2026-04-14T12:00:00Z"
  }
}
```

#### PUT /api/v1/departments/:id/move
Move a department to a different parent.

**Request Body:**
```json
{
  "new_parent_id": "d30e8400-e29b-41d4-a716-446655440003",
  "reason": "Restructuring IT department"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "d20e8400-e29b-41d4-a716-446655440002",
    "old_parent_id": "d10e8400-e29b-41d4-a716-446655440001",
    "new_parent_id": "d30e8400-e29b-41d4-a716-446655440003",
    "new_level": 2,
    "new_path": "/d10e8400.../d30e8400.../"
  }
}
```

#### DELETE /api/v1/departments/:id
Deactivate a department.

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Phong ban da duoc vo hieu hoa"
}
```

### Location Endpoints

#### GET /api/v1/locations
List locations with optional filtering.

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| per_page | integer | No | Items per page (default: 25, max: 100) |
| company_id | UUID | No | Filter by company |
| type | string | No | Filter by type |
| city | string | No | Filter by city |
| search | string | No | Search name, code, address |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "l10e8400-e29b-41d4-a716-446655440001",
        "company_id": "c10e8400-e29b-41d4-a716-446655440001",
        "name": "Truong so Ha Noi",
        "code": "HN-HQ",
        "type": "head_office",
        "address_line1": "123 Nguyen Trai",
        "city": "Ha Noi",
        "province": "Ha Noi",
        "country": "Vietnam",
        "employee_count": 500,
        "department_count": 10,
        "is_active": true,
        "created_at": "2020-01-15T08:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 25,
      "total_items": 15,
      "total_pages": 1
    }
  }
}
```

#### POST /api/v1/locations
Create a new location.

**Request Body:**
```json
{
  "company_id": "c10e8400-e29b-41d4-a716-446655440001",
  "name": "Van phong Da Nang",
  "code": "DN-VP",
  "type": "branch",
  "address_line1": "456 Tran Phu",
  "city": "Da Nang",
  "province": "Da Nang",
  "country": "Vietnam",
  "phone": "+84 23 6789 0123",
  "capacity": 200
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "l20e8400-e29b-41d4-a716-446655440002",
    "name": "Van phong Da Nang",
    "code": "DN-VP",
    "created_at": "2026-04-14T13:00:00Z"
  }
}
```

#### PUT /api/v1/locations/:id
Update a location.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "l10e8400-e29b-41d4-a716-446655440001",
    "updated_at": "2026-04-14T14:00:00Z"
  }
}
```

#### DELETE /api/v1/locations/:id
Deactivate a location.

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Dia diem da duoc vo hieu hoa"
}
```

### Subsidiary Endpoints

#### GET /api/v1/subsidiaries
List subsidiary relationships.

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| parent_company_id | UUID | No | Filter by parent company |
| status | string | No | Filter by status |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "s10e8400-e29b-41d4-a716-446655440001",
        "parent_company_id": "c10e8400-e29b-41d4-a716-446655440001",
        "parent_company_name": "MISA",
        "subsidiary_company_id": "c20e8400-e29b-41d4-a716-446655440002",
        "subsidiary_company_name": "ABC Tech",
        "ownership_percentage": 75.00,
        "voting_percentage": 75.00,
        "acquisition_date": "2024-01-15",
        "status": "active",
        "created_at": "2024-01-15T08:00:00Z"
      }
    ]
  }
}
```

#### POST /api/v1/subsidiaries
Create a subsidiary relationship.

**Request Body:**
```json
{
  "parent_company_id": "c10e8400-e29b-41d4-a716-446655440001",
  "subsidiary_company_id": "c20e8400-e29b-41d4-a716-446655440002",
  "ownership_percentage": 75.00,
  "voting_percentage": 75.00,
  "acquisition_date": "2024-01-15",
  "acquisition_cost": 5000000000
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "s10e8400-e29b-41d4-a716-446655440001",
    "ownership_percentage": 75.00,
    "created_at": "2026-04-14T15:00:00Z"
  }
}
```

#### PUT /api/v1/subsidiaries/:id
Update a subsidiary relationship (e.g., change ownership percentage).

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "s10e8400-e29b-41d4-a716-446655440001",
    "updated_at": "2026-04-14T16:00:00Z"
  }
}
```

#### DELETE /api/v1/subsidiaries/:id
Remove a subsidiary relationship (divestment).

**Request Body:**
```json
{
  "divestment_date": "2026-04-14",
  "reason": "Sold ownership stake"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Da tach cong ty con"
}
```

### Org Chart Endpoints

#### GET /api/v1/org-chart/:companyId
Get the organizational chart for a company.

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| format | string | No | "tree" (default) or "flat" |
| include_inactive | boolean | No | Include inactive departments (default: false) |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "company_id": "c10e8400-e29b-41d4-a716-446655440001",
    "company_name": "MISA",
    "total_departments": 25,
    "total_employees": 1253,
    "tree": {
      "id": "d10e8400-e29b-41d4-a716-446655440001",
      "name": "MISA",
      "code": "ROOT",
      "is_root": true,
      "manager_name": null,
      "employee_count": 1253,
      "children": [
        {
          "id": "d20e8400-e29b-41d4-a716-446655440002",
          "name": "Phong Cong nghe",
          "code": "IT",
          "manager_name": "Nguyen Van A",
          "employee_count": 150,
          "location_name": "Truong so Ha Noi",
          "children": [
            {
              "id": "d30e8400-e29b-41d4-a716-446655440003",
              "name": "Phong Phat trien",
              "code": "DEV",
              "manager_name": "Tran Thi B",
              "employee_count": 80,
              "children": []
            }
          ]
        }
      ]
    },
    "version": 42,
    "snapshot_at": "2026-04-14T08:00:00Z"
  }
}
```

#### GET /api/v1/org-chart/:companyId/versions
List historical org chart snapshots.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "version": 42,
        "created_by": "admin",
        "created_at": "2026-04-14T08:00:00Z",
        "department_count": 25,
        "change_summary": "Added department 'Phong QA'"
      },
      {
        "version": 41,
        "created_by": "admin",
        "created_at": "2026-03-01T08:00:00Z",
        "department_count": 24,
        "change_summary": "Merged 'Phong Testing' into 'Phong QA'"
      }
    ]
  }
}
```

### Error Format

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Du lieu khong hop le",
    "details": [
      {
        "field": "tax_id",
        "message": "Ma so thue da ton tai",
        "rule": "unique"
      }
    ]
  }
}
```

**Common Error Codes:**

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `VALIDATION_ERROR` | 400 | Request body validation failed |
| `UNAUTHORIZED` | 401 | Missing or invalid authentication |
| `FORBIDDEN` | 403 | Insufficient permissions |
| `NOT_FOUND` | 404 | Resource not found |
| `DEPARTMENT_HAS_CHILDREN` | 409 | Cannot delete department with children |
| `DEPARTMENT_HAS_EMPLOYEES` | 409 | Cannot deactivate department with employees |
| `COMPANY_HAS_EMPLOYEES` | 409 | Cannot deactivate company with employees |
| `CYCLE_DETECTED` | 409 | Operation would create hierarchy cycle |
| `LOCATION_HAS_EMPLOYEES` | 409 | Cannot deactivate location with employees |
| `CONFLICT` | 409 | Resource conflict (duplicate code) |
| `INTERNAL_ERROR` | 500 | Server error |

### Endpoints Summary Table

| Method | Endpoint | Auth Required | Permission | Description |
|--------|----------|---------------|------------|-------------|
| GET | `/api/v1/companies` | Yes | companies:read | List companies |
| POST | `/api/v1/companies` | Yes | companies:manage | Create company |
| GET | `/api/v1/companies/:id` | Yes | companies:read | Get company detail |
| PUT | `/api/v1/companies/:id` | Yes | companies:manage | Update company |
| DELETE | `/api/v1/companies/:id` | Yes | companies:manage | Deactivate company |
| GET | `/api/v1/departments` | Yes | departments:read | List departments |
| POST | `/api/v1/departments` | Yes | departments:manage | Create department |
| GET | `/api/v1/departments/:id` | Yes | departments:read | Get department detail |
| PUT | `/api/v1/departments/:id` | Yes | departments:manage | Update department |
| PUT | `/api/v1/departments/:id/move` | Yes | departments:manage | Move department |
| DELETE | `/api/v1/departments/:id` | Yes | departments:manage | Deactivate department |
| GET | `/api/v1/locations` | Yes | locations:read | List locations |
| POST | `/api/v1/locations` | Yes | locations:manage | Create location |
| PUT | `/api/v1/locations/:id` | Yes | locations:manage | Update location |
| DELETE | `/api/v1/locations/:id` | Yes | locations:manage | Deactivate location |
| GET | `/api/v1/subsidiaries` | Yes | subsidiaries:read | List subsidiaries |
| POST | `/api/v1/subsidiaries` | Yes | subsidiaries:manage | Create subsidiary |
| PUT | `/api/v1/subsidiaries/:id` | Yes | subsidiaries:manage | Update subsidiary |
| DELETE | `/api/v1/subsidiaries/:id` | Yes | subsidiaries:manage | Remove subsidiary |
| GET | `/api/v1/org-chart/:companyId` | Yes | org_chart:read | Get org chart |
| GET | `/api/v1/org-chart/:companyId/versions` | Yes | org_chart:read | List org chart versions |

---

## 6. Permissions Matrix

### Roles

| Role | Description | Scope |
|------|-------------|-------|
| System Administrator | Full org structure administration | Global |
| Group Director | Consolidated group-level view | All companies in group |
| Company Director | Single company management | Own company |
| Department Manager | Department tree management | Own department subtree |
| HR Manager | Department assignment and org changes | Global (within company) |
| Employee | View org chart and directory | Read-only, all accessible |
| Auditor | Compliance review | Read-only, global |

### CRUD Permission Matrix

| Permission | System Admin | Group Director | Company Director | Dept Manager | HR Manager | Employee | Auditor |
|------------|:-----------:|:--------------:|:----------------:|:------------:|:----------:|:--------:|:-------:|
| `companies:read` | Y | Y | Y (own) | Y | Y | Y | Y |
| `companies:manage` | Y | N | Y (own) | N | N | N | N |
| `departments:read` | Y | Y | Y (company) | Y (subtree) | Y | Y | Y |
| `departments:manage` | Y | N | Y (company) | Y (subtree) | Y (company) | N | N |
| `departments:move` | Y | N | Y (company) | N | Y (company) | N | N |
| `departments:merge` | Y | N | Y (company) | N | N | N | N |
| `locations:read` | Y | Y | Y (company) | Y | Y | Y | Y |
| `locations:manage` | Y | N | Y (company) | N | Y (company) | N | N |
| `subsidiaries:read` | Y | Y | Y (involved) | N | N | N | Y |
| `subsidiaries:manage` | Y | Y | N | N | N | N | N |
| `org_chart:read` | Y | Y | Y | Y | Y | Y | Y |
| `org_chart:snapshot` | Y | N | Y | N | Y | N | N |
| `org_change_logs:read` | Y | Y | Y (company) | Y (dept) | Y | N | Y |

### Row-Level Security Rules

| Rule | Description |
|------|-------------|
| **Company Isolation** | Company Directors can only see and modify data for their own company. They cannot see other companies' departments, locations, or subsidiary relationships unless they also have Group Director role. |
| **Department Subtree** | Department Managers can only manage departments within their own subtree (the department they manage plus all descendant departments). They cannot modify sibling departments or parent departments. |
| **Group Consolidation** | Group Directors see consolidated data across all group companies. When viewing a specific subsidiary, they see full detail; when viewing the group level, they see aggregated metrics weighted by ownership percentage. |
| **HR Global within Company** | HR Managers have company-wide department management access within their assigned company but cannot access other companies unless explicitly granted. |
| **Employee Read-Only** | Employees can view the full org chart and department directory but cannot see sensitive department data (budget, cost center code) unless their role grants additional access. |
| **Audit Read-Only** | Auditors have global read access to all org data including change logs, but cannot see budget or cost center financial details (these are masked). |
| **Root Department Protection** | The root department of each company (is_root=true) cannot be deleted or moved by any role. Only the company itself can be deactivated, which cascades to deactivate the root department. |
| **Admin Override** | System Admins bypass all row-level restrictions and have full access to all org data across all companies. |

---

## 7. State Machine

### Company Lifecycle

```
              +----------+
              |  Draft   |  (being configured, not yet operational)
              +----+-----+
                   |
            activate
                   |
                   v
              +----------+    deactivate    +-------------+
    create -->|  Active  |--------------->|  Inactive   |
              +----+-----+                +------+------+
                   |                             |
                   | reactivate                  | dissolve (after 90 days)
                   |                             |
                   v                             v
              +----------+                +-------------+
              |  Active  |                |  Dissolved  |  (terminal)
              +----------+                +-------------+
```

### Department Lifecycle

```
              +----------+
    create -->|  Active  |
              +----+-----+
                   |
            deactivate (after employees reassigned)
                   |
                   v
              +----------+    reactivate   +----------+
              | Inactive |--------------->|  Active  |
              +----+-----+                +----------+
                   |
                   | delete (after 90 days inactive)
                   v
              +----------+
              | Deleted  |  (terminal)
              +----------+
```

### Transition Table

| From State | Event | To State | Trigger | Notes |
|------------|-------|----------|---------|-------|
| `Draft` | Admin activates | `Active` | System Admin action | Company becomes operational |
| `Active` | Admin deactivates | `Inactive` | System Admin action | Requires zero active employees |
| `Inactive` | Admin reactivates | `Active` | System Admin action | Company resumes operations |
| `Inactive` | 90 days elapsed | `Dissolved` | System (scheduled job) | Legal dissolution |
| `Dissolved` | -- | -- | -- | Terminal state |
| `Active` (Dept) | Admin deactivates | `Inactive` (Dept) | System Admin or Dept Manager | Requires zero employees |
| `Inactive` (Dept) | Admin reactivates | `Active` (Dept) | System Admin or Dept Manager | Department resumes operations |
| `Inactive` (Dept) | 90 days elapsed | `Deleted` (Dept) | System (scheduled job) | Permanent removal |

### Approval Workflow

Organizational changes require approval based on impact:

| Operation | Approval Required | Approver | Notes |
|-----------|-------------------|----------|-------|
| Create company | Yes | Group Director (if part of group) or System Admin | Legal registration verification required |
| Deactivate company | Yes | Group Director + HR Director | Requires all employees reassigned |
| Create department | No | Company Director or System Admin | Standard operation |
| Delete department (with employees) | Yes | HR Director + Department Manager | Requires employee reassignment plan |
| Move department to different parent | Yes | Company Director | Impacts reporting structure |
| Merge departments | Yes | Company Director + HR Manager | Requires consolidation plan |
| Create subsidiary relationship | Yes | Group CEO + Legal | Legal and financial review required |
| Change ownership percentage | Yes | Group CEO + Finance Director | Financial impact assessment required |
| Divest subsidiary | Yes | Group CEO + Board | Major corporate action |
| Create location | No | Company Director | Standard operation |
| Deactivate location (with employees) | Yes | HR Director + Company Director | Requires employee relocation plan |

---

## 8. Reports & Analytics

### 8.1 Report: Group Structure Overview

**Description:** Consolidated view of the corporate group structure including all companies, subsidiaries, and ownership details (image_30).

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Company | Employee count | Company type, status | Horizontal bar chart |
| Subsidiary | Ownership percentage | Parent company | Table with percentage bars |
| Group | Total consolidated employees | None | Single value card |
| Group | Total consolidated revenue | Fiscal period | Single value card with trend |
| Company | Subsidiary count | Company type | Table |
| Ownership | Voting vs. ownership % delta | None | Scatter plot |

### 8.2 Report: Department Structure Analysis

**Description:** Analysis of department hierarchy, spans, and distributions (image_37, image_39).

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Department | Employee count | Company, status | Horizontal bar chart |
| Department | Hierarchy depth | Company | Histogram |
| Department | Manager span (direct reports) | Company, category | Box plot |
| Category | Department count | Company | Pie chart |
| City | Department count | Company type | Bar chart |
| Month | Department changes | Company, action type | Stacked bar chart |

### 8.3 Report: Geographic Distribution

**Description:** Geographic analysis of company locations and employee distribution.

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| City | Employee count | Company, location type | Bar chart |
| City | Department count | Company | Bar chart |
| Location type | Count | Company | Donut chart |
| City | Location capacity utilization | Company | Table with progress bars |
| Region | Employee density | None | Map visualization |

### 8.4 Report: Organizational Change History

**Description:** Audit trail of all structural changes to the organization.

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Month | Change events count | Entity type, action | Stacked bar chart |
| Entity | Change frequency | Entity type, time range | Table |
| User | Changes made count | Time range | Horizontal bar chart |
| Action | Distribution | Time range | Pie chart |
| Department | Change history | Department, time range | Timeline |

### Report Summary Table

| Report Name | Audience | Frequency | Export Formats |
|-------------|----------|-----------|----------------|
| Group Structure Overview | Group Director, System Admin | Real-time | PDF, Excel |
| Department Structure Analysis | Company Director, HR Manager | Weekly | PDF, Excel |
| Geographic Distribution | HR Manager, Company Director | Monthly | PDF, Excel, Map |
| Organizational Change History | Auditor, System Admin | On-demand | PDF, CSV |

---

## 9. Integration Points

### External APIs

| API | Provider | Purpose | Authentication | Rate Limit |
|-----|----------|---------|----------------|------------|
| Government Business Registry | Vietnamese General Department of Taxation | Verify company tax IDs and registration status | Government API key | 100 lookups/day |
| Google Maps / OpenStreetMap | Google / OSM | Geocode addresses, display location maps | API key | 2,500 requests/day |
| Email Service | SendGrid, AWS SES | Org change notifications, deactivation alerts | API key | 100 emails/minute |
| Accounting System | SAP, Oracle, MISA Accounting | Sync cost center codes, department budgets | OAuth2 / API key | 1,000 requests/hour |

### Webhooks

| Webhook | Event | Payload | Subscribers |
|---------|-------|---------|-------------|
| `company.created` | New company registered | `{company_id, name, code, tax_id, company_type, created_by}` | M02 (Roles), M05 (Employee Mgmt) |
| `company.deactivated` | Company deactivated | `{company_id, name, employee_count, department_count}` | M01 (Auth - deactivate users), M02 (Roles) |
| `department.created` | New department created | `{department_id, name, code, company_id, parent_id, manager_id}` | M02 (Roles - update scopes), M05 (Employee Mgmt) |
| `department.moved` | Department parent changed | `{department_id, old_parent_id, new_parent_id, changed_by}` | M02 (Roles - update data scopes), M06 (Tasks), M16 (Objectives) |
| `department.deactivated` | Department deactivated | `{department_id, name, company_id, affected_employee_count}` | M01 (Auth), M02 (Roles), M05 (Employee Mgmt - reassignment) |
| `location.created` | New location registered | `{location_id, name, code, type, city, country}` | M05 (Employee Mgmt) |
| `subsidiary.created` | New subsidiary relationship | `{parent_id, subsidiary_id, ownership_pct, acquisition_date}` | M40 (Group Mgmt), M39 (Dashboard) |
| `subsidiary.ownership_changed` | Ownership percentage updated | `{subsidiary_id, old_pct, new_pct, changed_by}` | M40 (Group Mgmt - recalculate consolidated) |
| `org_chart.snapshotted` | Org chart snapshot created | `{company_id, version, department_count, change_summary}` | M16 (Objectives - cascade check), M39 (Dashboard) |

### Events

| Event | Source | Consumers | Description |
|-------|--------|-----------|-------------|
| `CompanyCreated` | M03 | M01, M02, M05 | New company available for user assignment |
| `DepartmentHierarchyChanged` | M03 | M02, M06, M16 | Department tree modification; triggers role scope and objective recalculation |
| `DepartmentDeactivated` | M03 | M01, M02, M05 | Department no longer active; employees need reassignment |
| `LocationCreated` | M03 | M05 | New location available for employee assignment |
| `SubsidiaryOwnershipChanged` | M03 | M40, M39 | Consolidated metrics need recalculation |
| `OrgChangeLogged` | M03 | Audit System | Structural change recorded for compliance |

---

## 10. Technical Notes

### Performance
- Department tree queries use materialized path (`path` column) for O(1) subtree retrieval. Querying all descendants of a department is a simple `WHERE path LIKE '/{dept_id}/%'` with a B-tree index.
- Org chart rendering targets < 3 seconds for 500 departments, achieved by pre-computing the tree structure and serving it as a single JSON response.
- Location geocoding is done asynchronously: when a new location is created with an address, a background job geocodes it and updates lat/lng. The location is available immediately without coordinates.
- Consolidated metrics for groups with 20+ subsidiaries are pre-computed via materialized views refreshed every 15 minutes.
- Department move operations (changing parent) are processed asynchronously for departments with 100+ employees, with a progress indicator shown to the user.

### Caching
- Redis cache stores the full org chart per company (key: `org_chart:{company_id}`, TTL: 10 minutes). Cache invalidates on any department create/update/move/delete.
- Company list cached for 30 minutes (low-change data).
- Location list cached for 15 minutes.
- Subsidiary relationships cached for 5 minutes (ownership changes impact consolidated metrics).
- Department hierarchy cache includes a version number; when any department changes, all related company org charts are invalidated via Redis pub/sub.
- Materialized path updates cascade: when a department's path changes, all descendants' cached paths are invalidated and recomputed.

### File Storage
- Company logos stored in object storage (S3-compatible) with max 2MB upload.
- Org chart export files (PNG, PDF) generated on demand and stored temporarily (TTL: 1 hour) with presigned URLs.
- Location photos (building images) stored with max 5MB, auto-compressed to 1MB.

### Localization
- Company names support both Vietnamese and English: stored as JSONB `{vi: "Cong ty MISA", en: "MISA Company"}`.
- Department names stored in Vietnamese with optional English translation.
- Address fields follow Vietnamese administrative hierarchy: Country > Province/Tinh > City/Thanh pho > District/Huyen > Ward/Xa > Street/Duong.
- International addresses supported with a flexible address format (address_line1/2/3 + country-specific fields in JSONB).
- Currency codes use ISO 4217; default is VND for Vietnamese companies.

### Mobile
- Org chart accessible on mobile via responsive tree view with tap-to-expand nodes.
- Department directory available offline on mobile (last-synced version, refreshed on reconnect).
- Location map view uses native map integration on mobile devices.
- All org structure API endpoints are RESTful and mobile-compatible.
- Push notifications sent to mobile when user's department is changed.

### Security
- Department hierarchy integrity enforced at the database level: triggers prevent cycles in the parent-child relationship.
- Company deactivation cascades: deactivating a company automatically deactivates all its departments and locations.
- Org change logs are append-only and tamper-evident (cryptographic hash chaining).
- Materialized path values are validated on every department move to ensure consistency.
- Bulk org changes (5+ department moves in one operation) require System Admin role and are logged with mandatory reason.

### Data Retention
- Org change logs: retained for 7 years for compliance.
- Org chart snapshots: retained for 2 years (older snapshots archived to cold storage).
- Deactivated companies: soft-deleted, permanently purged after 7 years if no audit trail references them.
- Deleted departments: soft-deleted, permanently purged after 2 years.

### Scheduled Jobs
- **Inactive Entity Cleanup:** Runs daily at 3:00 AM. Finds departments and locations inactive for 90+ days and marks them as deleted.
- **Org Chart Snapshot:** Runs daily at 4:00 AM for each active company. Creates a snapshot if any department changed since the last snapshot.
- **Employee Count Recalculation:** Runs hourly. Updates cached employee_count on departments, locations, and companies by counting actual employee assignments.
- **Consolidated Metrics Refresh:** Runs every 15 minutes for groups with subsidiaries. Recalculates consolidated employee count and revenue.
- **Geocoding Queue Processor:** Runs every 5 minutes. Processes pending location geocoding requests.
