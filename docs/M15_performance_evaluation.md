# M15 - Performance Evaluation (Danh gia)

**Category:** HR  
**Priority:** P3  
**Reference Images:** assets/image_43.png, assets/image_44.png, assets/image_45.png, assets/image_46.png  
**Dependencies:** M01 (Organization), M02 (Employee), M12 (Payroll), M16 (Objectives & KPI)  
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Performance Evaluation module manages the complete employee assessment cycle including evaluation period setup, form design, self-evaluation, manager evaluation, peer feedback, competency assessment, results aggregation, and performance improvement planning. It supports multiple evaluation methodologies (MBO/Objectives, Competency-based, Work Report), provides visual analytics including radar charts for multi-axis competency assessment, and links results to bonus calculations and career development.

### Personas
| Persona | Role | Primary Use |
|---|---|---|
| HR Administrator | HR staff | Configure evaluation periods, methods, forms, templates |
| Employee | End user | Complete self-evaluation, view results, review radar charts |
| Department Manager | Manager | Evaluate direct reports, review team performance, calibrate ratings |
| HR Director | Executive | Monitor evaluation progress, review organizational performance, approve final ratings |
| Review Committee | Panel | Calibration meetings, rating normalization across departments |
| System Administrator | IT | Configure rating scales, form templates, automation rules |

### Dependencies
| Dependency | Module | Integration Point |
|---|---|---|
| Employee master data | M02 | Employee list, department, position, manager hierarchy |
| Organization structure | M01 | Department hierarchy, reporting lines |
| Payroll bonus | M12 | Performance bonus calculations based on ratings |
| Objectives & KPI | M16 | Objective achievement data for MBO evaluations |

### Business Rules
1. **Evaluation Period**: Configurable frequency (quarterly, semi-annual, annual). Annual evaluations align with fiscal year.
2. **Evaluation Methods**: Three primary methods: Objectives (MBO) - goal achievement percentage, Competency - skill/behavior assessment, Work Report - narrative performance summary. Multiple methods can be combined.
3. **Multi-Source Feedback**: Supports self-evaluation, manager evaluation, peer feedback, and subordinate feedback (360-degree). Configurable per evaluation type.
4. **Rating Scale**: Configurable scales (e.g., 1-5, 1-10, letter grades). Default: 5-point scale (1=Below Expectations, 2=Needs Improvement, 3=Meets Expectations, 4=Exceeds Expectations, 5=Outstanding).
5. **Weighted Scoring**: Overall score = Sum(Criterion Score x Weight). Weights must sum to 100%.
6. **Completion Deadline**: Each phase (self-eval, manager eval, calibration, final) has its own deadline. Auto-reminders sent before deadline.
7. **Calibration**: Managers participate in calibration sessions to normalize ratings across teams before finalizing.
8. **Result Distribution**: Optional forced distribution (bell curve) configurable: Outstanding 10%, Exceeds 20%, Meets 50%, Needs Improvement 15%, Below 5%.
9. **Performance Improvement Plan (PIP)**: Employees rated below threshold automatically flagged for PIP consideration.
10. **Confidentiality**: Evaluations visible only to employee, their manager chain, and authorized HR staff.
11. **Recurring Evaluations**: Annual evaluations auto-scheduled based on configuration.
12. **Radar Chart Visualization**: 8-axis competency radar showing individual scores against maximum (e.g., 200 points per axis per image data).

---

## 2. Screen Inventory

### 2.1 Create Evaluation Period (3-Step Wizard)
**Route:** `/performance/periods/create`  
**Purpose:** Set up a new evaluation period with configuration wizard (img_43).

| UI Component | Type | Description |
|---|---|---|
| Step 1: General Info | Form | Period name, applied unit (company/department), applicable positions, deadline |
| Step 2: Process | Form | Evaluation methods (Objectives/Competency/Work Report), objective collection time, report template |
| Step 3: Form | Form | Auto-reminder toggle, recurring schedule, evaluator assignments |
| Employee Table | Table | Employees included with positions, departments, assigned managers |
| Navigation | Buttons | Previous, Next, Finish |
| Preview | Panel | Summary of configuration before creation |

**User Interactions:**
- Complete each step sequentially.
- Select evaluation methods (multi-select).
- Configure deadlines and reminders.
- Review and confirm configuration.

**Data Displayed:** Period configuration form, employee enrollment table.

### 2.2 Evaluation Period List
**Route:** `/performance/periods`  
**Purpose:** View and manage all evaluation periods.

| UI Component | Type | Description |
|---|---|---|
| Period Table | Data table | Period name, method, status, employee count, completion rate, deadline |
| Status Filter | Dropdown | Draft, In Progress, Calibration, Finalized |
| Create Button | Button | Start new period |
| Progress Bar | Column | Completion percentage per period |

**User Interactions:**
- Click period to open detail view.
- Create new period via wizard.
- Filter by status.

**Data Displayed:** All evaluation periods with progress tracking.

### 2.3 Gantt Chart Timeline View
**Route:** `/performance/periods/:id/timeline`  
**Purpose:** Visual timeline of evaluation phases and milestones (img_44).

| UI Component | Type | Description |
|---|---|---|
| Gantt Chart | Chart | Timeline with bars for each phase: self-eval, manager eval, calibration, final |
| Regional Breakdown | Sections | Grouped by region/office (HN, HCM, Da Nang, etc.) |
| Objective Bars | Bars | Individual objectives aligned on timeline |
| Milestone Markers | Icons | Deadlines, review meetings |
| Zoom Controls | Slider | Week/month/quarter view |
| Today Line | Vertical line | Current date indicator |

**User Interactions:**
- Zoom in/out for different time granularity.
- Click phase bar for phase details.
- Scroll to navigate timeline.

**Data Displayed:** Evaluation schedule with phase timelines and milestones.

### 2.4 Evaluation Result (Radar Chart)
**Route:** `/performance/results/:employeeId`  
**Purpose:** Visual competency assessment results for an employee (img_45).

| UI Component | Type | Description |
|---|---|---|
| Employee Header | Card | Name, position, department, overall score (e.g., 66.67%) |
| Radar Chart | Chart | 8-axis competency radar: Attitude, Product knowledge, Business knowledge, Product management, Analysis, Project management, Problem solving (200pts), Responsibility |
| Score Breakdown | Table | Each competency with score, max score, percentage |
| Comparison Toggle | Switch | Compare vs previous period or team average |
| Comments Section | List | Evaluator comments per competency |
| Print/Export | Button | Export results as PDF |

**User Interactions:**
- Toggle comparison view (prior period, team average).
- Click axis for detailed breakdown.
- Export results for records.

**Data Displayed:** Individual competency scores with visual radar chart.

### 2.5 Self-Evaluation Form
**Route:** `/performance/evaluations/self`  
**Purpose:** Employee completes self-assessment.

| UI Component | Type | Description |
|---|---|---|
| Evaluation Form | Form | Criteria list with rating inputs and comment fields |
| Rating Input | Star/slider | 1-5 scale per criterion |
| Comment Field | Textarea | Self-assessment narrative per criterion |
| Evidence Upload | File upload | Supporting documents |
| Save Draft | Button | Save without submitting |
| Submit | Button | Submit to manager |
| Progress Indicator | Bar | Completion percentage |

**User Interactions:**
- Rate each criterion.
- Add supporting comments and evidence.
- Save draft or submit to manager.

**Data Displayed:** Evaluation criteria with rating inputs and comment fields.

### 2.6 Manager Evaluation Form
**Route:** `/performance/evaluations/manager/:employeeId`  
**Purpose:** Manager evaluates direct report.

| UI Component | Type | Description |
|---|---|---|
| Employee Summary | Card | Name, position, self-evaluation scores (if submitted) |
| Evaluation Form | Form | Same criteria as self-eval with manager rating inputs |
| Self vs Manager | Comparison | Side-by-side view of self and manager ratings |
| Overall Rating | Display | Calculated overall score |
| Comments | Textarea | Manager comments and development recommendations |
| Submit | Button | Submit to next stage (calibration or finalize) |

**User Interactions:**
- Review employee self-evaluation.
- Provide independent ratings.
- Compare self vs manager ratings.
- Add development comments.

**Data Displayed:** Manager evaluation form with self-evaluation comparison.

### 2.7 Template Library
**Route:** `/performance/settings/templates`  
**Purpose:** Manage evaluation form templates (img_46).

| UI Component | Type | Description |
|---|---|---|
| Template List | Table | 5 templates: HR, Marketing, Sales, Customer Care, Production |
| Template Name | Column | Template name, department applicability |
| Criteria Count | Column | Number of criteria per template |
| Preview | Button | Preview template content |
| Create/Edit | Button | New template or edit existing |
| Duplicate | Button | Clone template for modification |

**User Interactions:**
- Browse available templates.
- Preview template criteria and structure.
- Create new templates for specific departments.
- Duplicate and customize existing templates.

**Data Displayed:** Template inventory with department mapping.

### 2.8 Calibration Dashboard
**Route:** `/performance/calibration`  
**Purpose:** Manager calibration sessions for rating normalization.

| UI Component | Type | Description |
|---|---|---|
| Calibration Session | Card | Session name, participants, date, status |
| Rating Distribution | Chart | Current distribution vs target distribution |
| Employee List | Table | Employees with proposed ratings, manager, department |
| Rating Adjustment | Input | Adjust ratings with required justification |
| Comment Field | Textarea | Calibration notes |
| Finalize Button | Button | Lock calibrated ratings |

**User Interactions:**
- Review proposed ratings across departments.
- Compare distribution to target.
- Adjust ratings with justification.
- Finalize calibrated ratings.

**Data Displayed:** Calibration session data, rating distributions, adjustment tools.

### 2.9 Performance Improvement Plan (PIP)
**Route:** `/performance/pip`  
**Purpose:** Manage PIPs for underperforming employees.

| UI Component | Type | Description |
|---|---|---|
| PIP List | Table | Employee, manager, start date, end date, status, goals |
| Create PIP | Button | New PIP for flagged employee |
| PIP Form | Form | Goals, timeline, checkpoints, success criteria |
| Progress Tracker | Progress bar | PIP completion progress |
| Outcome | Dropdown | Successful, Extended, Terminated |

**User Interactions:**
- Create PIP for flagged employee.
- Track PIP progress through checkpoints.
- Record outcome.

**Data Displayed:** PIP inventory and progress tracking.

### 2.10 Team Performance Overview
**Route:** `/performance/teams`  
**Purpose:** Manager view of team performance results.

| UI Component | Type | Description |
|---|---|---|
| Team Summary | Cards | Team size, average score, distribution |
| Score Distribution | Bar chart | Rating distribution for team |
| Employee List | Table | Employee, score, rating, trend |
| Trend Indicator | Arrow | Up/down/stable vs prior period |
| Export | Button | Export team results |

**User Interactions:**
- Review team performance at a glance.
- Compare to prior periods.
- Export for reporting.

**Data Displayed:** Team-level performance aggregation.

---

## 3. Feature Specifications

### 3.1 Evaluation Period Management
**Description:** Create and manage evaluation cycles with configurable methods, deadlines, and participants.

**User Stories:**
- As an HR Administrator, I can create an evaluation period using a 3-step wizard.
- As an HR Administrator, I can configure which employees and departments are included.
- As an HR Administrator, I can set evaluation methods (Objectives, Competency, Work Report).
- As an HR Administrator, I can enable auto-reminders and recurring schedules.

**Acceptance Criteria:**
1. Period creation wizard has 3 steps: General Info, Process, Form.
2. General Info: period name, applied unit (company/department/position), deadline.
3. Process: evaluation method selection (multi-select: Objectives/Competency/Work Report), objective collection time, report template.
4. Form: auto-reminder toggle, recurring schedule configuration.
5. Employee table shows enrolled employees with positions, departments, managers.
6. Status flow: Draft -> In Progress -> Calibration -> Finalized.
7. Recurring periods auto-created based on schedule (annual, semi-annual, quarterly).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Required fields | All step fields completed | "Complete all required fields in step {step}" |
| Valid deadline | Deadline > today | "Deadline must be in the future" |
| Employee enrollment | At least one employee enrolled | "No employees enrolled for this period" |
| Method selection | At least one method selected | "Select at least one evaluation method" |

**Edge Cases:**
- Employee transfers department mid-evaluation: evaluated by current manager.
- Employee on leave during evaluation: deferred with extended deadline.
- Manager leaves company: evaluation reassigned to manager's manager.

### 3.2 Evaluation Form Builder
**Description:** Create and customize evaluation forms with criteria, weights, and rating scales.

**User Stories:**
- As an HR Administrator, I can create evaluation form templates for different departments.
- As an HR Administrator, I can add criteria with weights and rating scales to a form.
- As an HR Administrator, I can preview a form before publishing.

**Acceptance Criteria:**
1. Form template has: name, description, department applicability, criteria list.
2. Each criterion has: name, description, weight (%), rating scale reference, optional/required flag.
3. Weights must sum to 100% across all criteria in a form.
4. Rating scales configurable: 5-point, 10-point, custom.
5. Templates organized by department: HR, Marketing, Sales, Customer Care, Production (per image data).
6. Forms can be duplicated and customized.
7. Preview shows form as employee will see it.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Weight sum | Sum of weights = 100% | "Criterion weights must total 100% (current: {sum}%)" |
| Unique criterion name | No duplicates in form | "Duplicate criterion name: {name}" |
| Required fields | Name and weight filled | "Missing required field: {field}" |

**Edge Cases:**
- Adding criterion pushes weight over 100%: validation error, cannot save.
- Deleting criterion: recalculate remaining weights or require adjustment.
- Template used in active period: cannot modify, create new version instead.

### 3.3 Self-Evaluation
**Description:** Employees assess their own performance against configured criteria.

**User Stories:**
- As an Employee, I can rate myself on each evaluation criterion.
- As an Employee, I can add comments and upload evidence for each criterion.
- As an Employee, I can save my evaluation as draft and return later.
- As an Employee, I can submit my completed self-evaluation to my manager.

**Acceptance Criteria:**
1. Self-evaluation uses the form template assigned to employee's department/position.
2. Each criterion has rating input (configurable scale) and comment field.
3. Evidence files can be attached per criterion.
4. Draft saved automatically (auto-save every 30 seconds).
5. Submission locks self-evaluation; no further edits allowed.
6. Progress indicator shows completion percentage.
7. Reminder emails sent at configurable intervals before deadline.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| All required rated | All required criteria have ratings | "Rate all required criteria before submitting" |
| Within deadline | Submission before deadline | "Self-evaluation deadline has passed" |
| Rating in range | Rating within scale range | "Rating must be between {min} and {max}" |

**Edge Cases:**
- Employee misses deadline: HR can extend deadline or submit on behalf.
- Employee disputes criteria: HR can modify form for specific employee.
- Part-time employee: pro-rated expectations configurable.

### 3.4 Manager Evaluation
**Description:** Managers evaluate direct reports, with visibility into self-evaluations.

**User Stories:**
- As a Manager, I can view my direct reports' self-evaluations.
- As a Manager, I can rate each direct report on the same criteria.
- As a Manager, I can see side-by-side comparison of self vs manager ratings.
- As a Manager, I can add development recommendations and comments.

**Acceptance Criteria:**
1. Manager evaluation form mirrors self-evaluation form (same criteria, weights).
2. Self-evaluation scores displayed alongside for comparison.
3. Rating discrepancy highlighting: differences >1 level highlighted.
4. Manager overall score calculated using weighted formula.
5. Development comments required for ratings below "Meets Expectations."
6. Submission sends evaluation to calibration or finalization stage.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| All rated | All criteria rated | "Rate all criteria before submitting" |
| Comments for low ratings | Low ratings require comments | "Comments required for ratings below {threshold}" |
| Self-eval submitted | Self-evaluation must be submitted first | "Employee has not submitted self-evaluation" |

**Edge Cases:**
- Manager disagrees with all self-ratings: allowed, with justification comments.
- Employee has no self-evaluation: manager proceeds with independent evaluation.
- Chain of multiple managers: each level adds evaluation, final manager sets overall.

### 3.5 Calibration Process
**Description:** Cross-department calibration sessions to normalize rating distributions.

**User Stories:**
- As an HR Director, I can organize calibration sessions with participating managers.
- As a Manager, I can review and adjust ratings during calibration.
- As an HR Director, I can compare rating distribution across departments.

**Acceptance Criteria:**
1. Calibration groups managers from multiple departments.
2. Rating distribution chart shows current vs target distribution.
3. Managers can propose rating adjustments with required justification.
4. Adjustments logged with proposer and reason.
5. Finalized calibration locks ratings; no further changes without HR Director override.
6. Optional forced distribution enforcement (bell curve).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Justification required | Rating adjustment needs reason | "Provide justification for rating change" |
| Distribution compliance | Distribution within tolerance | "Distribution deviates significantly from target" |

**Edge Cases:**
- Small team (<5 employees): forced distribution not applicable, exempt.
- Manager refuses calibration adjustment: escalated to HR Director.
- Multiple calibration rounds: track version history of ratings.

### 3.6 Results and Analytics (Radar Chart)
**Description:** Visual presentation of evaluation results with multi-axis competency radar.

**User Stories:**
- As an Employee, I can view my evaluation results as a radar chart.
- As an Employee, I can see my scores across 8 competency axes.
- As a Manager, I can compare an employee's results across evaluation periods.
- As an HR Director, I can view organizational competency heatmap.

**Acceptance Criteria:**
1. Radar chart shows 8 axes: Attitude, Product knowledge, Business knowledge, Product management, Analysis, Project management, Problem solving (200pts), Responsibility (per image data).
2. Each axis displays score vs maximum (e.g., 66.67% completion).
3. Comparison overlay: prior period or team average.
4. Score breakdown table with individual criterion details.
5. Export to PDF includes radar chart and detailed scores.
6. Results visible only after period is finalized.

**Edge Cases:**
- Missing criteria: axis shows N/A, overall score excludes missing criteria.
- New criteria not in prior period: comparison shows "N/A" for prior.
- Very high scores (all max): chart shows full polygon, flag for review.

### 3.7 Performance Improvement Plan (PIP)
**Description:** Structured improvement programs for underperforming employees.

**User Stories:**
- As a Manager, I can create a PIP for an employee rated below threshold.
- As a Manager, I can track PIP progress through defined checkpoints.
- As an HR Administrator, I can monitor all active PIPs.

**Acceptance Criteria:**
1. PIP triggered automatically for employees below configurable threshold.
2. PIP includes: goals, timeline (30/60/90 days), checkpoints, success criteria.
3. Checkpoint meetings scheduled with manager and employee.
4. Progress tracked at each checkpoint.
5. Outcomes: Successful (PIP completed), Extended (more time needed), Terminated (employment ended).
6. PIP completion or failure noted in employee record.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Goals required | At least one improvement goal | "Define at least one improvement goal" |
| Valid timeline | Timeline within policy limits | "PIP timeline must be between {min} and {max} days" |

**Edge Cases:**
- Employee on PIP transfers: PIP transferred to new manager.
- Employee resigns during PIP: PIP closed, reason noted.
- PIP successful: follow-up evaluation scheduled at 90 days.

### 3.8 Recurring Evaluation Scheduling
**Description:** Auto-schedule evaluation periods based on recurring configuration.

**User Stories:**
- As an HR Administrator, I can set evaluations to recur annually, semi-annually, or quarterly.
- As a system, I automatically create the next evaluation period before the current one ends.

**Acceptance Criteria:**
1. Recurring frequency: annual, semi-annual, quarterly, custom (N months).
2. Next period created 30 days before current period deadline.
3. Configuration copied from prior period with option to modify.
4. Notification sent to HR Administrator before auto-creation.
5. Auto-creation can be disabled for manual control.

**Edge Cases:**
- Organizational restructuring: recurring schedule paused, manual review required.
- Evaluation method change: next period uses updated configuration.
- Deactivated employee: excluded from recurring enrollment.

---

## 4. Database Design

### ERD (ASCII)
```
+-------------------+       +-------------------+       +-------------------+
| EvaluationPeriod  |1-----*| EvaluationForm    |1-----*| EvaluationCriteria|
+-------------------+       +-------------------+       +-------------------+
| id (PK)           |       | id (PK)           |       | id (PK)           |
| name              |       | period_id (FK)    |       | form_id (FK)      |
| status            |       | template_id (FK)  |       | name              |
| deadline          |       | department_id     |       | weight            |
| methods           |       | created_at        |       | rating_scale_id   |
| auto_reminder     |       +-------------------+       | is_required       |
| recurring         |                                     +-------------------+
| created_at        |       +-------------------+
+-------------------+       | EvaluationTemplate|
                            +-------------------+
                            | id (PK)           |
                            | name              |
                            | department_type   |
                            | criteria_json     |
                            | is_active         |
                            +-------------------+

+-------------------+       +-------------------+       +-------------------+
| EmployeeEvaluation|1-----*| SelfEvaluation    |       | ManagerEvaluation |
+-------------------+       +-------------------+       +-------------------+
| id (PK)           |       | id (PK)           |       | id (PK)           |
| period_id (FK)    |       | emp_eval_id (FK)  |       | emp_eval_id (FK)  |
| employee_id (FK)  |       | criterion_id (FK) |       | criterion_id (FK) |
| form_id (FK)      |       | self_rating       |       | manager_rating    |
| manager_id (FK)   |       | self_comments     |       | manager_comments  |
| overall_score     |       | evidence_urls     |       | submitted_at      |
| status            |       | submitted_at      |       +-------------------+
+-------------------+       +-------------------+

+-------------------+       +-------------------+       +-------------------+
| EvaluationResult  |       | CompetencyModel   |       | RatingScale       |
+-------------------+       +-------------------+       +-------------------+
| id (PK)           |       | id (PK)           |       | id (PK)           |
| emp_eval_id (FK)  |       | name              |       | name              |
| overall_score     |       | description       |       | min_value         |
| rating            |       | competencies_json |       | max_value         |
| finalized_at      |       | is_active         |       | labels_json       |
| pip_flag          |       +-------------------+       | is_active         |
+-------------------+                                 +-------------------+

+-------------------+       +-------------------+
| EvaluationComment |       | PIPRecord         |
+-------------------+       +-------------------+
| id (PK)           |       | id (PK)           |
| emp_eval_id (FK)  |       | employee_id (FK)  |
| author_id (FK)    |       | period_id (FK)    |
| comment           |       | start_date        |
| created_at        |       | end_date          |
+-------------------+       | goals             |
                            | checkpoints_json  |
                            | status            |
                            | outcome           |
                            +-------------------+
```

### Table Schemas

#### evaluation_period
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(200) | NOT NULL | Period display name |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft/in_progress/calibration/finalized |
| start_date | DATE | NOT NULL | Period start |
| end_date | DATE | NOT NULL | Period end |
| deadline | DATE | NOT NULL | Final submission deadline |
| methods | VARCHAR(50)[] | NOT NULL | Array: objectives/competency/work_report |
| objective_collection_time | VARCHAR(50) | NULL | When objectives are collected |
| report_template_id | UUID | FK -> evaluation_template.id, NULL | Report template |
| auto_reminder | BOOLEAN | NOT NULL, DEFAULT true | Send auto-reminders |
| reminder_days_before | INT[] | NULL | Days before deadline to send reminders |
| recurring | VARCHAR(20) | NULL | annual/semi_annual/quarterly/custom |
| recurring_interval | INT | NULL | Months between recurring periods |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### evaluation_template
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(100) | NOT NULL | Template name (HR, Marketing, Sales, etc.) |
| department_type | VARCHAR(50) | NULL | Applicable department type |
| criteria_json | JSONB | NOT NULL | Criteria definitions with weights |
| description | TEXT | NULL | Template description |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### evaluation_form
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| period_id | UUID | FK -> evaluation_period.id, NOT NULL | Evaluation period |
| template_id | UUID | FK -> evaluation_template.id, NULL | Source template |
| department_id | UUID | FK -> departments.id, NULL | Applicable department |
| name | VARCHAR(200) | NOT NULL | Form name |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### evaluation_criteria
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| form_id | UUID | FK -> evaluation_form.id, NOT NULL | Evaluation form |
| name | VARCHAR(200) | NOT NULL | Criterion name (e.g., Attitude, Product knowledge) |
| description | TEXT | NULL | Criterion description |
| weight | DECIMAL(5,2) | NOT NULL | Weight percentage (must sum to 100 per form) |
| rating_scale_id | UUID | FK -> rating_scale.id, NOT NULL | Rating scale |
| max_score | DECIMAL(5,2) | NOT NULL | Maximum score (e.g., 200) |
| is_required | BOOLEAN | NOT NULL, DEFAULT true | Required to complete |
| sort_order | INT | NOT NULL | Display order |

#### employee_evaluation
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| period_id | UUID | FK -> evaluation_period.id, NOT NULL | Evaluation period |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee being evaluated |
| manager_id | UUID | FK -> users.id, NOT NULL | Evaluating manager |
| form_id | UUID | FK -> evaluation_form.id, NOT NULL | Assigned form |
| self_status | VARCHAR(20) | NOT NULL, DEFAULT 'not_started' | not_started/in_progress/submitted |
| manager_status | VARCHAR(20) | NOT NULL, DEFAULT 'not_started' | not_started/in_progress/submitted |
| overall_score | DECIMAL(5,2) | NULL | Final calculated score |
| overall_percentage | DECIMAL(5,2) | NULL | Score as percentage |
| rating | VARCHAR(50) | NULL | Final rating label |
| pip_flag | BOOLEAN | NOT NULL, DEFAULT false | Flagged for PIP |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### self_evaluation
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| emp_eval_id | UUID | FK -> employee_evaluation.id, NOT NULL | Employee evaluation |
| criterion_id | UUID | FK -> evaluation_criteria.id, NOT NULL | Criterion |
| self_rating | DECIMAL(5,2) | NULL | Self-rating score |
| self_comments | TEXT | NULL | Self-assessment comments |
| evidence_urls | TEXT[] | NULL | Supporting document URLs |
| submitted_at | TIMESTAMPTZ | NULL | Submission timestamp |

#### manager_evaluation
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| emp_eval_id | UUID | FK -> employee_evaluation.id, NOT NULL | Employee evaluation |
| criterion_id | UUID | FK -> evaluation_criteria.id, NOT NULL | Criterion |
| manager_rating | DECIMAL(5,2) | NULL | Manager rating score |
| manager_comments | TEXT | NULL | Manager evaluation comments |
| development_comments | TEXT | NULL | Development recommendations |
| submitted_at | TIMESTAMPTZ | NULL | Submission timestamp |

#### evaluation_result
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| emp_eval_id | UUID | FK -> employee_evaluation.id, NOT NULL | Employee evaluation |
| overall_score | DECIMAL(5,2) | NOT NULL | Final score |
| overall_percentage | DECIMAL(5,2) | NOT NULL | Percentage of maximum |
| rating | VARCHAR(50) | NOT NULL | Final rating label |
| rating_scale_id | UUID | FK -> rating_scale.id, NOT NULL | Rating scale used |
| finalized_at | TIMESTAMPTZ | NULL | Finalization timestamp |
| finalized_by | UUID | FK -> users.id, NULL | Who finalized |
| pip_flag | BOOLEAN | NOT NULL, DEFAULT false | PIP recommendation |
| calibration_notes | TEXT | NULL | Notes from calibration |

#### rating_scale
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(100) | NOT NULL | Scale name |
| min_value | INT | NOT NULL | Minimum value (e.g., 1) |
| max_value | INT | NOT NULL | Maximum value (e.g., 5) |
| labels_json | JSONB | NOT NULL | Labels per value (e.g., {"1": "Below", "5": "Outstanding"}) |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |

#### competency_model
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(100) | NOT NULL | Model name |
| description | TEXT | NULL | Model description |
| competencies_json | JSONB | NOT NULL | Competency definitions (8 axes) |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |

#### evaluation_comment
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| emp_eval_id | UUID | FK -> employee_evaluation.id, NOT NULL | Employee evaluation |
| author_id | UUID | FK -> users.id, NOT NULL | Comment author |
| comment | TEXT | NOT NULL | Comment text |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### pip_record
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| employee_id | UUID | FK -> employees.id, NOT NULL | Employee |
| period_id | UUID | FK -> evaluation_period.id, NULL | Triggering period |
| start_date | DATE | NOT NULL | PIP start date |
| end_date | DATE | NOT NULL | PIP end date |
| goals | JSONB | NOT NULL | Improvement goals |
| checkpoints_json | JSONB | NOT NULL | Checkpoint schedule |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active/completed/extended/terminated |
| outcome | VARCHAR(30) | NULL | successful/extended/terminated |
| manager_id | UUID | FK -> users.id, NOT NULL | Managing manager |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

### All Tables Summary
| Table | Purpose | Key Relationships |
|---|---|---|
| evaluation_period | Evaluation cycle management | Parent of EvaluationForm, EmployeeEvaluation |
| evaluation_template | Form templates (HR, Marketing, etc.) | Referenced by EvaluationForm |
| evaluation_form | Period-specific form instances | Links Period, Template |
| evaluation_criteria | Form criteria with weights | Links EvaluationForm |
| employee_evaluation | Per-employee evaluation record | Links Period, Employee, Manager, Form |
| self_evaluation | Self-rating per criterion | Links EmployeeEvaluation |
| manager_evaluation | Manager rating per criterion | Links EmployeeEvaluation |
| evaluation_result | Final evaluation outcome | Links EmployeeEvaluation |
| rating_scale | Rating scale definitions | Referenced by Criteria, Results |
| competency_model | Competency framework (8 axes) | Standalone configuration |
| evaluation_comment | Comments on evaluations | Links EmployeeEvaluation |
| pip_record | Performance improvement plans | Links Employee, Period |

---

## 5. API Specifications

### JSON Response Format
```json
{
  "success": true,
  "data": { ... },
  "meta": { "page": 1, "per_page": 20, "total": 55 }
}
```

### Error Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Criterion weights must total 100% (current: 85%)",
    "details": [
      { "field": "weight", "message": "Remaining weight to allocate: 15%" }
    ]
  }
}
```

### Endpoints Summary
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| GET | `/api/performance/periods` | List evaluation periods | perf.period.read |
| POST | `/api/performance/periods` | Create evaluation period | perf.period.create |
| GET | `/api/performance/periods/:id` | Get period detail | perf.period.read |
| PATCH | `/api/performance/periods/:id/status` | Update period status | perf.period.update |
| GET | `/api/performance/periods/:id/timeline` | Get Gantt chart data | perf.period.read |
| GET | `/api/performance/templates` | List form templates | perf.settings.read |
| POST | `/api/performance/templates` | Create template | perf.settings.write |
| PUT | `/api/performance/templates/:id` | Update template | perf.settings.write |
| GET | `/api/performance/evaluations/self` | Get own self-evaluation | perf.evaluation.read (self) |
| PUT | `/api/performance/evaluations/self` | Update self-evaluation | perf.evaluation.self_write |
| POST | `/api/performance/evaluations/self/submit` | Submit self-evaluation | perf.evaluation.self_submit |
| GET | `/api/performance/evaluations/manager` | List direct report evaluations | perf.evaluation.manager_read |
| GET | `/api/performance/evaluations/manager/:empId` | Get manager evaluation form | perf.evaluation.manager_read |
| PUT | `/api/performance/evaluations/manager/:empId` | Update manager evaluation | perf.evaluation.manager_write |
| POST | `/api/performance/evaluations/manager/:empId/submit` | Submit manager evaluation | perf.evaluation.manager_submit |
| GET | `/api/performance/results/:employeeId` | Get evaluation results (radar) | perf.result.read |
| GET | `/api/performance/calibration` | Calibration session data | perf.calibration.read |
| POST | `/api/performance/calibration/:id/adjust` | Adjust rating in calibration | perf.calibration.adjust |
| POST | `/api/performance/calibration/:id/finalize` | Finalize calibration | perf.calibration.finalize |
| GET | `/api/performance/pip` | List PIPs | perf.pip.read |
| POST | `/api/performance/pip` | Create PIP | perf.pip.create |
| PATCH | `/api/performance/pip/:id` | Update PIP | perf.pip.update |
| GET | `/api/performance/teams` | Team performance overview | perf.team.read |

### Endpoint Details

#### GET /api/performance/results/:employeeId
**Response:**
```json
{
  "success": true,
  "data": {
    "employee": { "id": "emp-123", "name": "Doan Thi Hien", "position": "Business Analyst" },
    "period": { "id": "ep-001", "name": "Annual Evaluation 2025" },
    "overall_score": 133.34,
    "overall_percentage": 66.67,
    "rating": "Meets Expectations",
    "radar_axes": [
      { "name": "Attitude", "score": 150, "max": 200 },
      { "name": "Product knowledge", "score": 140, "max": 200 },
      { "name": "Business knowledge", "score": 130, "max": 200 },
      { "name": "Product management", "score": 120, "max": 200 },
      { "name": "Analysis", "score": 160, "max": 200 },
      { "name": "Project management", "score": 110, "max": 200 },
      { "name": "Problem solving", "score": 140, "max": 200 },
      { "name": "Responsibility", "score": 150, "max": 200 }
    ]
  }
}
```

#### POST /api/performance/periods
**Request Body:**
```json
{
  "name": "Annual Evaluation 2026",
  "start_date": "2026-01-01",
  "end_date": "2026-01-31",
  "deadline": "2026-01-31",
  "methods": ["objectives", "competency"],
  "objective_collection_time": "2026-01-15",
  "auto_reminder": true,
  "reminder_days_before": [7, 3, 1],
  "recurring": "annual"
}
```

---

## 6. Permissions Matrix

### Roles
| Role | Description |
|---|---|
| HR Director | Full access to all performance functions, calibration finalization |
| HR Administrator | Configure periods, templates, manage PIPs, monitor progress |
| Department Manager | Evaluate direct reports, participate in calibration |
| Employee | View own evaluations, complete self-evaluation |
| Review Committee | Access to calibration sessions |

### Permission Matrix (CRUD)
| Permission | HR Director | HR Admin | Dept. Mgr. | Employee | Committee |
|---|---|---|---|---|---|
| perf.period.read | CRUD | CRUD | R | R | R |
| perf.period.create | C | C | - | - | - |
| perf.period.update | CRUD | CRUD | - | - | - |
| perf.settings.read | R | R | R | R | R |
| perf.settings.write | C | C | - | - | - |
| perf.evaluation.read | CRUD | CRUD | R (own dept) | R (own) | R |
| perf.evaluation.self_write | CRUD | - | - | C (own) | - |
| perf.evaluation.self_submit | C | - | - | C (own) | - |
| perf.evaluation.manager_read | CRUD | R | CRUD | - | - |
| perf.evaluation.manager_write | CRUD | - | C (own dept) | - | - |
| perf.evaluation.manager_submit | C | - | C (own dept) | - | - |
| perf.result.read | CRUD | CRUD | R (own dept) | R (own) | R |
| perf.calibration.read | CRUD | R | R (participant) | - | R |
| perf.calibration.adjust | C | - | C (participant) | - | C |
| perf.calibration.finalize | C | - | - | - | - |
| perf.pip.read | CRUD | CRUD | R (own dept) | R (own) | R |
| perf.pip.create | C | C | C (own dept) | - | - |
| perf.pip.update | CRUD | CRUD | C (own dept) | - | - |
| perf.team.read | R | R | R (own dept) | - | R |

### Row-Level Security
| Rule | Description |
|---|---|
| Employee self-service | Employees only see their own evaluation data |
| Manager scoping | Managers only evaluate and see data for their direct reports |
| Calibration access | Committee members see all evaluations in calibration scope |
| Confidentiality | Evaluation comments and ratings only visible to authorized roles |
| Cross-tenant isolation | All queries scoped to tenant_id |

---

## 7. State Machine / Workflows

### Evaluation Period Status Transitions
```
  +-------+     activate()     +------------+   deadline/self-eval   +-------------+
  | DRAFT | -----------------> | IN PROGRESS| ---------------------> | IN PROGRESS |
  +-------+                    +------------+  (self-eval phase)     | (mgr phase) |
                                                                      +-------------+
                                                                            |
                                                                     deadline/mgr-eval
                                                                            |
                                                                            v
                                                                   +-------------+
                                                                   | CALIBRATION |
                                                                   +-------------+
                                                                         |
                                                                   finalize()
                                                                         |
                                                                         v
                                                                   +-----------+
                                                                   | FINALIZED |
                                                                   +-----------+
```

### Status Transition Table
| From Status | To Status | Trigger | Required Role | Side Effects |
|---|---|---|---|---|
| draft | in_progress | activate() | HR Administrator | Notify all enrolled employees, open self-evaluation |
| in_progress (self) | in_progress (mgr) | self-eval deadline | System (auto) | Notify managers, open manager evaluation |
| in_progress (mgr) | calibration | mgr-eval deadline | System (auto) | Notify calibration participants |
| calibration | finalized | finalize() | HR Director | Lock all ratings, generate results, trigger bonus calc |

### Self-Evaluation Status
```
  +-------------+     start()      +------------+     submit()     +-----------+
  | NOT STARTED | ----------------> | IN PROGRESS | ---------------> | SUBMITTED |
  +-------------+                  +------------+                  +-----------+
```

### Manager Evaluation Status
```
  +-------------+     start()      +------------+     submit()     +-----------+
  | NOT STARTED | ----------------> | IN PROGRESS | ---------------> | SUBMITTED |
  +-------------+                  +------------+                  +-----------+
```

### PIP Status Transitions
```
  +--------+     create()     +--------+    checkpoint()    +-----------+
  | DRAFT  | ----------------> | ACTIVE | -----------------> |  ACTIVE   |
  +--------+                  +--------+  (per checkpoint)  +-----------+
                                     |                           |
                               complete()                  complete()/extend()
                                     |                           |
                                     v                           v
                               +-------------+            +-------------+
                               | COMPLETED   |            |  EXTENDED   |
                               | (successful)|            +-------------+
                               +-------------+
```

---

## 8. Reports & Analytics

### 8.1 Evaluation Completion Report
**Dimensions:** Period, Department, Manager  
**Metrics:** Total employees, self-eval submitted, manager eval submitted, completion rate  
**Filters:** Period, department, status

### 8.2 Rating Distribution Report
**Dimensions:** Department, Period, Rating level  
**Metrics:** Employee count per rating, percentage, vs target distribution  
**Filters:** Period, department

### 8.3 Competency Analysis Report
**Dimensions:** Competency axis, Department  
**Metrics:** Average score, min, max, distribution per competency  
**Filters:** Period, department, position

### 8.4 Performance Trend Report
**Dimensions:** Employee, Period-over-period  
**Metrics:** Score trend, rating trend, improvement/decline  
**Filters:** Year range, department, employee

### 8.5 Self vs Manager Gap Analysis
**Dimensions:** Department, Criterion  
**Metrics:** Average self-rating, average manager rating, gap percentage  
**Filters:** Period, department

### 8.6 PIP Report
**Dimensions:** Department, PIP status  
**Metrics:** Active PIPs, completed, extended, terminated, success rate  
**Filters:** Date range, department

### 8.7 Calibration Adjustment Report
**Dimensions:** Manager, Department  
**Metrics:** Ratings proposed, adjustments made, adjustment direction (up/down)  
**Filters:** Period

### Report Summary Table
| Report | Purpose | Primary Users | Export Formats |
|---|---|---|---|
| Completion Report | Progress tracking | HR Admin, HR Director | PDF, Excel |
| Rating Distribution | Rating fairness | HR Director, Committee | PDF, Excel |
| Competency Analysis | Skill gap identification | HR Director, Manager | PDF, Excel |
| Performance Trend | Individual progress | Manager, HR Director | PDF, Excel |
| Self vs Manager Gap | Perception alignment | HR Director | PDF, Excel |
| PIP Report | Underperformance tracking | HR Director, Manager | PDF, Excel |
| Calibration Report | Calibration effectiveness | HR Director | PDF, Excel |

---

## 9. Integration Points

### External APIs
| Integration | Direction | Purpose | Frequency |
|---|---|---|---|
| M02 Employee Service | Inbound | Employee list, hierarchy, department changes | Real-time (event-driven) |
| M16 Objectives Service | Inbound | Objective achievement data for MBO method | On evaluation calculation |
| M12 Payroll Service | Outbound | Performance bonus data | On period finalized |
| Email Service | Outbound | Reminders, notifications, result delivery | On event |
| Calendar Service | Outbound | Evaluation deadlines, checkpoint scheduling | On schedule |
| Notification Service | Outbound | Push notifications for deadlines | On event |

### Webhooks
| Event | Trigger | Payload |
|---|---|---|
| period.activated | Evaluation period starts | period_id, employee_count |
| self_eval.submitted | Employee submits self-eval | emp_eval_id, employee_id |
| mgr_eval.submitted | Manager submits evaluation | emp_eval_id, manager_id |
| period.finalized | Evaluation period finalized | period_id, employee_count |
| pip.created | PIP created | pip_id, employee_id |
| pip.completed | PIP outcome determined | pip_id, outcome |

### Events Published
| Event | Description | Consumers |
|---|---|---|
| PeriodStatusChanged | Period status update | Notification Service, Analytics |
| EvaluationSubmitted | Self or manager evaluation submitted | Period Progress Tracker |
| PeriodFinalized | Results ready, bonus calc triggered | Payroll Service (M12) |
| PIPCreated | Underperformance flagged | Manager notification, Employee notification |

---

## 10. Technical Notes

### Performance
- **Radar chart rendering**: Pre-compute scores, chart data served in <100ms.
- **Bulk evaluation generation**: Create evaluations for 1,000+ employees within 30 seconds.
- **Calibration dashboard**: Load all ratings for 500+ employees within 2 seconds.
- **Database indexing**: Composite indexes on (period_id, employee_id) for employee_evaluation, (period_id, manager_id) for manager queries.

### Caching
- **Form templates** cached with 1-hour TTL.
- **Rating scale definitions** cached with 24-hour TTL.
- **Competency model** cached with 1-hour TTL.
- **Radar chart data** cached per employee with 5-minute TTL.

### File Handling
- **Evidence documents** stored in object storage with signed URLs, 3-year retention.
- **Evaluation result PDFs** generated on demand, cached for 7 days.
- **Maximum file size**: 10MB per evidence document.

### Localization
- **Language**: Vietnamese (primary), English (secondary).
- **Rating scale labels**: Localized per language setting.
- **Competency names**: Vietnamese labels with English translations available.
- **Date format**: DD/MM/YYYY.

### Mobile Support
- **Self-evaluation form** fully mobile-optimized.
- **Radar chart results** viewable on mobile (responsive chart).
- **Manager evaluation form** tablet-optimized.
- **Push notifications** for evaluation reminders, deadline alerts.

### Security
- **Evaluation data** encrypted at rest (AES-256).
- **Confidential ratings**: Only authorized roles can view evaluation results.
- **Audit logging**: All evaluation create/update/delete operations logged.
- **Data retention**: Evaluation records retained for 5 years.

### Auto-Reminder System
- **Configurable intervals**: Reminders sent at configurable days before deadline (e.g., 7, 3, 1 days).
- **Escalation**: After deadline, escalation emails to manager and HR Administrator.
- **Channel**: Email + push notification.
- **Frequency**: Maximum 1 reminder per day per person.

### Calibration Algorithm
- **Distribution calculation**: Real-time calculation of rating distribution per department.
- **Variance detection**: Flag departments deviating >10% from target distribution.
- **Normalization suggestion**: Auto-suggest rating adjustments to align with target.
