# M19 - Workflow (Quy trinh)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_13.png, assets/image_56.png, assets/image_61.png
**Dependencies:** M01 (User & Authentication), M02 (Roles & Permissions), M03 (Company & Organization)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Workflow module provides a comprehensive business process automation platform for designing, executing, tracking, and approving organizational workflows. It enables departments to digitize approval chains, purchase requests, payment approvals, leave requests, resource allocation requests, and any custom multi-step business process. The module supports visual workflow design with drag-and-drop form builders, conditional branching, role-based step assignments, SLA deadlines, and comprehensive audit trails. With department-level statistics and customizable filters, managers gain visibility into operational throughput, bottlenecks, and team productivity.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Workflow Administrator | Designs and configures workflow templates | Full |
| Department Manager | Creates runs, approves steps, views department stats | Full (dept-scoped) |
| Workflow Participant | Executes assigned steps in active runs | Partial |
| Workflow Creator | Initiates new workflow runs | Partial |
| Executive Viewer | Monitors reports and analytics | Read-only |
| System Administrator | Manages global settings and integrations | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication), M02 (Roles & Permissions), M03 (Company & Organization)
- **Used by:** M07 (Purchasing), M11 (Contract Management), M12 (Payroll), M13 (Time Tracking), M18 (Labor Relations), M22 (Correspondence), M29 (Electronic Signature)

### Key Business Rules
1. Every workflow run must be associated with a workflow template that defines its steps, approval chain, and conditions.
2. Workflow steps are assigned to specific users, roles, or departments; assignment is resolved at run creation time.
3. Each step can have an SLA deadline; overdue steps trigger escalation notifications.
4. Conditional branching allows different approval paths based on form field values (e.g., amount thresholds).
5. A workflow run cannot be deleted once it has entered PENDING or IN_PROGRESS status; it must be cancelled first.
6. Rejected steps return the run to the previous step or to DRAFT depending on template configuration.
7. Workflow templates support versioning; existing runs use the template version they were created with.
8. Department statistics are computed in real-time from active and completed runs.
9. Custom filters are saved per-user and can be shared with the department.
10. All workflow actions are audited with timestamp, user, and before/after values.

---

## 2. Screen Inventory

### 2.1 Screen: Workflow Run History List
**Route:** `/workflows/runs`
**Purpose:** Main list of all workflow runs with filtering and status tracking (img_13).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Tab Navigation | Tab bar | Overview, Runs (active), Design, Reports, Settings |
| Left Sidebar Filters | Filter panel | All, To Do (5), Created, Participated, Draft |
| Run Table | Data grid | ID, Title, Workflow, Created Date, Creator, Executor, Deadline, Status |
| Run Workflow Button | CTA Button | Primary blue button to initiate new workflow run |
| Pagination | Footer control | Total 100 records, 50 per page, navigation |
| Status Badges | Inline indicator | In progress (blue), Completed (green), Rejected (red), Draft (gray), Cancelled (gray) |

**User Interactions:**
- Click "Run Workflow" to create a new workflow instance.
- Click a row to view run details and current step.
- Apply sidebar filters to narrow view.
- Sort columns by clicking headers.
- Paginate through results.

**Data Displayed:**
- Workflow run records with metadata and status.
- Sidebar filter counts (e.g., To Do shows 5 pending items).
- Total record count and pagination info.

### 2.2 Screen: Workflow Overview Dashboard
**Route:** `/workflows/overview`
**Purpose:** Summary dashboard showing operational statistics (img_61).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Summary Cards | Metric cards | Total operations, In progress, Completed, Cancelled, Draft |
| Department Breakdown | Bar chart / table | Workflow counts by department (Marketing 96, HCTH 81, QTKD 46, etc.) |
| Trend Chart | Line chart | Workflow volume over time |
| Recent Activity | List | Latest workflow actions and status changes |

**Data Displayed:**
- Operations summary: Total 750, In progress 120, Completed 600, Cancelled 30, Draft 30.
- Department breakdown: Marketing 96, HCTH 81, QTKD 46, Partners 41, CSKH 27, HR 26, Accounting 26, TCDN 21, NSBH 17, CNTT 15, VP 31, Other 57.
- Monthly trend data.

**User Interactions:**
- Click summary cards to drill down into filtered run lists.
- Hover on department bars to see exact counts.
- Click departments to filter runs by that department.

### 2.3 Screen: Workflow Design Canvas
**Route:** `/workflows/design`
**Purpose:** Visual workflow template designer with drag-and-drop step configuration.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Canvas | Drag-and-drop area | Visual workflow diagram with nodes and connectors |
| Step Palette | Sidebar | Available step types: Approval, Condition, Action, Notification, End |
| Step Properties | Panel | Configuration form for selected step (assignee, deadline, conditions) |
| Form Builder | Panel | Drag-and-drop form field designer for workflow input forms |
| Version Selector | Dropdown | View and manage template versions |
| Preview | Button | Test workflow with sample data |
| Save/Publish | Buttons | Save draft or publish template |

**User Interactions:**
- Drag step types onto canvas to build workflow.
- Connect steps with arrows to define flow.
- Configure step properties (assignee role, deadline, escalation rules).
- Design input forms using form builder.
- Save and publish workflow templates.

### 2.4 Screen: Workflow Reports
**Route:** `/workflows/reports`
**Purpose:** Analytics and reporting on workflow performance.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Date Range Picker | Input | Filter reports by date range |
| Department Selector | Multi-select | Select departments to include |
| Summary Metrics | Cards | Average completion time, rejection rate, SLA compliance |
| Bottleneck Chart | Bar chart | Steps with longest average wait time |
| SLA Compliance | Gauge chart | Percentage of steps completed within SLA |
| Export Button | CTA | Export report to PDF/Excel |

**User Interactions:**
- Select date range and departments.
- View performance metrics and charts.
- Export reports for sharing.

### 2.5 Screen: Workflow Settings
**Route:** `/workflows/settings`
**Purpose:** Module configuration and global settings management.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Notification Settings | Form | Email, in-app, SMS notification preferences |
| Escalation Rules | Table | Configure auto-escalation for overdue steps |
| Default SLA | Input | Default deadline for workflow steps |
| Integration Settings | Form | Connected modules and APIs |
| User Permissions | Table | Role-based access to workflow features |

**User Interactions:**
- Configure notification preferences.
- Set escalation rules and default SLA values.
- Manage user permissions for workflow module.

### 2.6 Screen: Workflow Run Detail
**Route:** `/workflows/runs/{id}`
**Purpose:** Detailed view of a single workflow run with step-by-step progress.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Run Header | Card | Title, workflow name, status, creator, dates |
| Progress Timeline | Vertical timeline | Each step with status indicator, assignee, completion time |
| Current Step Form | Dynamic form | Input form for the current step |
| Action Buttons | Button group | Approve, Reject, Return, Delegate, Add Comment |
| Audit Log | Table | Complete history of actions with timestamps and users |
| Attachments | File list | Documents attached to this run |

**User Interactions:**
- View step-by-step progress of the workflow run.
- Complete current step actions (approve, reject, delegate).
- Add comments and attachments.
- Review complete audit trail.

---

## 3. Feature Specifications

### 3.1 Feature: Workflow Template Design
**Description:** Visual drag-and-drop designer for creating workflow templates with configurable steps, conditions, and forms.

**User Stories:**
- As a workflow administrator, I want to drag step types onto a canvas and connect them visually, so that I can design approval chains without coding.
- As a workflow administrator, I want to configure form fields for each workflow, so that requesters provide the necessary information.
- As a workflow administrator, I want to set conditions that route the workflow differently based on field values, so that complex approval rules are automated.

**Acceptance Criteria:**
- Given a workflow administrator is on the design canvas
- When they drag an Approval step and connect it to a Start node
- Then the step appears on the canvas with a configuration panel
- Given a step is selected
- When the administrator sets the assignee to "Department Manager" and deadline to 3 days
- Then the step is configured with that role and SLA

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Template Name | Required, unique, max 200 chars | "Template name is required and must be unique" |
| Steps | At least 1 step required | "Workflow must have at least one step" |
| Form Fields | At least 1 field required | "Workflow must have at least one input field" |
| Connector | Every step must have at least one outgoing connector | "All steps must have a defined next step" |

**Edge Cases:**
- Circular dependencies in workflow design must be detected and prevented.
- Deleting a step with incoming connections prompts user to re-route.
- Template versioning preserves existing runs on the old version.

### 3.2 Feature: Workflow Run Execution
**Description:** Creating and executing workflow runs from published templates with real-time step tracking.

**User Stories:**
- As an employee, I want to initiate a workflow run by filling out a form, so that my request enters the approval process.
- As an approver, I want to see my pending tasks in the To Do filter, so that I can prioritize my approvals.
- As a participant, I want to track the real-time status of my submitted requests, so that I know where they are in the process.

**Acceptance Criteria:**
- Given a user selects a workflow template
- When they fill in the required form fields and submit
- Then a new run is created with status IN_PROGRESS and the first step assigned to the appropriate approver
- Given an approver has a pending step
- When they open the run and click Approve
- Then the step is marked complete and the next step is assigned

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Required Form Fields | All required fields must be filled | "Please fill in all required fields" |
| Attachments | Must meet size and type limits | "File exceeds maximum size of 10MB" |
| Deadline | Cannot be in the past | "Deadline must be a future date" |

**Edge Cases:**
- If an assignee is no longer employed, the step auto-escalates to their manager.
- If a workflow template is unpublished, runs cannot be created from it.
- Concurrent approvals on the same step are prevented with optimistic locking.

### 3.3 Feature: Custom Filters and Views
**Description:** Users can create, save, and share custom filters for workflow runs (img_56).

**User Stories:**
- As a department manager, I want to save a filter showing all pending approvals for my department, so that I can quickly access it daily.
- As a user, I want to share my custom filter with colleagues, so that we have consistent views.

**Acceptance Criteria:**
- Given a user is on the runs list
- When they apply filter criteria and click "Save Filter"
- Then the filter is saved with a custom name and appears in their sidebar

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Filter Name | Required, unique per user | "Filter name is required" |

**Edge Cases:**
- Saved filters reference workflow templates that may be deleted; handle gracefully with a warning.

### 3.4 Feature: SLA Monitoring and Escalation
**Description:** Automatic tracking of step deadlines with notifications and escalation for overdue items.

**User Stories:**
- As a manager, I want to be notified when a step I assigned is overdue, so that I can follow up.
- As a system, I want to auto-escalate overdue steps to the next level, so that approvals don't stall.

**Acceptance Criteria:**
- Given a step has a deadline of 3 days
- When the deadline passes without action
- Then an escalation notification is sent to the step assignee and their manager

**Edge Cases:**
- Holidays and non-business days can be excluded from SLA calculation based on settings.
- Escalation chains have configurable maximum depth to prevent infinite escalation.

### 3.5 Feature: Department Statistics Dashboard
**Description:** Real-time department-level workflow analytics with operation counts and breakdowns.

**User Stories:**
- As an executive, I want to see workflow volume by department, so that I can identify high-activity areas.
- As a manager, I want to see my department's completion rate, so that I can assess team productivity.

**Acceptance Criteria:**
- Given the overview dashboard is loaded
- When the user views department statistics
- Then they see operation counts: Marketing 96, HCTH 81, QTKD 46, Partners 41, CSKH 27, HR 26, Accounting 26, TCDN 21, NSBH 17, CNTT 15, VP 31, Other 57

**Edge Cases:**
- Employees with multiple department affiliations count toward their primary department.
- Cross-department workflows count toward both departments with configurable allocation.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+-------------------+       +-------------------+       +-------------------+
|     Workflow      |       |   WorkflowStep    |       |   WorkflowRun     |
+-------------------+       +-------------------+       +-------------------+
| id (PK)           |<----->| id (PK)           |<----->| id (PK)           |
| name              |       | workflow_id (FK)  |       | workflow_id (FK)  |
| description       |       | step_type         |       | template_version  |
| category          |       | assignee_role     |       | title             |
| status            |       | assignee_user_id  |       | creator_id (FK)   |
| created_by (FK)   |       | deadline_days     |       | status            |
| created_at        |       | escalation_depth  |       | created_at        |
| updated_at        |       | step_order        |       | updated_at        |
+-------------------+       | condition_expr    |       | deadline          |
                            | created_at        |       +-------------------+
                            +-------------------+               |
                                                                |
                            +-------------------+               |
                            | WorkflowAction    |               |
                            +-------------------+               |
                            | id (PK)           |               |
                            | run_id (FK)       |---------------+
                            | step_id (FK)      |
                            | action_type       |
                            | actor_id (FK)     |
                            | comment           |
                            | created_at        |
                            +-------------------+
```

### 4.2 Table Schemas

#### Table: workflows
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL, UNIQUE | Template name |
| description | TEXT | NULL | Template description |
| category | VARCHAR(100) | NULL | Workflow category |
| status | ENUM('draft','published','archived') | NOT NULL, DEFAULT 'draft' | Template status |
| version | INT | NOT NULL, DEFAULT 1 | Template version |
| form_schema | JSON | NULL | Form field definitions |
| created_by | BIGINT | FK -> users.id | Creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_workflows_status ON workflows(status);
CREATE INDEX idx_workflows_category ON workflows(category);
CREATE INDEX idx_workflows_created_by ON workflows(created_by);
```

#### Table: workflow_steps
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| workflow_id | BIGINT | NOT NULL, FK -> workflows.id | Parent workflow |
| step_type | ENUM('approval','condition','action','notification','start','end') | NOT NULL | Step type |
| step_order | INT | NOT NULL | Execution order |
| assignee_role | VARCHAR(100) | NULL | Role-based assignment |
| assignee_user_id | BIGINT | NULL, FK -> users.id | User-specific assignment |
| assignee_department_id | BIGINT | NULL, FK -> departments.id | Department assignment |
| deadline_days | INT | NULL, DEFAULT 3 | SLA deadline in days |
| escalation_depth | INT | NOT NULL, DEFAULT 0 | Max escalation levels |
| condition_expression | JSON | NULL | Conditional routing logic |
| next_step_ids | JSON | NULL | Array of next step IDs |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_workflow_steps_workflow_id ON workflow_steps(workflow_id);
CREATE INDEX idx_workflow_steps_step_order ON workflow_steps(workflow_id, step_order);
CREATE INDEX idx_workflow_steps_assignee_role ON workflow_steps(assignee_role);
```

#### Table: workflow_runs
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| code | VARCHAR(20) | NOT NULL, UNIQUE | Run code (e.g., WR-0001) |
| workflow_id | BIGINT | NOT NULL, FK -> workflows.id | Source workflow |
| template_version | INT | NOT NULL | Template version at creation |
| title | VARCHAR(500) | NOT NULL | Run title |
| creator_id | BIGINT | NOT NULL, FK -> users.id | Run creator |
| department_id | BIGINT | NULL, FK -> departments.id | Requester department |
| status | ENUM('draft','in_progress','completed','cancelled','rejected') | NOT NULL, DEFAULT 'draft' | Run status |
| current_step_id | BIGINT | NULL, FK -> workflow_steps.id | Current active step |
| form_data | JSON | NULL | Submitted form values |
| deadline | TIMESTAMP | NULL | Overall run deadline |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| completed_at | TIMESTAMP | NULL, DEFAULT NULL | Completion timestamp |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_workflow_runs_workflow_id ON workflow_runs(workflow_id);
CREATE INDEX idx_workflow_runs_creator_id ON workflow_runs(creator_id);
CREATE INDEX idx_workflow_runs_status ON workflow_runs(status);
CREATE INDEX idx_workflow_runs_department_id ON workflow_runs(department_id);
CREATE INDEX idx_workflow_runs_created_at ON workflow_runs(created_at);
CREATE INDEX idx_workflow_runs_current_step_id ON workflow_runs(current_step_id);
```

#### Table: workflow_actions
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| run_id | BIGINT | NOT NULL, FK -> workflow_runs.id | Parent run |
| step_id | BIGINT | NOT NULL, FK -> workflow_steps.id | Step being acted on |
| action_type | ENUM('approve','reject','return','delegate','comment','escalate') | NOT NULL | Action type |
| actor_id | BIGINT | NOT NULL, FK -> users.id | User performing action |
| comment | TEXT | NULL | Action comment |
| before_values | JSON | NULL | Field values before action |
| after_values | JSON | NULL | Field values after action |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Action timestamp |

**Indexes:**
```sql
CREATE INDEX idx_workflow_actions_run_id ON workflow_actions(run_id);
CREATE INDEX idx_workflow_actions_step_id ON workflow_actions(step_id);
CREATE INDEX idx_workflow_actions_actor_id ON workflow_actions(actor_id);
CREATE INDEX idx_workflow_actions_created_at ON workflow_actions(created_at);
```

#### Table: workflow_approvals
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| run_id | BIGINT | NOT NULL, FK -> workflow_runs.id | Parent run |
| step_id | BIGINT | NOT NULL, FK -> workflow_steps.id | Approval step |
| approver_id | BIGINT | NOT NULL, FK -> users.id | Approver |
| status | ENUM('pending','approved','rejected','delegated') | NOT NULL, DEFAULT 'pending' | Approval status |
| comment | TEXT | NULL | Approval comment |
| action_at | TIMESTAMP | NULL, DEFAULT NULL | When action was taken |
| delegated_to | BIGINT | NULL, FK -> users.id | Delegate if reassigned |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_workflow_approvals_run_id ON workflow_approvals(run_id);
CREATE INDEX idx_workflow_approvals_approver_id ON workflow_approvals(approver_id);
CREATE INDEX idx_workflow_approvals_status ON workflow_approvals(status);
```

#### Table: workflow_templates
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Template display name |
| workflow_id | BIGINT | NOT NULL, FK -> workflows.id | Source workflow |
| version | INT | NOT NULL | Version number |
| form_schema | JSON | NOT NULL | Form field definitions |
| step_definitions | JSON | NOT NULL | Serialized step graph |
| created_by | BIGINT | NOT NULL, FK -> users.id | Creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| published_at | TIMESTAMP | NULL, DEFAULT NULL | Publication timestamp |

**Indexes:**
```sql
CREATE INDEX idx_workflow_templates_workflow_id ON workflow_templates(workflow_id);
CREATE INDEX idx_workflow_templates_version ON workflow_templates(workflow_id, version);
```

#### Table: workflow_filters
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Filter name |
| user_id | BIGINT | NOT NULL, FK -> users.id | Owner |
| filter_criteria | JSON | NOT NULL | Saved filter conditions |
| is_shared | BOOLEAN | NOT NULL, DEFAULT FALSE | Whether filter is shared |
| shared_with_dept | BOOLEAN | NOT NULL, DEFAULT FALSE | Share with department |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_workflow_filters_user_id ON workflow_filters(user_id);
```

#### Table: workflow_conditions
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| step_id | BIGINT | NOT NULL, FK -> workflow_steps.id | Parent step |
| field_name | VARCHAR(100) | NOT NULL | Form field to evaluate |
| operator | ENUM('eq','neq','gt','gte','lt','lte','in','contains') | NOT NULL | Comparison operator |
| value | VARCHAR(500) | NOT NULL | Comparison value |
| next_step_id | BIGINT | NOT NULL, FK -> workflow_steps.id | Target step if condition matches |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_workflow_conditions_step_id ON workflow_conditions(step_id);
```

#### Table: workflow_notifications
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| run_id | BIGINT | NOT NULL, FK -> workflow_runs.id | Related run |
| recipient_id | BIGINT | NOT NULL, FK -> users.id | Notification recipient |
| notification_type | ENUM('step_assigned','overdue','escalation','completed','rejected') | NOT NULL | Notification type |
| channel | ENUM('in_app','email','sms') | NOT NULL, DEFAULT 'in_app' | Delivery channel |
| message | TEXT | NOT NULL | Notification content |
| is_read | BOOLEAN | NOT NULL, DEFAULT FALSE | Read status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |
| read_at | TIMESTAMP | NULL, DEFAULT NULL | Read timestamp |

**Indexes:**
```sql
CREATE INDEX idx_workflow_notifications_recipient_id ON workflow_notifications(recipient_id);
CREATE INDEX idx_workflow_notifications_is_read ON workflow_notifications(is_read);
CREATE INDEX idx_workflow_notifications_run_id ON workflow_notifications(run_id);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| workflows | Workflow template definitions | 200 |
| workflow_steps | Step definitions per workflow | 2,000 |
| workflow_runs | Active and historical run instances | 10,000 |
| workflow_actions | Audit trail of all actions taken | 50,000 |
| workflow_approvals | Approval tracking per step | 30,000 |
| workflow_templates | Versioned template snapshots | 500 |
| workflow_filters | User-saved custom filters | 1,000 |
| workflow_conditions | Conditional routing rules | 1,500 |
| workflow_notifications | Notification records | 100,000 |

---

## 5. API Specifications

### 5.1 Workflow Endpoints

#### GET /api/v1/workflows
**Description:** List all workflow templates with pagination and filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number (default: 1) |
| limit | integer | No | Items per page (default: 20, max: 100) |
| status | string | No | Filter by status: draft, published, archived |
| category | string | No | Filter by category |
| sort | string | No | Sort field: name, created_at, status |
| order | string | No | asc/desc (default: desc) |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "name": "IT Resource Allocation Request",
      "description": "Request for IT resource allocation or recovery",
      "category": "IT",
      "status": "published",
      "version": 3,
      "created_by": {"id": 10, "name": "Nguyen Van A"},
      "created_at": "2024-01-15T08:00:00Z",
      "updated_at": "2024-03-20T10:30:00Z"
    }
  ],
  "meta": {
    "total": 45,
    "page": 1,
    "limit": 20,
    "last_page": 3
  }
}
```

**Permission:** `workflow:template:read`

#### POST /api/v1/workflows
**Description:** Create a new workflow template

**Request Body:**
```json
{
  "name": "Purchase Approval Workflow",
  "description": "Multi-level purchase request approval",
  "category": "Purchasing",
  "form_schema": [...],
  "steps": [
    {
      "step_type": "approval",
      "step_order": 1,
      "assignee_role": "department_manager",
      "deadline_days": 3,
      "next_step_ids": [2]
    }
  ]
}
```

**Response (201 Created):**
```json
{
  "id": 46,
  "name": "Purchase Approval Workflow",
  "status": "draft",
  "version": 1,
  "created_at": "2024-04-14T09:00:00Z"
}
```

**Permission:** `workflow:template:write`

#### GET /api/v1/workflows/{id}
**Description:** Get single workflow template with steps and form schema

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "IT Resource Allocation Request",
  "status": "published",
  "version": 3,
  "form_schema": [...],
  "steps": [...],
  "created_at": "2024-01-15T08:00:00Z"
}
```

**Permission:** `workflow:template:read`

#### PUT /api/v1/workflows/{id}
**Description:** Update a workflow template (draft only)

**Permission:** `workflow:template:write`

#### DELETE /api/v1/workflows/{id}
**Description:** Soft delete a workflow template (draft only)

**Permission:** `workflow:template:delete`

#### POST /api/v1/workflows/{id}/publish
**Description:** Publish a draft workflow template, creating a new version

**Permission:** `workflow:template:publish`

### 5.2 Workflow Run Endpoints

#### GET /api/v1/workflow-runs
**Description:** List workflow runs with filtering

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page |
| filter | string | No | all, todo, created, participated, draft |
| workflow_id | bigint | No | Filter by workflow |
| status | string | No | draft, in_progress, completed, cancelled, rejected |
| department_id | bigint | No | Filter by department |
| sort | string | No | Sort field |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 4,
      "code": "WR-0004",
      "title": "IT Resource Allocation Request",
      "workflow": {"id": 1, "name": "IT Resource Allocation"},
      "creator": {"id": 10, "name": "Bui Ngoc Anh"},
      "created_at": "2024-03-15T09:00:00Z",
      "status": "in_progress",
      "deadline": "2024-03-22T17:00:00Z"
    }
  ],
  "meta": {
    "total": 100,
    "page": 1,
    "limit": 50
  }
}
```

**Permission:** `workflow:run:read`

#### POST /api/v1/workflow-runs
**Description:** Create a new workflow run

**Request Body:**
```json
{
  "workflow_id": 1,
  "title": "Request laptop for new developer",
  "form_data": {
    "resource_type": "laptop",
    "quantity": 1,
    "justification": "New developer onboarding",
    "department_id": 5
  },
  "deadline": "2024-04-21T17:00:00Z"
}
```

**Response (201 Created):**
```json
{
  "id": 101,
  "code": "WR-0101",
  "status": "in_progress",
  "current_step": {"id": 1, "assignee": "Nguyen Van B"},
  "created_at": "2024-04-14T10:00:00Z"
}
```

**Permission:** `workflow:run:create`

#### GET /api/v1/workflow-runs/{id}
**Description:** Get workflow run detail with steps, actions, and audit log

**Permission:** `workflow:run:read`

#### POST /api/v1/workflow-runs/{id}/actions
**Description:** Perform an action on the current step

**Request Body:**
```json
{
  "action_type": "approve",
  "comment": "Approved, budget allocated",
  "step_id": 2
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "run_status": "in_progress",
  "next_step": {"id": 3, "assignee": "Finance Manager"},
  "action": {"id": 501, "type": "approve", "created_at": "2024-04-14T11:00:00Z"}
}
```

**Permission:** `workflow:run:execute`

### 5.3 Custom Filter Endpoints

#### GET /api/v1/workflow-filters
**Description:** List user's saved filters

**Permission:** `workflow:filter:read`

#### POST /api/v1/workflow-filters
**Description:** Save a new custom filter

**Permission:** `workflow:filter:write`

#### DELETE /api/v1/workflow-filters/{id}
**Description:** Delete a saved filter

**Permission:** `workflow:filter:delete`

### 5.4 Reports Endpoints

#### GET /api/v1/workflow-reports/department-stats
**Description:** Get department-level workflow statistics

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date_from | date | No | Start date |
| date_to | date | No | End date |

**Response (200 OK):**
```json
{
  "summary": {
    "total": 750,
    "in_progress": 120,
    "completed": 600,
    "cancelled": 30,
    "draft": 30
  },
  "departments": [
    {"id": 1, "name": "Marketing", "count": 96},
    {"id": 2, "name": "HCTH", "count": 81},
    {"id": 3, "name": "QTKD", "count": 46},
    {"id": 4, "name": "Partners", "count": 41},
    {"id": 5, "name": "CSKH", "count": 27},
    {"id": 6, "name": "HR", "count": 26},
    {"id": 7, "name": "Accounting", "count": 26},
    {"id": 8, "name": "TCDN", "count": 21},
    {"id": 9, "name": "NSBH", "count": 17},
    {"id": 10, "name": "CNTT", "count": 15},
    {"id": 11, "name": "VP", "count": 31},
    {"id": 12, "name": "Other", "count": 57}
  ]
}
```

**Permission:** `workflow:report:read`

### 5.5 Error Response Format
```json
{
  "error": {
    "code": "WORKFLOW_NOT_FOUND",
    "message": "Workflow with id 999 not found",
    "details": {}
  }
}
```

### 5.6 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/workflows | List templates | workflow:template:read |
| POST | /api/v1/workflows | Create template | workflow:template:write |
| GET | /api/v1/workflows/{id} | Get template | workflow:template:read |
| PUT | /api/v1/workflows/{id} | Update template | workflow:template:write |
| DELETE | /api/v1/workflows/{id} | Delete template | workflow:template:delete |
| POST | /api/v1/workflows/{id}/publish | Publish template | workflow:template:publish |
| GET | /api/v1/workflow-runs | List runs | workflow:run:read |
| POST | /api/v1/workflow-runs | Create run | workflow:run:create |
| GET | /api/v1/workflow-runs/{id} | Get run detail | workflow:run:read |
| POST | /api/v1/workflow-runs/{id}/actions | Execute action | workflow:run:execute |
| GET | /api/v1/workflow-filters | List filters | workflow:filter:read |
| POST | /api/v1/workflow-filters | Save filter | workflow:filter:write |
| DELETE | /api/v1/workflow-filters/{id} | Delete filter | workflow:filter:delete |
| GET | /api/v1/workflow-reports/department-stats | Department stats | workflow:report:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Workflow Admin | workflow_admin | Full access to design, configure, and manage workflows |
| Department Manager | dept_manager | Create runs, approve steps, view department stats |
| Workflow Participant | workflow_participant | Execute assigned steps in active runs |
| Workflow Creator | workflow_creator | Initiate new workflow runs |
| Executive Viewer | executive_viewer | View reports and analytics only |

### Permission Matrix
| Role | Create Template | Read Template | Update Template | Delete Template | Create Run | Execute Step | View Reports | Manage Filters |
|------|----------------|---------------|-----------------|----------------|-----------|-------------|-------------|---------------|
| Workflow Admin | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Department Manager | ❌ | ✅ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| Workflow Participant | ❌ | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ✅ |
| Workflow Creator | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ |
| Executive Viewer | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

### Row-Level Security
- Users can only view workflow runs they created, are assigned to, or belong to their department.
- Department managers can view all runs within their department scope.
- Workflow admins can view all runs across all departments.
- Users can only execute steps explicitly assigned to them (by user ID, role, or department).
- Custom filters are private by default; shared filters are visible to the designated audience.

---

## 7. State Machine / Workflows

### 7.1 Workflow Run Status Transitions

```
[DRAFT] ──submit──→ [IN_PROGRESS] ──complete──→ [COMPLETED]
   │                      │
   │                      ├──reject──→ [REJECTED]
   │                      │
   └──delete──────────────┘
                          │
                          └──cancel──→ [CANCELLED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| DRAFT | IN_PROGRESS | submit | Creator | Assign first step, send notifications |
| IN_PROGRESS | COMPLETED | complete | System (last step approved) | Archive run, send completion notification |
| IN_PROGRESS | REJECTED | reject | Any approver | Notify creator, mark as rejected |
| IN_PROGRESS | CANCELLED | cancel | Creator or Admin | Notify all participants, void pending steps |
| DRAFT | DRAFT | delete | Creator | Remove draft permanently |

### 7.2 Approval Workflow Example

```
Submitter → Department Manager → Finance Manager → Director
     ↓              ↓                ↓              ↓
  [DRAFT]     [Dept Approved]  [Finance OK]   [Final Approved]
                                          ↓
                                     [COMPLETED]

Any step can reject: [REJECTED] back to submitter
```

### 7.3 Step-Level Status Transitions

```
[PENDING] ──approve──→ [APPROVED] ──auto-advance──→ [COMPLETED]
    │                      │
    └──reject──────────────┘
    │
    └──delegate──→ [PENDING] (new assignee)
```

---

## 8. Reports & Analytics

### 8.1 Report: Department Workflow Statistics
**Type:** Real-time
**Purpose:** Show workflow volume and status breakdown by department for operational visibility.

**Dimensions:**
- Department (Marketing, HCTH, QTKD, Partners, CSKH, HR, Accounting, TCDN, NSBH, CNTT, VP, Other)
- Time period (daily, weekly, monthly, quarterly)

**Metrics:**
- Total operations: 750
- In progress: 120
- Completed: 600
- Cancelled: 30
- Draft: 30
- Average completion time (hours)
- SLA compliance rate (%)

**Filters:**
- Date range
- Department selection
- Workflow type
- Status

**Chart Type:** Bar chart (department breakdown), Summary cards (aggregate metrics)

### 8.2 Report: Workflow Completion Time Analysis
**Type:** Real-time
**Purpose:** Identify bottlenecks and measure SLA compliance.

**Dimensions:**
- Workflow template
- Step type
- Department

**Metrics:**
- Average time per step (hours)
- Median completion time
- 95th percentile completion time
- SLA breach count

**Filters:**
- Date range
- Workflow template
- Department

**Chart Type:** Bar chart (step-level breakdown), Line chart (trend over time)

### 8.3 Report: User Activity Summary
**Type:** Real-time
**Purpose:** Track individual contributor participation in workflows.

**Metrics:**
- Runs created
- Runs approved
- Average response time
- Overdue count

**Chart Type:** Table with sortable columns

### 8.4 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Department Statistics | Real-time | On-demand | Bar chart, summary cards |
| Completion Time Analysis | Real-time | On-demand | Bar/line charts |
| SLA Compliance Report | Real-time | On-demand | Gauge chart, table |
| User Activity Summary | Real-time | On-demand | Table |
| Monthly Trend Report | Scheduled | Monthly | PDF export |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Email Service (SMTP/SendGrid) | Send notification emails | POST | API Key |
| SMS Gateway (Twilio) | Send SMS alerts for overdue steps | POST | API Key |
| M01 Auth Service | Resolve user roles and department assignments | REST | JWT |
| M02 Permission Service | Check user permissions for workflow actions | REST | JWT |
| M03 Org Service | Get department hierarchy and org structure | REST | JWT |
| M29 E-Signature | Trigger signature requests after workflow approval | POST | OAuth2 |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| workflow.run.created | {run_id, workflow_id, creator_id, department_id} | M07 Purchasing, M22 Correspondence |
| workflow.run.completed | {run_id, workflow_id, completion_time, all_approvals} | M29 E-Signature, M04 Accounting |
| workflow.step.approved | {run_id, step_id, approver_id, action_type} | M20 Chat, M23 Social Network |
| workflow.step.overdue | {run_id, step_id, assignee_id, deadline} | M06 Task Management |
| workflow.run.rejected | {run_id, workflow_id, rejector_id, reason} | M22 Correspondence |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| employee.transferred | M18 Labor Relations | Reassign pending steps to new department |
| employee.departed | M01 Auth Service | Escalate all pending steps to manager |
| department.restructured | M03 Org Service | Update department-scoped workflows |
| role.changed | M02 Permission Service | Refresh role-based step assignments |

---

## 10. Technical Notes

### Performance
- Expected data volume: 10,000+ workflow runs per year, 50,000+ action records
- Query complexity: Medium (multi-table joins for run status, department stats)
- Expected response time: <200ms for list views, <500ms for complex reports
- Pagination required for all list endpoints (max 100 per page)

### Caching Strategy
- Department statistics cached with 5-minute TTL (Redis)
- Workflow template definitions cached with 1-hour TTL
- Custom filter definitions cached per user with 30-minute TTL
- Cache invalidation on workflow run creation, step action, or template update

### File Attachments
- Supported types: pdf, docx, doc, jpg, png, xlsx, csv
- Max size: 10MB per file, 50MB total per run
- Storage: S3-compatible object storage with CDN distribution

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY (Vietnamese), ISO 8601 for API
- Currency: VND for amount fields in workflow forms
- Number format: Vietnamese (1.000.000 for one million)

### Mobile Considerations
- Mobile-optimized run detail view for approvals on-the-go
- Push notifications for step assignments and overdue alerts
- Offline support for viewing saved filter definitions
- Simplified action interface (approve/reject with comment) optimized for touch
- Mobile app icon shown in app grid (image_05)

### Security
- All workflow form data encrypted at rest (AES-256)
- Audit log is append-only; no modifications allowed after creation
- Sensitive fields in form_data can be masked based on user permissions
- CSRF protection on all state-changing endpoints
- Rate limiting: 100 requests per minute per user

### Scalability
- Horizontal scaling for API layer behind load balancer
- Read replicas for reporting queries
- Asynchronous processing for notifications and escalations (message queue)
- Workflow step execution engine designed for concurrent run processing
