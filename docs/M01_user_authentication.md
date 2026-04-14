# M01 - User & Authentication (Dang nhap va Xac thuc Nguoi dung)

**Category:** Foundation  
**Priority:** P0  
**Reference Images:** assets/image_02.png, assets/image_35.png, assets/image_37.png  
**Dependencies:** None  
**Generated:** April 14, 2026

---

## 1. Module Overview

The User & Authentication module is the foundational entry point for the entire ABN ERP platform. It manages the complete lifecycle of user identities from onboarding through active employment to offboarding. When a new employee joins the organization, the system automatically generates a welcome email (as shown in image_02) that includes activation links, available workflow assignments, and AI assistant configuration. This onboarding flow ensures every employee is provisioned with the correct access, departmental context, and platform tools from day one.

The module also serves as the authoritative source of employee master data used by every downstream module -- Payroll (M12), Task Management (M06), Workflow (M19), and all HR suite modules. Employee profiles contain six tabbed sections (image_35): Basic Info (employee ID, full name, gender, date of birth, place of birth, marital status, tax ID, ethnicity, religion), Contact Information, Work Details, Account Settings, Family Members, and Other Information. The HR dashboard (image_37) provides real-time workforce analytics: 1,253 total employees, 15 new hires (+50%), 11 probation completions (+22%), and 10 resignations (-33%), with departmental distribution across Hanoi (40%), Da Nang (20%), Ho Chi Minh City (40%), and Can Tho (5%).

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| System Administrator | Manages all user accounts, password resets, system-wide authentication settings | Full access to all user data and auth configuration |
| HR Manager | Views employee directory, manages onboarding/offboarding, monitors HR dashboard | Read/write employee profiles, read-only auth settings |
| Department Manager | Views team member profiles, initiates onboarding for new hires in their department | Read access to department-scoped employee data |
| Employee (Self-Service) | Views and edits own profile, updates contact info, views family records | Read/write own profile only, read-only reference data |
| IT Support | Handles password reset requests, email verification issues, session troubleshooting | Read-only user data, write access to password reset tokens |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M02 - Roles & Permissions | Users are assigned roles; authentication validates role-based access |
| M03 - Company & Organization | Users are linked to departments, companies, and locations |
| M05 - Employee Management | Employee records extend user accounts with HR-specific data |
| M19 - Workflow | Welcome email assigns initial workflows to new users |

### Key Business Rules

1. Every employee must have exactly one User account and one Employee record; the relationship is 1:1 and enforced at the database level.
2. Welcome emails are sent automatically within 5 minutes of account creation and expire after 24 hours if not activated.
3. Password must meet complexity requirements: minimum 8 characters, at least 1 uppercase, 1 lowercase, 1 digit, and 1 special character.
4. A user account is locked for 30 minutes after 5 consecutive failed login attempts.
5. Email verification is mandatory before a user can access any module beyond the onboarding wizard.
6. Multi-factor authentication (MFA) is optional but required for System Administrator and Finance-related roles.
7. Users belonging to inactive departments or terminated employment contracts are automatically deactivated within 24 hours.
8. An employee can hold multiple roles across different departments, but the primary department determines default data scope.

---

## 2. Screen Inventory

### 2.1 Screen: Welcome Email (Onboarding)

**Route:** `/email/welcome/:token` | **Purpose:** Display the automated welcome email sent to new employees with platform activation link and workflow assignments.

| UI Component | Description |
|--------------|-------------|
| Email Header | Company logo, greeting with employee full name (e.g., "Chao anh/chị Truong Thanh Son") |
| Welcome Message | Paragraph explaining the company is adopting MISA AMIS for daily operations |
| Workflow List | Ordered list of 6 assigned workflows: Tạm ứng, Thanh toán, Thanh toán tạm ứng, Quy trình mua hàng, Duyet báo giá hợp đồng tài liệu, Duyet đi muộn về sớm nghỉ phép |
| AI Assistant Info | Section listing available AI models: ChatGPT, Gemini, Grok, Claude via OneAI integration |
| AMIS Chat Notice | Text about internal communication via AMIS Chat |
| CTA Button | "Truy cap MISA AMIS" (Access MISA AMIS) -- blue button linking to activation page |
| Validity Notice | "Email kích hoạt có hiệu lực trong 24 giờ" (Activation email valid for 24 hours) |
| Support Link | Link to internal support/help desk |
| Auto-generated Footer | "Đây là email tự động, vui lòng không trả lời" (Auto-generated email, do not reply) |

**User Interactions:**
- Click CTA button to navigate to account activation page
- Click support link to open help desk
- Click individual workflow names to view workflow descriptions (tooltip)

**Data Displayed:**
- Employee full name
- List of assigned workflow names
- Available AI model names
- Activation token validity countdown
- Company name and logo

### 2.2 Screen: Employee Detail (6 Tabs)

**Route:** `/employees/:id` | **Purpose:** Display and edit comprehensive employee profile across six tabbed sections.

| UI Component | Description |
|--------------|-------------|
| Employee Header | Photo, employee ID, full name, current department, status badge (Active/Probation/Terminated) |
| Tab Navigation | 6 tabs: Basic Info, Contact, Work, Account, Family, Other |
| Basic Info Tab | Fields: Employee ID, Full name, Gender (radio), Date of birth, Place of birth, Marital status (dropdown), Personal tax ID, Ethnicity (dropdown), Religion (dropdown) |
| Contact Tab | Fields: Personal phone, Work phone, Personal email, Work email, Permanent address, Temporary address, Emergency contact name, Emergency contact phone |
| Work Tab | Fields: Department (dropdown), Position (dropdown), Hire date, Contract type (dropdown: Probation/Fixed-term/Indefinite/Seasonal), Manager (dropdown), Work location (dropdown) |
| Account Tab | Fields: Username, Email (verified badge), MFA status, Last login date, Account status, Password reset button |
| Family Tab | Table: Name, Relationship, DOB, Occupation, Phone; Add/Edit/Delete actions |
| Other Tab | Fields: Bank account number, Bank name, Social insurance number, Health insurance number, Union card number, Notes |
| Action Bar | Save, Cancel, Export PDF, Print buttons |

**User Interactions:**
- Switch between tabs without losing unsaved changes (client-side state)
- Add new family member records via modal dialog
- Upload employee photo (drag-and-drop or file picker)
- Trigger password reset from Account tab
- Export profile as PDF document
- Inline validation on all form fields on blur

**Data Displayed:**
- Complete employee master data
- Family member records (1:N relationship)
- Account security status
- Contract and department assignments
- Insurance and banking details

### 2.3 Screen: Employee List

**Route:** `/employees` | **Purpose:** Display searchable, filterable list of all employees with pagination.

| UI Component | Description |
|--------------|-------------|
| Page Header | Title "Nhan vien" (Employees), total count badge (e.g., "100 nhan vien") |
| Search Bar | Text input with placeholder "Tìm kiếm theo tên, mã nhân viên, phòng ban" |
| Filter Panel | Dropdowns: Department, Status, Contract type, Position; Date range picker for hire date |
| Action Buttons | "Thêm mới" (Add New), "Xuat Excel" (Export Excel), "Nhập khẩu" (Import) |
| Data Table | Columns: Checkbox, Employee ID, Full name, Department, Position, Status, Contract type, Hire date, Actions |
| Row Actions | View detail, Edit, Deactivate (context menu) |
| Pagination | Records per page selector (10/25/50/100), page navigation, total count |
| Bulk Actions | Bulk status change, bulk department reassignment (shown when rows selected) |

**User Interactions:**
- Type to search with debounce (300ms) across name, employee ID, department
- Apply multiple filters simultaneously (AND logic)
- Click row to navigate to employee detail
- Select multiple rows for bulk operations
- Export filtered results to Excel
- Sort by clicking column headers

**Data Displayed:**
- Employee ID, full name, department, position
- Status indicator (Active, Probation, Terminated, On Leave)
- Contract type badge
- Hire date
- Quick action icons per row

### 2.4 Screen: HR Dashboard

**Route:** `/hr/dashboard` | **Purpose:** Provide HR managers and leadership with real-time workforce analytics and key metrics.

| UI Component | Description |
|--------------|-------------|
| Summary Cards | 4 metric cards: Total employees (1,253), New hires (15, +50%), Probation completions (11, +22%), Resignations (10, -33%) |
| Department Chart | Horizontal bar chart: Hanoi 40%, Da Nang 20%, Ho Chi Minh City 40%, Can Tho 5% |
| Contract Type Chart | Donut chart: Probation 5%, Indefinite 40%, Fixed-term 34%, Seasonal 20% |
| Trend Chart | Line chart showing monthly employee count trend over past 12 months |
| Recent Activity | List: Latest hires, resignations, transfers, promotions with timestamps |
| Quick Actions | "Add Employee", "Run Report", "Export Data" buttons |

**User Interactions:**
- Click on department bar to filter employee list by that department
- Click donut slice to filter by contract type
- Hover on trend chart for detailed monthly data
- Click recent activity items to navigate to relevant employee profiles
- Click quick action buttons to initiate workflows

**Data Displayed:**
- Headcount summary with period-over-period comparison
- Departmental distribution percentages
- Contract type distribution
- Monthly hiring/attrition trend
- Recent personnel actions log

---

## 3. Feature Specifications

### 3.1 Feature: User Registration & Onboarding

**Description:** Automated process for creating new user accounts and sending welcome emails. When HR creates an employee record, the system provisions a user account, generates an activation token, and sends a welcome email containing assigned workflows and platform access instructions.

**User Stories:**
- As an HR Manager, I want to create a new employee record and have their user account automatically provisioned so that I do not need to manually create credentials.
- As a new employee, I want to receive a welcome email with an activation link so that I can set up my account and start using the platform.
- As a System Administrator, I want activation tokens to expire after 24 hours so that unused invitations cannot be exploited.

**Acceptance Criteria:**
- **Given** HR creates a new employee record with a valid email, **When** the record is saved, **Then** a User account is created with status "Pending Activation" and a welcome email is sent within 5 minutes.
- **Given** a user clicks the activation link, **When** the token is valid and not expired, **Then** they are prompted to set a password and complete the onboarding wizard.
- **Given** the activation token has expired, **When** a user clicks the link, **Then** they see an error message and a "Request new activation email" button.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Email | Not null, valid format | "Email là bắt buộc và phải đúng định dạng" |
| Unique | Email | Not existing in Users table | "Email đã tồn tại trong hệ thống" |
| Required | Full Name | Not null, 2-100 characters | "Họ tên là bắt buộc (2-100 ký tự)" |
| Required | Department | Not null, must exist | "Phòng ban là bắt buộc" |
| Format | Personal Tax ID | 10-13 digits, valid checksum | "Mã số thuế không hợp lệ" |
| Required | Hire Date | Not null, not in future | "Ngày nhận việc không được ở tương lai" |

**Edge Cases:**
- Employee record created without email (manual provisioning by IT -- system generates temporary password and requires IT to deliver credentials in person).
- Welcome email bounces (system retries 3 times over 2 hours, then flags account for IT follow-up).
- User clicks activation link on mobile device (responsive activation page with mobile-optimized form).
- Two employees created with same email within seconds (unique constraint prevents duplicate; second creation fails with clear error).

### 3.2 Feature: Login & Session Management

**Description:** Secure authentication flow supporting username/password login, MFA, session tracking, and automatic session timeout.

**User Stories:**
- As an employee, I want to log in with my username and password so that I can access the ERP platform.
- As a System Administrator, I want to see all active sessions per user so that I can detect unauthorized access.
- As a security-conscious user, I want to enable MFA so that my account is protected even if my password is compromised.

**Acceptance Criteria:**
- **Given** valid credentials, **When** a user submits the login form, **Then** they are authenticated and redirected to their dashboard with a new session created.
- **Given** 5 consecutive failed login attempts, **When** a user attempts to login again, **Then** the account is locked for 30 minutes and a lockout notification email is sent.
- **Given** MFA is enabled, **When** credentials are valid, **Then** the user is prompted for an OTP code before session creation.
- **Given** an active session, **When** 30 minutes of inactivity pass, **Then** the session expires and the user is redirected to login.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Username/Email | Not null | "Vui lòng nhập tên đăng nhập hoặc email" |
| Required | Password | Not null | "Vui lòng nhập mật khẩu" |
| Lockout | Failed attempts | <= 5 in 30 minutes | "Tài khoản bị khóa. Vui lòng thử lại sau 30 phút" |
| MFA | OTP code | 6 digits, matches TOTP | "Mã OTP không chính xác" |
| Session | Inactivity | < 30 minutes | "Phiên làm việc đã hết hạn. Vui lòng đăng nhập lại" |

**Edge Cases:**
- User logs in from two devices simultaneously (both sessions active; configurable limit per role).
- Password changed while session is active (all other sessions for that user are invalidated immediately).
- Login from new IP/country (system sends email notification to user with device and location info).
- Session hijacking attempt (IP changes mid-session; system invalidates session and requires re-authentication).

### 3.3 Feature: Password Reset

**Description:** Self-service password reset via email verification with secure token generation and expiration.

**User Stories:**
- As an employee who forgot my password, I want to reset it via email so that I can regain access to my account.
- As an IT Support staff, I want to initiate a password reset on behalf of a user so that I can help them recover access.
- As a security auditor, I want all password resets to be logged so that I can audit access patterns.

**Acceptance Criteria:**
- **Given** a valid email associated with an account, **When** the user submits the "Forgot Password" form, **Then** a reset email with a time-limited token is sent.
- **Given** a valid reset token, **When** the user submits a new password meeting complexity requirements, **Then** the password is updated and all existing sessions are invalidated.
- **Given** a reset token older than 1 hour, **When** the user tries to use it, **Then** they receive an error and must request a new reset link.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Email | Not null, valid format | "Vui lòng nhập email" |
| Exists | Email | Must exist in Users table | "Email không tồn tại trong hệ thống" |
| Complexity | New Password | Min 8 chars, upper, lower, digit, special | "Mật khẩu phải có ít nhất 8 ký tự, bao gồm chữ hoa, chữ thường, số và ký tự đặc biệt" |
| Match | Confirm Password | Must equal New Password | "Xác nhận mật khẩu không khớp" |
| History | New Password | Not in last 5 passwords | "Mật khẩu mới không được trùng với 5 mật khẩu gần nhất" |

**Edge Cases:**
- User requests multiple reset emails within minutes (only the latest token is valid; previous tokens are invalidated).
- Reset email lands in spam folder (email includes "not spam" instructions and alternative SMS reset option if phone is registered).
- IT-initiated reset for employee without email access (system generates temporary password displayed once to IT staff).

### 3.4 Feature: Employee Profile Management (6 Tabs)

**Description:** Comprehensive employee profile editing across six tabbed sections with inline validation, audit logging, and file attachments.

**User Stories:**
- As an HR Manager, I want to edit any field in an employee's profile so that I can keep records up to date.
- As an employee, I want to update my own contact information and emergency contacts so that HR has current details.
- As an auditor, I want to see the history of changes to employee profiles so that I can verify data integrity.

**Acceptance Criteria:**
- **Given** edit permissions, **When** a user modifies a profile field and clicks Save, **Then** the change is validated, persisted, and logged in the audit trail.
- **Given** an employee viewing their own profile, **When** they try to edit restricted fields (department, position, salary), **Then** those fields are read-only with a tooltip explaining they must contact HR.
- **Given** a family member record with invalid data, **When** the user tries to save, **Then** inline validation highlights the error and prevents save.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Required | Full Name | Not null, 2-100 chars | "Họ tên là bắt buộc" |
| Format | Date of Birth | Valid date, must be >= 15 years old | "Ngày sinh không hợp lệ (tuổi >= 15)" |
| Format | Tax ID | 10 or 13 digits | "Mã số thuế phải có 10 hoặc 13 chữ số" |
| Required | Emergency Contact Name | Not null if Contact Phone provided | "Tên liên hệ khẩn cấp là bắt buộc khi có số điện thoại" |
| Format | Bank Account | 8-16 digits | "Số tài khoản ngân hàng không hợp lệ" |
| Unique | Family Member | No duplicate name + relationship combo | "Thành viên gia đình đã tồn tại" |

**Edge Cases:**
- HR edits employee profile while employee is simultaneously editing their own profile (last-write-wins with conflict notification to both parties).
- Deleting a family member who is referenced in emergency contact (system prompts to reassign emergency contact first).
- Uploading a profile photo exceeding 5MB (system compresses to 1MB max and shows preview before save).

### 3.5 Feature: Employee Directory Search & Filter

**Description:** Full-text search and multi-criteria filtering of the employee directory with export capability.

**User Stories:**
- As an HR Manager, I want to search employees by name, ID, or department so that I can quickly find specific records.
- As a Department Manager, I want to filter employees by contract type and status so that I can review probation completions.
- As an HR Analyst, I want to export filtered results to Excel so that I can perform offline analysis.

**Acceptance Criteria:**
- **Given** search text, **When** the user types in the search bar, **Then** results are filtered in real-time with 300ms debounce across name, employee ID, and department fields.
- **Given** multiple active filters, **When** the user applies them, **Then** only employees matching ALL criteria are displayed (AND logic).
- **Given** filtered results, **When** the user clicks Export Excel, **Then** a .xlsx file is generated and downloaded with all matching records.

**Validation Rules:**

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| Min Length | Search Query | >= 2 characters | "Từ khóa tìm kiếm phải có ít nhất 2 ký tự" |
| Valid Range | Date Filter | From date <= To date | "Ngày bắt đầu phải trước ngày kết thúc" |
| Max Results | Export | <= 10,000 rows | "Không thể xuất quá 10,000 dòng. Vui lòng thu hẹp bộ lọc" |

**Edge Cases:**
- Search query returns zero results (system shows "No results found" with suggestion to broaden filters).
- Search with Vietnamese diacritics vs. non-diacritics (system normalizes: "Truong" matches "Trương").
- Export with 10,001 matching records (system shows error; user must refine filters).
- Concurrent search by many users (system uses read replicas; no performance impact).

---

## 4. Database Design

### ASCII ERD Diagram

```
+-----------+       1:N        +--------------+       1:N        +----------------+
|   User    |----------------->|   Employee   |----------------->|  FamilyMember  |
+-----------+                  +--------------+                  +----------------+
| id (PK)   |                  | id (PK)      |                  | id (PK)        |
| email     |                  | user_id (FK) |                  | employee_id(FK)|
| password  |                  | employee_code|                  | name           |
| mfa_secret|                  | full_name    |                  | relationship   |
| status    |                  | gender       |                  | date_of_birth  |
| created_at|                  | date_of_birth|                  | occupation     |
| updated_at|                  | place_of_birth|                 | phone          |
+-----------+                  | marital_status|                 +----------------+
      |                        | tax_id       |
      | 1:N                    | ethnicity    |
      |                        | religion     |
      |                        | department_id|
+--------------------+         | position     |
|   LoginSession     |         | hire_date    |
+--------------------+         | contract_type|
| id (PK)            |         | manager_id   |
| user_id (FK)       |         | work_location|
| token_hash         |         | status       |
| ip_address         |         | created_at   |
| user_agent         |         | updated_at   |
| expires_at         |         +--------------+
| created_at         |                |
+--------------------+                | 1:N
      |                               |
      |                        +--------------+
      |                        |  UserProfile |
      |                        +--------------+
      |                        | id (PK)      |
+---------------------+        | employee_id  |
| PasswordReset       |        | personal_phone|
+---------------------+        | work_phone   |
| id (PK)             |        | personal_email|
| user_id (FK)        |        | work_email   |
| token_hash          |        | permanent_addr|
| expires_at          |        | temporary_addr|
| used_at             |        | emergency_name|
| created_at          |        | emergency_phone|
+---------------------+        | bank_account  |
                               | bank_name    |
+---------------------+        | si_number    |
|EmailVerification    |        | hi_number    |
+---------------------+        | union_number |
| id (PK)             |        | notes        |
| user_id (FK)        |        +--------------+
| token_hash          |
| verified_at         |
| created_at          |
+---------------------+

+--------------------+
|   Department       |
+--------------------+
| id (PK)            |
| company_id (FK)    |
| name               |
| code               |
| parent_id (FK)     |
| manager_id (FK)    |
| location           |
+--------------------+
```

### Table Schemas

#### Table: `users`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique user identifier |
| email | VARCHAR(255) | UNIQUE, NOT NULL, INDEX | User email address, used for login |
| username | VARCHAR(100) | UNIQUE, NOT NULL, INDEX | Login username |
| password_hash | VARCHAR(255) | NOT NULL | Bcrypt hashed password |
| mfa_secret | VARCHAR(255) | NULL | TOTP secret for MFA (encrypted) |
| mfa_enabled | BOOLEAN | NOT NULL, DEFAULT false | Whether MFA is active |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending_activation', CHECK (status IN ('pending_activation', 'active', 'locked', 'deactivated', 'deleted')) | Account status |
| failed_login_attempts | INTEGER | NOT NULL, DEFAULT 0 | Consecutive failed login count |
| locked_until | TIMESTAMP | NULL | Account lockout expiration |
| last_login_at | TIMESTAMP | NULL | Last successful login timestamp |
| last_login_ip | INET | NULL | Last login IP address |
| password_changed_at | TIMESTAMP | NULL | Last password change timestamp |
| created_by | UUID | FK -> users.id, NULL | User who created this account |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### Table: `employees`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique employee identifier |
| user_id | UUID | FK -> users.id, UNIQUE, NOT NULL | Linked user account |
| employee_code | VARCHAR(20) | UNIQUE, NOT NULL, INDEX | Human-readable employee ID |
| full_name | VARCHAR(200) | NOT NULL, INDEX | Full legal name |
| preferred_name | VARCHAR(100) | NULL | Nickname or preferred name |
| gender | VARCHAR(10) | CHECK (gender IN ('male', 'female', 'other')) | Gender identity |
| date_of_birth | DATE | NOT NULL | Date of birth |
| place_of_birth | VARCHAR(200) | NOT NULL | Birthplace (city/province) |
| marital_status | VARCHAR(20) | CHECK (marital_status IN ('single', 'married', 'divorced', 'widowed')) | Marital status |
| personal_tax_id | VARCHAR(13) | UNIQUE, NULL, INDEX | Personal tax identification number |
| ethnicity | VARCHAR(50) | DEFAULT 'Kinh' | Ethnic group |
| religion | VARCHAR(50) | NULL | Religious affiliation |
| department_id | UUID | FK -> departments.id, NOT NULL, INDEX | Primary department |
| position_id | UUID | FK -> positions.id, NOT NULL | Job position |
| manager_id | UUID | FK -> employees.id, NULL | Direct manager |
| work_location_id | UUID | FK -> locations.id, NOT NULL | Primary work location |
| contract_type | VARCHAR(20) | CHECK (contract_type IN ('probation', 'fixed_term', 'indefinite', 'seasonal')) | Employment contract type |
| hire_date | DATE | NOT NULL | Official hire date |
| contract_start_date | DATE | NOT NULL | Current contract start date |
| contract_end_date | DATE | NULL | Contract expiration (NULL for indefinite) |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active', CHECK (status IN ('active', 'probation', 'on_leave', 'terminated', 'resigned')), INDEX | Employment status |
| resignation_date | DATE | NULL | Date of resignation (if applicable) |
| termination_reason | TEXT | NULL | Reason for leaving |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### Table: `user_profiles`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique profile identifier |
| employee_id | UUID | FK -> employees.id, UNIQUE, NOT NULL | Linked employee |
| personal_phone | VARCHAR(20) | NULL | Personal phone number |
| work_phone | VARCHAR(20) | NULL | Work phone extension |
| personal_email | VARCHAR(255) | NULL | Personal email (backup contact) |
| work_email | VARCHAR(255) | UNIQUE, NULL | Work email address |
| permanent_address | TEXT | NOT NULL | Legal permanent address |
| temporary_address | TEXT | NULL | Current temporary address |
| emergency_contact_name | VARCHAR(200) | NULL | Emergency contact full name |
| emergency_contact_phone | VARCHAR(20) | NULL | Emergency contact phone |
| emergency_contact_relationship | VARCHAR(50) | NULL | Relationship to emergency contact |
| bank_account_number | VARCHAR(16) | NULL | Bank account for payroll |
| bank_name | VARCHAR(100) | NULL | Bank name |
| bank_branch | VARCHAR(100) | NULL | Bank branch |
| social_insurance_number | VARCHAR(20) | NULL | Social insurance number |
| health_insurance_number | VARCHAR(20) | NULL | Health insurance number |
| union_card_number | VARCHAR(20) | NULL | Labor union card number |
| photo_url | VARCHAR(500) | NULL | Profile photo URL |
| notes | TEXT | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### Table: `login_sessions`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique session identifier |
| user_id | UUID | FK -> users.id, NOT NULL, INDEX | Associated user |
| token_hash | VARCHAR(255) | UNIQUE, NOT NULL, INDEX | Hashed session token |
| ip_address | INET | NOT NULL | Client IP at session start |
| user_agent | TEXT | NOT NULL | Browser/device user agent |
| device_type | VARCHAR(20) | CHECK (device_type IN ('desktop', 'mobile', 'tablet', 'api')) | Device category |
| location | VARCHAR(200) | NULL | Geo-approximated location |
| expires_at | TIMESTAMP | NOT NULL | Session expiration time |
| last_activity_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last request timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Session creation timestamp |

#### Table: `password_resets`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique reset request identifier |
| user_id | UUID | FK -> users.id, NOT NULL, INDEX | Associated user |
| token_hash | VARCHAR(255) | NOT NULL, INDEX | Hashed reset token |
| expires_at | TIMESTAMP | NOT NULL | Token expiration (1 hour from creation) |
| used_at | TIMESTAMP | NULL | When token was consumed |
| ip_address | INET | NULL | IP that requested reset |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Request creation timestamp |

#### Table: `email_verifications`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique verification identifier |
| user_id | UUID | FK -> users.id, UNIQUE, NOT NULL | Associated user |
| token_hash | VARCHAR(255) | NOT NULL | Hashed verification token |
| expires_at | TIMESTAMP | NOT NULL | Token expiration (24 hours) |
| verified_at | TIMESTAMP | NULL | When email was verified |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Request creation timestamp |

#### Table: `family_members`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique family member identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL, INDEX | Parent employee |
| full_name | VARCHAR(200) | NOT NULL | Family member full name |
| relationship | VARCHAR(50) | NOT NULL | Relationship (spouse, child, parent, sibling) |
| date_of_birth | DATE | NOT NULL | Date of birth |
| occupation | VARCHAR(100) | NULL | Occupation |
| phone | VARCHAR(20) | NULL | Contact phone |
| address | TEXT | NULL | Address |
| is_dependent | BOOLEAN | NOT NULL, DEFAULT false | Whether this person is a tax dependent |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### Table: `departments`

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL, DEFAULT gen_random_uuid() | Unique department identifier |
| company_id | UUID | FK -> companies.id, NOT NULL, INDEX | Parent company |
| name | VARCHAR(200) | NOT NULL | Department name |
| code | VARCHAR(20) | UNIQUE, NOT NULL | Department code |
| parent_id | UUID | FK -> departments.id, NULL | Parent department (for hierarchy) |
| manager_id | UUID | FK -> employees.id, NULL | Department head |
| location_id | UUID | FK -> locations.id, NULL | Primary location |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

### Indexes (SQL)

```sql
-- Users
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
CREATE INDEX idx_users_status ON users(status);
CREATE INDEX idx_users_created_at ON users(created_at);

-- Employees
CREATE INDEX idx_employees_user_id ON employees(user_id);
CREATE INDEX idx_employees_code ON employees(employee_code);
CREATE INDEX idx_employees_department ON employees(department_id);
CREATE INDEX idx_employees_status ON employees(status);
CREATE INDEX idx_employees_name ON employees(full_name);
CREATE INDEX idx_employees_manager ON employees(manager_id);

-- Login Sessions
CREATE INDEX idx_login_sessions_user_id ON login_sessions(user_id);
CREATE INDEX idx_login_sessions_token ON login_sessions(token_hash);
CREATE INDEX idx_login_sessions_expires ON login_sessions(expires_at);

-- Password Resets
CREATE INDEX idx_password_resets_user ON password_resets(user_id);
CREATE INDEX idx_password_resets_token ON password_resets(token_hash);
CREATE INDEX idx_password_resets_expires ON password_resets(expires_at);

-- Family Members
CREATE INDEX idx_family_members_employee ON family_members(employee_id);

-- Departments
CREATE INDEX idx_departments_company ON departments(company_id);
CREATE INDEX idx_departments_parent ON departments(parent_id);
```

### All Tables Summary

| Table Name | Purpose | Row Count Estimate | Key Relationships |
|------------|---------|-------------------|-------------------|
| `users` | Core authentication accounts | 2,800 | 1:1 with employees |
| `employees` | Employee master records | 1,253 | FK to users, departments, positions |
| `user_profiles` | Extended contact and personal info | 1,253 | 1:1 with employees |
| `login_sessions` | Active and historical login sessions | 5,000 (rolling) | FK to users |
| `password_resets` | Password reset token records | 500 (rolling) | FK to users |
| `email_verifications` | Email verification tokens | 2,800 | 1:1 with users |
| `family_members` | Employee family member dependents | 3,750 (avg 3/employee) | FK to employees |
| `departments` | Organizational department structure | 50-100 | FK to companies, self-referencing |

---

## 5. API Specifications

### Base URL
```
/api/v1/auth
/api/v1/employees
```

### Authentication Endpoints

#### POST /api/v1/auth/register
Create a new user account (HR/Admin only).

**Request Body:**
```json
{
  "email": "son.tt@company.com",
  "username": "son.tt",
  "full_name": "Truong Thanh Son",
  "department_id": "550e8400-e29b-41d4-a716-446655440000",
  "position_id": "660e8400-e29b-41d4-a716-446655440001",
  "hire_date": "2026-04-14",
  "contract_type": "probation",
  "date_of_birth": "1995-06-15",
  "place_of_birth": "Ha Noi",
  "gender": "male",
  "marital_status": "single",
  "send_welcome_email": true
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "user_id": "770e8400-e29b-41d4-a716-446655440002",
    "employee_id": "880e8400-e29b-41d4-a716-446655440003",
    "employee_code": "EMP-2026-0001",
    "status": "pending_activation",
    "welcome_email_sent": true
  }
}
```

#### POST /api/v1/auth/login
Authenticate a user and create a session.

**Request Body:**
```json
{
  "username": "son.tt",
  "password": "Str0ng!Pass",
  "mfa_code": "123456",
  "remember_me": false
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "dGhpcyBpcyBhIHJlZnJlc2ggdG9rZW4...",
    "token_type": "Bearer",
    "expires_in": 1800,
    "user": {
      "id": "770e8400-e29b-41d4-a716-446655440002",
      "email": "son.tt@company.com",
      "full_name": "Truong Thanh Son",
      "department": "Phong IT",
      "roles": ["employee"]
    }
  }
}
```

#### POST /api/v1/auth/logout
Invalidate the current session.

**Request Headers:** `Authorization: Bearer <token>`

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Dang xuat thanh cong"
}
```

#### POST /api/v1/auth/forgot-password
Request a password reset email.

**Request Body:**
```json
{
  "email": "son.tt@company.com"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Email dat lai mat khau da duoc gui. Vui long kiem tra hop thu."
}
```

#### POST /api/v1/auth/reset-password
Reset password using token.

**Request Body:**
```json
{
  "token": "reset-token-from-email",
  "new_password": "N3w!Str0ngPass",
  "confirm_password": "N3w!Str0ngPass"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Mat khau da duoc dat lai thanh cong"
}
```

#### POST /api/v1/auth/verify-email
Verify email using token from welcome email.

**Request Body:**
```json
{
  "token": "email-verification-token",
  "password": "MyF1rst!Pass",
  "confirm_password": "MyF1rst!Pass"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "user_id": "770e8400-e29b-41d4-a716-446655440002",
    "status": "active",
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

#### POST /api/v1/auth/refresh
Refresh an access token using a valid refresh token.

**Request Body:**
```json
{
  "refresh_token": "dGhpcyBpcyBhIHJlZnJlc2ggdG9rZW4..."
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "bmV3IHJlZnJlc2ggdG9rZW4...",
    "expires_in": 1800
  }
}
```

#### GET /api/v1/auth/me
Get current authenticated user's profile.

**Request Headers:** `Authorization: Bearer <token>`

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "770e8400-e29b-41d4-a716-446655440002",
    "email": "son.tt@company.com",
    "username": "son.tt",
    "full_name": "Truong Thanh Son",
    "department": "Phong IT",
    "position": "Software Engineer",
    "status": "active",
    "mfa_enabled": false,
    "last_login_at": "2026-04-14T08:30:00Z",
    "roles": ["employee"],
    "permissions": ["employee:read:self"]
  }
}
```

#### PUT /api/v1/auth/mfa/enable
Enable multi-factor authentication.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "qr_code": "data:image/png;base64,...",
    "secret": "JBSWY3DPEHPK3PXP",
    "backup_codes": ["12345678", "23456789", "34567890", "45678901", "56789012"]
  }
}
```

#### DELETE /api/v1/auth/sessions/:sessionId
Revoke a specific session (admin or self).

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Phien lam viec da duoc thu hoi"
}
```

### Employee Endpoints

#### GET /api/v1/employees
List employees with search, filter, and pagination.

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| per_page | integer | No | Items per page (default: 25, max: 100) |
| search | string | No | Search name, code, department |
| department_id | UUID | No | Filter by department |
| status | string | No | Filter by status |
| contract_type | string | No | Filter by contract type |
| sort_by | string | No | Sort field (default: created_at) |
| sort_order | string | No | asc or desc |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "880e8400-e29b-41d4-a716-446655440003",
        "employee_code": "EMP-2026-0001",
        "full_name": "Truong Thanh Son",
        "department": "Phong IT",
        "position": "Software Engineer",
        "status": "active",
        "contract_type": "probation",
        "hire_date": "2026-04-14",
        "gender": "male"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 25,
      "total_items": 1253,
      "total_pages": 51
    }
  }
}
```

#### GET /api/v1/employees/:id
Get a single employee's full profile.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "880e8400-e29b-41d4-a716-446655440003",
    "employee_code": "EMP-2026-0001",
    "user_id": "770e8400-e29b-41d4-a716-446655440002",
    "full_name": "Truong Thanh Son",
    "preferred_name": "Son",
    "gender": "male",
    "date_of_birth": "1995-06-15",
    "place_of_birth": "Ha Noi",
    "marital_status": "single",
    "personal_tax_id": "01234567890",
    "ethnicity": "Kinh",
    "religion": null,
    "department_id": "550e8400-e29b-41d4-a716-446655440000",
    "department_name": "Phong IT",
    "position": "Software Engineer",
    "manager_id": "990e8400-e29b-41d4-a716-446655440004",
    "manager_name": "Nguyen Van A",
    "work_location": "Ha Noi",
    "contract_type": "probation",
    "hire_date": "2026-04-14",
    "contract_start_date": "2026-04-14",
    "contract_end_date": "2026-07-14",
    "status": "active",
    "profile": {
      "personal_phone": "+84 90 123 4567",
      "work_phone": "Ext. 1001",
      "work_email": "son.tt@company.com",
      "permanent_address": "123 Nguyen Trai, Thanh Xuan, Ha Noi",
      "emergency_contact_name": "Truong Van B",
      "emergency_contact_phone": "+84 91 234 5678",
      "bank_account_number": "1234567890",
      "bank_name": "Vietcombank",
      "social_insurance_number": "SI-001234567"
    },
    "family_members": [
      {
        "id": "aa0e8400-e29b-41d4-a716-446655440005",
        "full_name": "Truong Thi C",
        "relationship": "spouse",
        "date_of_birth": "1996-03-20",
        "occupation": "Teacher",
        "phone": "+84 92 345 6789",
        "is_dependent": true
      }
    ],
    "created_at": "2026-04-14T08:00:00Z",
    "updated_at": "2026-04-14T08:00:00Z"
  }
}
```

#### PUT /api/v1/employees/:id
Update an employee's profile.

**Request Body:**
```json
{
  "full_name": "Truong Thanh Son",
  "personal_phone": "+84 90 123 4567",
  "temporary_address": "456 Tran Duy Hung, Cau Giay, Ha Noi",
  "emergency_contact_name": "Truong Van B",
  "emergency_contact_phone": "+84 91 234 5678"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "880e8400-e29b-41d4-a716-446655440003",
    "full_name": "Truong Thanh Son",
    "updated_at": "2026-04-14T10:30:00Z"
  }
}
```

#### DELETE /api/v1/employees/:id
Deactivate (soft delete) an employee.

**Request Body:**
```json
{
  "reason": "resignation",
  "effective_date": "2026-05-01"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Nhan vien da duoc vo hieu hoa"
}
```

#### POST /api/v1/employees/:id/family-members
Add a family member to an employee.

**Request Body:**
```json
{
  "full_name": "Truong Van D",
  "relationship": "child",
  "date_of_birth": "2020-01-15",
  "is_dependent": true
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "bb0e8400-e29b-41d4-a716-446655440006",
    "full_name": "Truong Van D",
    "relationship": "child",
    "date_of_birth": "2020-01-15",
    "is_dependent": true
  }
}
```

#### PUT /api/v1/employees/:id/family-members/:fmId
Update a family member record.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "bb0e8400-e29b-41d4-a716-446655440006",
    "full_name": "Truong Van D",
    "updated_at": "2026-04-14T11:00:00Z"
  }
}
```

#### DELETE /api/v1/employees/:id/family-members/:fmId
Remove a family member record.

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Da xoa thanh vien gia dinh"
}
```

### Error Format

All error responses follow this structure:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Du lieu khong hop le",
    "details": [
      {
        "field": "email",
        "message": "Email da ton tai trong he thong",
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
| `ACCOUNT_LOCKED` | 423 | Account is temporarily locked |
| `TOKEN_EXPIRED` | 410 | Token has expired |
| `TOKEN_INVALID` | 400 | Token is malformed or invalid |
| `CONFLICT` | 409 | Resource conflict (duplicate) |
| `RATE_LIMITED` | 429 | Too many requests |
| `INTERNAL_ERROR` | 500 | Server error |

### Endpoints Summary Table

| Method | Endpoint | Auth Required | Description |
|--------|----------|---------------|-------------|
| POST | `/api/v1/auth/register` | Admin | Create new user account |
| POST | `/api/v1/auth/login` | No | Authenticate and create session |
| POST | `/api/v1/auth/logout` | Yes | Invalidate session |
| POST | `/api/v1/auth/forgot-password` | No | Request password reset |
| POST | `/api/v1/auth/reset-password` | No | Reset password with token |
| POST | `/api/v1/auth/verify-email` | No | Verify email on activation |
| POST | `/api/v1/auth/refresh` | No | Refresh access token |
| GET | `/api/v1/auth/me` | Yes | Get current user profile |
| PUT | `/api/v1/auth/mfa/enable` | Yes | Enable MFA |
| DELETE | `/api/v1/auth/sessions/:id` | Yes | Revoke session |
| GET | `/api/v1/employees` | Yes | List employees |
| GET | `/api/v1/employees/:id` | Yes | Get single employee |
| PUT | `/api/v1/employees/:id` | Yes | Update employee |
| DELETE | `/api/v1/employees/:id` | HR/Admin | Deactivate employee |
| POST | `/api/v1/employees/:id/family-members` | Yes | Add family member |
| PUT | `/api/v1/employees/:id/family-members/:fmId` | Yes | Update family member |
| DELETE | `/api/v1/employees/:id/family-members/:fmId` | Yes | Delete family member |

---

## 6. Permissions Matrix

### Roles

| Role | Description | Scope |
|------|-------------|-------|
| System Admin | Full system administration | Global |
| HR Manager | Full HR management | Global |
| HR Staff | Employee record management | Department-scoped |
| Department Manager | View team profiles | Own department |
| Employee (Self-Service) | View and edit own profile | Self only |
| IT Support | Password resets, account unlocks | Global (read + specific actions) |
| Auditor | Read-only access for compliance | Global (read-only) |

### CRUD Permission Matrix

| Permission | System Admin | HR Manager | HR Staff | Dept Manager | Employee | IT Support | Auditor |
|------------|:-----------:|:----------:|:--------:|:------------:|:--------:|:----------:|:-------:|
| `users:create` | Y | Y | Y | N | N | N | N |
| `users:read` | Y | Y | Y | Y (dept) | Y (self) | Y | Y |
| `users:update` | Y | Y | Y | N | Y (self) | Y (password) | N |
| `users:delete` | Y | Y | N | N | N | N | N |
| `employees:create` | Y | Y | Y | Y (dept) | N | N | N |
| `employees:read` | Y | Y | Y | Y (dept) | Y (self) | Y | Y |
| `employees:update` | Y | Y | Y | N | Y (self, limited) | N | N |
| `employees:deactivate` | Y | Y | Y | N | N | N | N |
| `sessions:read` | Y | N | N | N | Y (self) | Y | N |
| `sessions:revoke` | Y | N | N | N | Y (self) | Y | N |
| `password_reset:initiate` | Y | Y | N | N | Y (self) | Y | N |
| `mfa:manage` | Y | N | N | N | Y (self) | Y | N |
| `family_members:crud` | Y | Y | Y | N | Y (self) | N | N |
| `dashboard:read` | Y | Y | Y | Y (dept) | N | N | Y |

### Row-Level Security Rules

| Rule | Description |
|------|-------------|
| **Self-Access** | Every authenticated user can always read and partially update their own profile (name, contact info, emergency contacts). Restricted fields (department, position, salary, contract) require HR role. |
| **Department Scope** | Department Managers can only read employee profiles within their assigned department hierarchy. Cross-department access returns 403. |
| **HR Global Scope** | HR Managers have global read/write. HR Staff have read global, write scoped to their assigned departments. |
| **IT Support Scope** | IT Support can read all user accounts and perform password resets, session revokes, and account unlocks. Cannot read sensitive HR fields (salary, tax ID, family data). |
| **Audit Read-Only** | Auditors have global read access but cannot see password hashes, MFA secrets, or active session tokens. |
| **Admin Override** | System Admins bypass all row-level restrictions and have full access to all data including sensitive fields. |
| **Soft-Deleted Users** | Deactivated user records are hidden from all roles except System Admin and Auditor. |

---

## 7. State Machine

### User Account Status Transitions

```
                        +------------------+
                        |  pending_activation |
                        +--------+---------+
                                 |
                          activate email
                                 |
                                 v
              +-----------+  lockout   +--------+
    create -->|  active   |<----------| locked |
              +-----+-----+   (30m)   +--------+
                    |                      ^
                    |                      |
          deactivate|                5 failed
                    |                  attempts
                    v
              +-----------+   reactivate  +-------------+
              |deactivated|------------->|    active    |
              +-----------+              +-------------+
                    |
                    | purge (after 90 days)
                    v
              +---------+
              | deleted |
              +---------+
```

### Transition Table

| From State | Event | To State | Trigger | Notes |
|------------|-------|----------|---------|-------|
| `pending_activation` | Email verified + password set | `active` | User action | First login after welcome email |
| `pending_activation` | Token expired (24h) | `pending_activation` | System | Token invalidated, can resend |
| `active` | 5 failed login attempts | `locked` | System | Auto-lock, email notification sent |
| `locked` | 30 minutes elapsed | `active` | System | Auto-unlock |
| `locked` | Admin manual unlock | `active` | IT Support/Admin | Immediate unlock |
| `active` | HR deactivates | `deactivated` | HR action | Employment terminated |
| `active` | Contract expired | `deactivated` | System | Scheduled job checks daily |
| `deactivated` | HR reactivates | `active` | HR action | Re-hired employee |
| `deactivated` | 90 days elapsed | `deleted` | System | Data purged per retention policy |
| `deleted` | -- | -- | -- | Terminal state, cannot transition |

### Approval Workflow

The User & Authentication module does not require multi-level approval for standard operations. However, the following operations trigger notifications that may require acknowledgment:

| Operation | Notified Parties | Approval Required |
|-----------|-----------------|-------------------|
| New employee creation | HR Manager, Department Manager, IT Support | No (HR Manager creates directly) |
| Account deactivation | Employee (email), HR Manager, IT Support | No (HR Manager action) |
| Admin password reset | User (email), IT Support Manager | No (IT Support action) |
| MFA enable/disable | User (email notification) | No (user self-service) |
| Bulk department deactivation | HR Director, IT Director | Yes (HR Director approval required for 5+ users) |

---

## 8. Reports & Analytics

### 8.1 Report: HR Dashboard Summary

**Description:** Real-time workforce overview displayed on the HR dashboard (image_37).

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Time period (monthly) | Total headcount | Department | Single value card with trend indicator |
| Time period (monthly) | New hires count | Department | Single value card with % change |
| Time period (monthly) | Probation completions | Department | Single value card with % change |
| Time period (monthly) | Resignations count | Department | Single value card with % change |
| Department | Employee count | None | Horizontal bar chart |
| Contract type | Employee count | Department | Donut chart |
| Month (12-month window) | Headcount trend | Department | Line chart |

### 8.2 Report: Employee Directory Report

**Description:** Exportable report of all employees with demographic and employment data.

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Department | Employee count by status | Contract type, hire date range | Stacked bar chart |
| Gender | Employee count | Department, status | Pie chart |
| Age group | Employee count | Department, gender | Histogram |
| Tenure (years) | Average tenure | Department, contract type | Box plot |
| Ethnicity | Employee count | Department | Bar chart |

### 8.3 Report: Onboarding Effectiveness

**Description:** Analysis of new employee onboarding completion rates and time-to-productivity.

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Month | Onboarding completion rate | Department, position | Line chart |
| Department | Avg days to email verification | Hire date range | Bar chart |
| Department | Avg days to first login | Hire date range | Bar chart |
| Position | Welcome email delivery rate | Time period | Table with percentages |

### 8.4 Report: Account Security Audit

**Description:** Security-focused report on authentication events and anomalies.

| Dimension | Metric | Filter | Chart Type |
|-----------|--------|--------|------------|
| Day | Failed login attempts count | User, department | Time series line chart |
| User | Active sessions count | Department, device type | Table |
| Month | Password reset count | Department | Bar chart |
| Month | MFA adoption rate | Department, role | Donut chart |
| IP range | Unique login locations | User, time range | Map visualization |

### Report Summary Table

| Report Name | Audience | Frequency | Export Formats |
|-------------|----------|-----------|----------------|
| HR Dashboard Summary | HR Manager, Leadership | Real-time | N/A (dashboard) |
| Employee Directory | HR, Department Managers | On-demand | Excel, PDF, CSV |
| Onboarding Effectiveness | HR Manager, HR Director | Monthly | PDF, Excel |
| Account Security Audit | IT Support, System Admin, Auditor | Weekly | PDF, CSV |

---

## 9. Integration Points

### External APIs

| API | Provider | Purpose | Authentication | Rate Limit |
|-----|----------|---------|----------------|------------|
| SMTP/Email Service | SendGrid or AWS SES | Welcome emails, password reset emails, account notifications | API key | 100 emails/minute |
| SMS Gateway | VNPT SMS or Twilio | MFA OTP codes, account lockout notifications | API key | 50 SMS/minute |
| LDAP/Active Directory | Corporate AD | Optional SSO integration, user sync | Service account | N/A |
| Geo-IP Service | MaxMind or ipinfo.io | Login location detection for security alerts | API key | 1,000 lookups/hour |

### Webhooks

| Webhook | Event | Payload | Subscribers |
|---------|-------|---------|-------------|
| `user.created` | New user account created | `{user_id, email, employee_code, department_id, created_at}` | M02 (Roles), M05 (Employee Mgmt), M19 (Workflow) |
| `user.activated` | Email verified, account activated | `{user_id, email, activated_at}` | M19 (Workflow - assign onboarding tasks) |
| `user.deactivated` | Account deactivated | `{user_id, email, reason, effective_date}` | M02 (Roles - revoke), M06 (Tasks - reassign), M19 (Workflow - cancel pending) |
| `user.password_changed` | Password updated | `{user_id, changed_at, ip_address}` | M02 (Access Log), M06 (Security alert) |
| `user.locked` | Account locked due to failed attempts | `{user_id, email, failed_attempts, locked_until}` | IT Support notification system |
| `user.session_created` | New login session | `{user_id, session_id, ip_address, device_type, location}` | M02 (Access Log) |

### Events

| Event | Source | Consumers | Description |
|-------|--------|-----------|-------------|
| `EmployeeCreated` | M01 | M02, M05, M12, M19 | Triggers role assignment, payroll enrollment, workflow provisioning |
| `EmployeeDeactivated` | M01 | M02, M05, M06, M12, M19 | Revokes all access, reassigns tasks, stops payroll, cancels workflows |
| `DepartmentChanged` | M01 | M02, M06 | Updates role scope and task assignments |
| `MFAEnabled` | M01 | M02 (Audit Log) | Security audit trail update |
| `PasswordResetRequested` | M01 | Email Service | Triggers password reset email delivery |

---

## 10. Technical Notes

### Performance
- Employee list search uses PostgreSQL full-text search with trigram indexes on `full_name` and `employee_code` for sub-100ms response times on 10,000+ records.
- Dashboard aggregate queries are pre-computed via materialized views refreshed every 5 minutes.
- Login endpoint targets P99 latency of < 200ms including password hash verification (bcrypt cost factor 12).
- Bulk employee export (10,000 rows) generates Excel file in < 10 seconds using streaming write.

### Caching
- Redis cache layer for employee profile data (TTL: 15 minutes). Cache invalidates on any profile update.
- Department list cached for 30 minutes (low-change data).
- Login session data cached in Redis with TTL matching session expiration (30 minutes, sliding).
- HR dashboard metrics cached for 5 minutes with automatic invalidation on employee status changes.
- Rate limiting stored in Redis: 10 login attempts per IP per minute, 5 password reset requests per email per hour.

### File Storage
- Employee photos stored in object storage (S3-compatible) with max 5MB upload, auto-compressed to 1MB JPEG.
- Welcome email attachments (company handbook PDF) stored separately with CDN distribution.
- All uploaded files scanned for malware before storage.
- File URLs are presigned with 1-hour expiration for security.

### Localization
- All user-facing strings stored as i18n keys with Vietnamese (vi-VN) as default and English (en-US) as secondary.
- Date formats: Vietnamese uses DD/MM/YYYY, ISO 8601 for API responses.
- Vietnamese name ordering: Family Name + Middle Name + Given Name (displayed as full_name field).
- Address fields support Vietnamese administrative divisions: Province/Tinh, District/Huyen, Ward/Xa, Street/Duong.
- Phone numbers stored in E.164 format (+84...) for international compatibility.

### Mobile
- All authentication endpoints support mobile clients via REST API.
- MFA TOTP codes compatible with Google Authenticator, Authy, and Microsoft Authenticator.
- Welcome email activation link opens responsive mobile web page for account setup.
- Session token refresh supports silent background refresh for mobile apps.
- Mobile-specific user agent detection enables device-appropriate session metadata.

### Security
- Passwords hashed with bcrypt (cost factor 12, minimum).
- All tokens (session, password reset, email verification) are hashed before storage (bcrypt).
- JWT access tokens are short-lived (30 minutes) with RSA-256 signing.
- Refresh tokens are rotated on each use (refresh token rotation) with family detection for reuse.
- TLS 1.3 required for all API endpoints.
- CORS restricted to known frontend origins.
- Audit log captures all authentication events (login, logout, password change, MFA change, session revoke).

### Data Retention
- Login sessions: retained for 90 days for audit purposes, then purged.
- Password reset tokens: purged immediately after use or expiration.
- Deactivated user accounts: soft-deleted, fully purged after 90 days per data retention policy.
- Email verification tokens: purged after use or 24-hour expiration.
- Audit logs: retained for 7 years for compliance.
