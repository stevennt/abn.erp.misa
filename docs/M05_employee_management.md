# M05 Employee Management Module Specification

## 1. Module Overview

### 1.1 Purpose

The Employee Management Module (M05) is the comprehensive human resource management system within the ABN ERP platform. It handles the complete employee lifecycle from onboarding through offboarding, including profile management, contract tracking, document management, family member records, work history, timesheet tracking, attendance monitoring, and HR reporting. The module supports a workforce of 100 employees across 6 tabbed interface sections, managing employee identification, personal details, employment contracts, attendance records, and timesheet data.

Key operational metrics: Timesheet tracking for CA SANG (morning shift 08:00-12:00) and CA CHIEU (afternoon shift 13:30-17:30). Employee attendance statistics show 27.5 days paid, 20 hours overtime, 2 late arrivals, and 1 leave day. The module handles 1,253 HR records with complete employee profiles including ID format D02-0067, name Luc Phong Vu, gender female, date of birth 04/12/1995, location Hanoi, marital status single.

### 1.2 Personas

| Persona | Role | Key Responsibilities | Primary Screens | Access Level |
|---------|------|---------------------|-----------------|--------------|
| HR Manager | Department leadership | Approve hires, manage policies, generate HR reports | Overview, Reports, Settings | Full |
| HR Specialist | Employee administration | Create/update profiles, manage documents, process contracts | Employee Profile, Contract, Document | Read/Write |
| Timekeeper | Attendance management | Monitor timesheets, record attendance, track overtime | Timesheet, AttendanceRecord | Read/Write |
| Payroll Officer | Compensation processing | Calculate pay based on attendance, contracts, overtime | Contract, Timesheet, AttendanceRecord | Read/Write |
| Department Manager | Team management | View team attendance, approve leave, assign shifts | Timesheet, Attendance | Read-only |
| Employee | Self-service | View own profile, submit leave requests, view timesheet | Self-Service Portal | Read (own data) |
| Finance Manager | Budget oversight | Headcount cost analysis, salary budget tracking | Reports, Contracts | Read-only |
| Director | Strategic HR | Workforce analytics, headcount trends, cost analysis | Reports, Dashboard | Read-only |

### 1.3 Dependencies

| Dependency | Type | Direction | Description |
|-----------|------|-----------|-------------|
| M04 Accounting | Downstream | Outbound | Payroll expenses flow to GL, salary payments |
| M06 Task Management | Peer | Bidirectional | Task assignments linked to employees |
| M07 Purchasing | Peer | Inbound | Purchase request approvals by role |
| M08 Sales | Peer | Inbound | Sales commission data by employee |
| M10 Asset Management | Peer | Bidirectional | Asset allocation to employees |
| M11 Contract Management | Peer | Inbound | Employment contract templates |
| External Social Insurance | External | Outbound | Social insurance registration and reporting |
| External Tax Authority | External | Outbound | PIT withholding reporting |

### 1.4 Business Rules

1. **Unique Employee ID**: Each employee receives a unique ID following the format D{dept_code}-{sequential_number} (e.g., D02-0067).
2. **Mandatory Fields**: Employee ID, full name, date of birth, gender, and at least one contact method are mandatory.
3. **Contract Requirement**: Every active employee must have at least one valid employment contract on file.
4. **Timesheet Accuracy**: Timesheet entries must be submitted within 3 days of the work date; late submissions require supervisor approval.
5. **Shift Definition**: CA SANG (morning shift) runs 08:00-12:00, CA CHIEU (afternoon shift) runs 13:30-17:30. Overtime requires prior approval.
6. **Overtime Limit**: Maximum 20 hours overtime per month per labor law compliance.
7. **Leave Accrual**: Annual leave accrues monthly at 1.25 days per month for standard entitlement.
8. **Late Arrival Tracking**: 3+ late arrivals in a month trigger HR review and documented warning.
9. **Document Expiry Alerts**: System generates alerts 30 days before ID card, passport, or certification expiry.
10. **Probation Period**: New employees have a 2-month probation period with simplified contract terms.
11. **Data Privacy**: Employee personal data (family members, contact info) visible only to HR and the employee.
12. **Offboarding Checklist**: Termination requires completion of offboarding checklist including asset return, access revocation, and final settlement.

---

## 2. Screen Inventory

### 2.1 Screen Routes and Purposes

| Route | Purpose | Personas | Priority |
|-------|---------|----------|----------|
| `/hr/overview` | HR dashboard with headcount, attendance summary, alerts | HR Manager, Director | Critical |
| `/hr/employees` | Employee list with search, filter, bulk actions | HR Specialist, HR Manager | Critical |
| `/hr/employees/{id}/profile` | Complete employee profile (6 tabs) | HR Specialist, Employee | Critical |
| `/hr/employees/{id}/contracts` | Employment contract history and active contract | HR Specialist, Payroll Officer | High |
| `/hr/employees/{id}/documents` | Document management (ID, certificates, etc.) | HR Specialist | High |
| `/hr/employees/{id}/work-history` | Employment history, promotions, transfers | HR Specialist, HR Manager | Medium |
| `/hr/employees/{id}/family` | Family member records for benefits | HR Specialist, Payroll Officer | Medium |
| `/hr/timesheet` | Timesheet entry and review by shift | Timekeeper, Department Manager | Critical |
| `/hr/attendance` | Attendance records, late/absent tracking | Timekeeper, HR Manager | High |
| `/hr/leave-management` | Leave request submission and approval | Employee, Department Manager | High |
| `/hr/overtime` | Overtime request and tracking | Timekeeper, Department Manager | Medium |
| `/hr/reports` | HR reports: headcount, turnover, attendance analysis | HR Manager, Director | High |
| `/hr/settings` | HR configuration: shifts, leave types, departments | HR Manager | Medium |

### 2.2 UI Components by Screen

#### /hr/overview (HR Dashboard)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Total Headcount KPI | Metric Card | Display total 100 employees | Employee count query |
| Active Employees KPI | Metric Card | Currently active employees | Employee WHERE status='active' |
| New Hires This Month | Metric Card | Employees joined current month | Employee WHERE hire_date this month |
| On Leave Today | Metric Card | Employees currently on leave | LeaveRequest WHERE active |
| Late Arrivals Today | Metric Card | Late arrivals count today | AttendanceRecord WHERE late=true |
| Overtime Hours (MTD) | Metric Card | Total overtime hours this month | Timesheet overtime sum |
| Headcount Trend Chart | Line Chart | Monthly headcount over 12 months | Historical employee count |
| Attendance Rate Chart | Bar Chart | Daily attendance rate by department | AttendanceRecord aggregation |
| Department Breakdown | Pie Chart | Employee distribution by department | Employee department grouping |
| Alert Panel | List | Document expiries, contract renewals, compliance alerts | HR alert system |
| Quick Actions Bar | Action Bar | New employee, bulk import, timesheet approval | N/A |

#### /hr/employees (Employee List)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Search Bar | Input | Search by ID, name, department | Employee search |
| Filter Panel | Multi-select | Filter by department, status, gender, position | Employee metadata |
| Employee Grid | Data Table | Employee list with key columns | Employee master |
| Bulk Actions | Dropdown | Mass update, export, email selected | Employee selection |
| Pagination | Pager | Navigate through employee list | Pagination controls |
| Export Button | Action Button | Export to Excel | Employee data |
| Import Button | Action Button | Bulk import from Excel | File upload |
| Add Employee Button | Action Button | Create new employee record | Form navigation |

#### /hr/employees/{id}/profile (Employee Profile - 6 Tabs)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Profile Header | Info Panel | Photo, name, ID, department, position | EmployeeProfile |
| Tab Navigation | Tab Bar | 6 tabs: Personal, Work, Contact, Family, Documents, Contracts | N/A |
| Personal Info Form | Form Section | Name, DOB, gender, marital status, nationality | EmployeeProfile |
| Work Info Form | Form Section | Department, position, hire date, manager, status | EmployeeProfile |
| Contact Info Grid | Data Table | Phone, email, address, emergency contact | ContactInfo |
| Work History Timeline | Timeline | Employment history, promotions, transfers | WorkHistory |
| Family Member Grid | Data Table | Family members, relationship, DOB, occupation | FamilyMember |
| Document List | Data Table | ID cards, certificates, licenses with expiry | Document |
| Contract Summary | Data Table | Contract history, current contract details | Contract |
| Timesheet Summary | Widget | Days paid 27.5, overtime 20h, late 2, leave 1 | Timesheet aggregation |

#### /hr/timesheet (Timesheet Management)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Shift Selector | Toggle | CA SANG (08:00-12:00) / CA CHIEU (13:30-17:30) | Shift configuration |
| Calendar View | Calendar | Monthly timesheet grid with daily entries | Timesheet |
| Employee Selector | Multi-select | Select employees for timesheet entry | Employee list |
| Daily Entry Form | Form | Date, shift, hours worked, overtime, notes | User input |
| Attendance Status | Badge | Present, absent, late, leave, holiday | AttendanceRecord |
| Weekly Summary | Summary Row | Week totals: regular hours, overtime, leave | Timesheet calculation |
| Approval Status | Badge | Pending, approved, rejected | Timesheet status |
| Submit Button | Action Button | Submit timesheet for approval | Timesheet |
| Approve Button | Action Button | Approve submitted timesheet | Department Manager |

### 2.3 User Interactions

1. **Employee Profile Creation**: HR Specialist clicks "Add Employee" -> fills personal info form -> system generates ID (D02-0067) -> assigns to department -> creates initial contract -> saves profile.
2. **Timesheet Entry**: Timekeeper selects shift (CA SANG) -> selects employees -> enters daily attendance (present/absent/late) -> records overtime hours -> submits for manager approval -> manager reviews and approves.
3. **Leave Request**: Employee selects dates -> selects leave type -> submits request -> department manager receives notification -> approves/rejects -> timesheet updated automatically.
4. **Document Upload**: HR Specialist selects employee -> navigates to Documents tab -> uploads ID card scan -> sets expiry date -> system sets reminder for 30 days before expiry.

### 2.4 Data Displayed

- Employee Profile: ID (D02-0067), Full Name (Luc Phong Vu), Gender (Female), DOB (04/12/1995), Location (Hanoi), Marital Status (Single), Department, Position, Hire Date, Status, Photo
- Timesheet Summary: Days Paid (27.5), Overtime Hours (20), Late Arrivals (2), Leave Days (1)
- Attendance Record: Date, Shift, Clock-in Time, Clock-out Time, Regular Hours, Overtime Hours, Status
- Contract: Contract Number, Type, Start Date, End Date, Salary, Probation End Date, Status

---

## 3. Feature Specifications

### 3.1 Employee Profile Management

**Description**: Comprehensive employee profile with 6-tab interface covering personal information, work details, contact information, family members, documents, and contracts.

**User Stories**:
- As an HR Specialist, I want to create and maintain complete employee profiles so that all employee information is centralized and accessible.
- As a Department Manager, I want to view my team members' profiles so that I have their contact and work information.
- As an Employee, I want to view my own profile and update my contact information so that my records stay current.

**Acceptance Criteria**:
1. Profile includes 6 tabs: Personal Information, Work Details, Contact Information, Family Members, Documents, Contracts.
2. Employee ID is auto-generated in format D{dept_code}-{sequential_number}.
3. Photo upload with validation (JPG/PNG, max 2MB).
4. All mandatory fields validated before save.
5. Change history tracked for sensitive fields (salary, position, department).

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-001 | Employee ID | Auto-generated, unique | "Employee ID already exists" |
| VR-002 | Full Name | Required, 2-100 chars | "Full name is required (2-100 characters)" |
| VR-003 | Date of Birth | Required, must be 18+ years | "Employee must be at least 18 years old" |
| VR-004 | Gender | Required | "Gender is required" |
| VR-005 | Email | Valid email format, unique if provided | "Invalid email format" |
| VR-006 | Phone | Valid Vietnamese phone format | "Invalid phone number format" |
| VR-007 | Department | Required for active employees | "Department is required" |
| VR-008 | Hire Date | Required, cannot be future date | "Hire date cannot be in the future" |
| VR-009 | ID Number | Unique, valid Vietnamese ID format | "ID number is invalid or duplicate" |

**Edge Cases**:
- Employee transfer between departments: History preserved, effective date tracked.
- Rehire after termination: New employee record with link to previous record.
- Name change (marriage): Previous name stored in history, current name updated.
- Multiple contracts: Only one active contract at a time; others marked as expired/superseded.

### 3.2 Timesheet Management

**Description**: Track daily attendance by shift (CA SANG 08:00-12:00, CA CHIEU 13:30-17:30), regular hours, overtime, late arrivals, and leave days.

**User Stories**:
- As a Timekeeper, I want to record daily attendance by shift so that payroll calculations are accurate.
- As a Department Manager, I want to approve my team's timesheets so that attendance records are verified.
- As a Payroll Officer, I want approved timesheet data so that salary calculations are based on verified attendance.

**Acceptance Criteria**:
1. Calendar view shows all days of the month with attendance status per employee.
2. Shift selection: CA SANG (08:00-12:00) or CA CHIEU (13:30-17:30).
3. Auto-calculate regular hours based on shift and attendance.
4. Overtime hours capped at 20 per month with warning at threshold.
5. Late arrival automatically flagged when clock-in exceeds shift start by > 5 minutes.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-010 | Date | Must be within current or prior month | "Cannot enter timesheet for closed period" |
| VR-011 | Hours Worked | 0-12 hours per day | "Hours worked must be between 0 and 12" |
| VR-012 | Overtime | Max 20 hours/month per employee | "Overtime limit of 20 hours/month exceeded" |
| VR-013 | Clock-in/Clock-out | Clock-out must be after clock-in | "Clock-out must be after clock-in" |
| VR-014 | Status | Cannot be both present and on leave | "Conflicting attendance status" |

**Edge Cases**:
- Holiday attendance: Auto-detect public holidays, apply holiday pay rules.
- Half-day attendance: Support half-day entries with pro-rated hours.
- Timesheet correction after approval: Requires HR Manager override with reason.
- Missing clock-out: System flags incomplete record for manual correction.

### 3.3 Contract Management

**Description**: Manage employment contracts including type, term, salary, probation period, and renewal tracking.

**User Stories**:
- As an HR Specialist, I want to create and track employment contracts so that terms are documented.
- As a Payroll Officer, I want to see the current contract salary so that payroll calculations are accurate.
- As an HR Manager, I want contract renewal alerts so that contracts are renewed before expiry.

**Acceptance Criteria**:
1. Contract types: Indefinite, Fixed-term (1 year, 2 years, 3 years), Probation, Seasonal.
2. Probation period: Maximum 2 months with 85% of full salary.
3. Renewal alert at 60, 30, and 7 days before expiry.
4. Salary field supports base salary, allowance breakdown.
5. Contract history preserved when new contract supersedes old one.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-020 | Start Date | Required, cannot be before hire date | "Contract start date cannot be before hire date" |
| VR-021 | End Date | Required for fixed-term, after start date | "End date must be after start date" |
| VR-022 | Salary | Must be >= minimum wage | "Salary below minimum wage" |
| VR-023 | Contract Type | Required | "Contract type is required" |
| VR-024 | Overlapping | No overlapping active contracts | "Employee already has an active contract" |

**Edge Cases**:
- Contract extension: Extend existing contract end date vs. creating new contract.
- Salary adjustment mid-contract: Create contract amendment record.
- Probation to permanent: Convert probation contract to indefinite/fixed-term.
- Early termination: Record termination reason, calculate severance if applicable.

### 3.4 Document Management

**Description**: Track employee documents including ID cards, passports, certifications, licenses with expiry monitoring.

**User Stories**:
- As an HR Specialist, I want to upload and track employee documents so that records are complete and accessible.
- As an HR Specialist, I want expiry alerts for documents so that employees renew before expiration.
- As an auditor, I want to verify that all required documents are on file so that compliance is maintained.

**Acceptance Criteria**:
1. Document types: National ID, Passport, Driver License, Degree Certificate, Professional Certificate, Health Certificate, Work Permit.
2. Document upload with file attachment (PDF, JPG, PNG, max 5MB per file).
3. Expiry date tracking with alerts at 60, 30, 7 days before expiry.
4. Document verification status: Pending, Verified, Rejected.
5. Document completeness report by employee and department.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-030 | Document Type | Required | "Document type is required" |
| VR-031 | Document Number | Required, unique per type per employee | "Document number already recorded" |
| VR-032 | Expiry Date | Required for expiring documents | "Expiry date is required" |
| VR-033 | File Size | Max 5MB per file | "File size exceeds 5MB limit" |
| VR-034 | File Type | PDF, JPG, PNG only | "File type not supported" |

**Edge Cases**:
- Document replacement: Old document marked as superseded, new one active.
- Lost document: Flag as lost, track reissuance status.
- Foreign employee work permit: Special expiry tracking with immigration compliance.

### 3.5 Attendance Record Management

**Description**: Comprehensive attendance tracking with clock-in/clock-out, late arrival detection, absence tracking, and attendance reports.

**User Stories**:
- As a Timekeeper, I want to record clock-in and clock-out times so that attendance is accurately tracked.
- As a Department Manager, I want to see real-time attendance status so that I know who is present.
- As an HR Manager, I want attendance trend reports so that I can identify patterns.

**Acceptance Criteria**:
1. Clock-in/clock-out recording with timestamp.
2. Late arrival detection: > 5 minutes after shift start.
3. Absence tracking: Unauthorized absence flagged after 1 day.
4. Attendance summary per employee: present days, late count, absence count, leave days.
5. Department-level attendance rate calculation.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-040 | Clock-in Time | Required for present status | "Clock-in time required for present status" |
| VR-041 | Clock-out Time | Required, must be after clock-in | "Clock-out must be after clock-in" |
| VR-042 | Date | Cannot be future date | "Cannot record attendance for future date" |
| VR-043 | Employee | Must be active employee | "Employee is not active" |

**Edge Cases**:
- Missing clock-out: End of day auto-close with estimated hours.
- Multiple clock-ins: Use earliest clock-in, latest clock-out.
- Attendance on leave day: Override with leave status if leave approved.
- Biometric integration: Support for fingerprint/face recognition devices.

### 3.6 Family Member Management

**Description**: Track employee family members for insurance, tax deduction, and benefit purposes.

**User Stories**:
- As an HR Specialist, I want to record employee family members so that insurance and tax benefits are correctly applied.
- As a Payroll Officer, I want family member information so that dependent tax deductions are calculated.
- As an Employee, I want to update my family information so that my benefits are accurate.

**Acceptance Criteria**:
1. Family member types: Spouse, Child, Parent, Dependent.
2. Record: Name, relationship, DOB, ID number, occupation, dependent status.
3. Child records flag for education benefit eligibility.
4. Dependent status for tax deduction calculation.
5. Maximum 10 family members per employee.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-050 | Relationship | Required, valid type | "Valid relationship is required" |
| VR-051 | Name | Required, 2-100 chars | "Name is required" |
| VR-052 | DOB | Required | "Date of birth is required" |
| VR-053 | Dependent | Boolean, affects tax calculation | "Dependent status must be specified" |
| VR-054 | Max Count | Max 10 family members | "Maximum 10 family members allowed" |

**Edge Cases**:
- New child birth: Add new dependent, update tax deduction.
- Child reaches age limit: Remove dependent status, update benefits.
- Divorce: Update spouse status, adjust benefits.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+-------------------+       +--------------------+       +-------------------+
|    Employee       |       |  EmployeeProfile   |       |   ContactInfo     |
+-------------------+       +--------------------+       +-------------------+
| PK employee_id    |<----->| PK profile_id      |       | PK contact_id     |
| employee_code     |       | FK employee_id     |       | FK employee_id    |
| full_name         |       | date_of_birth      |       | contact_type      |
| gender            |       | marital_status     |       | contact_value     |
| status            |       | nationality        |       | is_primary        |
| hire_date         |       | id_number          |       | verified          |
| FK department_id  |       | id_issue_date      |       | created_at        |
| FK position_id    |       | id_issue_place     |       +-------------------+
| FK manager_id     |       | photo_url          |
+-------------------+       | created_at         |       +-------------------+
         |                  +--------------------+       |   WorkHistory     |
         |                                              +-------------------+
         |                       +-------------------+  | PK history_id     |
         |                       |    FamilyMember   |  | FK employee_id    |
         |                       +-------------------+  | change_type       |
         |                       | PK family_id      |  | change_date       |
         +--------------------->| FK employee_id    |  | old_value         |
                                | relationship      |  | new_value         |
                                | full_name         |  | FK changed_by     |
                                | date_of_birth     |  | notes             |
                                | id_number         |  +-------------------+
                                | is_dependent      |
                                +-------------------+

+-------------------+       +--------------------+       +-------------------+
|    Contract       |       |    Document        |       |    Timesheet      |
+-------------------+       +--------------------+       +-------------------+
| PK contract_id    |       | PK document_id     |       | PK timesheet_id   |
| FK employee_id    |       | FK employee_id     |       | FK employee_id    |
| contract_number   |       | document_type      |       | work_date         |
| contract_type     |       | document_number    |       | shift_type        |
| start_date        |       | issue_date         |       | clock_in          |
| end_date          |       | expiry_date        |       | clock_out         |
| salary            |       | file_url           |       | regular_hours     |
| probation_end     |       | verification_status|       | overtime_hours    |
| status            |       | notes              |       | status            |
| FK created_by     |       | created_at         |       | notes             |
+-------------------+       +--------------------+       | approved_by       |
                                |                       +-------------------+
                                v                                |
                       +-------------------+                     v
                       | AttendanceRecord  |              +-------------------+
                       +-------------------+              | AttendanceRecord  |
                       | PK attendance_id  |              +-------------------+
                       | FK employee_id    |<-------------| FK timesheet_id   |
                       | attendance_date   |              | status            |
                       | shift_type        |              | late_flag         |
                       | clock_in          |              | absence_flag      |
                       | clock_out         |              +-------------------+
                       | regular_hours     |
                       | overtime_hours    |
                       | status            |
                       +-------------------+
```

### 4.2 Table Schemas

#### Employee

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| employee_id | BIGINT | PK, SERIAL | Unique employee identifier |
| employee_code | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated code (e.g., D02-0067) |
| full_name | NVARCHAR(100) | NOT NULL | Full Vietnamese name |
| gender | VARCHAR(10) | NOT NULL, CHECK | Male, Female, Other |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active, inactive, terminated, probation, leave |
| hire_date | DATE | NOT NULL | Date of hire |
| termination_date | DATE | NULL | Date of termination (if applicable) |
| FK department_id | BIGINT | NOT NULL, FK REFERENCES Department | Department assignment |
| FK position_id | BIGINT | NOT NULL, FK REFERENCES Position | Current position |
| FK manager_id | BIGINT | NULL, FK REFERENCES Employee | Direct manager |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |
| created_by | BIGINT | FK REFERENCES User | User who created record |

#### EmployeeProfile

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| profile_id | BIGINT | PK, SERIAL | Unique profile ID |
| FK employee_id | BIGINT | NOT NULL, UNIQUE, FK REFERENCES Employee | Employee reference |
| date_of_birth | DATE | NOT NULL | Date of birth |
| place_of_birth | NVARCHAR(200) | NULL | Place of birth |
| marital_status | VARCHAR(20) | NOT NULL | Single, Married, Divorced, Widowed |
| nationality | VARCHAR(50) | NOT NULL, DEFAULT 'Vietnamese' | Nationality |
| ethnicity | NVARCHAR(50) | NULL | Ethnic group |
| religion | NVARCHAR(50) | NULL | Religion |
| id_number | VARCHAR(20) | NOT NULL, UNIQUE | National ID number |
| id_issue_date | DATE | NOT NULL | ID card issue date |
| id_issue_place | NVARCHAR(100) | NOT NULL | ID card issue location |
| passport_number | VARCHAR(20) | NULL | Passport number (if any) |
| passport_expiry | DATE | NULL | Passport expiry date |
| photo_url | VARCHAR(255) | NULL | Employee photo URL |
| tax_code | VARCHAR(20) | NULL | Personal tax code |
| social_insurance_number | VARCHAR(20) | NULL | Social insurance number |
| health_insurance_number | VARCHAR(20) | NULL | Health insurance number |
| bank_account_number | VARCHAR(30) | NULL | Salary bank account |
| bank_name | NVARCHAR(100) | NULL | Bank name |

#### ContactInfo

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| contact_id | BIGINT | PK, SERIAL | Unique contact ID |
| FK employee_id | BIGINT | NOT NULL, FK REFERENCES Employee | Employee reference |
| contact_type | VARCHAR(20) | NOT NULL | phone, email, address, emergency |
| contact_value | VARCHAR(255) | NOT NULL | Contact value |
| is_primary | BOOLEAN | NOT NULL, DEFAULT FALSE | Primary contact flag |
| verified | BOOLEAN | NOT NULL, DEFAULT FALSE | Verification status |
| notes | NVARCHAR(200) | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### WorkHistory

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| history_id | BIGINT | PK, SERIAL | Unique history ID |
| FK employee_id | BIGINT | NOT NULL, FK REFERENCES Employee | Employee reference |
| change_type | VARCHAR(30) | NOT NULL | hire, promotion, transfer, salary_change, termination |
| change_date | DATE | NOT NULL | Effective date of change |
| old_value | JSONB | NULL | Previous values |
| new_value | JSONB | NOT NULL | New values after change |
| FK changed_by | BIGINT | FK REFERENCES User | User who made change |
| reason | NVARCHAR(300) | NULL | Reason for change |
| notes | NVARCHAR(500) | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### FamilyMember

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| family_id | BIGINT | PK, SERIAL | Unique family member ID |
| FK employee_id | BIGINT | NOT NULL, FK REFERENCES Employee | Employee reference |
| relationship | VARCHAR(30) | NOT NULL | spouse, child, parent, dependent |
| full_name | NVARCHAR(100) | NOT NULL | Family member name |
| date_of_birth | DATE | NOT NULL | Date of birth |
| gender | VARCHAR(10) | NOT NULL | Male, Female |
| id_number | VARCHAR(20) | NULL | ID number (if applicable) |
| occupation | NVARCHAR(100) | NULL | Occupation |
| is_dependent | BOOLEAN | NOT NULL, DEFAULT FALSE | Tax dependent flag |
| notes | NVARCHAR(200) | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### Contract

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| contract_id | BIGINT | PK, SERIAL | Unique contract ID |
| FK employee_id | BIGINT | NOT NULL, FK REFERENCES Employee | Employee reference |
| contract_number | VARCHAR(30) | NOT NULL, UNIQUE | Contract number |
| contract_type | VARCHAR(30) | NOT NULL | indefinite, fixed_term, probation, seasonal |
| start_date | DATE | NOT NULL | Contract start date |
| end_date | DATE | NULL | Contract end date (NULL for indefinite) |
| base_salary | DECIMAL(12,2) | NOT NULL | Base monthly salary |
| allowance | DECIMAL(12,2) | DEFAULT 0 | Monthly allowance |
| probation_end | DATE | NULL | Probation period end date |
| working_hours_per_day | DECIMAL(4,2) | DEFAULT 8.0 | Standard working hours |
| working_days_per_week | INT | DEFAULT 6 | Standard working days |
| leave_days_per_year | INT | DEFAULT 12 | Annual leave entitlement |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | draft, active, expired, terminated |
| terms | TEXT | NULL | Contract terms and conditions |
| FK created_by | BIGINT | FK REFERENCES User | User who created |
| signed_date | DATE | NULL | Date contract was signed |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### Document

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| document_id | BIGINT | PK, SERIAL | Unique document ID |
| FK employee_id | BIGINT | NOT NULL, FK REFERENCES Employee | Employee reference |
| document_type | VARCHAR(30) | NOT NULL | id_card, passport, degree, certificate, license, health_cert, work_permit |
| document_number | VARCHAR(30) | NOT NULL | Document number |
| issue_date | DATE | NOT NULL | Document issue date |
| expiry_date | DATE | NULL | Document expiry date |
| issue_place | NVARCHAR(200) | NULL | Document issue location |
| file_url | VARCHAR(255) | NULL | Uploaded file URL |
| verification_status | VARCHAR(20) | DEFAULT 'pending' | pending, verified, rejected |
| notes | NVARCHAR(300) | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### Timesheet

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| timesheet_id | BIGINT | PK, SERIAL | Unique timesheet ID |
| FK employee_id | BIGINT | NOT NULL, FK REFERENCES Employee | Employee reference |
| work_date | DATE | NOT NULL | Date of work |
| shift_type | VARCHAR(20) | NOT NULL | ca_sang (08:00-12:00), ca_chieu (13:30-17:30) |
| clock_in | TIME | NULL | Clock-in time |
| clock_out | TIME | NULL | Clock-out time |
| regular_hours | DECIMAL(4,2) | NOT NULL, DEFAULT 0 | Regular hours worked |
| overtime_hours | DECIMAL(4,2) | NOT NULL, DEFAULT 0 | Overtime hours |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'present' | present, absent, late, leave, holiday, sick |
| notes | NVARCHAR(300) | NULL | Additional notes |
| is_approved | BOOLEAN | NOT NULL, DEFAULT FALSE | Approval status |
| FK approved_by | BIGINT | NULL, FK REFERENCES User | Approver |
| approved_at | TIMESTAMP | NULL | Approval timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### AttendanceRecord

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| attendance_id | BIGINT | PK, SERIAL | Unique attendance ID |
| FK employee_id | BIGINT | NOT NULL, FK REFERENCES Employee | Employee reference |
| attendance_date | DATE | NOT NULL | Attendance date |
| shift_type | VARCHAR(20) | NOT NULL | ca_sang, ca_chieu |
| clock_in | TIME | NULL | Actual clock-in time |
| clock_out | TIME | NULL | Actual clock-out time |
| regular_hours | DECIMAL(4,2) | DEFAULT 0 | Regular hours |
| overtime_hours | DECIMAL(4,2) | DEFAULT 0 | Overtime hours |
| is_late | BOOLEAN | DEFAULT FALSE | Late arrival flag |
| late_minutes | INT | DEFAULT 0 | Minutes late |
| is_absent | BOOLEAN | DEFAULT FALSE | Absence flag |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'present' | present, late, absent, leave, holiday, sick |
| notes | NVARCHAR(200) | NULL | Additional notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

### 4.3 Indexes

```sql
-- Employee indexes
CREATE INDEX idx_employee_code ON Employee(employee_code);
CREATE INDEX idx_employee_department ON Employee(department_id);
CREATE INDEX idx_employee_status ON Employee(status);
CREATE INDEX idx_employee_manager ON Employee(manager_id);
CREATE INDEX idx_employee_hire_date ON Employee(hire_date);

-- EmployeeProfile indexes
CREATE INDEX idx_profile_dob ON EmployeeProfile(date_of_birth);
CREATE INDEX idx_profile_id_number ON EmployeeProfile(id_number);
CREATE INDEX idx_profile_tax_code ON EmployeeProfile(tax_code);
CREATE UNIQUE INDEX idx_profile_employee ON EmployeeProfile(employee_id);

-- ContactInfo indexes
CREATE INDEX idx_contact_employee ON ContactInfo(employee_id);
CREATE INDEX idx_contact_type ON ContactInfo(contact_type);
CREATE INDEX idx_contact_primary ON ContactInfo(employee_id, is_primary) WHERE is_primary = TRUE;

-- WorkHistory indexes
CREATE INDEX idx_history_employee ON WorkHistory(employee_id);
CREATE INDEX idx_history_date ON WorkHistory(change_date);
CREATE INDEX idx_history_type ON WorkHistory(change_type);

-- FamilyMember indexes
CREATE INDEX idx_family_employee ON FamilyMember(employee_id);
CREATE INDEX idx_family_dependent ON FamilyMember(employee_id) WHERE is_dependent = TRUE;

-- Contract indexes
CREATE INDEX idx_contract_employee ON Contract(employee_id);
CREATE INDEX idx_contract_number ON Contract(contract_number);
CREATE INDEX idx_contract_status ON Contract(status);
CREATE INDEX idx_contract_dates ON Contract(start_date, end_date);
CREATE INDEX idx_contract_type ON Contract(contract_type);
CREATE UNIQUE INDEX idx_contract_active_employee ON Contract(employee_id) WHERE status = 'active';

-- Document indexes
CREATE INDEX idx_document_employee ON Document(employee_id);
CREATE INDEX idx_document_type ON Document(document_type);
CREATE INDEX idx_document_expiry ON Document(expiry_date) WHERE expiry_date IS NOT NULL;
CREATE INDEX idx_document_verification ON Document(verification_status);

-- Timesheet indexes
CREATE INDEX idx_timesheet_employee_date ON Timesheet(employee_id, work_date);
CREATE INDEX idx_timesheet_date ON Timesheet(work_date);
CREATE INDEX idx_timesheet_status ON Timesheet(status);
CREATE INDEX idx_timesheet_approved ON Timesheet(is_approved);
CREATE INDEX idx_timesheet_shift ON Timesheet(shift_type);
CREATE UNIQUE INDEX idx_timesheet_unique ON Timesheet(employee_id, work_date, shift_type);

-- AttendanceRecord indexes
CREATE INDEX idx_attendance_employee_date ON AttendanceRecord(employee_id, attendance_date);
CREATE INDEX idx_attendance_date ON AttendanceRecord(attendance_date);
CREATE INDEX idx_attendance_late ON AttendanceRecord(is_late) WHERE is_late = TRUE;
CREATE INDEX idx_attendance_status ON AttendanceRecord(status);
```

### 4.4 Summary of Tables

| Table | Primary Key | Foreign Keys | Purpose | Estimated Rows |
|-------|-------------|--------------|---------|----------------|
| Employee | employee_id | Department, Position, Employee (manager) | Employee master record | 1,500 |
| EmployeeProfile | profile_id | Employee | Personal details, ID, insurance | 1,253 |
| ContactInfo | contact_id | Employee | Phone, email, address records | 3,759 |
| WorkHistory | history_id | Employee, User | Employment change history | 5,000 |
| FamilyMember | family_id | Employee | Family/dependent records | 2,500 |
| Contract | contract_id | Employee, User | Employment contracts | 1,500 |
| Document | document_id | Employee | Employee documents/certificates | 3,759 |
| Timesheet | timesheet_id | Employee, User | Daily attendance records | 365,000/year |
| AttendanceRecord | attendance_id | Employee | Attendance tracking details | 365,000/year |

---

## 5. API Specifications

### 5.1 Employee API

#### POST /api/v1/hr/employees

Create a new employee record.

**Request Body**:
```json
{
  "full_name": "Luc Phong Vu",
  "gender": "female",
  "date_of_birth": "1995-12-04",
  "place_of_birth": "Ha Noi",
  "marital_status": "single",
  "nationality": "Vietnamese",
  "id_number": "001095012345",
  "id_issue_date": "2020-01-15",
  "id_issue_place": "Cuc Canh sat DKQL cu tru va DLQG ve dan cu",
  "department_id": 2,
  "position_id": 15,
  "hire_date": "2026-01-02",
  "tax_code": "001095012345-001",
  "social_insurance_number": "01.26.0012345",
  "bank_account_number": "1234567890",
  "bank_name": "Vietcombank",
  "contacts": [
    {
      "contact_type": "phone",
      "contact_value": "+84 912 345 678",
      "is_primary": true
    },
    {
      "contact_type": "email",
      "contact_value": "vulp@abn.com.vn",
      "is_primary": true
    },
    {
      "contact_type": "address",
      "contact_value": "123 Nguyen Trai, Thanh Xuan, Ha Noi",
      "is_primary": true
    }
  ],
  "contract": {
    "contract_type": "probation",
    "start_date": "2026-01-02",
    "end_date": "2026-07-02",
    "base_salary": 8500000,
    "probation_end": "2026-03-02"
  }
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "employee_id": 1253,
    "employee_code": "D02-0067",
    "full_name": "Luc Phong Vu",
    "gender": "female",
    "status": "probation",
    "hire_date": "2026-01-02",
    "department_id": 2,
    "department_name": "Phong Ke toan",
    "position_id": 15,
    "position_name": "Nhan vien ke toan",
    "profile": {
      "profile_id": 1253,
      "date_of_birth": "1995-12-04",
      "marital_status": "single",
      "nationality": "Vietnamese",
      "id_number": "001095012345",
      "tax_code": "001095012345-001",
      "social_insurance_number": "01.26.0012345"
    },
    "contacts": [
      {
        "contact_id": 3760,
        "contact_type": "phone",
        "contact_value": "+84 912 345 678",
        "is_primary": true
      },
      {
        "contact_id": 3761,
        "contact_type": "email",
        "contact_value": "vulp@abn.com.vn",
        "is_primary": true
      }
    ],
    "contract": {
      "contract_id": 1253,
      "contract_number": "HD-2026-0067",
      "contract_type": "probation",
      "start_date": "2026-01-02",
      "end_date": "2026-07-02",
      "base_salary": 8500000,
      "probation_end": "2026-03-02",
      "status": "active"
    },
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

#### GET /api/v1/hr/employees

List employees with filtering.

**Query Parameters**:
- `department_id` (int): Filter by department
- `status` (string): Filter by status (active, inactive, terminated, probation, leave)
- `search` (string): Search by name, code, ID number
- `page` (int): Page number (default 1)
- `per_page` (int): Items per page (default 20, max 100)

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "employees": [
      {
        "employee_id": 1253,
        "employee_code": "D02-0067",
        "full_name": "Luc Phong Vu",
        "gender": "female",
        "status": "probation",
        "hire_date": "2026-01-02",
        "department_id": 2,
        "department_name": "Phong Ke toan",
        "position_id": 15,
        "position_name": "Nhan vien ke toan",
        "photo_url": "/uploads/employees/1253/photo.jpg"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 20,
      "total_pages": 63,
      "total_count": 1253
    }
  }
}
```

#### GET /api/v1/hr/employees/{employee_id}/profile

Get complete employee profile with all tabs.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "employee": {
      "employee_id": 1253,
      "employee_code": "D02-0067",
      "full_name": "Luc Phong Vu",
      "gender": "female",
      "status": "probation",
      "hire_date": "2026-01-02",
      "department_name": "Phong Ke toan",
      "position_name": "Nhan vien ke toan"
    },
    "profile": {
      "date_of_birth": "1995-12-04",
      "place_of_birth": "Ha Noi",
      "marital_status": "single",
      "nationality": "Vietnamese",
      "id_number": "001095012345",
      "id_issue_date": "2020-01-15",
      "id_issue_place": "Cuc Canh sat DKQL cu tru va DLQG ve dan cu",
      "tax_code": "001095012345-001",
      "social_insurance_number": "01.26.0012345",
      "bank_account_number": "1234567890",
      "bank_name": "Vietcombank"
    },
    "contacts": [
      {"contact_type": "phone", "contact_value": "+84 912 345 678", "is_primary": true},
      {"contact_type": "email", "contact_value": "vulp@abn.com.vn", "is_primary": true},
      {"contact_type": "address", "contact_value": "123 Nguyen Trai, Thanh Xuan, Ha Noi", "is_primary": true}
    ],
    "family_members": [],
    "documents": [
      {
        "document_id": 2500,
        "document_type": "id_card",
        "document_number": "001095012345",
        "issue_date": "2020-01-15",
        "verification_status": "verified"
      }
    ],
    "contracts": [
      {
        "contract_id": 1253,
        "contract_number": "HD-2026-0067",
        "contract_type": "probation",
        "start_date": "2026-01-02",
        "end_date": "2026-07-02",
        "base_salary": 8500000,
        "probation_end": "2026-03-02",
        "status": "active"
      }
    ],
    "work_history": [
      {
        "history_id": 5000,
        "change_type": "hire",
        "change_date": "2026-01-02",
        "new_value": {"department": "Phong Ke toan", "position": "Nhan vien ke toan", "salary": 8500000}
      }
    ],
    "timesheet_summary": {
      "days_paid": 27.5,
      "overtime_hours": 20,
      "late_count": 2,
      "leave_days": 1,
      "period": "2026-04"
    }
  }
}
```

### 5.2 Timesheet API

#### POST /api/v1/hr/timesheets

Record timesheet entry.

**Request Body**:
```json
{
  "employee_id": 1253,
  "work_date": "2026-04-14",
  "shift_type": "ca_sang",
  "clock_in": "07:55:00",
  "clock_out": "12:05:00",
  "regular_hours": 4.0,
  "overtime_hours": 0,
  "status": "present",
  "notes": ""
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "timesheet_id": 45001,
    "employee_id": 1253,
    "employee_name": "Luc Phong Vu",
    "work_date": "2026-04-14",
    "shift_type": "ca_sang",
    "clock_in": "07:55:00",
    "clock_out": "12:05:00",
    "regular_hours": 4.0,
    "overtime_hours": 0,
    "status": "present",
    "is_late": false,
    "is_approved": false,
    "created_at": "2026-04-14T12:10:00Z"
  }
}
```

#### GET /api/v1/hr/timesheets/summary

Get timesheet summary for employee and period.

**Query Parameters**:
- `employee_id` (int): Employee ID
- `month` (int): Month (1-12)
- `year` (int): Year

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "employee_id": 1253,
    "employee_name": "Luc Phong Vu",
    "period": "2026-04",
    "summary": {
      "days_paid": 27.5,
      "overtime_hours": 20,
      "late_count": 2,
      "leave_days": 1,
      "absent_days": 0,
      "holiday_days": 4,
      "total_regular_hours": 192.0,
      "total_overtime_hours": 20
    },
    "daily_entries": [
      {
        "work_date": "2026-04-01",
        "shift_type": "ca_sang",
        "clock_in": "07:58:00",
        "clock_out": "12:02:00",
        "regular_hours": 4.0,
        "status": "present",
        "is_approved": true
      }
    ]
  }
}
```

### 5.3 Contract API

#### POST /api/v1/hr/employees/{employee_id}/contracts

Create employment contract.

**Request Body**:
```json
{
  "contract_type": "fixed_term",
  "start_date": "2026-07-01",
  "end_date": "2028-07-01",
  "base_salary": 10000000,
  "allowance": 1500000,
  "working_hours_per_day": 8.0,
  "working_days_per_week": 6,
  "leave_days_per_year": 12,
  "terms": "Standard employment terms per Vietnamese labor law"
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "contract_id": 1254,
    "contract_number": "HD-2026-0067-02",
    "contract_type": "fixed_term",
    "start_date": "2026-07-01",
    "end_date": "2028-07-01",
    "base_salary": 10000000,
    "allowance": 1500000,
    "status": "draft",
    "created_at": "2026-04-14T10:00:00Z"
  }
}
```

### 5.4 Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Employee profile validation failed",
    "details": [
      {
        "field": "date_of_birth",
        "message": "Employee must be at least 18 years old"
      }
    ],
    "request_id": "req-hr123"
  }
}
```

### 5.5 API Summary Table

| Endpoint | Method | Auth Required | Rate Limit | Description |
|----------|--------|---------------|------------|-------------|
| /api/v1/hr/employees | POST | Yes | 50/min | Create employee |
| /api/v1/hr/employees | GET | Yes | 200/min | List employees |
| /api/v1/hr/employees/{id} | GET | Yes | 200/min | Get employee detail |
| /api/v1/hr/employees/{id} | PUT | Yes | 50/min | Update employee |
| /api/v1/hr/employees/{id}/profile | GET | Yes | 200/min | Get full profile |
| /api/v1/hr/employees/{id}/contracts | POST | Yes | 30/min | Create contract |
| /api/v1/hr/employees/{id}/contracts | GET | Yes | 200/min | List contracts |
| /api/v1/hr/employees/{id}/documents | POST | Yes | 30/min | Upload document |
| /api/v1/hr/employees/{id}/documents | GET | Yes | 200/min | List documents |
| /api/v1/hr/employees/{id}/family | POST | Yes | 30/min | Add family member |
| /api/v1/hr/employees/{id}/family | GET | Yes | 200/min | List family members |
| /api/v1/hr/timesheets | POST | Yes | 100/min | Record timesheet |
| /api/v1/hr/timesheets | GET | Yes | 200/min | List timesheets |
| /api/v1/hr/timesheets/summary | GET | Yes | 100/min | Timesheet summary |
| /api/v1/hr/timesheets/{id}/approve | POST | Yes | 50/min | Approve timesheet |
| /api/v1/hr/attendance | POST | Yes | 100/min | Record attendance |
| /api/v1/hr/attendance | GET | Yes | 200/min | List attendance records |
| /api/v1/hr/reports/headcount | GET | Yes | 30/min | Headcount report |
| /api/v1/hr/reports/turnover | GET | Yes | 30/min | Turnover report |
| /api/v1/hr/reports/attendance | GET | Yes | 30/min | Attendance report |

---

## 6. Permissions Matrix

### 6.1 Role Definitions

| Role | Code | Description | Department |
|------|------|-------------|------------|
| HR Manager | HR_MGR | Full HR module access | Human Resources |
| HR Specialist | HR_SPEC | Employee administration | Human Resources |
| Timekeeper | TIMEKEEPER | Timesheet and attendance | Human Resources |
| Payroll Officer | PAYROLL | Payroll processing | Human Resources |
| Department Manager | DEPT_MGR | Team management | Various |
| Employee | EMPLOYEE | Self-service | Various |
| Finance Manager | FIN_MGR | Budget oversight | Finance |
| Director | DIRECTOR | Strategic oversight | Executive |

### 6.2 CRUD Permissions

| Resource | HR_MGR | HR_SPEC | TIMEKEEPER | PAYROLL | DEPT_MGR | EMPLOYEE | FIN_MGR | DIRECTOR |
|----------|--------|---------|------------|---------|----------|----------|---------|----------|
| Employee | CRUD | CRUD | R | R | R (dept) | R (self) | R | R |
| EmployeeProfile | CRUD | CRUD | R | R | R (dept) | R (self) | R | R |
| ContactInfo | CRUD | CRUD | R | R | R (dept) | CRUD (self) | R | R |
| FamilyMember | CRUD | CRUD | - | R | - | CRUD (self) | R | R |
| Contract | CRUD | CRUD | - | CRUD | R (dept) | R (self) | R | R |
| Document | CRUD | CRUD | - | - | - | CRUD (self) | - | - |
| WorkHistory | CRUD | CRUD | - | - | R (dept) | R (self) | R | R |
| Timesheet | CRUD | CRUD | CRUD | R | CRUD (dept) | CRUD (self) | R | R |
| AttendanceRecord | CRUD | CRUD | CRUD | R | R (dept) | R (self) | R | R |
| LeaveRequest | CRUD | CRUD | - | - | Approve | CRUD (self) | R | R |

### 6.3 Row-Level Security

| Resource | Rule | Description |
|----------|------|-------------|
| Employee | Department managers see only their department | `department_id = user.department_id OR user.role IN ('HR_MGR', 'HR_SPEC', 'DIRECTOR')` |
| Timesheet | Department managers see only their team | `employee_id IN (team members) OR user.role IN ('HR_MGR', 'TIMEKEEPER')` |
| ContactInfo | Employees see only their own contacts | `employee_id = user.employee_id OR user.role IN ('HR_MGR', 'HR_SPEC')` |
| FamilyMember | Employees see only their own family | `employee_id = user.employee_id OR user.role IN ('HR_MGR', 'HR_SPEC', 'PAYROLL')` |
| Contract | Department managers see salary only for their team | Same as Employee with salary masking for DEPT_MGR |

---

## 7. State Machine

### 7.1 Employee State Diagram

```
                    +-----------+
                    |  PROSPECT |
                    +-----+-----+
                          |
                    (Hired)
                          v
                    +-----------+
             +----->| PROBATION |<----+
             |      +-----+-----+     |
             |            |           |
             |      (Probation End)   |
             |            |           |
             |      +-----------+     |
             |      |  ACTIVE   |     |
             |      +-----+-----+     |
             |            |           |
        +----+----+  +----+----+      |
        |         |  |         |      |
        v         v  v         v      |
    +-------+ +-----+ +-----------+  |
    | LEAVE | | INACT | TERMINATED |--+
    +---+---+ +-----+ +-----------+
        |                      (Rehire)
        v
    +-------+
    | ACTIVE| (return from leave)
    +-------+
```

### 7.2 Contract State Diagram

```
                    +---------+
                    |  DRAFT  |
                    +----+----+
                         |
                   (Signed)
                         v
                    +---------+
                    |  ACTIVE |
                    +----+----+
                         |
              +----------+----------+
              |                     |
              v                     v
        +----------+         +-----------+
        | EXPIRED  |         | TERMINATED|
        +----------+         +-----------+
```

### 7.3 State Transition Tables

#### Employee Transitions

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| PROSPECT | PROBATION | Hire | HR Specialist | Contract created |
| PROBATION | ACTIVE | Probation confirmed | HR Manager | Probation end date passed |
| PROBATION | TERMINATED | Probation failed | HR Manager | Before probation end |
| ACTIVE | LEAVE | Leave approved | HR Manager | Leave type: maternity, study |
| LEAVE | ACTIVE | Return from leave | HR Specialist | Leave end date passed |
| ACTIVE | INACTIVE | Suspension | HR Manager | Disciplinary action |
| INACTIVE | ACTIVE | Reinstatement | HR Manager | Suspension lifted |
| ACTIVE | TERMINATED | Termination | HR Manager | Offboarding complete |
| TERMINATED | PROSPECT | Rehire | HR Specialist | New hire process |

#### Contract Transitions

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| DRAFT | ACTIVE | Both parties sign | HR Specialist | Start date reached |
| ACTIVE | EXPIRED | End date reached | System | Automatic |
| ACTIVE | TERMINATED | Early termination | HR Manager | Termination agreement |
| EXPIRED | DRAFT | Renewal | HR Specialist | New contract created |

#### Timesheet Approval

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| DRAFT | SUBMITTED | Submit | Employee/Timekeeper | All days filled |
| SUBMITTED | APPROVED | Approve | Department Manager | Review complete |
| SUBMITTED | REJECTED | Reject | Department Manager | Corrections needed |
| REJECTED | DRAFT | Revise | Employee/Timekeeper | Corrections made |
| APPROVED | LOCKED | Payroll run | Payroll Officer | Period locked |

### 7.4 Approval Workflow

#### Leave Request Approval

```
Employee submits leave request -> Pending
     |
     v
Department Manager Review
     /           \
  Approve        Reject
     |             |
     v             v
Approved      Returned to Employee
     |             |
     v             v
Timesheet      Employee revises
Updated           |
     |             v
     v          Resubmit
Payroll
Processing
```

---

## 8. Reports

### 8.1 Report Inventory

| Report | Description | Dimensions | Metrics | Filters | Chart Type |
|--------|-------------|------------|---------|---------|------------|
| Headcount Report | Employee count by department | Department, position, status | Total count, new hires, terminations | Date, department | Stacked bar |
| Turnover Report | Employee turnover analysis | Department, month, reason | Turnover count, turnover rate | Period, department | Line chart |
| Attendance Report | Attendance summary | Department, employee, month | Present days, late count, absence days | Period, department | Heat map |
| Overtime Report | Overtime analysis | Department, employee, month | Overtime hours, overtime cost | Period, department | Bar chart |
| Leave Report | Leave usage analysis | Department, leave type, month | Leave days taken, remaining balance | Period, department | Stacked bar |
| Salary Report | Salary analysis | Department, position, grade | Total salary, average salary, salary range | Period, department | Box plot |
| Contract Expiry | Upcoming contract renewals | Department, contract type | Count of expiring contracts | Date range | Table |
| Document Expiry | Document renewal needed | Document type | Count of expiring documents | Date range | Table |
| Demographics | Employee demographics | Gender, age group, education | Count by category | Date | Pie chart |
| Cost per Employee | HR cost analysis | Department, cost type | Total HR cost, cost per employee | Period, department | Bar chart |

### 8.2 Report Summary Table

| Report ID | Report Name | Schedule | Auto-Generate | Distribution | Retention |
|-----------|-------------|----------|---------------|--------------|-----------|
| RPT-HR-001 | Headcount Report | Monthly | Yes | HR Manager, Director | 5 years |
| RPT-HR-002 | Turnover Report | Quarterly | Yes | HR Manager, Director | 5 years |
| RPT-HR-003 | Attendance Report | Monthly | Yes | HR Manager, Dept Managers | 3 years |
| RPT-HR-004 | Overtime Report | Monthly | Yes | HR Manager, Payroll | 3 years |
| RPT-HR-005 | Leave Report | Monthly | Yes | HR Manager, Dept Managers | 3 years |
| RPT-HR-006 | Salary Report | Monthly | Yes | HR Manager, Finance Manager | 5 years |
| RPT-HR-007 | Contract Expiry | Weekly | Yes | HR Specialist | 1 year |
| RPT-HR-008 | Document Expiry | Weekly | Yes | HR Specialist | 1 year |
| RPT-HR-009 | Demographics | Quarterly | Yes | Director, HR Manager | 5 years |
| RPT-HR-010 | Cost per Employee | Monthly | Yes | Finance Manager, Director | 5 years |

---

## 9. Integration Points

### 9.1 External API Integrations

| Integration | Provider | Protocol | Direction | Frequency | Data Exchanged |
|------------|----------|----------|-----------|-----------|----------------|
| Social Insurance | Vietnam Social Insurance | HTTPS/API | Bidirectional | Monthly | Registration, contribution records |
| Tax Authority | General Dept of Taxation | HTTPS/XML | Outbound | Monthly | PIT withholding reports |
| Bank Payroll | Vietcombank, BIDV | SFTP/API | Outbound | Monthly | Salary payment files |
| Health Insurance | Vietnam Health Insurance | HTTPS/API | Bidirectional | Monthly | Insurance enrollment, claims |
| Labor Authority | Department of Labor | HTTPS/API | Outbound | Quarterly | Labor force reports |

### 9.2 Internal Module Integrations

| Source Module | Target | Data Flow | Trigger | Frequency |
|---------------|--------|-----------|---------|-----------|
| M05 Employee Mgmt | M04 Accounting | Payroll expenses -> GL entries | Payroll run | Monthly |
| M05 Employee Mgmt | M10 Asset Mgmt | Asset allocation -> Employee | Asset assignment | Real-time |
| M05 Employee Mgmt | M06 Task Mgmt | Employee -> Task assignee | Task creation | Real-time |
| M06 Task Mgmt | M05 Employee Mgmt | Task completion -> Performance | Task complete | Real-time |
| M08 Sales | M05 Employee Mgmt | Sales commission -> Employee | Commission calc | Monthly |
| M04 Accounting | M05 Employee Mgmt | Salary payment confirmation | Payment posted | Real-time |

### 9.3 Webhook Events

| Event | Trigger | Payload | Subscribers |
|-------|---------|---------|-------------|
| employee.hired | New employee created | employee_id, employee_code, name, department | Payroll, IT provisioning |
| employee.terminated | Employee terminated | employee_id, termination_date, reason | Payroll, IT deprovisioning |
| contract.expiring | 30 days before contract expiry | employee_id, contract_id, end_date | HR Specialist |
| document.expiring | 30 days before document expiry | employee_id, document_id, expiry_date | HR Specialist, Employee |
| timesheet.submitted | Timesheet submitted for approval | employee_id, period, status | Department Manager |
| timesheet.approved | Timesheet approved | employee_id, period, approved_by | Payroll |
| overtime.exceeded | Overtime exceeds 20h/month | employee_id, current_overtime, month | HR Manager, Employee |
| leave.approved | Leave request approved | employee_id, leave_type, start_date, end_date | Timesheet, Department |

---

## 10. Technical Notes

### 10.1 Performance Requirements

| Metric | Target | Measurement | Notes |
|--------|--------|-------------|-------|
| Employee Search | < 500ms | P95 latency | With name/code/department filters |
| Profile Load | < 1s | P95 latency | Full 6-tab profile |
| Timesheet Save (bulk) | < 2s for 100 entries | P95 latency | Monthly timesheet entry |
| Attendance Query | < 500ms | P95 latency | Daily attendance for department |
| Report Generation | < 5s | P95 latency | Standard HR reports |
| Document Upload | < 3s per file | P95 latency | Max 5MB files |
| Concurrent Users | 200+ | Simultaneous | Including self-service portal |

### 10.2 Caching Strategy

| Cache Type | Data | TTL | Invalidation |
|------------|------|-----|--------------|
| Redis | Employee directory | 15 minutes | On employee update |
| Redis | Department hierarchy | 1 hour | On org change |
| Redis | Position catalog | 1 hour | On position change |
| Redis | Timesheet summary (current month) | 5 minutes | On timesheet update |
| Redis | Attendance dashboard | 2 minutes | On attendance record |
| Application | Shift configuration | 1 day | On settings change |
| Browser | Employee photo | 1 hour | On photo upload |

### 10.3 Key Files and Components

| File/Component | Path | Purpose |
|----------------|------|---------|
| Employee Service | `src/modules/hr/services/employee.service.ts` | Employee CRUD, ID generation |
| Profile Service | `src/modules/hr/services/profile.service.ts` | Profile management |
| Timesheet Service | `src/modules/hr/services/timesheet.service.ts` | Timesheet entry and approval |
| Contract Service | `src/modules/hr/services/contract.service.ts` | Contract lifecycle |
| Document Service | `src/modules/hr/services/document.service.ts` | Document upload and tracking |
| Attendance Service | `src/modules/hr/services/attendance.service.ts` | Attendance recording |
| Leave Service | `src/modules/hr/services/leave.service.ts` | Leave request workflow |
| HR Report Generator | `src/modules/hr/services/report-generator.service.ts` | HR report generation |
| Employee Code Generator | `src/modules/hr/services/code-generator.service.ts` | Auto-generate employee codes |
| Overtime Calculator | `src/modules/hr/services/overtime-calculator.service.ts` | Overtime calculation and limits |

### 10.4 Localization

| Aspect | Configuration | Notes |
|--------|---------------|-------|
| Language | Vietnamese (primary), English | All labels, messages, forms |
| Date Format | DD/MM/YYYY | Vietnamese standard |
| Name Format | Full Vietnamese name | Family name first |
| ID Number Format | 12-digit Vietnamese national ID | Per government standard |
| Phone Format | +84 XXX XXX XXX | Vietnamese phone format |
| Address Format | Vietnamese address hierarchy | Street, Ward, District, City |
| Currency | VND | Salary and allowance amounts |

### 10.5 Mobile Support

| Feature | Mobile Support | Notes |
|---------|----------------|-------|
| Employee Directory | Responsive | Search and view |
| Self-Service Profile | Responsive | View own profile, update contacts |
| Leave Request | Responsive | Submit and track leave requests |
| Timesheet Entry | Responsive | Daily timesheet entry |
| Attendance View | Responsive | View own attendance |
| Push Notifications | Push | Leave approval, document expiry |
| Document Upload | Camera integration | Take photo of documents |
| Clock In/Out | GPS + Camera | Location-verified clock-in |

### 10.6 Security Considerations

| Aspect | Implementation |
|--------|----------------|
| PII Encryption | AES-256 for ID numbers, bank accounts, personal data |
| Access Control | RBAC with row-level security |
| Audit Trail | Complete history of all profile changes |
| Document Storage | Encrypted file storage with access logging |
| Data Retention | Retain employee records 5 years post-termination |
| Salary Masking | Salary visible only to HR, Payroll, and direct manager |
| GDPR-like Rights | Employee can request data export and correction |
| Session Management | JWT tokens with 30-minute expiry for HR operations |
| Photo Privacy | Employee photos access-controlled |
| Family Data Privacy | Family member data restricted to HR and employee |

### 10.7 Database Considerations

- Employee photos stored as file URLs, not in database
- Soft deletes for employee records (is_deleted flag)
- Timesheet and AttendanceRecord partitioned by month
- Full-text search on employee name and ID number
- Materialized views for headcount and attendance summaries
- Archive terminated employee data to cold storage after 5 years
- Unique constraints on employee_code, id_number, contract_number

---

*Document Version: 1.0*
*Last Updated: 2026-04-14*
*Author: ERP Design Team*
*Review Status: Draft*
