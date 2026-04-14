# M13 - Time Tracking (Cham cong)

**Category:** HR  
**Priority:** P3  
**Reference Images:** assets/image_21.png, assets/image_38.png, assets/image_47.png, assets/image_48.png, assets/image_49.png  
**Dependencies:** M01 (Organization), M02 (Employee), M03 (Contract), M12 (Payroll)  
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Time Tracking module manages employee attendance, shift scheduling, overtime recording, leave requests, and absence analytics. It provides real-time attendance monitoring, calendar-based timesheet views, shift configuration, and integrates with Payroll (M12) for salary calculation based on working days, overtime hours, and leave records.

### Personas
| Persona | Role | Primary Use |
|---|---|---|
| HR Administrator | HR staff | Configure shifts, manage schedules, monitor attendance overview |
| Department Manager | Manager | Approve leave requests, view team attendance, approve overtime |
| Employee | End user | View personal timesheet, submit leave requests, clock in/out |
| Payroll Specialist | HR staff | Retrieve working days, overtime, leave data for payroll calculation |
| Receptionist / Security | Front desk | Manual attendance entry, visitor-related attendance |
| System Administrator | IT | Configure shift templates, attendance rules, device integration |

### Dependencies
| Dependency | Module | Integration Point |
|---|---|---|
| Employee master data | M02 | Employee list, department, position, status |
| Contract working hours | M03 | Standard working hours, probation status |
| Payroll calculation | M12 | Working days, overtime hours, leave days for salary |
| Organization structure | M01 | Department hierarchy for approval routing |

### Business Rules
1. **Standard Working Day**: 8 hours per day, typically split into morning shift (CA SANG: 08:00-12:00) and afternoon shift (CA CHIEU: 13:30-17:30).
2. **Paid Working Days**: Calculated per month excluding Sundays and public holidays. Standard month = 22-27 paid days depending on schedule.
3. **Overtime Limit**: Maximum 300 hours per year per Vietnam Labor Code. Overtime requires prior manager approval.
4. **Late/Early Threshold**: Arriving >10 minutes late or leaving >10 minutes early is recorded. >60 minutes late triggers special flag.
5. **Leave Types**: Annual leave (paid), unpaid leave, sick leave, marriage leave (80 days per image data), maternity leave, bereavement leave, other leave.
6. **Leave Balance**: Annual leave accrues at 1.67 days/month (20 days/year for standard employees). Unused leave carries over up to configurable limit.
7. **Shift Assignment**: Each employee assigned to a shift schedule. Multiple shifts supported (HC_SA, HC_CH, PT_SA, CA_1, CA_2, CA_3).
8. **Attendance Methods**: Fingerprint scanner, face recognition, mobile GPS check-in, manual entry by HR.
9. **Business Trip**: Marked as working day even if not at office. Requires prior approval.
10. **Timesheet Approval**: Monthly timesheet requires manager confirmation before sending to payroll.
11. **Special Day Marking**: Leave, unpaid leave, business trip, late 60min+ marked distinctly on timesheet calendar.

---

## 2. Screen Inventory

### 2.1 Timesheet Calendar View
**Route:** `/time-tracking/timesheet`  
**Purpose:** Monthly calendar showing daily attendance for an employee (img_21/38).

| UI Component | Type | Description |
|---|---|---|
| Calendar Grid | Calendar | Days of month with attendance status per day |
| Shift Labels | Badge | CA SANG (08:00-12:00), CA CHIEU (13:30-17:30) per day |
| Day Status | Color code | Present (green), Late (yellow), Leave (blue), Absent (red), Holiday (gray) |
| Month Navigator | Arrows | Previous/next month |
| Employee Selector | Dropdown | View different employee timesheets (manager view) |
| Summary Bar | Cards | Paid days 27.5, Overtime 20h, Late/Early 2, Leave 1 |
| Special Legend | Legend | Leave, unpaid leave, business trip, late 60min+ icons |
| Confirm Button | Button | Manager confirm timesheet |

**User Interactions:**
- Click any day to view detailed check-in/out times.
- Navigate months to review historical timesheets.
- Managers select employee to review team timesheets.
- Confirm timesheet at month end to finalize for payroll.

**Data Displayed:** Daily attendance with shift labels, check-in/out times, summary totals.

### 2.2 Attendance Overview Dashboard
**Route:** `/time-tracking/overview`  
**Purpose:** Real-time attendance snapshot for the organization (img_47).

| UI Component | Type | Description |
|---|---|---|
| Late/Early Today | KPI card | Count of late/early arrivals today (e.g., 40) |
| Absent Today | KPI card | Count of absent employees today (e.g., 16) |
| Absent This Week | KPI card | Count of absent employees this week (e.g., 23) |
| Leave Trend Chart | Line chart | Leave count trend over time |
| Most Late Table | Table | Top late employees with count (e.g., Nguyen Danh Tuyen 36) |
| Leave Type Donut | Donut chart | Leave 60, Unpaid 60, Sick 40, Marriage 80, Other 120 |
| Late Frequency Table | Table | Employee, department, late count, trend |
| Refresh | Button | Real-time refresh |

**User Interactions:**
- Click KPI card to drill down to employee list.
- Click chart segments to filter.
- Refresh for latest data.
- Export overview as PDF.

**Data Displayed:** Real-time attendance metrics, leave analytics, late frequency rankings.

### 2.3 Shift Configuration
**Route:** `/time-tracking/settings/shifts`  
**Purpose:** Create and manage shift definitions and schedules (img_48).

| UI Component | Type | Description |
|---|---|---|
| Shift Form | Form | Shift code, name, start time, end time, break times |
| Recurrence Rules | Multi-select | Days of week, date range, repeat pattern |
| Calendar Preview | Calendar | Visual preview of shift schedule |
| Time Inputs | Time picker | Start, end, break start, break end |
| Save Button | Button | Persist shift configuration |
| Test Button | Button | Preview shift for specific date range |

**User Interactions:**
- Create new shift with name, code, times.
- Set recurrence rules (weekly, monthly, custom).
- Preview calendar to verify schedule.
- Save to activate shift.

**Data Displayed:** Shift definition form with recurrence and preview.

### 2.4 Shift Assignment Table
**Route:** `/time-tracking/settings/shift-assignments`  
**Purpose:** Assign employees to shifts, view all shift assignments (img_49).

| UI Component | Type | Description |
|---|---|---|
| Assignment Table | Data table | 120 records: Employee, shift code, effective dates, status |
| Shift Code Display | Badge | HC_SA, HC_CH, PT_SA, CA_1, CA_2, CA_3 |
| Filter | Dropdowns | Department, shift, date range |
| Assign Modal | Modal | Select employee(s), select shift, set dates |
| Bulk Assign | Button | Assign shift to multiple employees |
| Search | Input | Search by employee name/code |

**User Interactions:**
- Open modal to assign shift to employee.
- Bulk assign for department-wide shift changes.
- Filter by shift code or department.
- Edit existing assignments.

**Data Displayed:** All shift assignments with employee and shift details.

### 2.4 Leave Request Management
**Route:** `/time-tracking/leave-requests`  
**Purpose:** Submit, review, and approve leave requests.

| UI Component | Type | Description |
|---|---|---|
| Request List | Table | Employee, leave type, start/end dates, status, approver |
| New Request | Button | Submit new leave request |
| Approval Form | Modal | Approve/reject with comment |
| Balance Display | Card | Remaining leave balance for selected type |
| Status Filter | Dropdown | Pending, Approved, Rejected, Cancelled |

**User Interactions:**
- Submit leave request with type, dates, reason.
- Manager reviews pending requests and approves/rejects.
- View leave balance before submitting.
- Cancel pending request.

**Data Displayed:** Leave request list with status and balances.

### 2.5 Overtime Management
**Route:** `/time-tracking/overtime`  
**Purpose:** Record and approve overtime hours.

| UI Component | Type | Description |
|---|---|---|
| Overtime List | Table | Employee, date, hours, reason, status, approver |
| New Overtime | Button | Record new overtime |
| Approval Form | Modal | Approve/reject with comment |
| Total Display | Card | Total overtime hours (current month, year-to-date) |
| Limit Warning | Alert | Approaching 300h/year limit |

**User Interactions:**
- Record overtime with date, hours, reason.
- Manager approves overtime before it counts toward payroll.
- View overtime totals and limit warnings.

**Data Displayed:** Overtime records with approval status and totals.

### 2.6 Late/Early Attendance Log
**Route:** `/time-tracking/late-early`  
**Purpose:** Review and manage late arrivals and early departures.

| UI Component | Type | Description |
|---|---|---|
| Late Log | Table | Employee, date, actual time, scheduled time, minutes late |
| Early Log | Table | Employee, date, actual time, scheduled time, minutes early |
| 60min+ Flag | Highlight | Entries with >60 minutes late marked prominently |
| Reason Field | Input | Employee explanation for late/early |
| Filter | Date range, employee | Filter log entries |

**User Interactions:**
- Review daily late/early log.
- Add reason codes for frequent late arrivals.
- Export for disciplinary review.

**Data Displayed:** Late and early attendance records with severity flags.

### 2.7 Business Trip Management
**Route:** `/time-tracking/business-trips`  
**Purpose:** Register and manage business trip attendance.

| UI Component | Type | Description |
|---|---|---|
| Trip List | Table | Employee, destination, dates, purpose, status |
| New Trip | Button | Register business trip |
| Approval Form | Modal | Manager approve/reject |
| Calendar View | Calendar | Visual trip schedule |

**User Interactions:**
- Register business trip with details.
- Manager approves trip.
- Approved trips marked as "present" on timesheet.

**Data Displayed:** Business trip records with approval status.

### 2.8 Attendance Device Management
**Route:** `/time-tracking/settings/devices`  
**Purpose:** Configure attendance tracking devices.

| UI Component | Type | Description |
|---|---|---|
| Device List | Table | Device name, type, location, status, last sync |
| Device Form | Modal | Device configuration |
| Sync Log | Table | Sync history, record count, status |
| Test Connection | Button | Verify device connectivity |

**User Interactions:**
- Register new attendance device.
- Configure device parameters.
- Monitor sync status and troubleshoot.

**Data Displayed:** Device inventory and sync status.

---

## 3. Feature Specifications

### 3.1 Shift Management
**Description:** Define work shifts with start/end times, breaks, and recurrence rules. Assign employees to shifts for automated attendance tracking.

**User Stories:**
- As an HR Administrator, I can create shift definitions with time ranges and break periods.
- As an HR Administrator, I can set recurrence rules so shifts repeat on specified days.
- As an HR Administrator, I can assign employees to specific shifts.
- As an HR Administrator, I can preview a shift schedule on a calendar before saving.

**Acceptance Criteria:**
1. Shift has: code, name, start time, end time, break start, break end, working hours.
2. Recurrence supports: daily, weekly (select days), monthly, custom date ranges.
3. Shift code is unique and follows company naming convention (e.g., HC_SA, CA_1).
4. Calendar preview shows shift distribution over selected date range.
5. Employee can be assigned to only one active shift at a time.
6. Shift assignment has effective start and end dates.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid time range | End time > start time | "End time must be after start time" |
| Working hours > 0 | Calculated hours > 0 | "Shift must have positive working hours" |
| Unique code | No duplicate shift code | "Shift code '{code}' already exists" |
| Break within shift | Break times within shift range | "Break time must be within shift hours" |
| No overlap | Employee has no overlapping shift assignments | "Employee already assigned to another shift for this date" |

**Edge Cases:**
- Night shift crossing midnight (e.g., 22:00-06:00).
- Shift with multiple breaks.
- Employee transferred between shifts mid-month.
- Holiday shift with reduced hours.

### 3.2 Attendance Recording
**Description:** Capture employee check-in and check-out events from multiple sources (devices, mobile, manual).

**User Stories:**
- As an Employee, I can clock in and out using my assigned attendance method.
- As a system, I automatically record attendance from fingerprint/face recognition devices.
- As an HR Administrator, I can manually enter attendance for employees with device issues.
- As a system, I automatically determine attendance status (present, late, early, absent).

**Acceptance Criteria:**
1. Each attendance record has: employee, date, check-in time, check-out time, source.
2. Status auto-calculated: present (within tolerance), late (>10 min after start), early (>10 min before end), absent (no record).
3. Late >60 minutes flagged for special attention.
4. Manual entries require HR authorization and reason.
5. Multiple check-in/checkout events per day: earliest in and latest out used.
6. Device records synced in near real-time (<5 minute delay).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid date | Date not in future | "Cannot record attendance for future date" |
| Check-out after check-in | Checkout >= checkin | "Check-out must be after check-in" |
| One record per day | No duplicate date/employee | "Attendance already recorded for this date" |

**Edge Cases:**
- Employee forgets to check out: system uses shift end time with flag.
- Employee checks in but not out for multiple days: HR intervention required.
- Device malfunction: manual entry with source marked "manual".
- Employee works multiple shifts in one day: record all, use longest.

### 3.3 Timesheet Calendar
**Description:** Monthly calendar view showing daily attendance status with shift labels and special day marking.

**User Stories:**
- As an Employee, I can view my monthly timesheet calendar to verify my attendance.
- As a Manager, I can view any employee's timesheet calendar to review attendance.
- As a Manager, I can confirm an employee's timesheet at month end.

**Acceptance Criteria:**
1. Calendar shows all days of month with status color coding.
2. Each day displays shift label (CA SANG, CA CHIEU) and check-in/out times.
3. Special days marked: leave (L), unpaid leave (KL), business trip (CC), late 60min+ (T60).
4. Summary bar shows: paid days, overtime hours, late/early count, leave days.
5. Manager confirmation locks the timesheet for payroll processing.
6. Confirmed timesheet cannot be modified without manager unconfirmation.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Confirmable | Month has ended or manager override | "Cannot confirm timesheet for current month" |
| Complete records | No missing attendance days | "Missing attendance records for {dates}" |

**Edge Cases:**
- Public holiday: automatically marked as holiday, no attendance required.
- Sunday: marked as day off per standard schedule.
- Employee on extended leave: calendar shows leave for full duration.

### 3.4 Leave Request Management
**Description:** Employees submit leave requests, managers approve/reject, leave balances tracked.

**User Stories:**
- As an Employee, I can submit a leave request with type, dates, and reason.
- As an Employee, I can view my remaining leave balances before submitting.
- As a Manager, I can approve or reject leave requests from my team.
- As a system, I automatically deduct approved leave from the employee's balance.

**Acceptance Criteria:**
1. Leave request has: employee, type, start date, end date, reason, status.
2. Multi-day leave supported (consecutive or non-consecutive).
3. Balance check before submission: insufficient balance triggers warning.
4. Approval workflow follows organizational hierarchy.
5. Approved leave reflected on timesheet calendar.
6. Cancelled leave restores balance.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Sufficient balance | Available balance >= requested | "Insufficient {type} leave balance. Available: {days}" |
| Future date | Start date >= today | "Leave cannot start in the past" |
| Valid duration | End date >= start date | "End date must be on or after start date" |
| Max consecutive | Within policy limit | "Leave request exceeds maximum consecutive days ({max})" |

**Edge Cases:**
- Leave spanning across month boundary: split into two month records.
- Emergency leave (past dates): manager override with reason.
- Leave during probation: may be unpaid or restricted.
- Overlapping requests: system prevents double-booking.

### 3.5 Leave Balance Tracking
**Description:** Track accrued and used leave balances per employee per leave type.

**User Stories:**
- As an Employee, I can view my current leave balances for all leave types.
- As a system, I automatically accrue annual leave monthly.
- As an HR Administrator, I can manually adjust leave balances.

**Acceptance Criteria:**
1. Leave balance per type: annual, sick, unpaid, marriage, maternity, bereavement, other.
2. Annual leave accrues at 1.67 days/month (20 days/year standard).
3. Seniority-based accrual: 1 extra day per 5 years of service (configurable).
4. Carry-over: unused annual leave carries over up to max (e.g., 10 days).
5. Balance = Accrued - Used + Adjusted.
6. Manual adjustments require reason and approval.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Non-negative used | Used <= accrued + adjusted | "Cannot use more leave than available balance" |
| Carry-over limit | Carried <= max allowed | "Carry-over exceeds maximum ({max} days)" |

**Edge Cases:**
- Employee termination: unused leave paid out or forfeited per policy.
- Mid-year hire: prorate annual leave for remaining months.
- Leave type deactivated: existing balances preserved, no new accrual.

### 3.6 Overtime Management
**Description:** Record, approve, and track overtime hours with legal limit enforcement.

**User Stories:**
- As an Employee, I can record overtime hours worked.
- As a Manager, I can approve or reject overtime records.
- As an HR Administrator, I can monitor total overtime against the 300h/year legal limit.
- As a system, I send a warning when an employee approaches the overtime limit.

**Acceptance Criteria:**
1. Overtime record: employee, date, start time, end time, hours, reason, status.
2. Manager approval required before overtime counts toward payroll.
3. Year-to-date overtime tracked per employee.
4. Warning at 80% of limit (240h), block at 100% (300h).
5. Overtime rates: normal day 1.5x, weekend 2.0x, holiday 3.0x.
6. Approved overtime hours sent to payroll for compensation calculation.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Legal limit | YTD overtime <= 300h | "Overtime exceeds annual legal limit (300h)" |
| Max daily | Daily overtime <= 4h | "Daily overtime cannot exceed 4 hours" |
| Approval required | Manager approval before payroll | "Overtime pending approval, not included in payroll" |

**Edge Cases:**
- Emergency overtime exceeding daily limit: requires HR Director approval.
- Overtime on leave day: not allowed, system rejects.
- Overtime on rest day: recorded at weekend rate.

### 3.7 Attendance Overview & Analytics
**Description:** Real-time dashboard showing organization-wide attendance metrics (img_47).

**User Stories:**
- As an HR Administrator, I can see who is late, absent, or on leave today.
- As an HR Administrator, I can view leave trends and leave type distribution.
- As an HR Administrator, I can identify employees with frequent late arrivals.

**Acceptance Criteria:**
1. Dashboard shows: late/early today count, absent today count, absent week count.
2. Leave trend chart shows daily/weekly leave count over time.
3. Leave type donut chart shows distribution by leave type.
4. Late frequency table ranks employees by late count.
5. Data refreshes in near real-time (<1 minute delay).
6. Click through from any metric to detailed employee list.

**Edge Cases:**
- Very large organizations: data aggregated by department first, drill down on demand.
- Data source latency: show last sync time, warn if data is stale.

### 3.8 Attendance Rule Configuration
**Description:** Configure attendance rules: late thresholds, auto-approval, tolerance settings.

**User Stories:**
- As an HR Administrator, I can configure what constitutes "late" (minutes threshold).
- As an HR Administrator, I can set auto-approval rules for certain leave types.
- As an HR Administrator, I can configure overtime approval requirements.

**Acceptance Criteria:**
1. Late threshold: configurable minutes (default 10).
2. Early departure threshold: configurable minutes (default 10).
3. Severe late threshold: configurable minutes (default 60, triggers special flag).
4. Auto-approval rules: by leave type, by duration, by employee level.
5. Overtime pre-approval vs post-recording mode.
6. Tolerance for device clock-in: configurable grace period (e.g., 5 minutes).

**Edge Cases:**
- Rule changes mid-period: apply from effective date forward, not retroactive.
- Department-specific rules: override global rules per department.

---

## 4. Database Design

### ERD (ASCII)
```
+----------------+       +-------------------+       +-------------------+
| Shift          |1-----*| ShiftAssignment   |1-----*| Employee          |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| code           |       | shift_id (FK)     |       | name              |
| name           |       | employee_id (FK)  |       +-------------------+
| start_time     |       | start_date        |
| end_time       |       | end_date          |
| break_start    |       | created_at        |
| break_end      |       +-------------------+
| working_hours  |              |
| is_active      |              |
+----------------+              |
       |                        |
       | 1                     *|
       v                        v
+----------------+       +-------------------+
| ShiftSchedule  |       | AttendanceRecord  |
+----------------+       +-------------------+
| id (PK)        |       | id (PK)           |
| shift_id (FK)  |       | employee_id (FK)  |
| date           |       | date              |
| is_working_day |       | check_in          |
| notes          |       | check_out         |
+----------------+       | source            |
                         | status            |
                         | late_minutes      |
                         | early_minutes     |
                         | created_at        |
                         +-------------------+
                                |
                    1----------*|
                                v
              +-------------------+       +-------------------+
              | OvertimeRecord    |       | LeaveRequest      |
              +-------------------+       +-------------------+
              | id (PK)           |       | id (PK)           |
              | employee_id (FK)  |       | employee_id (FK)  |
              | date              |       | leave_type_id (FK)|
              | start_time        |       | start_date        |
              | end_time          |       | end_date          |
              | hours             |       | reason            |
              | rate_multiplier   |       | status            |
              | status            |       | approved_by (FK)  |
              | approved_by (FK)  |       | approved_at       |
              +-------------------+       +-------------------+

+----------------+       +-------------------+       +-------------------+
| LeaveType      |       | LeaveBalance      |       | LateEarlyRecord   |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| code           |       | employee_id (FK)  |       | employee_id (FK)  |
| name           |       | leave_type_id (FK)|       | date              |
| is_paid        |       | accrued           |       | actual_time       |
| accrual_rate   |       | used              |       | scheduled_time    |
| carry_over_max |       | adjusted          |       | type              |
+----------------+       | period            |       | minutes           |
                         +-------------------+       | reason            |
                                                     +-------------------+

+----------------+       +-------------------+
| BusinessTrip   |       | Timesheet         |
+----------------+       +-------------------+
| id (PK)        |       | id (PK)           |
| employee_id    |       | employee_id (FK)  |
| destination    |       | month             |
| start_date     |       | year              |
| end_date       |       | working_days      |
| purpose        |       | overtime_hours    |
| status         |       | leave_days        |
| approved_by    |       | late_count        |
+----------------+       | status            |
                         | confirmed_by      |
                         | confirmed_at      |
                         +-------------------+
```

### Table Schemas

#### shift
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| code | VARCHAR(20) | UNIQUE, NOT NULL | Shift code (e.g., HC_SA, CA_1) |
| name | VARCHAR(100) | NOT NULL | Display name |
| start_time | TIME | NOT NULL | Shift start time |
| end_time | TIME | NOT NULL | Shift end time |
| break_start | TIME | NULL | Break start time |
| break_end | TIME | NULL | Break end time |
| working_hours | DECIMAL(4,2) | NOT NULL | Net working hours |
| description | TEXT | NULL | Shift description |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### shift_assignment
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| shift_id | UUID | FK -> shift.id, NOT NULL | Assigned shift |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| start_date | DATE | NOT NULL | Assignment start date |
| end_date | DATE | NULL | Assignment end date (NULL = ongoing) |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### shift_schedule
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| shift_id | UUID | FK -> shift.id, NOT NULL | Reference shift |
| date | DATE | NOT NULL | Scheduled date |
| is_working_day | BOOLEAN | NOT NULL, DEFAULT true | Whether this is a working day |
| notes | TEXT | NULL | Schedule notes |

#### attendance_record
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| date | DATE | NOT NULL | Attendance date |
| check_in | TIMESTAMPTZ | NULL | Check-in timestamp |
| check_out | TIMESTAMPTZ | NULL | Check-out timestamp |
| source | VARCHAR(30) | NOT NULL | device/mobile/manual |
| device_id | UUID | FK -> attendance_device.id, NULL | Source device |
| status | VARCHAR(20) | NOT NULL | present/late/early/absent/holiday/leave |
| late_minutes | INT | NOT NULL, DEFAULT 0 | Minutes late |
| early_minutes | INT | NOT NULL, DEFAULT 0 | Minutes early |
| notes | TEXT | NULL | Attendance notes |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### overtime_record
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| date | DATE | NOT NULL | Overtime date |
| start_time | TIME | NOT NULL | Overtime start |
| end_time | TIME | NOT NULL | Overtime end |
| hours | DECIMAL(5,2) | NOT NULL | Total overtime hours |
| rate_multiplier | DECIMAL(3,2) | NOT NULL, DEFAULT 1.5 | 1.5/2.0/3.0 |
| reason | TEXT | NOT NULL | Reason for overtime |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/approved/rejected |
| approved_by | UUID | FK -> users.id, NULL | Approver |
| approved_at | TIMESTAMPTZ | NULL | Approval timestamp |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### leave_request
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| leave_type_id | UUID | FK -> leave_type.id, NOT NULL | Leave type |
| start_date | DATE | NOT NULL | Leave start date |
| end_date | DATE | NOT NULL | Leave end date |
| reason | TEXT | NOT NULL | Reason for leave |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/approved/rejected/cancelled |
| approved_by | UUID | FK -> users.id, NULL | Approver |
| approved_at | TIMESTAMPTZ | NULL | Approval timestamp |
| rejection_reason | TEXT | NULL | Reason if rejected |
| attachment_urls | TEXT[] | NULL | Supporting documents |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### leave_type
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| code | VARCHAR(20) | UNIQUE, NOT NULL | Leave type code |
| name | VARCHAR(100) | NOT NULL | Display name |
| is_paid | BOOLEAN | NOT NULL, DEFAULT true | Paid or unpaid |
| accrual_rate | DECIMAL(5,2) | NULL | Days accrued per month |
| carry_over_max | INT | NULL | Max carry-over days |
| requires_approval | BOOLEAN | NOT NULL, DEFAULT true | Requires manager approval |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |

#### leave_balance
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| leave_type_id | UUID | FK -> leave_type.id, NOT NULL | Leave type |
| year | INT | NOT NULL | Balance year |
| accrued | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Total accrued days |
| used | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Total used days |
| adjusted | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Manual adjustments |
| carried_over | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Carried from prior year |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### late_early_record
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| date | DATE | NOT NULL | Date of occurrence |
| type | VARCHAR(10) | NOT NULL | late/early |
| actual_time | TIME | NOT NULL | Actual check-in/out time |
| scheduled_time | TIME | NOT NULL | Scheduled time |
| minutes | INT | NOT NULL | Minutes late/early |
| reason | TEXT | NULL | Employee explanation |
| action_taken | VARCHAR(30) | NULL | warning/deduction/none |

#### business_trip
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| destination | VARCHAR(200) | NOT NULL | Trip destination |
| start_date | DATE | NOT NULL | Trip start |
| end_date | DATE | NOT NULL | Trip end |
| purpose | TEXT | NOT NULL | Trip purpose |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending/approved/rejected/completed |
| approved_by | UUID | FK -> users.id, NULL | Approver |
| approved_at | TIMESTAMPTZ | NULL | Approval timestamp |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### timesheet
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| month | INT | NOT NULL | Timesheet month |
| year | INT | NOT NULL | Timesheet year |
| working_days | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Actual working days |
| overtime_hours | DECIMAL(6,2) | NOT NULL, DEFAULT 0 | Total approved overtime |
| leave_days | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Total leave days |
| unpaid_leave_days | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Unpaid leave days |
| late_count | INT | NOT NULL, DEFAULT 0 | Late arrival count |
| early_count | INT | NOT NULL, DEFAULT 0 | Early departure count |
| absent_count | INT | NOT NULL, DEFAULT 0 | Absent day count |
| business_trip_days | DECIMAL(5,2) | NOT NULL, DEFAULT 0 | Business trip days |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft/confirmed/locked |
| confirmed_by | UUID | FK -> users.id, NULL | Confirmer |
| confirmed_at | TIMESTAMPTZ | NULL | Confirmation timestamp |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### attendance_device
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(100) | NOT NULL | Device name |
| device_type | VARCHAR(30) | NOT NULL | fingerprint/face/gps/manual |
| location | VARCHAR(200) | NULL | Physical location |
| ip_address | VARCHAR(45) | NULL | Device IP address |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active/inactive/maintenance |
| last_sync_at | TIMESTAMPTZ | NULL | Last sync timestamp |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

### All Tables Summary
| Table | Purpose | Key Relationships |
|---|---|---|
| shift | Shift definitions with times | Parent of ShiftAssignment, ShiftSchedule |
| shift_assignment | Employee-to-shift mapping | Links Shift, Employee |
| shift_schedule | Calendar of working days | Links Shift |
| attendance_record | Daily check-in/out events | Links Employee, Device |
| overtime_record | Overtime hours and approval | Links Employee |
| leave_request | Leave applications | Links Employee, LeaveType |
| leave_type | Leave category definitions | Referenced by LeaveRequest, LeaveBalance |
| leave_balance | Accrued vs used leave | Links Employee, LeaveType |
| late_early_record | Late/early event log | Links Employee |
| business_trip | Business trip records | Links Employee |
| timesheet | Monthly attendance summary | Links Employee |
| attendance_device | Device inventory | Referenced by AttendanceRecord |

---

## 5. API Specifications

### JSON Response Format
```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 120
  }
}
```

### Error Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Insufficient annual leave balance. Available: 3.5 days",
    "details": [
      { "field": "end_date", "message": "Requested duration exceeds balance" }
    ]
  }
}
```

### Endpoints Summary
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| GET | `/api/time-tracking/shifts` | List all shifts | time.shift.read |
| POST | `/api/time-tracking/shifts` | Create shift | time.shift.create |
| PUT | `/api/time-tracking/shifts/:id` | Update shift | time.shift.update |
| DELETE | `/api/time-tracking/shifts/:id` | Deactivate shift | time.shift.delete |
| GET | `/api/time-tracking/shift-assignments` | List shift assignments | time.shift.read |
| POST | `/api/time-tracking/shift-assignments` | Assign employee to shift | time.shift.assign |
| DELETE | `/api/time-tracking/shift-assignments/:id` | Remove shift assignment | time.shift.assign |
| GET | `/api/time-tracking/attendance` | List attendance records | time.attendance.read |
| POST | `/api/time-tracking/attendance` | Record attendance | time.attendance.create |
| PUT | `/api/time-tracking/attendance/:id` | Update attendance | time.attendance.update |
| GET | `/api/time-tracking/timesheets` | List timesheets | time.timesheet.read |
| GET | `/api/time-tracking/timesheets/:id` | Get timesheet detail | time.timesheet.read |
| POST | `/api/time-tracking/timesheets/:id/confirm` | Confirm timesheet | time.timesheet.confirm |
| GET | `/api/time-tracking/leave-requests` | List leave requests | time.leave.read |
| POST | `/api/time-tracking/leave-requests` | Submit leave request | time.leave.create |
| PATCH | `/api/time-tracking/leave-requests/:id/status` | Approve/reject leave | time.leave.approve |
| GET | `/api/time-tracking/leave-balances` | Get leave balances | time.leave.read |
| POST | `/api/time-tracking/leave-balances/:id/adjust` | Adjust leave balance | time.leave.admin |
| GET | `/api/time-tracking/overtime` | List overtime records | time.overtime.read |
| POST | `/api/time-tracking/overtime` | Record overtime | time.overtime.create |
| PATCH | `/api/time-tracking/overtime/:id/status` | Approve/reject overtime | time.overtime.approve |
| GET | `/api/time-tracking/overview` | Attendance overview dashboard | time.overview.read |
| GET | `/api/time-tracking/late-early` | Late/early log | time.attendance.read |
| GET | `/api/time-tracking/business-trips` | List business trips | time.trip.read |
| POST | `/api/time-tracking/business-trips` | Register business trip | time.trip.create |
| PATCH | `/api/time-tracking/business-trips/:id/status` | Approve/reject trip | time.trip.approve |
| GET | `/api/time-tracking/devices` | List attendance devices | time.settings.read |
| POST | `/api/time-tracking/devices` | Register device | time.settings.write |
| POST | `/api/time-tracking/devices/:id/sync` | Trigger device sync | time.settings.write |

### Endpoint Details

#### POST /api/time-tracking/attendance
**Request Body:**
```json
{
  "employee_id": "emp-123",
  "date": "2026-04-14",
  "check_in": "2026-04-14T08:05:00+07:00",
  "check_out": "2026-04-14T17:35:00+07:00",
  "source": "device",
  "device_id": "dev-001"
}
```
**Response:** 201 Created with attendance_record object including auto-calculated status.

#### POST /api/time-tracking/leave-requests
**Request Body:**
```json
{
  "employee_id": "emp-123",
  "leave_type_id": "lt-annual",
  "start_date": "2026-04-20",
  "end_date": "2026-04-22",
  "reason": "Family vacation"
}
```
**Response:** 201 Created with leave_request object including balance check result.

#### GET /api/time-tracking/overview
**Query Parameters:** `date` (optional, defaults to today), `department_id` (optional)
**Response:**
```json
{
  "success": true,
  "data": {
    "late_early_today": 40,
    "absent_today": 16,
    "absent_week": 23,
    "leave_trend": [
      { "date": "2026-04-07", "count": 12 },
      { "date": "2026-04-08", "count": 15 },
      { "date": "2026-04-09", "count": 18 }
    ],
    "most_late": [
      { "employee_name": "Nguyen Danh Tuyen", "late_count": 36 },
      { "employee_name": "Le Van A", "late_count": 28 }
    ],
    "leave_by_type": [
      { "type": "Leave", "count": 60 },
      { "type": "Unpaid", "count": 60 },
      { "type": "Sick", "count": 40 },
      { "type": "Marriage", "count": 80 },
      { "type": "Other", "count": 120 }
    ]
  }
}
```

---

## 6. Permissions Matrix

### Roles
| Role | Description |
|---|---|
| HR Director | Full access to all time tracking functions |
| HR Administrator | Manage shifts, assignments, attendance, leave, overtime |
| Department Manager | View team attendance, approve leave/overtime for direct reports |
| Employee | View own attendance, submit leave requests, view own leave balance |
| Payroll Specialist | Read-only access to timesheets, overtime, leave for payroll |
| System Administrator | Device configuration, system settings |

### Permission Matrix (CRUD)
| Permission | HR Director | HR Admin | Dept. Mgr. | Employee | Payroll Spec. | Sys Admin |
|---|---|---|---|---|---|---|
| time.shift.read | CRUD | CRUD | R | R | R | R |
| time.shift.create | C | C | - | - | - | - |
| time.shift.assign | CRUD | CRUD | - | - | - | - |
| time.attendance.read | CRUD | CRUD | R (own dept) | R (own) | R | CRUD |
| time.attendance.create | CRUD | CRUD | - | R (own) | - | CRUD |
| time.attendance.update | CRUD | CRUD | - | - | - | CRUD |
| time.timesheet.read | CRUD | CRUD | R (own dept) | R (own) | R | R |
| time.timesheet.confirm | C | C | C | - | - | - |
| time.leave.read | CRUD | CRUD | R (own dept) | R (own) | R | R |
| time.leave.create | CRUD | CRUD | - | C (own) | - | CRUD |
| time.leave.approve | C | C | C (own dept) | - | - | - |
| time.leave.admin | C | C | - | - | - | - |
| time.overtime.read | CRUD | CRUD | R (own dept) | R (own) | R | R |
| time.overtime.create | CRUD | CRUD | - | C (own) | - | CRUD |
| time.overtime.approve | C | C | C (own dept) | - | - | - |
| time.trip.read | CRUD | CRUD | R (own dept) | R (own) | R | R |
| time.trip.create | CRUD | CRUD | - | C (own) | - | CRUD |
| time.trip.approve | C | C | C (own dept) | - | - | - |
| time.overview.read | R | R | R (own dept) | - | R | R |
| time.settings.read | R | R | - | - | - | R |
| time.settings.write | C | - | - | - | - | C |

### Row-Level Security
| Rule | Description |
|---|---|
| Employee self-service | Employees can only access their own attendance, leave, overtime data |
| Department manager | Managers can view and approve data for employees in their department chain |
| HR Administrator | Full access within assigned company/tenant |
| Payroll Specialist | Read-only access to all time data for payroll calculation |
| Cross-tenant isolation | All queries scoped to tenant_id |

---

## 7. State Machine / Workflows

### Leave Request Status Transitions
```
                 +---------+
                 |  DRAFT  |  (employee prepares)
                 +---------+
                      |
                submit()
                      v
                 +---------+
                 | PENDING |  (awaiting manager approval)
                 +---------+
                  /       \
         approve()         reject()
            /                 \
           v                   v
      +----------+       +-----------+
      | APPROVED |       | REJECTED  |
      +----------+       +-----------+
           |                   |
           |             cancel() (if unused)
           v                   v
      +----------+       +-----------+
      |  TAKEN   |       | CANCELLED |
      +----------+       +-----------+
```

### Leave Request Status Table
| From Status | To Status | Trigger | Required Role | Side Effects |
|---|---|---|---|---|
| draft | pending | submit() | Employee | Notify manager, check balance |
| pending | approved | approve() | Manager | Deduct from balance, mark on timesheet |
| pending | rejected | reject() | Manager | Notify employee, no balance change |
| approved | taken | auto (date reached) | System | Mark on timesheet calendar |
| pending | cancelled | cancel() | Employee | Release hold on balance |
| approved | cancelled | cancel() (unused) | Employee/HR | Restore balance, remove from timesheet |

### Overtime Approval Workflow
```
  +---------+     record()      +---------+    approve()     +----------+
  |  DRAFT  | ----------------> | PENDING | ---------------> | APPROVED |
  +---------+                   +---------+                  +----------+
                                    |                              |
                              reject()                     send to payroll
                                    |                              |
                                    v                              v
                              +-----------+                 +-------------+
                              | REJECTED  |                 | IN PAYROLL  |
                              +-----------+                 +-------------+
```

### Timesheet Confirmation Workflow
```
  +-------+     auto-generate     +-------+    confirm()     +----------+
  | DRAFT | --------------------> | DRAFT | ---------------> | CONFIRMED|
  +-------+  (month end)          +-------+                  +----------+
                                                             |
                                                       lock() (auto)
                                                             |
                                                             v
                                                        +--------+
                                                        | LOCKED |
                                                        +--------+
```

### Attendance Status Auto-Calculation
| Condition | Calculated Status |
|---|---|
| Check-in within tolerance of shift start | present |
| Check-in > late_threshold after shift start | late |
| Check-in > severe_threshold after shift start | late (flagged 60min+) |
| Check-out > early_threshold before shift end | early |
| No check-in and no approved leave/ trip | absent |
| Approved leave day | leave (type-specific) |
| Approved business trip day | present (business trip) |
| Sunday/holiday | holiday |

---

## 8. Reports & Analytics

### 8.1 Attendance Summary Report
**Dimensions:** Employee, Department, Month  
**Metrics:** Working days, absent days, leave days, late count, early count, overtime hours  
**Filters:** Date range, department, shift

### 8.2 Overtime Report
**Dimensions:** Employee, Department, Date, Overtime type (normal/weekend/holiday)  
**Metrics:** Total hours, total cost (hours x rate), YTD cumulative  
**Filters:** Date range, department, approval status

### 8.3 Leave Report
**Dimensions:** Employee, Leave type, Department  
**Metrics:** Days taken, days accrued, remaining balance, approval rate  
**Filters:** Date range, department, leave type

### 8.4 Late/Early Frequency Report
**Dimensions:** Employee, Department, Date  
**Metrics:** Late count, total late minutes, early count, total early minutes  
**Filters:** Date range, department, minimum late minutes

### 8.5 Leave Balance Report
**Dimensions:** Employee, Leave type  
**Metrics:** Accrued, used, adjusted, carried over, remaining  
**Filters:** Year, department, leave type

### 8.6 Shift Utilization Report
**Dimensions:** Shift, Department  
**Metrics:** Assigned employee count, attendance rate, average hours  
**Filters:** Date range, department

### 8.7 Attendance Trend Report
**Dimensions:** Month-over-month  
**Metrics:** Attendance rate trend, late rate trend, leave rate trend  
**Filters:** Year range, department

### 8.8 Business Trip Report
**Dimensions:** Employee, Destination, Department  
**Metrics:** Trip count, total trip days, total cost estimate  
**Filters:** Date range, department

### Report Summary Table
| Report | Purpose | Primary Users | Export Formats |
|---|---|---|---|
| Attendance Summary | Monthly attendance overview | HR Admin, Payroll | PDF, Excel |
| Overtime Report | Overtime hours and costs | HR Admin, CFO | PDF, Excel |
| Leave Report | Leave usage analysis | HR Admin, Manager | PDF, Excel |
| Late/Early Report | Attendance discipline | HR Admin, Manager | PDF, Excel |
| Leave Balance | Balance audit | HR Admin, Employee | PDF, Excel |
| Shift Utilization | Shift coverage analysis | HR Admin | PDF, Excel |
| Attendance Trend | Historical patterns | HR Director, CFO | PDF, Excel |
| Business Trip Report | Trip tracking | HR Admin, Manager | PDF, Excel |

---

## 9. Integration Points

### External APIs
| Integration | Direction | Purpose | Frequency |
|---|---|---|---|
| Fingerprint/Face Device API | Inbound | Attendance record ingestion | Real-time (push) or every 5 min (pull) |
| Mobile GPS Check-in API | Inbound | Employee mobile attendance | Real-time |
| M03 Contract Service | Inbound | Employee working hours, status | On shift assignment, on change |
| M12 Payroll Service | Outbound | Timesheet data for salary calculation | On timesheet confirmation |
| M02 Employee Service | Inbound | Employee list, department changes | Real-time (event-driven) |
| Email Service | Outbound | Leave approval notifications | On status change |
| Notification Service | Outbound | Late arrival alerts, overtime pending | On event |
| Calendar Service | Outbound | Sync leave/trip to calendar | On approval |
| Public Holiday API | Inbound | Holiday calendar for attendance | Annual refresh |

### Webhooks
| Event | Trigger | Payload |
|---|---|---|
| attendance.recorded | New attendance event | employee_id, date, status, check_in, check_out |
| leave.submitted | Leave request submitted | request_id, employee_id, type, dates |
| leave.approved | Leave approved | request_id, employee_id, type, dates |
| overtime.recorded | Overtime recorded | overtime_id, employee_id, date, hours |
| overtime.approved | Overtime approved | overtime_id, employee_id, hours, rate |
| timesheet.confirmed | Timesheet confirmed | timesheet_id, employee_id, month, year |
| shift.assigned | Employee assigned to shift | assignment_id, employee_id, shift_code |
| attendance.anomaly | Abnormal attendance detected | employee_id, date, anomaly_type |

### Events Published
| Event | Description | Consumers |
|---|---|---|
| AttendanceRecorded | Check-in/out event | Timesheet Aggregator, Real-time Dashboard |
| LeaveStatusChanged | Leave request status change | Calendar Service, Notification Service, LeaveBalance |
| OvertimeApproved | Overtime approved for payroll | Payroll Service (M12) |
| TimesheetConfirmed | Monthly timesheet finalized | Payroll Service (M12) |
| AttendanceAnomaly | Unusual pattern detected | HR Admin notification |
| OvertimeLimitWarning | Approaching 300h limit | HR Admin, Manager notification |

---

## 10. Technical Notes

### Performance
- **Daily attendance ingestion** from devices: batch process 10,000+ records within 2 minutes.
- **Timesheet generation**: auto-generate for all employees at month-end within 5 minutes.
- **Overview dashboard**: queries use pre-aggregated materialized views, refreshed every 30 seconds.
- **Database indexing**: Composite indexes on (employee_id, date) for attendance_record, (employee_id, month, year) for timesheet.

### Caching
- **Shift definitions** cached in application memory with 1-hour TTL.
- **Leave balances** cached per employee with 5-minute TTL.
- **Overview dashboard** cached with 30-second TTL.
- **Holiday calendar** cached with 24-hour TTL.

### File Handling
- **Leave request attachments** stored in object storage with signed URLs.
- **Timesheet PDF exports** generated on demand, cached for 24 hours.
- **Device sync logs** retained for 90 days.

### Localization
- **Time format**: 24-hour format (HH:mm).
- **Date format**: DD/MM/YYYY.
- **Language**: Vietnamese (primary), English (secondary).
- **Shift names**: Vietnamese labels (CA SANG, CA CHIEU) with English translations.

### Mobile Support
- **Mobile GPS check-in/out** via responsive web or native app.
- **Leave request submission** fully mobile-optimized.
- **Timesheet view** responsive on mobile.
- **Push notifications** for leave approval, late arrival reminders.

### Security
- **Attendance data integrity**: Device records signed to prevent tampering.
- **Manual attendance edits**: Full audit trail with editor identity and reason.
- **Access logging**: All attendance view/edit operations logged.
- **Data retention**: Attendance records retained for 5 years.

### Real-Time Processing
- **Device sync**: Attendance records pushed from devices via webhook or polled every 5 minutes.
- **Status calculation**: Attendance status (present/late/absent) calculated on record creation, recalculated on shift change.
- **Dashboard updates**: Server-sent events (SSE) or WebSocket for real-time dashboard updates.

### Data Integrity
- **Timesheet aggregation**: Nightly job aggregates daily attendance into monthly timesheet summaries.
- **Balance reconciliation**: Monthly job reconciles leave balances with approved requests.
- **Orphan detection**: Periodic job detects and flags attendance records without matching shift assignment.
