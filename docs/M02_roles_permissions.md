# M02 - Roles, Permissions & Access Control (Vai tro, Quyen han va Kiem soat Truy cap)

**Category:** Foundation  
**Priority:** P0  
**Reference Images:** assets/image_02.png, assets/image_35.png, assets/image_60.png  
**Dependencies:** M01 - User & Authentication  
**Generated:** April 14, 2026

---

## 1. Module Overview

The Roles, Permissions & Access Control module provides the centralized Role-Based Access Control (RBAC) engine that governs who can do what across the entire ABN ERP platform. Every module -- from Accounting (M04) to Payroll (M12) to Workflow (M19) -- consults this module to determine whether a user's action is authorized. The system implements a three-tier permission model: Roles (named collections of permissions), Permissions (granular action-resource pairs), and Data Scopes (row-level filters that limit visibility to department, company, or self). Image_02 illustrates this in practice: when a new employee receives their welcome email, the workflows listed (Tạm ứng, Thanh toán, Quy trình mua hàng, etc.) are automatically assigned based on the roles provisioned during onboarding.

The system administration dashboard (image_60) provides real-time visibility into access patterns: 2,800 total registered users, per-application usage metrics, access trends over time, and a detailed usage table showing which modules are most actively consumed. Department-based access control (image_35) ensures that Department Managers only see data for their own teams, while HR Managers retain global visibility. The module also maintains a comprehensive AccessLog that records every authentication event, permission check failure, and privilege escalation attempt, enabling forensic security audits and compliance reporting.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| System Administrator | Configures roles, permissions, and access policies across the entire platform | Full CRUD on all RBAC objects, global access log visibility |
| Security Officer | Monitors access patterns, reviews audit logs, investigates anomalies | Read-only on access logs, write on security policies |
| Department Manager | Manages role assignments for team members within their department | Assign pre-defined roles to department members, read department-scoped logs |
| HR Manager | Assigns initial roles during employee onboarding, modifies roles on promotion/transfer | Assign roles globally, read role-employee mapping |
| Employee (Self-Service) | Views own roles and permissions, requests additional access | Read-only on own role assignments, can submit access requests |
| Auditor | Reviews access patterns for compliance audits | Read-only on access logs and role definitions, no permission to modify |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M01 - User & Authentication | Users are assigned roles; authentication checks are enriched with role data |
| M03 - Company & Organization | Data scope is determined by department and company hierarchy |
| M05 - Employee Management | Role assignments are tied to employee records and employment status |
| M19 - Workflow | Workflow approval steps check user roles for approval authority |

### Key Business Rules

1. Every user must have at least one role; a user with zero roles cannot access any module beyond the onboarding wizard.
2. Role assignment is cumulative: if a user has multiple roles, they receive the UNION of all permissions from all assigned roles.
3. Deny overrides allow: if any role explicitly denies a permission, the user is blocked regardless of other roles granting it.
4. Department-level data scope is enforced: a Department Manager role in Department A cannot view data from Department B even if the permission is granted.
5. Role changes take effect immediately for new sessions; existing active sessions are notified but not forcibly terminated (configurable per role).
6. The System Administrator role cannot be assigned to more than 5 users simultaneously, enforced by policy.
7. All permission changes are logged in the AccessLog with before/after values for audit trail reconstruction.
8. Temporary role assignments (e.g., "Acting Manager" during a manager's absence) have an automatic expiration date and are revoked by a scheduled job.
9. Inactive employees (terminated, resigned) automatically have all roles stripped within 1 hour of deactivation in M01.
10. Permission inheritance flows down the department hierarchy: a manager role in a parent department implicitly inherits child department permissions unless explicitly scoped otherwise.

---

## 2. Screen Inventory

### 2.1 Screen: System Admin Dashboard (User & Access Overview)

**Route:** `/admin/dashboard` | **Purpose:** Provide system administrators with platform-wide visibility into user access, role distribution, and per-application usage metrics.

| UI Component | Description |
|--------------|-------------|
| Page Header | "Quan tri he thong" (System Administration), user count badge (2,800 users) |
| Summary Cards | Total registered users (2,800), Active sessions (1,450), Failed login attempts today (23), Roles defined (45) |
| Access Trend Chart | Line chart showing daily active users over past 30 days with peak/average indicators |
| Per-App Usage Table | Table listing each ERP module with: app name, active users count, total API calls today, last accessed timestamp |
| Role Distribution Chart | Donut chart showing user count per role (e.g., Employee 2,200, Dept Manager 180, HR 50, Admin 15, Auditor 5) |
| Recent Access Events | Scrollable log: latest permission denials, role changes, session creations with timestamps and user details |
| Quick Actions | "Manage Roles", "View Access Logs", "Export Report" buttons |

**User Interactions:**
- Click on a module row in the usage table to drill down into that module's access details
- Click on a role slice in the donut chart to see all users assigned that role
- Hover on access trend chart points for detailed daily metrics
- Click recent access events to view full event details in a side panel
- Click quick actions to navigate to respective management screens
- Export usage report to PDF or Excel

**Data Displayed:**
- Total user count and active session count
- Daily active user trend (30-day window)
- Per-module usage statistics (user count, API call count)
- Role distribution across the user base
- Recent access events with severity indicators

### 2.2 Screen: Role Management

**Route:** `/admin/roles` | **Purpose:** Create, edit, and manage roles with their associated permissions and data scopes.

| UI Component | Description |
|--------------|-------------|
| Page Header | "Quan ly vai tro" (Role Management), "Them moi" (Add New) button, total role count |
| Search Bar | Filter roles by name, description, category |
| Role List Table | Columns: Role name, Description, Category, Assigned users count, Created date, Actions |
| Role Detail Modal/Panel | When editing: role name, description, category, permission tree (checkboxes), data scope selector |
| Permission Tree | Hierarchical tree grouped by module: each module shows CRUD checkboxes (Create, Read, Update, Delete) plus custom actions |
| Data Scope Selector | Dropdown: Global, Department (select dept), Company (select company), Self-only |
| Deny Overrides Section | Table of explicitly denied permissions for this role (override any grant from other roles) |
| Assigned Users Tab | List of users currently assigned this role with assignment date and assigned-by |

**User Interactions:**
- Create new role with name, description, and initial permissions
- Edit existing role by clicking the row or edit icon
- Toggle permissions via checkboxes in the hierarchical tree
- Set data scope via dropdown with cascading department/company selector
- Add deny overrides to block specific permissions
- View and search assigned users
- Clone an existing role as a template for a new role
- Delete role (with confirmation if users are assigned)

**Data Displayed:**
- Role metadata (name, description, category, created date)
- Full permission matrix per role
- Data scope configuration
- Assigned user list with assignment metadata
- Deny override rules

### 2.3 Screen: Access Log Viewer

**Route:** `/admin/access-logs` | **Purpose:** Search, filter, and review the comprehensive audit log of all authentication and authorization events.

| UI Component | Description |
|--------------|-------------|
| Page Header | "Nhat ky truy cap" (Access Log), date range selector, export button |
| Filter Panel | Dropdowns: Event type, User, Role, Module, Result (allowed/denied); Text search for IP, user agent |
| Event Log Table | Columns: Timestamp, Event type, User, Role, Module, Action, Resource, Result, IP address, Details |
| Event Detail Panel | Expandable row showing full event context: request body, permission check result, data scope applied |
| Pagination | Records per page (50/100/500), page navigation, total count |
| Summary Bar | Event type distribution, top denied users, top accessed modules |

**User Interactions:**
- Apply multiple filters simultaneously (AND logic)
- Search by user name, IP address, or resource identifier
- Click event type to filter to that event category
- Expand individual log entries for full details
- Export filtered logs to CSV for external analysis
- Set up alert rules on specific event patterns (e.g., 10+ denials from same user in 1 hour)

**Data Displayed:**
- Timestamped event records with full context
- Event type classification (login, logout, permission_grant, permission_deny, role_assign, role_revoke, etc.)
- User and role information per event
- Module and action being attempted
- Allow/deny result
- Source IP address and user agent
- Geographic location (if available)

### 2.4 Screen: User Role Assignment

**Route:** `/admin/users/:userId/roles` | **Purpose:** View and manage role assignments for a specific user.

| UI Component | Description |
|--------------|-------------|
| User Header | User photo, name, email, employee code, current status badge |
| Current Roles Table | Columns: Role name, Assigned date, Assigned by, Data scope, Expiration date (if temporary), Actions |
| Add Role Modal | Role selector (searchable dropdown), data scope selector, expiration date picker (for temporary), reason field |
| Permission Preview Panel | Aggregated permission view showing all permissions the user has from all assigned roles |
| Conflict Warning Banner | Shown when conflicting permissions exist (e.g., one role grants, another denies the same permission) |
| Access History Chart | Mini timeline showing role changes over time |

**User Interactions:**
- Add a new role assignment with scope and optional expiration
- Remove a role assignment (with confirmation and audit log entry)
- Modify data scope for an existing role assignment
- Set or change expiration date for temporary assignments
- View aggregated permissions across all roles
- Review conflict warnings and resolve them
- View role change history timeline

**Data Displayed:**
- User identity and status
- All currently assigned roles with metadata
- Aggregated permission set (union of all roles)
- Permission conflicts (grant vs. deny)
- Role assignment history

### 2.5 Screen: Permission Matrix (Cross-Module)

**Route:** `/admin/permissions/matrix` | **Purpose:** Visualize the full permission matrix across all modules and roles in a grid view.

| UI Component | Description |
|--------------|-------------|
| Page Header | "Ma tran quyen han" (Permission Matrix), module selector, role selector |
| Matrix Grid | Rows = permissions (grouped by module), Columns = roles, Cells = check/X icons |
| Filter Controls | Module filter (multi-select), role filter (multi-select), permission type filter (CRUD/custom) |
| Legend | Check = granted, X = denied, dash = not specified, highlight = conflict |
| Export Button | Export matrix to Excel for documentation and compliance review |

**User Interactions:**
- Filter matrix to specific modules or roles
- Click a cell to toggle grant/deny (requires System Admin role)
- Hover on a cell for detailed permission description
- Export full or filtered matrix to Excel
- Highlight conflicts between roles

**Data Displayed:**
- Full permission matrix: role x permission grant/deny status
- Module grouping
- Conflict indicators
- Summary counts per role (total grants, total denials)

---

## 3. Feature Specifications

### 3.1 Feature: Role CRUD Management

**Description:** Full lifecycle management of roles including creation, editing, cloning, and deletion. Each role defines a named collection of permissions, a data scope, and metadata for categorization and documentation.

**User Stories:**
- As a System Administrator, I want to create a new role with specific permissions so that I can control what users with that role can access.
- As an HR Manager, I want to clone an existing role and modify it slightly so that I do not have to start from scratch when creating similar roles.
- As an Auditor, I want to see the history of role changes so that I can verify compliance with access policies.

**Acceptance Criteria:**
- **Given** valid role name and at least one permission, **When** a System Admin creates a role, **Then** the role is saved with a unique ID, audit log entry, and is immediately available for assignment.
- **Given** a role with assigned users, **When** a System Admin attempts to delete it, **Then** a confirmation dialog shows the count of affected users and requires explicit acknowledgment before deletion.
- **Given** a role name already exists, **When** a System Admin tries to create a role with the same name, **Then** a validation error prevents creation.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Role Name | Not null, 2-100 characters | "Ten vai tro là bắt buộc (2-100 ký tự)" |
| Unique | Role Name | Not existing in Roles table | "Ten vai tro đã tồn tại" |
| Min Permissions | Permissions | At least 1 permission selected | "Phải chọn ít nhất 1 quyền hạn" |
| Required | Data Scope | Not null | "Phạm vi dữ liệu là bắt buộc" |
| Valid | Category | Must be from predefined list | "Danh mục không hợp lệ" |

**Edge Cases:**
- Deleting a role that is the only role assigned to a user (system prevents deletion; shows error "Cannot delete: X users would have zero roles").
- Renaming a role that is referenced in workflow approval configurations (system updates references automatically via foreign key; display name change is cosmetic).
- Creating a role with 500+ permissions (system allows but shows warning "This role has an unusually large number of permissions. Consider splitting into sub-roles").
- Cloning a system-defined role (system creates a copy with "(Copy)" suffix; original system role remains protected from deletion).

### 3.2 Feature: Permission Assignment (Hierarchical Tree)

**Description:** Hierarchical permission tree grouped by module, allowing administrators to grant or deny specific action-resource combinations to roles.

**User Stories:**
- As a System Administrator, I want to grant CRUD permissions per module so that roles have precisely the access they need.
- As a Security Officer, I want to set deny overrides so that certain sensitive operations are blocked even if another role grants them.
- As a Department Manager, I want to see what permissions a role grants so that I understand what my team members can do.

**Acceptance Criteria:**
- **Given** a permission tree, **When** a System Admin checks a parent module node, **Then** all child permissions under that module are automatically checked (cascading grant).
- **Given** a parent node is partially checked (some children granted, some not), **When** the Admin unchecks the parent, **Then** all children are unchecked.
- **Given** a deny override is set for a permission, **When** a user with that role attempts the action, **Then** the request is blocked regardless of other role grants.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Valid Module | Module ID | Must exist in registered modules | "Module không tồn tại" |
| Valid Action | Action | Must be in {create, read, update, delete, approve, export, import, delete_all} | "Hành động không hợp lệ" |
| No Self-Deny | System Admin role | Cannot deny permissions on users:manage or roles:manage | "Không thể từ chối quyền quản trị hệ thống" |

**Edge Cases:**
- Granting a permission that does not exist in the system (validation rejects unknown permissions).
- Circular deny conflict (Role A grants `employees:read`, Role B denies `employees:read` for same user -- deny wins, conflict banner shown in UI).
- Bulk permission import from JSON template (system validates each permission against registered module definitions before applying).

### 3.3 Feature: Data Scope Enforcement (Row-Level Security)

**Description:** Row-level security that filters data access based on the user's role data scope: Global, Department, Company, or Self.

**User Stories:**
- As a Department Manager, I want to see only employees in my department so that I do not access data outside my authority.
- As an HR Manager, I want global visibility into all employee records so that I can manage the entire workforce.
- As an employee, I want to see only my own profile data so that my privacy is protected.

**Acceptance Criteria:**
- **Given** a user with Department-scoped role, **When** they query the employee list API, **Then** only employees in their department (and child departments) are returned.
- **Given** a user with Global scope, **When** they query any module's data, **Then** all records are returned (subject to module-specific filters).
- **Given** a user with Self-only scope, **When** they attempt to access another user's profile, **Then** a 403 Forbidden response is returned.
- **Given** a user with multiple roles of different scopes, **When** they access data, **Then** the broadest scope is applied (Global > Company > Department > Self).

**Validation Rules:**

| Rule | Condition | Error Message |
|------|-----------|---------------|
| Scope Hierarchy | If multiple roles, use broadest scope | N/A (automatic resolution) |
| Department Required | If scope=Department, user must have primary department | "Không thể áp dụng phạm vi phòng ban khi người dùng chưa có phòng ban chính" |
| Company Required | If scope=Company, user must have company assignment | "Không thể áp dụng phạm vi công ty khi người dùng chưa được gán công ty" |

**Edge Cases:**
- User is transferred to a new department mid-session (existing session continues with old scope until refresh; next login uses new scope).
- User has Department A scope via Role X and Department B scope via Role Y (broadest scope does not help here; system computes UNION of both department trees).
- Department manager role applied to a department with no sub-departments (scope is limited to that single department).
- Cross-company data access attempt (company scope strictly isolates data; no cross-company visibility unless Global scope is granted).

### 3.4 Feature: Access Logging & Audit Trail

**Description:** Comprehensive logging of all authentication and authorization events for security monitoring and compliance auditing.

**User Stories:**
- As a Security Officer, I want to see all permission denials so that I can investigate potential security incidents.
- As an Auditor, I want a complete history of role changes so that I can verify compliance with access policies.
- As a System Administrator, I want to monitor active sessions per user so that I can detect account sharing.

**Acceptance Criteria:**
- **Given** any authentication or authorization event, **When** the event occurs, **Then** it is logged with timestamp, user, action, resource, result, IP, and user agent within 100ms.
- **Given** a role assignment change, **When** the change is saved, **Then** the audit log captures before/after role lists for the user.
- **Given** an access log query, **When** a Security Officer applies filters, **Then** results are returned within 2 seconds for up to 1 million log entries.

**Validation Rules:**

| Rule | Condition | Error Message |
|------|-----------|---------------|
| Log Integrity | Logs are append-only, never mutable or deletable by application users | N/A (enforced at database level with INSERT-only triggers) |
| Retention | Logs retained for minimum 7 years | N/A (scheduled purge only affects logs older than 7 years) |

**Edge Cases:**
- Logging system itself experiences high load (logs are written to a queue first, then batch-inserted; queue overflow triggers alert to IT Support).
- Attempt to delete or modify a log entry (application-level and database-level controls prevent this; attempt itself is logged as a security event).
- Log query spanning 7 years of data (system requires date range limit of max 90 days per query; broader analysis requires scheduled report job).

### 3.5 Feature: Temporary Role Assignment & Auto-Expiration

**Description:** Assign roles to users with an expiration date, after which the role is automatically revoked by a scheduled job.

**User Stories:**
- As a Department Manager, I want to assign an "Acting Manager" role to a senior team member during my leave so that they can approve requests in my absence.
- As an HR Manager, I want temporary roles to auto-expire so that I do not need to manually revoke access after the temporary period ends.
- As a Security Officer, I want to be notified when temporary roles are assigned so that I can monitor elevated privilege grants.

**Acceptance Criteria:**
- **Given** a role assignment with an expiration date, **When** the expiration date passes, **Then** a scheduled job automatically revokes the role and logs the revocation.
- **Given** a temporary role is about to expire (within 24 hours), **When** the assigned user has active sessions, **Then** they receive an in-app notification warning of impending role change.
- **Given** a temporary role assignment, **When** it is created, **Then** a notification is sent to the Security Officer team.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Expiration Date | Not null, must be in future | "Ngày hết hạn phải ở tương lai" |
| Max Duration | Expiration Date | <= 90 days from now | "Thời gian gán tạm thời tối đa là 90 ngày" |
| Required | Reason | Not null, min 10 characters | "Lý do gán tạm thời là bắt buộc (tối thiểu 10 ký tự)" |

**Edge Cases:**
- Temporary role expiration falls on a weekend/holiday (scheduled job runs daily at 2 AM; role is revoked at that time regardless of calendar).
- Manager extends temporary assignment before expiration (system updates expiration date and logs the extension; notification sent again to Security Officer).
- User with only a temporary role (no permanent roles) reaches expiration (user is left with zero roles and loses access; HR is notified to assign a permanent role).

---

## 4. Database Design

### ASCII ERD Diagram

```
+----------+       M:N        +----------------+       M:N        +-------------+
|   User   |----------------->|   UserRole     |<-----------------|    Role     |
+----------+                  +----------------+                  +-------------+
| id (PK)  |                  | id (PK)        |                  | id (PK)     |
| email    |                  | user_id (FK)   |                  | name        |
| username |                  | role_id (FK)   |                  | description |
| status   |                  | data_scope     |                  | category    |
+----------+                  | scope_value    |                  | is_system   |
      |                       | assigned_by    |                  | created_at  |
      |                       | assigned_at    |                  | updated_at  |
      |                       | expires_at     |                  +-------------+
      |                       +----------------+                        |
      |                                                                 | 1:N
      |                                                                 |
+-------------+           +---------------------+                  +------------------+
|  Permission |<----------|  RolePermission     |                  |   AccessLog      |
+-------------+           +---------------------+                  +------------------+
| id (PK)     |           | id (PK)             |                  | id (PK)         |
| module      |           | role_id (FK)        |                  | timestamp       |
| resource    |           | permission_id (FK)  |                  | event_type      |
| action      |           | effect (grant/deny) |                  | user_id (FK)    |
| description |           | created_by          |                  | role_id (FK)    |
+-------------+           +---------------------+                  | module          |
                                                                   | action          |
                                                                   | resource        |
                                                                   | result          |
                                                                   | ip_address      |
                                                                   | user_agent      |
                                                                   | details (JSON)  |
                                                                   +------------------+

+--------------------+
|  TemporaryRoleLog  |
+--------------------+
| id (PK)            |
| user_role_id (FK)  |
| action             |
| executed_at        |
| executed_by        |
+--------------------+
```

### Table Schemas

#### Table: `roles`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique role identifier |
| name | VARCHAR(100) | UNIQUE, NOT NULL, INDEX | Role name (e.g., "HR Manager", "Department Manager") |
| description | TEXT | NULL | Role description and purpose |
| category | VARCHAR(50) | NOT NULL, CHECK (category IN ('system', 'hr', 'finance', 'operations', 'management', 'custom')), INDEX | Role category for grouping |
| is_system | BOOLEAN | NOT NULL, DEFAULT false | Whether this is a system-defined role (protected from deletion) |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Whether role is active and assignable |
| max_assignments | INTEGER | NULL | Maximum number of users who can hold this role (NULL = unlimited) |
| created_by | UUID | FK -> users.id, NOT NULL | User who created this role |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### Table: `permissions`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique permission identifier |
| module | VARCHAR(50) | NOT NULL, INDEX | Module identifier (e.g., "employees", "accounting", "workflow") |
| resource | VARCHAR(100) | NOT NULL, INDEX | Resource within module (e.g., "profile", "invoice", "purchase_order") |
| action | VARCHAR(20) | NOT NULL, CHECK (action IN ('create', 'read', 'update', 'delete', 'approve', 'export', 'import', 'delete_all', 'manage')), INDEX | Action allowed on the resource |
| description | VARCHAR(200) | NOT NULL | Human-readable description of this permission |
| UNIQUE(module, resource, action) | -- | Composite unique constraint | Prevents duplicate permission definitions |

#### Table: `role_permissions`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique assignment identifier |
| role_id | UUID | FK -> roles.id, NOT NULL, INDEX | Associated role |
| permission_id | UUID | FK -> permissions.id, NOT NULL, INDEX | Associated permission |
| effect | VARCHAR(10) | NOT NULL, DEFAULT 'grant', CHECK (effect IN ('grant', 'deny')) | Whether this is a grant or deny |
| created_by | UUID | FK -> users.id, NOT NULL | User who configured this permission |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| UNIQUE(role_id, permission_id) | -- | Composite unique constraint | Prevents duplicate permission assignments per role |

#### Table: `user_roles`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique assignment identifier |
| user_id | UUID | FK -> users.id, NOT NULL, INDEX | Associated user |
| role_id | UUID | FK -> roles.id, NOT NULL, INDEX | Associated role |
| data_scope | VARCHAR(20) | NOT NULL, CHECK (data_scope IN ('global', 'company', 'department', 'self')) | Data access scope for this role assignment |
| scope_value | UUID | NULL | The specific department_id or company_id this scope applies to (NULL for global/self) |
| assigned_by | UUID | FK -> users.id, NOT NULL | User who made this assignment |
| assigned_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Assignment timestamp |
| expires_at | TIMESTAMP | NULL | Expiration for temporary assignments (NULL = permanent) |
| reason | TEXT | NULL | Reason for assignment (required for temporary) |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Whether assignment is currently active |
| UNIQUE(user_id, role_id, scope_value, COALESCE(expires_at, 'infinity')) | -- | Composite unique | Prevents duplicate active assignments |

#### Table: `access_logs`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | BIGSERIAL | PK, NOT NULL | Auto-incrementing log entry ID |
| timestamp | TIMESTAMP | NOT NULL, DEFAULT NOW(), INDEX | Event timestamp |
| event_type | VARCHAR(50) | NOT NULL, INDEX | Event classification (login, logout, permission_grant, permission_deny, role_assign, role_revoke, session_create, session_revoke, password_change, mfa_change) |
| user_id | UUID | FK -> users.id, NULL, INDEX | Associated user (NULL for anonymous events) |
| role_id | UUID | FK -> roles.id, NULL | Role involved (for role-related events) |
| module | VARCHAR(50) | NULL, INDEX | Module involved in the event |
| action | VARCHAR(50) | NULL | Action attempted or performed |
| resource | VARCHAR(200) | NULL | Resource identifier (e.g., employee ID, document ID) |
| result | VARCHAR(20) | NOT NULL, CHECK (result IN ('allowed', 'denied', 'error', 'warning')) | Outcome of the event |
| ip_address | INET | NULL | Client IP address |
| user_agent | TEXT | NULL | Browser/device user agent |
| location | VARCHAR(200) | NULL | Geo-approximated location |
| details | JSONB | NULL | Additional event-specific context |

#### Table: `temporary_role_logs`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique log entry identifier |
| user_role_id | UUID | FK -> user_roles.id, NOT NULL, INDEX | Associated user role assignment |
| action | VARCHAR(20) | NOT NULL, CHECK (action IN ('assigned', 'extended', 'revoked', 'expired')) | Action performed |
| executed_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | When the action occurred |
| executed_by | VARCHAR(50) | NOT NULL | Who/what performed the action ("system" for auto-expiration) |
| notes | TEXT | NULL | Additional context |

### Indexes (SQL)

```sql
-- Roles
CREATE INDEX idx_roles_category ON roles(category);
CREATE INDEX idx_roles_is_active ON roles(is_active);

-- Permissions
CREATE INDEX idx_permissions_module ON permissions(module);
CREATE INDEX idx_permissions_resource ON permissions(resource);
CREATE INDEX idx_permissions_action ON permissions(action);
CREATE INDEX idx_permissions_module_resource_action ON permissions(module, resource, action);

-- Role Permissions
CREATE INDEX idx_role_permissions_role ON role_permissions(role_id);
CREATE INDEX idx_role_permissions_permission ON role_permissions(permission_id);
CREATE INDEX idx_role_permissions_effect ON role_permissions(effect);

-- User Roles
CREATE INDEX idx_user_roles_user ON user_roles(user_id);
CREATE INDEX idx_user_roles_role ON user_roles(role_id);
CREATE INDEX idx_user_roles_scope ON user_roles(data_scope, scope_value);
CREATE INDEX idx_user_roles_expires ON user_roles(expires_at) WHERE expires_at IS NOT NULL;
CREATE INDEX idx_user_roles_active ON user_roles(user_id, is_active) WHERE is_active = true;

-- Access Logs (time-series optimized)
CREATE INDEX idx_access_logs_timestamp ON access_logs(timestamp DESC);
CREATE INDEX idx_access_logs_user ON access_logs(user_id);
CREATE INDEX idx_access_logs_event ON access_logs(event_type);
CREATE INDEX idx_access_logs_result ON access_logs(result);
CREATE INDEX idx_access_logs_module ON access_logs(module);
CREATE INDEX idx_access_logs_user_event ON access_logs(user_id, event_type, timestamp DESC);
CREATE INDEX idx_access_logs_denied ON access_logs(result, timestamp DESC) WHERE result = 'denied';

-- Partition access_logs by month for performance
-- CREATE TABLE access_logs_2026_04 PARTITION OF access_logs
--     FOR VALUES FROM ('2026-04-01') TO ('2026-05-01');
```

### All Tables Summary

| Table Name | Purpose | Row Count Estimate | Key Relationships |
|------------|---------|-------------------|-------------------|
| `roles` | Role definitions | 45-100 | 1:N with role_permissions, user_roles |
| `permissions` | Granular action-resource definitions | 500-1,000 | M:N with roles via role_permissions |
| `role_permissions` | Role-to-permission mappings with grant/deny | 2,000-5,000 | FK to roles, permissions |
| `user_roles` | User-to-role assignments with scope and expiration | 5,000-10,000 | FK to users, roles |
| `access_logs` | Comprehensive audit trail of all auth events | 10M+ (partitioned) | FK to users, roles |
| `temporary_role_logs` | History of temporary role lifecycle events | 5,000-10,000 | FK to user_roles |

---

## 5. API Specifications

### Base URL
```
/api/v1/roles
/api/v1/permissions
/api/v1/access-logs
```

### Role Endpoints

#### GET /api/v1/roles
List all roles with optional filtering.

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| per_page | integer | No | Items per page (default: 25, max: 100) |
| category | string | No | Filter by category |
| is_active | boolean | No | Filter by active status |
| search | string | No | Search role name and description |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "a10e8400-e29b-41d4-a716-446655440001",
        "name": "HR Manager",
        "description": "Full human resources management access",
        "category": "hr",
        "is_system": true,
        "is_active": true,
        "assigned_users_count": 12,
        "permission_count": 145,
        "created_at": "2025-01-15T08:00:00Z",
        "updated_at": "2026-03-20T14:30:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 25,
      "total_items": 45,
      "total_pages": 2
    }
  }
}
```

#### POST /api/v1/roles
Create a new role.

**Request Body:**
```json
{
  "name": "Procurement Specialist",
  "description": "Manages purchase orders and supplier quotations",
  "category": "operations",
  "permissions": [
    {"permission_id": "p001", "effect": "grant"},
    {"permission_id": "p002", "effect": "grant"},
    {"permission_id": "p003", "effect": "deny"}
  ]
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "b20e8400-e29b-41d4-a716-446655440002",
    "name": "Procurement Specialist",
    "description": "Manages purchase orders and supplier quotations",
    "category": "operations",
    "is_system": false,
    "permission_count": 3,
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

#### GET /api/v1/roles/:id
Get a single role with full details.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "a10e8400-e29b-41d4-a716-446655440001",
    "name": "HR Manager",
    "description": "Full human resources management access",
    "category": "hr",
    "is_system": true,
    "is_active": true,
    "assigned_users_count": 12,
    "permissions": [
      {
        "id": "p001",
        "module": "employees",
        "resource": "profile",
        "action": "read",
        "description": "View employee profiles",
        "effect": "grant"
      },
      {
        "id": "p002",
        "module": "employees",
        "resource": "profile",
        "action": "update",
        "description": "Edit employee profiles",
        "effect": "grant"
      }
    ],
    "created_at": "2025-01-15T08:00:00Z",
    "updated_at": "2026-03-20T14:30:00Z"
  }
}
```

#### PUT /api/v1/roles/:id
Update a role's properties and permissions.

**Request Body:**
```json
{
  "name": "HR Manager (Updated)",
  "description": "Updated description",
  "permissions": [
    {"permission_id": "p001", "effect": "grant"},
    {"permission_id": "p004", "effect": "grant"}
  ]
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "a10e8400-e29b-41d4-a716-446655440001",
    "name": "HR Manager (Updated)",
    "updated_at": "2026-04-14T10:00:00Z"
  }
}
```

#### DELETE /api/v1/roles/:id
Delete a role (fails if users are assigned).

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Vai tro da duoc xoa"
}
```

#### GET /api/v1/roles/:id/users
List users assigned to a specific role.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "user_id": "u001",
        "username": "son.tt",
        "full_name": "Truong Thanh Son",
        "department": "Phong IT",
        "data_scope": "department",
        "scope_value": "dept-hr-001",
        "assigned_at": "2026-01-10T08:00:00Z",
        "expires_at": null
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 25,
      "total_items": 12,
      "total_pages": 1
    }
  }
}
```

### Permission Endpoints

#### GET /api/v1/permissions
List all registered permissions (for building permission trees).

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| module | string | No | Filter by module |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "p001",
        "module": "employees",
        "resource": "profile",
        "action": "read",
        "description": "View employee profiles"
      },
      {
        "id": "p002",
        "module": "employees",
        "resource": "profile",
        "action": "update",
        "description": "Edit employee profiles"
      }
    ]
  }
}
```

#### GET /api/v1/permissions/matrix
Get the full permission matrix (role x permission).

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "roles": [
      {"id": "r001", "name": "HR Manager"},
      {"id": "r002", "name": "Department Manager"}
    ],
    "permissions": [
      {"id": "p001", "module": "employees", "resource": "profile", "action": "read"},
      {"id": "p002", "module": "employees", "resource": "profile", "action": "update"}
    ],
    "matrix": {
      "r001": {"p001": "grant", "p002": "grant"},
      "r002": {"p001": "grant", "p002": "deny"}
    }
  }
}
```

### User Role Assignment Endpoints

#### GET /api/v1/users/:userId/roles
Get all role assignments for a user.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "ur001",
        "role_id": "r001",
        "role_name": "HR Manager",
        "data_scope": "global",
        "scope_value": null,
        "assigned_by": "admin",
        "assigned_at": "2026-01-10T08:00:00Z",
        "expires_at": null,
        "is_active": true
      },
      {
        "id": "ur002",
        "role_id": "r005",
        "role_name": "Acting IT Manager",
        "data_scope": "department",
        "scope_value": "dept-it-001",
        "assigned_by": "admin",
        "assigned_at": "2026-04-01T08:00:00Z",
        "expires_at": "2026-06-01T00:00:00Z",
        "is_active": true,
        "reason": "Manager on leave"
      }
    ]
  }
}
```

#### POST /api/v1/users/:userId/roles
Assign a role to a user.

**Request Body:**
```json
{
  "role_id": "r005",
  "data_scope": "department",
  "scope_value": "dept-it-001",
  "expires_at": "2026-06-01T00:00:00Z",
  "reason": "Manager on leave, covering responsibilities"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "ur002",
    "role_name": "Acting IT Manager",
    "assigned_at": "2026-04-14T11:00:00Z",
    "expires_at": "2026-06-01T00:00:00Z"
  }
}
```

#### DELETE /api/v1/users/:userId/roles/:userRoleId
Revoke a role assignment from a user.

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Da thu hoi vai tro"
}
```

#### GET /api/v1/users/:userId/permissions
Get the aggregated permission set for a user (across all roles).

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "user_id": "u001",
    "roles": ["HR Manager", "Acting IT Manager"],
    "permissions": [
      {
        "module": "employees",
        "resource": "profile",
        "action": "read",
        "effect": "grant",
        "source_role": "HR Manager"
      },
      {
        "module": "employees",
        "resource": "profile",
        "action": "update",
        "effect": "deny",
        "source_role": "Acting IT Manager"
      }
    ],
    "effective_scope": "global",
    "conflicts": [
      {
        "permission": "employees:profile:update",
        "granted_by": ["HR Manager"],
        "denied_by": ["Acting IT Manager"],
        "resolution": "deny_wins"
      }
    ]
  }
}
```

### Access Log Endpoints

#### GET /api/v1/access-logs
Query the access audit log.

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| per_page | integer | No | Items per page (default: 50, max: 500) |
| event_type | string | No | Filter by event type |
| user_id | UUID | No | Filter by user |
| module | string | No | Filter by module |
| result | string | No | Filter by result (allowed/denied) |
| from | timestamp | No | Start of date range |
| to | timestamp | No | End of date range (max 90 days from 'from') |
| search | string | No | Search IP, user agent, resource |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 12345678,
        "timestamp": "2026-04-14T08:30:15Z",
        "event_type": "permission_deny",
        "user_id": "u001",
        "username": "son.tt",
        "role_name": "Employee",
        "module": "accounting",
        "action": "read",
        "resource": "journal_entry:je-2026-0001",
        "result": "denied",
        "ip_address": "192.168.1.100",
        "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)",
        "location": "Ha Noi, Vietnam"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 50,
      "total_items": 45230,
      "total_pages": 905
    }
  }
}
```

#### POST /api/v1/access-logs/export
Export filtered access logs to CSV.

**Request Body:**
```json
{
  "event_type": "permission_deny",
  "from": "2026-04-01T00:00:00Z",
  "to": "2026-04-14T23:59:59Z",
  "format": "csv"
}
```

**Response (200 OK):** Returns CSV file download.

### Error Format

```json
{
  "success": false,
  "error": {
    "code": "FORBIDDEN",
    "message": "Ban khong co quyen thuc hien hanh dong nay",
    "details": {
      "required_permission": "roles:manage",
      "user_roles": ["Employee"],
      "attempted_action": "DELETE /api/v1/roles/r001"
    }
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
| `ROLE_HAS_USERS` | 409 | Cannot delete role with assigned users |
| `MAX_ASSIGNMENTS` | 409 | Role has reached maximum assignment limit |
| `CONFLICT` | 409 | Resource conflict |
| `RATE_LIMITED` | 429 | Too many requests |
| `INTERNAL_ERROR` | 500 | Server error |

### Endpoints Summary Table

| Method | Endpoint | Auth Required | Permission | Description |
|--------|----------|---------------|------------|-------------|
| GET | `/api/v1/roles` | Yes | roles:read | List all roles |
| POST | `/api/v1/roles` | Yes | roles:manage | Create a role |
| GET | `/api/v1/roles/:id` | Yes | roles:read | Get single role |
| PUT | `/api/v1/roles/:id` | Yes | roles:manage | Update a role |
| DELETE | `/api/v1/roles/:id` | Yes | roles:manage | Delete a role |
| GET | `/api/v1/roles/:id/users` | Yes | roles:read | List users with role |
| GET | `/api/v1/permissions` | Yes | permissions:read | List all permissions |
| GET | `/api/v1/permissions/matrix` | Yes | roles:manage | Get permission matrix |
| GET | `/api/v1/users/:userId/roles` | Yes | users:read | Get user's roles |
| POST | `/api/v1/users/:userId/roles` | Yes | users:manage | Assign role to user |
| DELETE | `/api/v1/users/:userId/roles/:id` | Yes | users:manage | Revoke role from user |
| GET | `/api/v1/users/:userId/permissions` | Yes | users:read | Get user's aggregated permissions |
| GET | `/api/v1/access-logs` | Yes | access_logs:read | Query access logs |
| POST | `/api/v1/access-logs/export` | Yes | access_logs:export | Export access logs |

---

## 6. Permissions Matrix

### Roles

| Role | Description | Scope | Assignment Limit |
|------|-------------|-------|-----------------|
| System Admin | Full platform administration | Global | Max 5 users |
| Security Officer | Security monitoring and incident response | Global (read-only + policy write) | Max 10 users |
| HR Manager | Full HR management | Global | Max 20 users |
| HR Staff | Employee record management | Department-scoped | Unlimited |
| Department Manager | Team management and approval | Own department + children | Unlimited |
| Employee (Self-Service) | Basic self-service access | Self only | All users |
| IT Support | Technical support and account recovery | Global (limited actions) | Max 15 users |
| Auditor | Compliance auditing and reporting | Global (read-only, masked sensitive) | Max 10 users |

### CRUD Permission Matrix

| Permission | System Admin | Security Officer | HR Manager | HR Staff | Dept Manager | Employee | IT Support | Auditor |
|------------|:-----------:|:----------------:|:----------:|:--------:|:------------:|:--------:|:----------:|:-------:|
| `roles:read` | Y | Y | Y | Y | Y | Y (own) | Y | Y |
| `roles:manage` | Y | N | Y | N | N | N | N | N |
| `permissions:read` | Y | Y | Y | Y | Y | Y (own) | Y | Y |
| `permissions:manage` | Y | N | N | N | N | N | N | N |
| `users:read` | Y | Y | Y | Y | Y (dept) | Y (self) | Y | Y (masked) |
| `users:manage` | Y | N | Y | Y (dept) | N | N | Y (password/sessions) | N |
| `user_roles:assign` | Y | N | Y | N | Y (dept) | N | N | N |
| `user_roles:revoke` | Y | Y | Y | N | Y (dept) | N | N | N |
| `access_logs:read` | Y | Y | N | N | N | N | N | Y |
| `access_logs:export` | Y | Y | N | N | N | N | N | Y |
| `data_scope:global` | Y | N | Y | N | N | N | N | N |
| `data_scope:department` | Y | Y | Y | Y | Y | N | Y | Y |
| `data_scope:self` | Y | Y | Y | Y | Y | Y | Y | Y |
| `security_policy:manage` | Y | Y | N | N | N | N | N | N |

### Row-Level Security Rules

| Rule | Description |
|------|-------------|
| **Self-Access** | Every user can always read their own role assignments and aggregated permissions. Cannot read other users' roles unless granted by a higher-scope role. |
| **Department Scope** | Department Managers and HR Staff can only see role assignments for users within their department hierarchy. Cross-department role visibility returns empty results. |
| **Global Scope** | System Admins, HR Managers, and Security Officers (read-only) have global visibility into all role assignments and access logs. |
| **IT Support Scope** | IT Support can see user identity and session data globally but cannot see role permission details, data scope values, or deny overrides (sensitive security configuration). |
| **Audit Masking** | Auditors see access logs with sensitive fields masked: password hashes, MFA secrets, and session tokens are never included. PII in log details (full name, phone) is partially redacted. |
| **Admin Override** | System Admins bypass all row-level restrictions. However, even System Admin actions on access logs are logged (meta-audit trail). |
| **System Role Protection** | System-defined roles (is_system=true) cannot be deleted or have their core permissions modified. Only System Admins can modify system roles, and changes are logged with mandatory reason field. |
| **Deny Override Enforcement** | When checking permissions, deny effects are evaluated AFTER grants. If ANY assigned role has a deny for a permission, the final result is DENIED regardless of other grants. |

---

## 7. State Machine

### Role Lifecycle

```
              +----------+
              |  Draft   |  (newly created, not yet assignable)
              +----+-----+
                   |
            activate
                   |
                   v
              +----------+    deactivate    +-------------+
              |  Active  |--------------->|  Inactive   |
              +----+-----+                +------+------+
                   |                             |
                   |                             | reactivate
                   | assign to users             |
                   v                             v
              +----------+                +-------------+
              | Assigned |                |  Inactive   |
              | (has users)|               | (no users) |
              +----------+                +-------------+
                   |
                   | delete (after reassigning users)
                   v
              +----------+
              | Deleted  |  (terminal state)
              +----------+
```

### Temporary Role Assignment Lifecycle

```
              +----------+
              | Assigned |  (temporary role with expiration)
              +----+-----+
                   |
                   | 24h before expiry: notify user
                   |
                   v
              +----------+
              | Expiring |  (within 24h of expiration)
              +----+-----+
                   |
            expires_at reached
                   |
                   v
              +----------+
              | Expired  |  (role automatically revoked)
              +----+-----+
                   |
                   | (log entry created)
                   v
              [Removed from user_roles with is_active=false]
```

### Transition Table

| From State | Event | To State | Trigger | Notes |
|------------|-------|----------|---------|-------|
| `Draft` | Admin activates | `Active` | System Admin action | Role becomes assignable |
| `Active` | Admin deactivates | `Inactive` | System Admin action | Role cannot be assigned to new users; existing assignments remain |
| `Inactive` | Admin reactivates | `Active` | System Admin action | Role becomes assignable again |
| `Active` | First user assigned | `Assigned` | System (automatic) | Metadata flag, no state change needed |
| `Assigned` | All users unassigned | `Active` | System (automatic) | Metadata flag |
| `Active`/`Assigned` | Admin deletes | `Deleted` | System Admin action | Requires zero assigned users or forced reassignment |
| `Deleted` | -- | -- | -- | Terminal state |
| `Assigned` (temporary) | 24h before expiry | `Expiring` | System (scheduled check) | User notified |
| `Expiring` (temporary) | Expiration reached | `Expired` | System (scheduled job) | Role revoked, log entry created |
| `Expired` (temporary) | -- | `Removed` | System (immediate) | is_active set to false in user_roles |

### Approval Workflow

Role and permission management requires approval only for high-impact changes:

| Operation | Approval Required | Approver | Notes |
|-----------|-------------------|----------|-------|
| Create standard role | No | -- | System Admin or HR Manager creates directly |
| Create System Admin role assignment | Yes | Existing System Admin (1 of) | Requires acknowledgment from at least 1 existing admin |
| Assign System Admin role to new user | Yes | Security Officer + 1 existing System Admin | Dual approval required |
| Modify system-defined role permissions | Yes | Security Officer | Changes logged with mandatory reason |
| Delete role with 10+ assigned users | Yes | HR Director | Prevents accidental mass access loss |
| Grant Global data scope to non-admin role | Yes | Security Officer | Elevated privilege requires security review |
| Bulk role changes (5+ users at once) | Yes | Department Manager or HR Manager | Confirmation required |
| Temporary role > 30 days | Yes | Security Officer | Extended temporary access requires review |

---

## 8. Reports & Analytics

### 8.1 Report: System Admin Dashboard Overview

**Description:** Platform-wide access and role analytics displayed on the system administration dashboard (image_60).

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Time period (daily) | Total registered users | None | Single value card |
| Time period (daily) | Active sessions | None | Single value card |
| Day (30-day window) | Daily active users | Module | Line chart with peak/average |
| Module | Active users per module | Date range | Table with sortable columns |
| Role | User count per role | Department | Donut chart |
| Day (30-day window) | Failed login attempts | User, IP | Time series bar chart |

### 8.2 Report: Role Distribution Analysis

**Description:** Analysis of how roles are distributed across the organization.

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Role | User count | Department, category | Horizontal bar chart |
| Department | Average roles per user | Role category | Bar chart |
| Category | Role count | None | Pie chart |
| Time period (monthly) | Role assignment changes | Role, department | Stacked bar chart |
| User | Role count per user | Department | Histogram |

### 8.3 Report: Access Security Audit

**Description:** Security-focused analysis of access patterns and anomalies.

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Day | Permission denial count | User, module, resource | Time series line chart |
| User | Denial count (top 20) | Time range | Horizontal bar chart |
| Module | Denial count by module | Time range | Treemap |
| IP address | Unique IPs with denials | Time range | Table with geo-map |
| Hour of day | Event distribution | Event type | Heatmap |
| User | Active sessions count | Role, device type | Table |
| Month | Role change events | Role, action type | Stacked bar chart |

### 8.4 Report: Temporary Role Tracking

**Description:** Monitoring report for temporary role assignments and their lifecycle.

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| User | Active temporary roles count | Department | Table |
| Week | Temporary role expirations | Role, department | Bar chart |
| Role | Average assignment duration | Department | Box plot |
| Month | Temporary role extensions | User, role | Table |

### Report Summary Table

| Report Name | Audience | Frequency | Export Formats |
|-------------|----------|-----------|----------------|
| System Admin Dashboard | System Admin, Security Officer | Real-time | N/A (dashboard) |
| Role Distribution Analysis | HR Manager, System Admin | Weekly | PDF, Excel |
| Access Security Audit | Security Officer, Auditor | Daily | PDF, CSV |
| Temporary Role Tracking | Security Officer, HR Manager | Daily | Excel |

---

## 9. Integration Points

### External APIs

| API | Provider | Purpose | Authentication | Rate Limit |
|-----|----------|---------|----------------|------------|
| LDAP/Active Directory | Corporate AD | User-directory sync, group-to-role mapping | Service account (LDAP bind) | N/A (internal) |
| SAML/OIDC Provider | Okta, Azure AD, Keycloak | Single Sign-On integration | OAuth2 client credentials | 1,000 requests/hour |
| SIEM System | Splunk, ELK, Datadog | Access log forwarding for centralized security monitoring | API key | 10,000 events/minute |
| Email Service | SendGrid, AWS SES | Role assignment notifications, expiration warnings | API key | 100 emails/minute |
| Slack/Microsoft Teams | Slack API, Graph API | Security alert notifications for denials and role changes | OAuth2 / Bot token | 50 messages/minute |

### Webhooks

| Webhook | Event | Payload | Subscribers |
|---------|-------|---------|-------------|
| `role.created` | New role created | `{role_id, name, category, created_by, created_at}` | M05 (Employee Mgmt), M19 (Workflow) |
| `role.deleted` | Role deleted | `{role_id, name, former_user_count}` | M01 (Auth), M05 (Employee Mgmt) |
| `role.permissions_changed` | Role permissions modified | `{role_id, added_permissions[], removed_permissions[], changed_by}` | All modules (cache invalidation) |
| `user.role_assigned` | Role assigned to user | `{user_id, role_id, data_scope, scope_value, expires_at, assigned_by}` | M01 (Auth session update), M05 (Employee Mgmt), M19 (Workflow) |
| `user.role_revoked` | Role revoked from user | `{user_id, role_id, revoked_by, reason}` | M01 (Auth session notify), M06 (Task reassignment) |
| `user.role_expired` | Temporary role expired | `{user_id, role_id, user_role_id}` | M01 (Auth), Email notification to user and manager |
| `access.permission_denied` | Permission denial event | `{user_id, module, action, resource, ip_address}` | Security Officer alert system |
| `access.anomaly_detected` | Suspicious access pattern | `{user_id, event_type, anomaly_score, details}` | Security Officer, SIEM |

### Events

| Event | Source | Consumers | Description |
|-------|--------|-----------|-------------|
| `RoleCreated` | M02 | All modules | New role available for assignment |
| `RolePermissionsChanged` | M02 | All modules | Permission cache invalidation for affected users |
| `UserRoleAssigned` | M02 | M01, M05, M19 | User gains new permissions; sessions updated |
| `UserRoleRevoked` | M02 | M01, M06 | User loses permissions; affected sessions notified |
| `UserRoleExpired` | M02 (scheduled job) | M01, M05 | Temporary role automatically revoked |
| `AccessPermissionDenied` | M02 | Security monitoring | Permission denial logged and potentially alerted |
| `BulkRoleChange` | M02 | M01, Audit system | Multiple role changes in single operation |

---

## 10. Technical Notes

### Performance
- Permission checks target P99 latency of < 5ms. All role-permission mappings are cached in Redis as a flattened permission set per user.
- Access log writes use an asynchronous queue (Kafka or RabbitMQ) to avoid blocking the main request path. Logs are batch-inserted into the database every 1 second.
- Role list API response targets < 100ms even with 100 roles and 5,000 role-permission mappings, achieved via pre-computed materialized view refreshed on role changes.
- Access log queries on 10M+ records use time-partitioned tables (monthly partitions) and cover indexes for sub-2-second response times.

### Caching
- Redis cache stores each user's aggregated permission set (key: `perms:{user_id}`, TTL: 15 minutes). Cache invalidates on any role assignment change, role permission change, or manual flush.
- Role definitions cached for 30 minutes (low-change data).
- Permission tree cached for 60 minutes (registered permissions change rarely).
- Access log summary statistics (dashboard cards) cached for 5 minutes.
- Cache stampede prevention: permission cache rebuild uses distributed lock so only one worker recomputes per user at a time.
- Permission cache includes a version number; when role permissions change, all affected user caches are invalidated via Redis pub/sub.

### File Storage
- Role export reports (Excel) generated on demand and stored temporarily (TTL: 1 hour) in object storage with presigned download URLs.
- No user-uploaded files are stored directly by this module.

### Localization
- Role names and descriptions support i18n: stored as JSONB `{vi: "...", en: "..."}` for multilingual display.
- Permission descriptions are defined in code and translated via i18n resource files.
- Access log UI labels translated; raw log data (module names, action codes) stored in English for consistency.
- Error messages for permission denials displayed in user's preferred language.

### Mobile
- All RBAC API endpoints are RESTful and mobile-compatible.
- Mobile apps receive a compact permission set in the login response (only module-level permissions, not granular resource-level) to minimize payload size.
- Role change notifications pushed to mobile via FCM/APNs when user's roles are modified during an active session.
- Offline mode: mobile apps cache the user's permission set locally (encrypted) for up to 15 minutes to support basic navigation without network.

### Security
- All permission checks performed server-side; client-side permission checks are for UI convenience only and never trusted.
- Access log entries are cryptographically chained (each entry includes hash of previous entry) to detect tampering.
- Role definitions for system roles (is_system=true) are version-controlled; changes trigger a CI/CD pipeline validation to ensure no critical permissions are removed.
- Session tokens include a role version hash; if role permissions change, the next API call with an old token triggers a soft refresh.
- Rate limiting on role management endpoints: 10 role creates/edits per minute per admin to prevent accidental bulk changes.
- Access log storage is encrypted at rest (AES-256) and in transit (TLS 1.3).
- Deny overrides are evaluated last to ensure they always win over grants, preventing accidental privilege escalation through role stacking.

### Data Retention
- Access logs: retained for 7 years minimum, partitioned by month. Old partitions archived to cold storage after 2 years.
- Temporary role logs: retained for 3 years.
- Deleted roles: soft-deleted (is_active=false), permanently purged after 1 year if no audit trail references them.
- Role change audit trail: retained for 7 years (same as access logs).

### Scheduled Jobs
- **Temporary Role Expiration Check:** Runs daily at 2:00 AM. Finds all user_roles where expires_at < NOW() and is_active=true. Sets is_active=false, logs to temporary_role_logs, notifies user and manager.
- **Inactive Employee Role Cleanup:** Runs hourly. Finds all users with status=deactivated in M01 and strips all roles.
- **Stale Session Cleanup:** Runs every 15 minutes. Removes sessions where expires_at < NOW().
- **Access Log Partition Rotation:** Runs monthly on the 1st. Creates new partition for the upcoming month, archives partition older than 2 years.
