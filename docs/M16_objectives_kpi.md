# M16 - Objectives & KPI (Muc tieu)

**Category:** HR  
**Priority:** P3  
**Reference Images:** assets/image_39.png, assets/image_40.png, assets/image_41.png, assets/image_42.png  
**Dependencies:** M01 (Organization), M02 (Employee), M15 (Performance Evaluation)  
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Objectives & KPI module enables strategic goal management from organizational vision down to individual assignments. It supports MBO (Management by Objectives), OKR (Objectives and Key Results), and BSC (Balanced Scorecard) frameworks. Objectives cascade from company-level strategy through departments to individuals, with progress tracking, value measurement, mind map visualization, and BSC strategy maps covering Financial, Customer, Internal Process, and Learning & Growth perspectives.

### Personas
| Persona | Role | Primary Use |
|---|---|---|
| CEO / Executive | Leadership | Define company-level strategic objectives, review BSC strategy map |
| Department Manager | Manager | Define department objectives, cascade to team members, track progress |
| Employee | End user | View assigned objectives, update progress, track personal goals |
| Strategy Officer | Planning | Design BSC perspectives, define strategic maps, monitor alignment |
| HR Director | HR | Link objectives to performance evaluations, monitor goal completion |
| System Administrator | IT | Configure objective types, KPI templates, calculation methods |

### Dependencies
| Dependency | Module | Integration Point |
|---|---|---|
| Organization structure | M01 | Department hierarchy for objective cascading |
| Employee master data | M02 | Objective assignments to employees |
| Performance Evaluation | M15 | Objective achievement feeds into performance ratings |
| Payroll | M12 | KPI achievement may trigger bonus calculations |

### Business Rules
1. **Objective Types**: MBO (Management by Objectives) - specific measurable targets, OKR (Objectives and Key Results) - aspirational objectives with measurable key results, KPI (Key Performance Indicator) - ongoing metric tracking.
2. **Cascading Hierarchy**: Company -> Department -> Team -> Individual. Each level inherits and breaks down parent objectives.
3. **BSC Four Perspectives**: Financial (revenue, profit, cost), Customer (satisfaction, retention, market share), Internal Process (efficiency, quality, cycle time), Learning & Growth (employee development, innovation, culture).
4. **Progress Calculation**: Progress = (Current Value - Start Value) / (Target Value - Start Value) x 100%. Capped at 100%.
5. **Value Units**: Configurable units: VND (billions), percentage, count, score, custom.
6. **Period Types**: Annual, quarterly, monthly, custom date range.
7. **Target Setting**: Targets set at department level and/or individual level. Department target = sum of individual targets or independent target.
8. **Calculation Methods**: Sum, Average, Minimum, Maximum, Weighted Average. Determines how child objective values aggregate to parent.
9. **Parent-Child Link**: Child objectives roll up to parent. Parent progress = aggregation of child progress per calculation method.
10. **Mind Map Visualization**: Objectives displayed as hierarchical mind map with completion status, quarterly breakdown, individual assignments.
11. **Objective Templates**: Pre-defined objective templates for common goals.
12. **Attachments**: Supporting documents can be attached to objectives.

---

## 2. Screen Inventory

### 2.1 Objective Table View
**Route:** `/objectives/list`  
**Purpose:** Hierarchical table view of all objectives with org tree (img_39).

| UI Component | Type | Description |
|---|---|---|
| Org Tree | Tree panel | Department hierarchy on left |
| Objective Table | Data table | Objective name, type (MBO/OKR/KPI), progress bar (50%), target value, current value, assignee, period |
| Value Display | Column | Values in billions VND (e.g., 50B) |
| Progress Bar | Visual | Percentage completion with color coding |
| Type Badge | Badge | MBO, OKR, KPI labels |
| Expand/Collapse | Toggle | Expand objective tree to show children |
| Filter | Multi-select | Type, period, department, status |
| Add Button | Button | Create new objective |

**User Interactions:**
- Expand/collapse objective hierarchy.
- Filter by type, period, department.
- Click objective to edit detail.
- Add new objective with parent selection.

**Data Displayed:** Hierarchical objective list with progress, values, types.

### 2.2 Mind Map View
**Route:** `/objectives/mindmap`  
**Purpose:** Visual mind map of objectives with quarterly breakdown (img_40).

| UI Component | Type | Description |
|---|---|---|
| Mind Map Canvas | Diagram | Central objective branching to sub-objectives |
| Quarterly Nodes | Node | Q1, Q2, Q3, Q4 breakdown nodes |
| Assignment Labels | Badge | Individual assignments per sub-objective |
| Completion Status | Color | Green (complete), Yellow (in progress), Red (behind) |
| Zoom/Pan | Controls | Navigate large mind maps |
| Collapse/Expand | Click | Toggle node details |

**User Interactions:**
- Navigate mind map with zoom and pan.
- Click node to see objective details.
- Expand/collapse branches.
- Color indicates completion status at a glance.

**Data Displayed:** Visual objective hierarchy with assignments and status.

### 2.3 BSC Strategy Map
**Route:** `/objectives/bsc-map`  
**Purpose:** Balanced Scorecard strategy map with four perspectives (img_41).

| UI Component | Type | Description |
|---|---|---|
| Financial Perspective | Section | 4 objectives in financial category |
| Customer Perspective | Section | 6 objectives in customer category |
| Internal Process | Section | 5 objectives in process category |
| Learning & Growth | Section | 5 objectives in L&G category |
| Causal Links | Arrows | Cause-effect relationships between perspectives |
| Objective Cards | Card | Name, KPI, target, current value, progress |
| Legend | Panel | Perspective color coding |

**User Interactions:**
- View objectives organized by BSC perspective.
- Follow causal links to understand strategy logic.
- Click objective card for details.
- Drag to reorganize (if strategy officer).

**Data Displayed:** BSC strategy map with 4 perspectives: Financial (4), Customer (6), Internal Process (5), Learning & Growth (5).

### 2.4 Add Objective Form
**Route:** `/objectives/create`  
**Purpose:** Create new objective with full configuration (img_42).

| UI Component | Type | Description |
|---|---|---|
| Type Selector | Radio | MBO / OKR / KPI |
| Name Field | Input | Objective name |
| Max Value | Number | Maximum achievable value |
| Unit Selector | Dropdown | VND, percentage, count, score, custom |
| Period | Date range | Objective time period |
| Target Selector | Radio | Department target / Individual target |
| Department | Dropdown | Responsible department |
| Representative | Dropdown | Objective owner/responsible person |
| Calculation Method | Dropdown | Sum / Average / Min / Max / Weighted Avg |
| Parent Objective | Tree select | Parent objective in hierarchy |
| Attachments | File upload | Supporting documents |
| Description | Textarea | Objective description |
| Save Button | Button | Create objective |

**User Interactions:**
- Select objective type and configure all fields.
- Link to parent objective for cascading.
- Upload supporting documents.
- Save to create objective.

**Data Displayed:** Full objective creation form.

### 2.5 Objective Detail View
**Route:** `/objectives/:id`  
**Purpose:** Detailed view of a single objective.

| UI Component | Type | Description |
|---|---|---|
| Objective Header | Card | Name, type, status, period |
| Progress Section | Progress bar | Current vs target with percentage |
| Value History | Line chart | Value trend over time |
| Child Objectives | Table | Sub-objectives with individual progress |
| Assignees | List | Assigned individuals with personal targets |
| Comments | Thread | Discussion thread |
| Edit Button | Button | Modify objective (if authorized) |

**User Interactions:**
- View progress and value history.
- Review child objectives and assignees.
- Participate in discussion.
- Edit objective configuration.

**Data Displayed:** Comprehensive objective detail with progress, values, assignments.

### 2.6 Key Result Management (OKR)
**Route:** `/objectives/:id/key-results`  
**Purpose:** Manage key results for OKR-type objectives.

| UI Component | Type | Description |
|---|---|---|
| Key Result List | Table | Key result name, target, current, progress |
| Add Key Result | Button | New key result |
| Key Result Form | Modal | Name, target value, unit, start value |
| Progress Update | Input | Update current value |

**User Interactions:**
- Add key results to OKR objective.
- Update key result progress.
- View aggregated OKR progress.

**Data Displayed:** Key results with individual and aggregated progress.

### 2.7 Objective Comments and Updates
**Route:** `/objectives/:id/comments`  
**Purpose:** Track discussion and progress updates on objectives.

| UI Component | Type | Description |
|---|---|---|
| Comment Thread | List | Comments with author, timestamp, content |
| Progress Update | Form | Update current value with note |
| Add Comment | Textarea | New comment |
| Attachment | File upload | Attach supporting documents |

**User Interactions:**
- Add comments and progress updates.
- Attach evidence of progress.
- Follow discussion thread.

**Data Displayed:** Comment history and progress update log.

---

## 3. Feature Specifications

### 3.1 Objective Hierarchy Management
**Description:** Create and manage cascading objectives from company level to individual.

**User Stories:**
- As a CEO, I can define company-level strategic objectives.
- As a Department Manager, I can create department objectives that cascade from company objectives.
- As a Manager, I can assign individual objectives to team members.
- As an Employee, I can view my assigned objectives and track progress.

**Acceptance Criteria:**
1. Objective types: MBO, OKR, KPI.
2. Cascading hierarchy: Parent -> Child with unlimited depth.
3. Each objective has: name, type, target value, current value, unit, period, assignee, department, calculation method.
4. Parent objective progress auto-calculated from children using configured calculation method.
5. Department and individual targets can be set independently.
6. Attachments supported for evidence and reference.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| No circular reference | Parent chain has no loops | "Cannot set parent: would create circular reference" |
| Valid target | Target value > 0 | "Target value must be positive" |
| Current <= Max | Current value <= max value | "Current value cannot exceed maximum" |
| Required fields | Name, type, period filled | "Missing required field: {field}" |

**Edge Cases:**
- Objective period changes: child periods must be within parent period.
- Department restructuring: objectives reassigned to new department.
- Objective deleted: children can be orphaned or reassigned to new parent.

### 3.2 Progress Tracking
**Description:** Track and calculate objective progress with configurable methods.

**User Stories:**
- As an Employee, I can update progress on my assigned objectives.
- As a system, I automatically calculate parent objective progress from children.
- As a Manager, I can view progress trends for my department's objectives.

**Acceptance Criteria:**
1. Progress formula: (Current - Start) / (Target - Start) x 100%, capped at 100%.
2. Calculation methods: Sum, Average, Min, Max, Weighted Average.
3. Progress updates logged with timestamp and author.
4. Value history chart shows progress over time.
5. Visual indicators: Green >=80%, Yellow 50-79%, Red <50%.
6. Over-achievement allowed: progress can exceed 100% (displayed as actual percentage).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid value | Current value >= 0 | "Value must be non-negative" |
| Within period | Updates only within objective period | "Objective period has ended" |
| Reason for large changes | Changes >20% require note | "Provide reason for significant value change" |

**Edge Cases:**
- No start value defined: progress = Current / Target x 100%.
- Target changed mid-period: recalculate progress with new target.
- Objective completed early: mark as complete, no further updates.

### 3.3 BSC Strategy Map
**Description:** Visual strategy map organized by Balanced Scorecard perspectives.

**User Stories:**
- As a Strategy Officer, I can define objectives under each BSC perspective.
- As a Strategy Officer, I can draw causal links between objectives across perspectives.
- As an Executive, I can view the complete BSC strategy map.

**Acceptance Criteria:**
1. Four perspectives: Financial, Customer, Internal Process, Learning & Growth.
2. Each perspective displays assigned objectives with status.
3. Causal links drawn between objectives to show cause-effect relationships.
4. Color-coded by perspective: Financial (blue), Customer (green), Process (orange), L&G (purple).
5. Objective cards show: name, KPI, target, current, progress.
6. Map layout follows standard BSC format: L&G (bottom) -> Process -> Customer -> Financial (top).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid link | Link connects different perspectives | "Links should connect objectives across perspectives" |
| No circular links | No circular causal chains | "Circular causal link detected" |

**Edge Cases:**
- Objective fits multiple perspectives: assign primary perspective, note secondary.
- Strategy restructuring: reorganize map, preserve objective data.

### 3.4 OKR Key Results
**Description:** Define and track key results for OKR-type objectives.

**User Stories:**
- As an objective owner, I can define multiple key results for an OKR objective.
- As an objective owner, I can update individual key result progress.
- As a viewer, I can see aggregated OKR progress from all key results.

**Acceptance Criteria:**
1. OKR objective has 2-5 key results.
2. Each key result has: name, start value, target value, current value, unit.
3. Key result progress calculated independently.
4. OKR overall progress = average of key result progress (configurable: average or weighted).
5. Key results are measurable (numeric values required).
6. Key results are time-bound (within objective period).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Min key results | At least 2 key results | "OKR requires at least 2 key results" |
| Max key results | Maximum 5 key results | "OKR supports maximum 5 key results" |
| Measurable | Target value defined | "Key result must have a measurable target" |

**Edge Cases:**
- Key result completed early: mark complete, overall progress updates.
- Key result modified mid-period: version history preserved.
- Binary key result (done/not done): use 0/100 values.

### 3.5 Mind Map Visualization
**Description:** Visual mind map representation of objective hierarchy.

**User Stories:**
- As an Executive, I can view objectives as a mind map to understand relationships.
- As a Manager, I can see quarterly breakdowns and individual assignments on the mind map.
- As a viewer, I can see completion status at a glance through color coding.

**Acceptance Criteria:**
1. Central node is top-level objective.
2. Branches expand to show child objectives.
3. Quarterly nodes (Q1-Q4) show temporal breakdown.
4. Assignment badges show responsible individuals.
5. Color coding: Green (>=80%), Yellow (50-79%), Red (<50%), Gray (not started).
6. Zoom and pan for large maps.
7. Click node to open objective detail panel.

**Edge Cases:**
- Very large mind maps: lazy-load branches, virtual rendering.
- Deep hierarchies (>10 levels): auto-collapse beyond configured depth.
- Multiple root objectives: display as separate mind maps or unified view.

### 3.6 Objective Templates
**Description:** Pre-defined objective templates for common goals.

**User Stories:**
- As an HR Administrator, I can create objective templates for standard goals.
- As a Manager, I can create objectives from templates to save time.

**Acceptance Criteria:**
1. Template has: name, type, description, default target value, unit, suggested period, default calculation method.
2. Creating from template pre-fills all fields; user modifies as needed.
3. Templates categorized by: department, objective type, function.
4. Templates versioned; existing objectives from old templates unaffected.

**Edge Cases:**
- Template deprecated: existing objectives remain, no new creation from template.
- Template modified: only new creations use updated version.

### 3.7 Objective Period Management
**Description:** Manage objective time periods with alignment to organizational cycles.

**User Stories:**
- As a Strategy Officer, I can define objective periods (annual, quarterly, monthly).
- As a system, I automatically roll over incomplete objectives to the next period.
- As a Manager, I can view objectives across multiple periods for trend analysis.

**Acceptance Criteria:**
1. Period types: Annual (Jan-Dec), Quarterly (Q1-Q4), Monthly, Custom.
2. Child objective period must be within parent objective period.
3. Incomplete objectives at period end: option to carry over, close, or extend.
4. Period comparison view: objective achievement across periods.
5. Objective values reset at period start (unless carry-over).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Child within parent | Child period subset of parent | "Child period must be within parent period" |
| Valid dates | End date > start date | "End date must be after start date" |

**Edge Cases:**
- Period boundaries shift: objective values prorated or reassigned.
- Fiscal year change: objectives aligned to new fiscal calendar.

---

## 4. Database Design

### ERD (ASCII)
```
+----------------+       +-------------------+       +-------------------+
| Objective      |1-----*| Objective         |       | ObjectiveValue    |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | (self-reference)  |       | id (PK)           |
| name           |       | parent_id (FK)    |       | objective_id (FK) |
| type           |       +-------------------+       | value             |
| max_value      |                                     | recorded_at       |
| unit           |                                     | recorded_by       |
| target_value   |                                     +-------------------+
| current_value  |
| start_value    |
| period_start   |       +-------------------+       +-------------------+
| period_end     |       | ObjectiveAssign.  |       | ObjectiveProgress |
| calc_method    |       +-------------------+       +-------------------+
| bsc_perspective|       | id (PK)           |       | id (PK)           |
| status         |       | objective_id (FK) |       | objective_id (FK) |
| parent_id (FK) |       | employee_id (FK)  |       | progress_pct      |
| department_id  |       | target_value      |       | calculated_at     |
| representative |       +-------------------+       +-------------------+
+----------------+

+----------------+       +-------------------+       +-------------------+
| BSC_Perspective|       | KeyResult         |       | ObjectiveComment  |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| name           |       | objective_id (FK) |       | objective_id (FK) |
| description    |       | name              |       | author_id (FK)    |
| sort_order     |       | start_value       |       | comment           |
| color          |       | target_value      |       | created_at        |
+----------------+       | current_value     |       +-------------------+
                         | unit              |
                         +-------------------+

+----------------+       +-------------------+
| StrategicMap   |       | ObjectiveTemplate |
+----------------+       +-------------------+
| id (PK)        |       | id (PK)           |
| name           |       | name              |
| description    |       | type              |
| perspectives   |       | default_target    |
| created_at     |       | unit              |
+----------------+       | calc_method       |
                         | category          |
                         +-------------------+
```

### Table Schemas

#### objective
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(300) | NOT NULL | Objective name |
| type | VARCHAR(10) | NOT NULL | mbo/okr/kpi |
| max_value | DECIMAL(15,4) | NULL | Maximum achievable value |
| unit | VARCHAR(20) | NOT NULL | vnd/percentage/count/score/custom |
| target_value | DECIMAL(15,4) | NOT NULL | Target value |
| current_value | DECIMAL(15,4) | NOT NULL, DEFAULT 0 | Current achieved value |
| start_value | DECIMAL(15,4) | NOT NULL, DEFAULT 0 | Starting baseline value |
| period_start | DATE | NOT NULL | Objective start date |
| period_end | DATE | NOT NULL | Objective end date |
| calc_method | VARCHAR(20) | NOT NULL, DEFAULT 'sum' | sum/average/min/max/weighted_avg |
| bsc_perspective | VARCHAR(30) | NULL | financial/customer/internal_process/learning_growth |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'not_started' | not_started/in_progress/on_track/completed/overdue |
| parent_id | UUID | FK -> objective.id, NULL | Parent objective (cascading) |
| department_id | UUID | FK -> departments.id, NOT NULL | Responsible department |
| representative_id | UUID | FK -> users.id, NOT NULL | Responsible person |
| description | TEXT | NULL | Objective description |
| attachment_urls | TEXT[] | NULL | Supporting documents |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### objective_value
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| objective_id | UUID | FK -> objective.id, NOT NULL | Objective |
| value | DECIMAL(15,4) | NOT NULL | Recorded value |
| recorded_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Recording timestamp |
| recorded_by | UUID | FK -> users.id, NOT NULL | Who recorded |
| notes | TEXT | NULL | Notes about this value |

#### objective_assignment
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| objective_id | UUID | FK -> objective.id, NOT NULL | Objective |
| employee_id | UUID | FK -> employees.id, NOT NULL | Assigned employee |
| target_value | DECIMAL(15,4) | NOT NULL | Individual target |
| current_value | DECIMAL(15,4) | NOT NULL, DEFAULT 0 | Individual current |
| weight | DECIMAL(5,2) | NULL | Weight in weighted average calc |
| assigned_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Assignment timestamp |

#### objective_progress
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| objective_id | UUID | FK -> objective.id, NOT NULL | Objective |
| progress_pct | DECIMAL(5,2) | NOT NULL | Calculated progress percentage |
| calculated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Calculation timestamp |
| calculation_type | VARCHAR(20) | NOT NULL | auto/manual |

#### bsc_perspective
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(100) | NOT NULL | Perspective name |
| description | TEXT | NULL | Perspective description |
| sort_order | INT | NOT NULL | Display order (1=Financial, 4=L&G) |
| color | VARCHAR(7) | NOT NULL | Hex color code |

#### key_result
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| objective_id | UUID | FK -> objective.id, NOT NULL | Parent OKR objective |
| name | VARCHAR(200) | NOT NULL | Key result name |
| start_value | DECIMAL(15,4) | NOT NULL, DEFAULT 0 | Starting value |
| target_value | DECIMAL(15,4) | NOT NULL | Target value |
| current_value | DECIMAL(15,4) | NOT NULL, DEFAULT 0 | Current value |
| unit | VARCHAR(20) | NOT NULL | Measurement unit |
| weight | DECIMAL(5,2) | NULL | Weight for aggregation |
| sort_order | INT | NOT NULL | Display order |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### objective_comment
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| objective_id | UUID | FK -> objective.id, NOT NULL | Objective |
| author_id | UUID | FK -> users.id, NOT NULL | Comment author |
| comment | TEXT | NOT NULL | Comment text |
| attachment_urls | TEXT[] | NULL | Attached files |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### strategic_map
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(200) | NOT NULL | Strategy map name |
| description | TEXT | NULL | Description |
| perspectives_json | JSONB | NOT NULL | Perspective configuration |
| layout_json | JSONB | NULL | Visual layout positions |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### objective_template
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(200) | NOT NULL | Template name |
| type | VARCHAR(10) | NOT NULL | mbo/okr/kpi |
| default_target | DECIMAL(15,4) | NULL | Suggested target value |
| unit | VARCHAR(20) | NOT NULL | Default unit |
| calc_method | VARCHAR(20) | NOT NULL, DEFAULT 'sum' | Default calculation method |
| category | VARCHAR(50) | NULL | Template category |
| description | TEXT | NULL | Template description |
| is_active | BOOLEAN | NOT NULL, DEFAULT true | Active flag |

### All Tables Summary
| Table | Purpose | Key Relationships |
|---|---|---|
| objective | Objective definitions and tracking | Self-referencing parent_id |
| objective_value | Value history per objective | Links Objective |
| objective_assignment | Individual objective assignments | Links Objective, Employee |
| objective_progress | Progress calculation history | Links Objective |
| bsc_perspective | BSC perspective definitions | Referenced by Objective |
| key_result | OKR key results | Links Objective (OKR type) |
| objective_comment | Discussion and updates | Links Objective |
| strategic_map | BSC strategy map definitions | References BSC_Perspective |
| objective_template | Objective templates | Standalone configuration |

---

## 5. API Specifications

### JSON Response Format
```json
{
  "success": true,
  "data": { ... },
  "meta": { "page": 1, "per_page": 20, "total": 150 }
}
```

### Error Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Child period must be within parent period",
    "details": [
      { "field": "period_end", "message": "End date exceeds parent objective end date" }
    ]
  }
}
```

### Endpoints Summary
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| GET | `/api/objectives` | List objectives (tree) | obj.objective.read |
| POST | `/api/objectives` | Create objective | obj.objective.create |
| GET | `/api/objectives/:id` | Get objective detail | obj.objective.read |
| PUT | `/api/objectives/:id` | Update objective | obj.objective.update |
| DELETE | `/api/objectives/:id` | Delete objective | obj.objective.delete |
| POST | `/api/objectives/:id/progress` | Update progress value | obj.objective.update |
| GET | `/api/objectives/:id/values` | Get value history | obj.objective.read |
| GET | `/api/objectives/mindmap` | Get mind map data | obj.objective.read |
| GET | `/api/objectives/bsc-map` | Get BSC strategy map | obj.bsc.read |
| GET | `/api/objectives/:id/key-results` | List key results | obj.objective.read |
| POST | `/api/objectives/:id/key-results` | Create key result | obj.objective.update |
| PUT | `/api/objectives/:id/key-results/:krId` | Update key result | obj.objective.update |
| GET | `/api/objectives/:id/assignments` | List assignments | obj.objective.read |
| POST | `/api/objectives/:id/assignments` | Assign employee | obj.objective.update |
| GET | `/api/objectives/templates` | List templates | obj.settings.read |
| POST | `/api/objectives/templates` | Create template | obj.settings.write |
| GET | `/api/objectives/bsc-perspectives` | List BSC perspectives | obj.bsc.read |
| GET | `/api/objectives/:id/comments` | List comments | obj.objective.read |
| POST | `/api/objectives/:id/comments` | Add comment | obj.objective.comment |

### Endpoint Details

#### GET /api/objectives/mindmap
**Query Parameters:** `objective_id` (optional, defaults to root), `expand_levels` (default: 3)
**Response:**
```json
{
  "success": true,
  "data": {
    "id": "obj-001",
    "name": "HR Stability Rate",
    "type": "kpi",
    "progress": 65,
    "status": "in_progress",
    "children": [
      {
        "id": "obj-002",
        "name": "Q1 Retention Target",
        "period": "Q1 2026",
        "progress": 85,
        "status": "on_track",
        "assignees": ["Nguyen Van A", "Tran Thi B"],
        "children": []
      },
      {
        "id": "obj-003",
        "name": "Q2 Retention Target",
        "period": "Q2 2026",
        "progress": 45,
        "status": "behind",
        "assignees": ["Le Van C"],
        "children": []
      }
    ]
  }
}
```

#### POST /api/objectives
**Request Body:**
```json
{
  "name": "Increase Revenue by 20%",
  "type": "mbo",
  "max_value": 100,
  "unit": "percentage",
  "target_value": 20,
  "start_value": 0,
  "period_start": "2026-01-01",
  "period_end": "2026-12-31",
  "calc_method": "sum",
  "bsc_perspective": "financial",
  "department_id": "dept-sales",
  "representative_id": "user-123",
  "parent_id": "obj-001",
  "description": "Achieve 20% revenue growth through new markets"
}
```

---

## 6. Permissions Matrix

### Roles
| Role | Description |
|---|---|
| CEO / Executive | Full access to all objectives, BSC maps, strategy |
| Strategy Officer | Create/edit BSC maps, perspectives, company-level objectives |
| HR Director | Full access to objectives, link to performance evaluation |
| Department Manager | Create/edit department objectives, assign to team |
| Employee | View assigned objectives, update own progress |

### Permission Matrix (CRUD)
| Permission | CEO | Strategy Officer | HR Director | Dept. Mgr. | Employee |
|---|---|---|---|---|---|
| obj.objective.read | CRUD | CRUD | CRUD | R (own dept) | R (own) |
| obj.objective.create | C | C | C | C (own dept) | - |
| obj.objective.update | CRUD | CRUD | CRUD | U (own dept) | U (own progress) |
| obj.objective.delete | C | C | C | C (own dept) | - |
| obj.bsc.read | R | R | R | R | R |
| obj.bsc.write | C | C | - | - | - |
| obj.settings.read | R | R | R | R | R |
| obj.settings.write | C | C | C | - | - |
| obj.objective.comment | CRUD | CRUD | CRUD | CRUD | C (own) |

### Row-Level Security
| Rule | Description |
|---|---|
| Department scoping | Managers only create/edit objectives for their department |
| Employee progress | Employees only update progress on their assigned objectives |
| Strategic visibility | All employees can view company-level objectives (read-only) |
| Cross-tenant isolation | All queries scoped to tenant_id |

---

## 7. State Machine / Workflows

### Objective Status Transitions
```
  +-------------+     start()      +-------------+    on_track()     +----------+
  | NOT STARTED | ----------------> | IN PROGRESS | ----------------> | ON TRACK |
  +-------------+                  +-------------+                   +----------+
                                        |                                |
                                   behind_schedule()                     |
                                        |                                |
                                        v                                v
                                  +----------+                     +-----------+
                                  | BEHIND   |                     | COMPLETED |
                                  +----------+                     +-----------+
                                        |
                                  complete() / overdue()
                                        |
                                  +-----------+    /  +---------+
                                  | COMPLETED |    | | OVERDUE |
                                  +-----------+    | +---------+
```

### Status Transition Table
| From Status | To Status | Trigger | Required Role | Side Effects |
|---|---|---|---|---|
| not_started | in_progress | start() or first value update | Objective owner | Notify assignees |
| in_progress | on_track | progress >= 80% and on schedule | System (auto) | Green indicator |
| in_progress | behind | progress < expected for elapsed time | System (auto) | Red indicator, notify manager |
| on_track | completed | progress >= 100% | System (auto) | Mark complete, notify stakeholders |
| behind | completed | progress >= 100% | System (auto) | Mark complete, note was behind |
| in_progress | overdue | period_end passed, progress < 100% | System (auto) | Red indicator, escalation to manager |
| behind | overdue | period_end passed | System (auto) | Red indicator, escalation |

### Progress Auto-Calculation Workflow
```
  +----------------+    value update    +------------------+
  | Child Value    | -----------------> | Recalculate      |
  | Changed        |                    | Parent Progress  |
  +----------------+                    +------------------+
                                               |
                                       calc_method: sum/avg/min/max
                                               |
                                               v
                                        +------------------+
                                        | Update Parent    |
                                        | current_value    |
                                        | & progress_pct   |
                                        +------------------+
                                               |
                                       cascade to grandparent
                                               |
                                               v
                                        +------------------+
                                        | Repeat until     |
                                        | root objective   |
                                        +------------------+
```

---

## 8. Reports & Analytics

### 8.1 Objective Achievement Report
**Dimensions:** Department, Objective type, Period  
**Metrics:** Total objectives, completed, in progress, behind, overdue, achievement rate  
**Filters:** Period, department, type

### 8.2 BSC Perspective Report
**Dimensions:** BSC perspective, Department  
**Metrics:** Objectives per perspective, average progress, completion rate  
**Filters:** Period, perspective

### 8.3 OKR Key Results Report
**Dimensions:** Objective, Key result  
**Metrics:** Key result progress, overall OKR progress, completion rate  
**Filters:** Period, department

### 8.4 Objective Trend Report
**Dimensions:** Objective, Period-over-period  
**Metrics:** Value trend, progress trend, achievement trajectory  
**Filters:** Year range, objective

### 8.5 Cascading Alignment Report
**Dimensions:** Parent-child objective pairs  
**Metrics:** Alignment score, child coverage, value consistency  
**Filters:** Department, period

### 8.6 Individual Objective Report
**Dimensions:** Employee  
**Metrics:** Assigned objectives, completion rate, average progress  
**Filters:** Employee, period, department

### Report Summary Table
| Report | Purpose | Primary Users | Export Formats |
|---|---|---|---|
| Objective Achievement | Overall status | CEO, HR Director | PDF, Excel |
| BSC Perspective | Strategy balance | CEO, Strategy Officer | PDF, Excel |
| OKR Key Results | OKR tracking | Manager, Employee | PDF, Excel |
| Objective Trend | Historical analysis | CEO, Manager | PDF, Excel |
| Cascading Alignment | Goal alignment | HR Director, CEO | PDF, Excel |
| Individual Report | Personal tracking | Manager, Employee | PDF, Excel |

---

## 9. Integration Points

### External APIs
| Integration | Direction | Purpose | Frequency |
|---|---|---|---|
| M01 Organization Service | Inbound | Department hierarchy | Real-time (event-driven) |
| M02 Employee Service | Inbound | Employee assignments | Real-time (event-driven) |
| M15 Performance Evaluation | Outbound | Objective achievement scores | On period end |
| M12 Payroll Service | Outbound | KPI-based bonus triggers | On objective completion |
| Email Service | Outbound | Progress notifications, deadline alerts | On event |
| Notification Service | Outbound | Push notifications | On event |
| Calendar Service | Outbound | Objective deadline reminders | On schedule |

### Webhooks
| Event | Trigger | Payload |
|---|---|---|
| objective.created | New objective created | objective_id, name, type, department |
| objective.progress_updated | Progress value updated | objective_id, old_value, new_value, progress_pct |
| objective.completed | Objective reaches 100% | objective_id, name, final_value |
| objective.overdue | Period ends incomplete | objective_id, name, final_progress |
| objective.status_changed | Status transition | objective_id, from_status, to_status |
| key_result.updated | Key result value changed | kr_id, objective_id, new_value |

### Events Published
| Event | Description | Consumers |
|---|---|---|
| ObjectiveProgressChanged | Any progress update | Parent objective recalculation, Notification |
| ObjectiveCompleted | 100% achievement | Performance Evaluation (M15), Payroll (M12) |
| ObjectiveOverdue | Missed deadline | Manager notification, Escalation |
| BSCMapUpdated | Strategy map modified | Executive dashboard |

---

## 10. Technical Notes

### Performance
- **Mind map rendering**: Pre-compute tree structure, serve in <200ms for 500-node maps.
- **Progress cascade**: Async recalculation for deep hierarchies; parent update within 5 seconds.
- **BSC strategy map**: Layout pre-computed, rendering <100ms.
- **Database indexing**: Index on (parent_id) for tree queries, (department_id, status) for filtering, (period_start, period_end) for period queries.

### Caching
- **Objective tree** cached with 1-minute TTL (frequent updates).
- **BSC perspectives** cached with 1-hour TTL.
- **Mind map data** cached with 2-minute TTL.
- **Progress calculations** cached per objective with 30-second TTL.

### File Handling
- **Objective attachments** stored in object storage with signed URLs, 3-year retention.
- **Maximum file size**: 10MB per attachment.
- **File types**: PDF, images, documents, spreadsheets.

### Localization
- **Language**: Vietnamese (primary), English (secondary).
- **Currency**: VND for value display, formatted in billions (e.g., 50B).
- **Date format**: DD/MM/YYYY.
- **BSC terminology**: Standard Vietnamese business terminology.

### Mobile Support
- **Objective list** responsive on mobile.
- **Progress updates** mobile-optimized.
- **Mind map** usable on tablet (not phone due to complexity).
- **BSC strategy map** tablet-optimized.
- **Push notifications** for deadline reminders, status changes.

### Security
- **Objective data** encrypted at rest (AES-256).
- **Strategic objectives** may have restricted visibility (e.g., M&A-related).
- **Audit logging**: All objective create/update/delete operations logged.
- **Data retention**: Objective records retained for 5 years.

### Progress Calculation Engine
- **Cascade recalculation**: When child value changes, propagate up tree using configured calc_method.
- **Debounced updates**: Multiple rapid updates batched into single cascade (500ms debounce).
- **Circular reference detection**: Validation prevents parent chains that would create loops.
- **Division by zero handling**: When target == start, progress = 100% if current > start, else 0%.
