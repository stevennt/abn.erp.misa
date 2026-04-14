# M06 Task Management Module Specification

## 1. Module Overview

### 1.1 Purpose

The Task Management Module (M06) is a collaborative project and task management system within the ABN ERP platform. It enables teams to create, assign, track, and complete tasks across projects with real-time collaboration features including chat, @mentions, file sharing, checklists, and automated notifications. The module provides comprehensive task tracking with status management, priority assignment, deadline monitoring, and progress reporting.

Key operational metrics: Current week shows 16 tasks total with 1 on-time, 3 late, 4 incomplete, 4 pending, and 4 overdue. Team chat supports 8 members with integrated Figma design sharing and @mention notifications. Task status distribution: 1 overdue, 2 due today, 0 soon, 0 awaiting, 1 assigned. The module integrates with design tools (Figma), supports file attachments, checklist items, and auto-generated notifications for task events.

### 1.2 Personas

| Persona | Role | Key Responsibilities | Primary Screens | Access Level |
|---------|------|---------------------|-----------------|--------------|
| Project Manager | Project oversight | Create tasks, assign work, monitor progress, manage deadlines | Project Board, Task List, Reports | Full |
| Team Member | Task execution | Complete assigned tasks, update status, collaborate | My Tasks, Chat, Task Detail | Read/Write (own) |
| Team Lead | Team coordination | Review team tasks, provide feedback, escalate issues | Team Board, Task Review | Read/Write (team) |
| Department Manager | Portfolio oversight | View all projects in department, resource allocation | Portfolio Dashboard | Read-only |
| Stakeholder | Progress monitoring | Review project status, milestones, deliverables | Project Summary, Reports | Read-only |
| External Collaborator | Limited participation | View assigned tasks, comment, upload files | Task Detail, Chat | Limited |
| Administrator | System management | Configure projects, manage access, templates | Settings, Templates | Full |

### 1.3 Dependencies

| Dependency | Type | Direction | Description |
|-----------|------|-----------|-------------|
| M05 Employee Management | Upstream | Inbound | Employee data for task assignment |
| M07 Purchasing | Peer | Bidirectional | Purchase-related tasks and approvals |
| M08 Sales | Peer | Bidirectional | Sales-related tasks and follow-ups |
| M09 Inventory/Warehouse | Peer | Bidirectional | Inventory-related tasks |
| M10 Asset Management | Peer | Bidirectional | Asset maintenance tasks |
| M11 Contract Management | Peer | Bidirectional | Contract milestone tracking |
| Figma Integration | External | Inbound | Design file preview and linking |
| Notification Service | External | Outbound | Email, push, in-app notifications |

### 1.4 Business Rules

1. **Task Uniqueness**: Each task must have a unique identifier within its project (e.g., PROJ-001, PROJ-002).
2. **Assignment Requirement**: Every task must have at least one assignee before it can be marked as in progress.
3. **Status Progression**: Tasks must follow defined status flow: Backlog -> To Do -> In Progress -> Review -> Done.
4. **Priority Levels**: Four priority levels: Low, Medium, High, Critical. Critical tasks require daily status updates.
5. **Due Date Enforcement**: Tasks without due dates cannot be set to In Progress status.
6. **Overdue Detection**: System checks for overdue tasks every hour and sends notifications to assignee and project manager.
7. **Chat History**: All task-related chat messages are preserved and linked to the task for audit purposes.
8. **@Mention Notifications**: When a user is @mentioned, they receive an immediate notification with a link to the task.
9. **File Attachment Limit**: Maximum 10 attachments per task, 25MB per file, supported formats: all common types.
10. **Checklist Completion**: Task can only be marked Done when all checklist items are checked.
11. **Project Access Control**: Users can only see tasks from projects they have been granted access to.
12. **Task Archival**: Completed tasks older than 90 days are archived but remain searchable.

---

## 2. Screen Inventory

### 2.1 Screen Routes and Purposes

| Route | Purpose | Personas | Priority |
|-------|---------|----------|----------|
| `/tasks/dashboard` | Task dashboard with weekly summary, status counts | All personas | Critical |
| `/tasks/my-tasks` | Personal task list with filters and status | Team Member, Team Lead | Critical |
| `/tasks/projects/{id}/board` | Kanban board for project tasks | Project Manager, Team Member | Critical |
| `/tasks/projects/{id}/list` | List view of project tasks | Project Manager, Team Lead | High |
| `/tasks/projects/{id}/timeline` | Gantt/timeline view of project schedule | Project Manager, Stakeholder | Medium |
| `/tasks/projects` | Project list and creation | Project Manager, Admin | High |
| `/tasks/{task_id}` | Task detail with chat, checklist, files | All personas | Critical |
| `/tasks/calendar` | Calendar view of task deadlines | All personas | High |
| `/tasks/reports` | Task reports: velocity, completion, overdue analysis | Project Manager, Dept Manager | High |
| `/tasks/settings` | Project settings, templates, access control | Project Manager, Admin | Medium |

### 2.2 UI Components by Screen

#### /tasks/dashboard

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Weekly Summary Card | Metric Card | 16 tasks: 1 on-time, 3 late, 4 incomplete, 4 pending, 4 overdue | Task aggregation |
| Status Breakdown | Badge Group | Overdue 1, Due Today 2, Soon 0, Awaiting 0, Assigned 1 | Task status query |
| My Tasks Widget | List | Personal task list with status | Task WHERE assignee = user |
| Project Progress | Progress Bar | Project completion percentage | Task completion rate |
| Overdue Alert Panel | Alert List | Overdue tasks with assignee and due date | Task WHERE overdue |
| Activity Feed | Timeline | Recent task updates, comments, completions | Task activity log |
| Quick Create Button | Action Button | Create new task with quick form | Task creation |
| Search Bar | Input | Search across all accessible tasks | Full-text search |

#### /tasks/{task_id} (Task Detail)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Task Header | Info Panel | Title, ID, project, status, priority, due date | Task master |
| Description Editor | Rich Text | Task description with formatting | Task.description |
| Assignee Selector | Avatar Group | Task assignees with add/remove | TaskAssignment |
| Status Dropdown | Select | Change task status with validation | Task.status |
| Priority Selector | Button Group | Set task priority level | Task.priority |
| Due Date Picker | Date Picker | Set or change due date | Task.due_date |
| Chat Panel | Message Thread | 8-member chat with @mentions, file sharing | TaskComment |
| Checklist | Checkbox List | Task checklist items with completion | TaskChecklist |
| File Attachments | File Grid | Attached files with preview | TaskAttachment |
| Figma Preview | Embedded View | Figma design preview and link | External Figma API |
| Activity Log | Timeline | Task history: status changes, assignments, edits | Task audit log |
| Notification Settings | Toggle | Subscribe/unsubscribe from task notifications | User preferences |

#### /tasks/projects/{id}/board (Kanban Board)

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Column Headers | Header Row | Backlog, To Do, In Progress, Review, Done | Status config |
| Task Cards | Card Component | Draggable task cards with key info | Task list |
| Drag-and-Drop | Interaction | Move tasks between columns | Task status update |
| Column Counters | Badge | Task count per status column | Task aggregation |
| Column WIP Limits | Config | Work-in-progress limits per column | Board configuration |
| Filter Bar | Filter Panel | Filter by assignee, priority, tag | User input |
| Swimlane Toggle | Toggle | Group by assignee or priority | Board view config |
| Bulk Actions | Dropdown | Mass update selected tasks | Task selection |

### 2.3 User Interactions

1. **Task Creation**: Project Manager clicks "Create Task" -> fills title, description, assigns to team member -> sets priority and due date -> adds checklist items -> saves task -> assignee receives notification.
2. **Status Update**: Team member drags task card from "To Do" to "In Progress" on Kanban board -> system validates assignment and due date -> updates status -> notifies project manager.
3. **Chat Collaboration**: Team member opens task detail -> types message in chat panel -> uses @mention to notify colleague -> attaches reference file -> colleague receives notification with mention.
4. **Checklist Management**: Task creator adds checklist items -> assignee checks items as completed -> all items checked -> task eligible for Done status.
5. **Figma Integration**: Designer clicks "Link Figma" -> authenticates with Figma -> selects design file -> preview embedded in task detail -> team can view without leaving task.

### 2.4 Data Displayed

- Task Card: ID (PROJ-001), Title, Assignee avatar, Priority badge, Due date, Status indicator, Tag chips
- Weekly Summary: Total 16, On-time 1, Late 3, Incomplete 4, Pending 4, Overdue 4
- Status Breakdown: Overdue 1, Due Today 2, Soon 0, Awaiting 0, Assigned 1
- Chat Panel: 8 members, message history, @mention highlights, file thumbnails
- Checklist: Items with checkboxes, completion percentage, overdue items highlighted

---

## 3. Feature Specifications

### 3.1 Task Creation and Management

**Description**: Create and manage tasks with title, description, assignees, priority, due date, tags, and project association.

**User Stories**:
- As a Project Manager, I want to create tasks with all necessary details so that team members have clear instructions.
- As a Team Member, I want to see my assigned tasks with priorities so that I know what to work on first.
- As a Project Manager, I want to create tasks from templates so that common task types are set up quickly.

**Acceptance Criteria**:
1. Task creation form with: title (required), description, project (required), assignees (at least one), priority, due date, tags.
2. Auto-generated task ID within project (PROJ-001, PROJ-002, etc.).
3. Task templates for recurring task types (bug fix, feature, review, testing).
4. Bulk task creation from spreadsheet import.
5. Task cloning for similar tasks.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-001 | Title | Required, 3-200 chars | "Title is required (3-200 characters)" |
| VR-002 | Project | Required | "Project must be selected" |
| VR-003 | Assignee | At least one assignee for In Progress | "Task must have an assignee" |
| VR-004 | Due Date | Required for In Progress status | "Due date required to start task" |
| VR-005 | Priority | Required, valid level | "Valid priority level is required" |
| VR-006 | Tags | Max 10 tags per task | "Maximum 10 tags allowed" |
| VR-007 | Description | Max 10,000 chars | "Description too long" |

**Edge Cases**:
- Task creation in archived project: Blocked with clear message.
- Bulk assignment: Assign multiple tasks to different users simultaneously.
- Recurring tasks: Auto-generate tasks on schedule (daily, weekly, monthly).
- Task dependency: Task B cannot start until Task A is complete.

### 3.2 Task Chat and Collaboration

**Description**: Real-time chat within task context supporting @mentions, file sharing, emoji reactions, and message threading.

**User Stories**:
- As a Team Member, I want to discuss task details in a dedicated chat so that all communication is contextual.
- As a Team Member, I want to @mention colleagues so that they are notified of important information.
- As a Designer, I want to share Figma designs directly in the task chat so that the team can review them.

**Acceptance Criteria**:
1. Chat panel within task detail shows all messages for that task.
2. @mention triggers immediate notification to mentioned user.
3. File attachments in chat limited to 25MB per file.
4. Figma design links render as embedded preview.
5. Message editing within 15 minutes of posting, with edit history.
6. Emoji reactions on messages.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-010 | Message | Required, max 5000 chars | "Message cannot be empty" |
| VR-011 | Attachment | Max 25MB per file | "File size exceeds 25MB limit" |
| VR-012 | File Type | Supported types only | "File type not supported" |
| VR-013 | Access | Must have project access | "No access to this task" |

**Edge Cases**:
- Offline messaging: Messages queued and delivered when user reconnects.
- Deleted messages: Marked as deleted but preserved in audit log.
- External collaborators: Limited chat visibility based on access level.
- Chat search: Full-text search within task chat history.

### 3.3 Checklist Management

**Description**: Task checklists with items that can be created, checked, reordered, and tracked for completion.

**User Stories**:
- As a Task Creator, I want to add checklist items to break down complex tasks so that progress is trackable.
- As a Team Member, I want to check off items as I complete them so that progress is visible.
- As a Project Manager, I want to see checklist completion percentage so that I can gauge task progress.

**Acceptance Criteria**:
1. Unlimited checklist items per task.
2. Items can be reordered by drag-and-drop.
3. Completion percentage calculated automatically.
4. Task can only be marked Done when all checklist items are checked.
5. Overdue checklist items highlighted (items with due date before task due date).

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-020 | Item Text | Required, max 500 chars | "Checklist item text required" |
| VR-021 | Done Status | Cannot mark task Done with unchecked items | "All checklist items must be completed" |
| VR-022 | Item Due Date | Cannot be after task due date (warning only) | "Item due date is after task due date" |

**Edge Cases**:
- Checklist template: Apply predefined checklist to task.
- Sub-items: Nested checklist items for complex breakdowns.
- Assignee per item: Different checklist items assigned to different people.

### 3.4 Task Notifications

**Description**: Automated notifications for task events including assignment, status changes, due date approaching, overdue, @mentions, and comments.

**User Stories**:
- As a Team Member, I want to be notified when I am assigned a task so that I can start working on it.
- As a Project Manager, I want to be notified when tasks become overdue so that I can take action.
- As a Team Member, I want notifications when I am @mentioned so that I can respond promptly.

**Acceptance Criteria**:
1. Notification types: assignment, status change, due date warning (24h before), overdue, @mention, comment, file attachment.
2. Notification channels: in-app, email, push (mobile).
3. Notification preferences per user per notification type.
4. Digest mode: daily summary of notifications.
5. Notification grouping: batch similar notifications.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-030 | User | Must be project member | "User does not have access" |
| VR-031 | Channel | Valid channel | "Invalid notification channel" |
| VR-032 | Frequency | Valid frequency | "Invalid notification frequency" |

**Edge Cases**:
- Notification flood: Rate limit notifications for rapidly changing tasks.
- Muted tasks: User can mute specific tasks to stop notifications.
- Escalation: Overdue critical tasks escalate to project manager's manager.

### 3.5 Project Board and Views

**Description**: Multiple project views including Kanban board, list view, timeline/Gantt view, and calendar view.

**User Stories**:
- As a Project Manager, I want a Kanban board so that I can visualize task flow.
- As a Team Member, I want a list view with sorting and filtering so that I can find my tasks quickly.
- As a Project Manager, I want a timeline view so that I can see project schedule and dependencies.

**Acceptance Criteria**:
1. Kanban board with drag-and-drop status updates.
2. List view with sortable columns, filters, and grouping.
3. Timeline/Gantt view showing task durations and dependencies.
4. Calendar view showing task due dates.
5. View preferences saved per user per project.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-040 | Access | Must have project access | "No access to this project" |
| VR-041 | WIP Limit | Cannot exceed column WIP limit | "WIP limit reached for this column" |
| VR-042 | Drag Target | Valid status transition | "Cannot transition to this status" |

**Edge Cases**:
- Large projects: Virtual scrolling for boards with 500+ tasks.
- Cross-project view: Aggregate board showing tasks across multiple projects.
- Custom columns: User-defined status columns for custom workflows.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+------------------+       +-------------------+       +-------------------+
|   TaskProject    |       |     Task          |       |  TaskPriority     |
+------------------+       +-------------------+       +-------------------+
| PK project_id    |<----->| PK task_id        |       | PK priority_id    |
| project_name     |       | FK project_id     |       | priority_name     |
| project_key      |       | task_number       |       | priority_level    |
| description      |       | title             |       | color             |
| status           |       | description       |       | is_active         |
| FK manager_id    |       | FK priority_id    |       +-------------------+
| start_date       |       | FK status_id      |
| end_date         |       | FK assigned_to    |       +-------------------+
+------------------+       | due_date          |       |   TaskStatus      |
                           | completed_date    |       +-------------------+
                           | status            |       | PK status_id      |
                           +-------------------+       | status_name       |
                                 |                     | status_order      |
                    +---------------------------+      | is_terminal       |
                    |    TaskAssignment         |      +-------------------+
                    +---------------------------+
                    | PK assignment_id          |
                    | FK task_id                |
                    | FK user_id                |
                    | assignment_role           |
                    | assigned_at               |
                    +---------------------------+

                    +-------------------+       +-------------------+
                    |   TaskComment     |       | TaskAttachment    |
                    +-------------------+       +-------------------+
                    | PK comment_id     |       | PK attachment_id  |
                    | FK task_id        |       | FK task_id        |
                    | FK user_id        |       | FK user_id        |
                    | message           |       | file_name         |
                    | mentioned_users   |       | file_url          |
                    | created_at        |       | file_size         |
                    | edited_at         |       | file_type         |
                    +-------------------+       | uploaded_at       |
                                                +-------------------+

                    +-------------------+       +-------------------+
                    | TaskChecklist     |       |    TaskTag        |
                    +-------------------+       +-------------------+
                    | PK checklist_id   |       | PK tag_id         |
                    | FK task_id        |       | tag_name          |
                    | item_text         |       | color             |
                    | is_completed      |       | is_active         |
                    | FK completed_by   |       +-------------------+
                    | completed_at      |
                    | item_order        |       +-------------------+
                    | due_date          |       |  ProjectTaskTag   |
                    | FK assigned_to    |       +-------------------+
                    +-------------------+       | PK project_id     |
                                                | PK tag_id         |
                                                +-------------------+
```

### 4.2 Table Schemas

#### TaskProject

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| project_id | BIGINT | PK, SERIAL | Unique project identifier |
| project_name | NVARCHAR(200) | NOT NULL | Project name |
| project_key | VARCHAR(10) | NOT NULL, UNIQUE | Short key (e.g., PROJ, DEV) |
| description | TEXT | NULL | Project description |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active, on_hold, completed, archived |
| FK manager_id | BIGINT | NOT NULL, FK REFERENCES User | Project manager |
| start_date | DATE | NULL | Project start date |
| end_date | DATE | NULL | Project target end date |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### Task

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| task_id | BIGINT | PK, SERIAL | Unique task identifier |
| FK project_id | BIGINT | NOT NULL, FK REFERENCES TaskProject | Parent project |
| task_number | INT | NOT NULL | Sequential number within project |
| title | NVARCHAR(200) | NOT NULL | Task title |
| description | TEXT | NULL | Task description (rich text) |
| FK priority_id | BIGINT | NOT NULL, FK REFERENCES TaskPriority | Priority level |
| FK status_id | BIGINT | NOT NULL, FK REFERENCES TaskStatus | Current status |
| due_date | DATE | NULL | Task due date |
| completed_date | TIMESTAMP | NULL | Actual completion timestamp |
| estimated_hours | DECIMAL(5,2) | NULL | Estimated effort hours |
| actual_hours | DECIMAL(5,2) | DEFAULT 0 | Actual effort hours |
| FK created_by | BIGINT | NOT NULL, FK REFERENCES User | Task creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |
| parent_task_id | BIGINT | NULL, FK REFERENCES Task | Parent task for subtasks |
| checklist_total | INT | DEFAULT 0 | Total checklist items |
| checklist_completed | INT | DEFAULT 0 | Completed checklist items |

#### TaskPriority

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| priority_id | BIGINT | PK, SERIAL | Unique priority ID |
| priority_name | NVARCHAR(50) | NOT NULL, UNIQUE | Priority name (Low, Medium, High, Critical) |
| priority_level | INT | NOT NULL, UNIQUE | Numeric level (1-4) |
| color | VARCHAR(7) | NOT NULL | Hex color code for display |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

#### TaskStatus

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| status_id | BIGINT | PK, SERIAL | Unique status ID |
| status_name | NVARCHAR(50) | NOT NULL, UNIQUE | Status name |
| status_order | INT | NOT NULL | Display/transition order |
| is_terminal | BOOLEAN | NOT NULL, DEFAULT FALSE | Final status (Done, Cancelled) |
| color | VARCHAR(7) | NOT NULL | Hex color code |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

#### TaskAssignment

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| assignment_id | BIGINT | PK, SERIAL | Unique assignment ID |
| FK task_id | BIGINT | NOT NULL, FK REFERENCES Task | Task reference |
| FK user_id | BIGINT | NOT NULL, FK REFERENCES User | Assigned user |
| assignment_role | VARCHAR(20) | NOT NULL, DEFAULT 'assignee' | assignee, reviewer, observer |
| assigned_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Assignment timestamp |
| FK assigned_by | BIGINT | FK REFERENCES User | User who made assignment |

#### TaskComment

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| comment_id | BIGINT | PK, SERIAL | Unique comment ID |
| FK task_id | BIGINT | NOT NULL, FK REFERENCES Task | Task reference |
| FK user_id | BIGINT | NOT NULL, FK REFERENCES User | Comment author |
| message | TEXT | NOT NULL | Comment message |
| mentioned_users | JSONB | DEFAULT '[]' | Array of mentioned user IDs |
| has_attachment | BOOLEAN | DEFAULT FALSE | Whether comment has files |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |
| edited_at | TIMESTAMP | NULL | Last edit timestamp |
| is_deleted | BOOLEAN | DEFAULT FALSE | Soft delete flag |

#### TaskAttachment

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| attachment_id | BIGINT | PK, SERIAL | Unique attachment ID |
| FK task_id | BIGINT | NOT NULL, FK REFERENCES Task | Task reference |
| FK comment_id | BIGINT | NULL, FK REFERENCES TaskComment | Associated comment |
| FK user_id | BIGINT | NOT NULL, FK REFERENCES User | Uploader |
| file_name | VARCHAR(255) | NOT NULL | Original file name |
| file_url | VARCHAR(500) | NOT NULL | Storage URL |
| file_size | BIGINT | NOT NULL | File size in bytes |
| file_type | VARCHAR(100) | NOT NULL | MIME type |
| thumbnail_url | VARCHAR(500) | NULL | Thumbnail URL (for images) |
| figma_file_id | VARCHAR(100) | NULL | Figma file ID (if linked) |
| uploaded_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Upload timestamp |

#### TaskChecklist

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| checklist_id | BIGINT | PK, SERIAL | Unique checklist item ID |
| FK task_id | BIGINT | NOT NULL, FK REFERENCES Task | Task reference |
| item_text | NVARCHAR(500) | NOT NULL | Checklist item text |
| is_completed | BOOLEAN | NOT NULL, DEFAULT FALSE | Completion status |
| FK completed_by | BIGINT | NULL, FK REFERENCES User | User who completed |
| completed_at | TIMESTAMP | NULL | Completion timestamp |
| item_order | INT | NOT NULL | Display order |
| due_date | DATE | NULL | Item-specific due date |
| FK assigned_to | BIGINT | NULL, FK REFERENCES User | Item assignee |

#### TaskTag

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| tag_id | BIGINT | PK, SERIAL | Unique tag ID |
| tag_name | NVARCHAR(50) | NOT NULL, UNIQUE | Tag name |
| color | VARCHAR(7) | NOT NULL | Hex color code |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

#### ProjectTaskTag

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| project_id | BIGINT | NOT NULL, FK REFERENCES TaskProject | Project reference |
| tag_id | BIGINT | NOT NULL, FK REFERENCES TaskTag | Tag reference |
| PRIMARY KEY | (project_id, tag_id) | Composite | Project-tag association |

### 4.3 Indexes

```sql
-- TaskProject indexes
CREATE INDEX idx_project_key ON TaskProject(project_key);
CREATE INDEX idx_project_status ON TaskProject(status);
CREATE INDEX idx_project_manager ON TaskProject(manager_id);
CREATE INDEX idx_project_dates ON TaskProject(start_date, end_date);

-- Task indexes
CREATE UNIQUE INDEX idx_task_project_number ON Task(project_id, task_number);
CREATE INDEX idx_task_project ON Task(project_id);
CREATE INDEX idx_task_status ON Task(status_id);
CREATE INDEX idx_task_priority ON Task(priority_id);
CREATE INDEX idx_task_due_date ON Task(due_date);
CREATE INDEX idx_task_assignee ON TaskAssignment(task_id, user_id);
CREATE INDEX idx_task_created_by ON Task(created_by);
CREATE INDEX idx_task_parent ON Task(parent_task_id);
CREATE INDEX idx_task_overdue ON Task(due_date, status_id) WHERE due_date < CURRENT_DATE;

-- TaskComment indexes
CREATE INDEX idx_comment_task ON TaskComment(task_id);
CREATE INDEX idx_comment_user ON TaskComment(user_id);
CREATE INDEX idx_comment_created ON TaskComment(created_at);
CREATE INDEX idx_comment_mentions ON TaskComment USING GIN(mentioned_users);

-- TaskAttachment indexes
CREATE INDEX idx_attachment_task ON TaskAttachment(task_id);
CREATE INDEX idx_attachment_user ON TaskAttachment(user_id);
CREATE INDEX idx_attachment_type ON TaskAttachment(file_type);

-- TaskChecklist indexes
CREATE INDEX idx_checklist_task ON TaskChecklist(task_id);
CREATE INDEX idx_checklist_completed ON TaskChecklist(is_completed);
CREATE INDEX idx_checklist_assigned ON TaskChecklist(assigned_to);
CREATE INDEX idx_checklist_order ON TaskChecklist(task_id, item_order);

-- TaskTag indexes
CREATE INDEX idx_project_tag_project ON ProjectTaskTag(project_id);
CREATE INDEX idx_project_tag_tag ON ProjectTaskTag(tag_id);
```

### 4.4 Summary of Tables

| Table | Primary Key | Foreign Keys | Purpose | Estimated Rows |
|-------|-------------|--------------|---------|----------------|
| TaskProject | project_id | User (manager) | Project definitions | 50 |
| Task | task_id | TaskProject, TaskPriority, TaskStatus, User | Task records | 10,000 |
| TaskPriority | priority_id | - | Priority level definitions | 4 |
| TaskStatus | status_id | - | Status definitions | 7 |
| TaskAssignment | assignment_id | Task, User | Task-to-user assignments | 25,000 |
| TaskComment | comment_id | Task, User | Task chat messages | 50,000 |
| TaskAttachment | attachment_id | Task, TaskComment, User | Task file attachments | 15,000 |
| TaskChecklist | checklist_id | Task, User | Task checklist items | 30,000 |
| TaskTag | tag_id | - | Tag definitions | 50 |
| ProjectTaskTag | (project_id, tag_id) | TaskProject, TaskTag | Project-tag mapping | 200 |

---

## 5. API Specifications

### 5.1 Task API

#### POST /api/v1/tasks/projects/{project_id}/tasks

Create a new task.

**Request Body**:
```json
{
  "title": "Implement user authentication flow",
  "description": "<p>Implement OAuth2 authentication flow for the web application.</p>",
  "priority_id": 3,
  "status_id": 1,
  "due_date": "2026-04-28",
  "estimated_hours": 16,
  "assignees": [
    {"user_id": 101, "role": "assignee"},
    {"user_id": 102, "role": "reviewer"}
  ],
  "checklist": [
    {"item_text": "Set up OAuth2 provider configuration", "item_order": 1},
    {"item_text": "Implement login endpoint", "item_order": 2, "assigned_to": 101},
    {"item_text": "Create login UI component", "item_order": 3, "assigned_to": 101},
    {"item_text": "Write unit tests", "item_order": 4}
  ],
  "tag_ids": [5, 12]
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "task_id": 1601,
    "project_id": 45,
    "project_key": "PROJ",
    "task_number": 16,
    "display_id": "PROJ-016",
    "title": "Implement user authentication flow",
    "priority_id": 3,
    "priority_name": "High",
    "status_id": 1,
    "status_name": "Backlog",
    "due_date": "2026-04-28",
    "estimated_hours": 16,
    "created_by": 100,
    "created_at": "2026-04-14T09:00:00Z",
    "assignees": [
      {"user_id": 101, "name": "Nguyen Van A", "role": "assignee"},
      {"user_id": 102, "name": "Tran Thi B", "role": "reviewer"}
    ],
    "checklist": [
      {"checklist_id": 5001, "item_text": "Set up OAuth2 provider configuration", "is_completed": false, "item_order": 1},
      {"checklist_id": 5002, "item_text": "Implement login endpoint", "is_completed": false, "item_order": 2, "assigned_to": 101},
      {"checklist_id": 5003, "item_text": "Create login UI component", "is_completed": false, "item_order": 3, "assigned_to": 101},
      {"checklist_id": 5004, "item_text": "Write unit tests", "is_completed": false, "item_order": 4}
    ],
    "tags": [
      {"tag_id": 5, "tag_name": "Backend", "color": "#3B82F6"},
      {"tag_id": 12, "tag_name": "Authentication", "color": "#8B5CF6"}
    ]
  }
}
```

#### GET /api/v1/tasks/projects/{project_id}/tasks

List tasks within a project.

**Query Parameters**:
- `status_id` (int): Filter by status
- `priority_id` (int): Filter by priority
- `assignee_id` (int): Filter by assignee
- `tag_id` (int): Filter by tag
- `search` (string): Full-text search on title and description
- `sort` (string): Sort field (due_date, priority, created_at, status)
- `order` (string): asc or desc
- `page` (int): Page number
- `per_page` (int): Items per page

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "tasks": [
      {
        "task_id": 1601,
        "display_id": "PROJ-016",
        "title": "Implement user authentication flow",
        "priority_name": "High",
        "priority_color": "#EF4444",
        "status_name": "Backlog",
        "status_color": "#6B7280",
        "due_date": "2026-04-28",
        "assignees": [{"user_id": 101, "name": "Nguyen Van A", "avatar_url": "/avatars/101.jpg"}],
        "checklist_progress": {"total": 4, "completed": 0, "percentage": 0},
        "attachment_count": 0,
        "comment_count": 0,
        "is_overdue": false,
        "created_at": "2026-04-14T09:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 20,
      "total_pages": 1,
      "total_count": 16
    }
  }
}
```

#### GET /api/v1/tasks/{task_id}

Get task detail with all associated data.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "task_id": 1601,
    "display_id": "PROJ-016",
    "project_key": "PROJ",
    "project_name": "ABN ERP Platform",
    "title": "Implement user authentication flow",
    "description": "<p>Implement OAuth2 authentication flow...</p>",
    "priority_id": 3,
    "priority_name": "High",
    "status_id": 1,
    "status_name": "Backlog",
    "due_date": "2026-04-28",
    "estimated_hours": 16,
    "actual_hours": 0,
    "created_by": {"user_id": 100, "name": "Project Manager"},
    "created_at": "2026-04-14T09:00:00Z",
    "updated_at": "2026-04-14T09:00:00Z",
    "assignees": [
      {"user_id": 101, "name": "Nguyen Van A", "role": "assignee", "avatar_url": "/avatars/101.jpg"},
      {"user_id": 102, "name": "Tran Thi B", "role": "reviewer", "avatar_url": "/avatars/102.jpg"}
    ],
    "checklist": {
      "total": 4,
      "completed": 0,
      "percentage": 0,
      "items": [
        {"checklist_id": 5001, "item_text": "Set up OAuth2 provider configuration", "is_completed": false, "item_order": 1},
        {"checklist_id": 5002, "item_text": "Implement login endpoint", "is_completed": false, "item_order": 2}
      ]
    },
    "attachments": [],
    "comments": {
      "count": 0,
      "recent": []
    },
    "tags": [
      {"tag_id": 5, "tag_name": "Backend", "color": "#3B82F6"},
      {"tag_id": 12, "tag_name": "Authentication", "color": "#8B5CF6"}
    ],
    "activity_log": [
      {"action": "created", "user": "Project Manager", "timestamp": "2026-04-14T09:00:00Z"}
    ]
  }
}
```

#### PUT /api/v1/tasks/{task_id}/status

Update task status.

**Request Body**:
```json
{
  "status_id": 2,
  "reason": "Starting work on this task"
}
```

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "task_id": 1601,
    "previous_status": "Backlog",
    "new_status": "To Do",
    "updated_at": "2026-04-14T10:00:00Z",
    "updated_by": 101
  }
}
```

### 5.2 Task Comment API

#### POST /api/v1/tasks/{task_id}/comments

Add a comment to a task.

**Request Body**:
```json
{
  "message": "@NguyenVanA Please review the Figma design for the login page: https://figma.com/file/abc123",
  "attachment_ids": [301, 302]
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "comment_id": 8001,
    "task_id": 1601,
    "user_id": 102,
    "user_name": "Tran Thi B",
    "message": "@NguyenVanA Please review the Figma design for the login page: https://figma.com/file/abc123",
    "mentioned_users": [101],
    "has_attachment": true,
    "attachments": [
      {"attachment_id": 301, "file_name": "login-mockup.png", "file_type": "image/png", "thumbnail_url": "/thumbnails/301.jpg"},
      {"attachment_id": 302, "file_name": "figma-link.txt", "file_type": "text/plain"}
    ],
    "created_at": "2026-04-14T11:00:00Z"
  }
}
```

### 5.3 Project Board API

#### GET /api/v1/tasks/projects/{project_id}/board

Get Kanban board data grouped by status.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "project_id": 45,
    "project_name": "ABN ERP Platform",
    "columns": [
      {
        "status_id": 1,
        "status_name": "Backlog",
        "status_color": "#6B7280",
        "wip_limit": null,
        "task_count": 4,
        "tasks": [
          {"task_id": 1601, "display_id": "PROJ-016", "title": "Implement user authentication flow", "priority_name": "High", "due_date": "2026-04-28"}
        ]
      },
      {
        "status_id": 2,
        "status_name": "To Do",
        "status_color": "#3B82F6",
        "wip_limit": 5,
        "task_count": 4,
        "tasks": []
      },
      {
        "status_id": 3,
        "status_name": "In Progress",
        "status_color": "#F59E0B",
        "wip_limit": 3,
        "task_count": 4,
        "tasks": []
      },
      {
        "status_id": 4,
        "status_name": "Review",
        "status_color": "#8B5CF6",
        "wip_limit": null,
        "task_count": 0,
        "tasks": []
      },
      {
        "status_id": 5,
        "status_name": "Done",
        "status_color": "#10B981",
        "wip_limit": null,
        "task_count": 4,
        "tasks": []
      }
    ]
  }
}
```

### 5.4 Dashboard API

#### GET /api/v1/tasks/dashboard

Get task dashboard summary.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "weekly_summary": {
      "total": 16,
      "on_time": 1,
      "late": 3,
      "incomplete": 4,
      "pending": 4,
      "overdue": 4
    },
    "status_breakdown": {
      "overdue": 1,
      "due_today": 2,
      "due_soon": 0,
      "awaiting_review": 0,
      "assigned": 1
    },
    "my_tasks": [
      {"task_id": 1601, "display_id": "PROJ-016", "title": "Implement user authentication flow", "due_date": "2026-04-28", "status_name": "Backlog", "priority_name": "High"}
    ],
    "recent_activity": [
      {"action": "comment", "task_id": 1600, "user": "Tran Thi B", "timestamp": "2026-04-14T11:00:00Z"}
    ],
    "team_chat_summary": {
      "active_members": 8,
      "messages_today": 24,
      "files_shared_today": 3
    }
  }
}
```

### 5.5 Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Task validation failed",
    "details": [
      {
        "field": "due_date",
        "message": "Due date is required for In Progress status"
      }
    ],
    "request_id": "req-task123"
  }
}
```

### 5.6 API Summary Table

| Endpoint | Method | Auth Required | Rate Limit | Description |
|----------|--------|---------------|------------|-------------|
| /api/v1/tasks/projects | POST | Yes | 30/min | Create project |
| /api/v1/tasks/projects | GET | Yes | 200/min | List projects |
| /api/v1/tasks/projects/{id}/tasks | POST | Yes | 100/min | Create task |
| /api/v1/tasks/projects/{id}/tasks | GET | Yes | 200/min | List tasks |
| /api/v1/tasks/projects/{id}/board | GET | Yes | 100/min | Get Kanban board |
| /api/v1/tasks/{id} | GET | Yes | 200/min | Get task detail |
| /api/v1/tasks/{id} | PUT | Yes | 100/min | Update task |
| /api/v1/tasks/{id} | DELETE | Yes | 30/min | Delete task |
| /api/v1/tasks/{id}/status | PUT | Yes | 100/min | Update status |
| /api/v1/tasks/{id}/assignees | POST | Yes | 50/min | Add assignee |
| /api/v1/tasks/{id}/comments | POST | Yes | 100/min | Add comment |
| /api/v1/tasks/{id}/comments | GET | Yes | 200/min | List comments |
| /api/v1/tasks/{id}/attachments | POST | Yes | 30/min | Upload attachment |
| /api/v1/tasks/{id}/attachments | GET | Yes | 200/min | List attachments |
| /api/v1/tasks/{id}/checklist | POST | Yes | 50/min | Add checklist item |
| /api/v1/tasks/{id}/checklist/{item_id} | PUT | Yes | 50/min | Toggle checklist item |
| /api/v1/tasks/dashboard | GET | Yes | 100/min | Dashboard summary |
| /api/v1/tasks/my-tasks | GET | Yes | 200/min | My assigned tasks |
| /api/v1/tasks/calendar | GET | Yes | 100/min | Calendar view |
| /api/v1/tasks/reports | GET | Yes | 30/min | Task reports |

---

## 6. Permissions Matrix

### 6.1 Role Definitions

| Role | Code | Description | Access Scope |
|------|------|-------------|--------------|
| Project Manager | PROJ_MGR | Full project control | All projects managed |
| Team Member | TEAM_MEMBER | Task execution | Assigned tasks |
| Team Lead | TEAM_LEAD | Team oversight | Team's projects |
| Department Manager | DEPT_MGR | Portfolio oversight | Department projects |
| Stakeholder | STAKEHOLDER | Progress monitoring | Read-only access |
| External Collaborator | EXTERNAL | Limited participation | Specific tasks |
| Administrator | ADMIN | System management | All projects |

### 6.2 CRUD Permissions

| Resource | PROJ_MGR | TEAM_MEMBER | TEAM_LEAD | DEPT_MGR | STAKEHOLDER | EXTERNAL | ADMIN |
|----------|----------|-------------|-----------|----------|-------------|----------|-------|
| TaskProject | CRUD | R (own) | R (team) | R (dept) | R | R (assigned) | CRUD |
| Task | CRUD | CRUD (own) | CRUD (team) | R (dept) | R | R/U (assigned) | CRUD |
| TaskComment | CRUD | CRUD | CRUD | R (dept) | R | R/U (own) | CRUD |
| TaskAttachment | CRUD | CRUD | CRUD | R (dept) | R | R (assigned) | CRUD |
| TaskChecklist | CRUD | CRUD | CRUD | R (dept) | R | R (assigned) | CRUD |
| TaskAssignment | CRUD | R | CRUD (team) | R (dept) | R | R | CRUD |

### 6.3 Row-Level Security

| Resource | Rule | Description |
|----------|------|-------------|
| TaskProject | User must be project member | `project_id IN (user.project_ids) OR user.role IN ('DEPT_MGR', 'ADMIN')` |
| Task | User must have access to parent project | Same as TaskProject |
| TaskComment | User must have access to parent task | Same as Task |
| TaskAttachment | User must have access to parent task | Same as Task |

---

## 7. State Machine

### 7.1 Task Status Flow

```
                    +-----------+
                    |  BACKLOG  |
                    +-----+-----+
                          |
                    (Plan)
                          v
                    +-----------+
             +----->|   TO DO   |
             |      +-----+-----+
             |            |
             |      (Start)
             |            v
             |      +-----------+
             |      | IN PROGRESS|
             |      +-----+-----+
             |            |
             |      (Submit Review)
             |            v
             |      +-----------+
             |      |  REVIEW   |
             |      +-----+-----+
             |            |
             |      +-----+-----+
             |      |           |
             v      v           |
        +------+ +-----------+  |
        | DONE | | REVISION  |--+
        +------+ +-----------+
           |
           v
        (Archived after 90 days)
```

### 7.2 State Transition Table

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| BACKLOG | TO DO | Plan task | Project Manager, Team Lead | Task has assignee |
| TO DO | IN PROGRESS | Start work | Assignee | Due date set, checklist exists |
| IN PROGRESS | REVIEW | Submit for review | Assignee | All checklist items done |
| REVIEW | DONE | Approve | Reviewer, Project Manager | Review passed |
| REVIEW | IN PROGRESS | Request revision | Reviewer | Changes needed |
| TO DO | BACKLOG | Deprioritize | Project Manager | - |
| IN PROGRESS | TO DO | Pause | Assignee, Project Manager | - |
| ANY | DONE | Emergency complete | Project Manager | Justification required |
| ANY | BACKLOG | Reopen | Project Manager | Only if not archived |

### 7.3 Approval Workflow

#### Task Completion Approval

```
Assignee completes work -> Submits for Review
     |
     v
Reviewer evaluates
     /           \
  Approve        Request Revision
     |             |
     v             v
  DONE          IN PROGRESS
  Archived        |
                  v
              Assignee revises
                  |
                  v
              Resubmit -> Loop
```

---

## 8. Reports

### 8.1 Report Inventory

| Report | Description | Dimensions | Metrics | Filters | Chart Type |
|--------|-------------|------------|---------|---------|------------|
| Task Completion | Tasks completed over time | Project, assignee, week | Completed count, completion rate | Period, project | Line chart |
| Velocity | Team velocity tracking | Sprint, team member | Story points completed, velocity | Sprint range | Bar chart |
| Overdue Analysis | Overdue task analysis | Project, assignee, priority | Overdue count, avg days overdue | Period, project | Heat map |
| Workload | Team workload distribution | Assignee, week | Assigned count, completed count | Period, team | Stacked bar |
| Cycle Time | Task cycle time analysis | Status transition, project | Avg days per transition, total cycle time | Period, project | Box plot |
| Checklist Completion | Checklist progress | Task, project | Completion %, items done | Period, project | Progress bar |
| Comment Activity | Collaboration metrics | Project, user, day | Comment count, response time | Period, project | Line chart |
| Project Status | Project health overview | Project, milestone | Task completion %, overdue count, budget | Current | Dashboard |

### 8.2 Report Summary Table

| Report ID | Report Name | Schedule | Auto-Generate | Distribution | Retention |
|-----------|-------------|----------|---------------|--------------|-----------|
| RPT-TASK-001 | Task Completion | Weekly | Yes | Project Manager, Team Lead | 2 years |
| RPT-TASK-002 | Velocity | Per Sprint | Yes | Project Manager, Team | 2 years |
| RPT-TASK-003 | Overdue Analysis | Weekly | Yes | Project Manager, Dept Manager | 1 year |
| RPT-TASK-004 | Workload | Weekly | Yes | Project Manager, Team Lead | 1 year |
| RPT-TASK-005 | Cycle Time | Monthly | Yes | Project Manager | 2 years |
| RPT-TASK-006 | Checklist Completion | Per Task | Yes | Assignee, Project Manager | 6 months |
| RPT-TASK-007 | Comment Activity | Monthly | Yes | Project Manager | 1 year |
| RPT-TASK-008 | Project Status | Daily | Yes | All stakeholders | Current only |

---

## 9. Integration Points

### 9.1 External API Integrations

| Integration | Provider | Protocol | Direction | Frequency | Data Exchanged |
|------------|----------|----------|-----------|-----------|----------------|
| Figma | Figma API | HTTPS/REST | Inbound | On demand | Design file previews, links |
| Email Notifications | SMTP/SendGrid | SMTP/API | Outbound | Real-time | Task notification emails |
| Push Notifications | Firebase/APNs | API | Outbound | Real-time | Mobile push notifications |
| Calendar Sync | Google Calendar, Outlook | API | Outbound | On change | Task due dates as calendar events |
| Slack Integration | Slack API | HTTPS/REST | Outbound | Real-time | Task notifications in channels |

### 9.2 Internal Module Integrations

| Source Module | Target | Data Flow | Trigger | Frequency |
|---------------|--------|-----------|---------|-----------|
| M06 Task Mgmt | M05 Employee Mgmt | Task assignee -> Employee | Task assignment | Real-time |
| M05 Employee Mgmt | M06 Task Mgmt | Employee -> Assignee profile | Employee update | Real-time |
| M07 Purchasing | M06 Task Mgmt | Purchase approval -> Task | Approval needed | Real-time |
| M08 Sales | M06 Task Mgmt | Sales follow-up -> Task | Sale created | Real-time |
| M09 Inventory | M06 Task Mgmt | Stock issue -> Task | Stock alert | Real-time |
| M10 Asset Mgmt | M06 Task Mgmt | Maintenance -> Task | Maintenance scheduled | Real-time |
| M11 Contract Mgmt | M06 Task Mgmt | Contract milestone -> Task | Milestone created | Real-time |

### 9.3 Webhook Events

| Event | Trigger | Payload | Subscribers |
|-------|---------|---------|-------------|
| task.created | New task created | task_id, project_id, title, assignees, due_date | Assignees, Project Manager |
| task.status_changed | Task status updated | task_id, from_status, to_status, user | Project Manager, Reviewer |
| task.overdue | Task becomes overdue | task_id, title, assignees, due_date | Assignee, Project Manager |
| task.comment | New comment added | task_id, comment_id, user, message, mentions | Task subscribers |
| task.mentioned | User @mentioned | task_id, comment_id, mentioned_user, message | Mentioned user |
| task.completed | Task marked Done | task_id, project_id, completed_by, completed_at | Project Manager, Subscribers |
| checklist.item_completed | Checklist item checked | task_id, checklist_id, completed_by | Task subscribers |
| project.created | New project created | project_id, project_name, manager_id | Department Manager |

---

## 10. Technical Notes

### 10.1 Performance Requirements

| Metric | Target | Measurement | Notes |
|--------|--------|-------------|-------|
| Task List Load | < 500ms | P95 latency | With 100 tasks, filters applied |
| Task Detail Load | < 800ms | P95 latency | With comments, attachments, checklist |
| Chat Message Send | < 200ms | P95 latency | Real-time WebSocket |
| Board Load | < 1s | P95 latency | Full Kanban board with all columns |
| File Upload | < 5s for 25MB | P95 latency | With thumbnail generation |
| Search | < 300ms | P95 latency | Full-text across tasks |
| Concurrent Users | 500+ | Simultaneous | Including real-time chat |

### 10.2 Caching Strategy

| Cache Type | Data | TTL | Invalidation |
|------------|------|-----|--------------|
| Redis | Task board data | 30 seconds | On task status change |
| Redis | Project list | 5 minutes | On project update |
| Redis | User profiles (assignees) | 15 minutes | On profile update |
| Redis | Task comments (recent) | 1 minute | On new comment |
| Redis | Dashboard summary | 2 minutes | On any task change |
| Application | Priority/Status catalogs | 1 hour | On catalog change |
| CDN | File thumbnails | 1 day | On file replacement |
| Browser | Task list (last view) | Session | On session end |

### 10.3 Key Files and Components

| File/Component | Path | Purpose |
|----------------|------|---------|
| Task Service | `src/modules/tasks/services/task.service.ts` | Task CRUD, validation, status transitions |
| Project Service | `src/modules/tasks/services/project.service.ts` | Project management |
| Comment Service | `src/modules/tasks/services/comment.service.ts` | Chat, @mention processing |
| Attachment Service | `src/modules/tasks/services/attachment.service.ts` | File upload, storage, thumbnail |
| Checklist Service | `src/modules/tasks/services/checklist.service.ts` | Checklist management |
| Notification Service | `src/modules/tasks/services/notification.service.ts` | Task event notifications |
| Board Service | `src/modules/tasks/services/board.service.ts` | Kanban board data assembly |
| Search Service | `src/modules/tasks/services/search.service.ts` | Full-text task search |
| Figma Service | `src/modules/tasks/services/figma.service.ts` | Figma integration |
| Report Generator | `src/modules/tasks/services/report-generator.service.ts` | Task report generation |

### 10.4 Localization

| Aspect | Configuration | Notes |
|--------|---------------|-------|
| Language | Vietnamese (primary), English | All labels, messages, notifications |
| Date Format | DD/MM/YYYY | Vietnamese standard |
| Time Format | 24-hour (HH:mm) | Vietnamese standard |
| Number Format | 1.234.567 (dot separator) | Vietnamese standard |
| Task ID Format | {PROJECT_KEY}-{NUMBER} | e.g., PROJ-016 |
| Priority Names | Vietnamese | Thap, Trung Binh, Cao, Nghiem Trong |
| Status Names | Vietnamese | Cho xu ly, Dang thuc hien, Da hoan thanh |

### 10.5 Mobile Support

| Feature | Mobile Support | Notes |
|---------|----------------|-------|
| Task List | Responsive | Swipe actions for status change |
| Task Detail | Responsive | Full detail with tabs |
| Kanban Board | Touch-optimized | Drag-and-drop on touch |
| Chat | Responsive | Real-time messaging |
| File Upload | Camera integration | Take photo, select from gallery |
| Notifications | Push | Real-time push notifications |
| Calendar | Touch-optimized | Tap to view task details |
| Checklist | Touch-friendly | Tap to check/uncheck |

### 10.6 Security Considerations

| Aspect | Implementation |
|--------|----------------|
| Project Access Control | Role-based with row-level security |
| File Storage | Encrypted at rest, signed URLs for access |
| Chat Encryption | TLS for in-transit, encrypted storage |
| Audit Trail | Complete history of all task changes |
| Data Retention | Archive completed tasks after 90 days |
| External Access | Time-limited access tokens for external collaborators |
| XSS Prevention | Sanitize rich text descriptions and comments |
| File Validation | MIME type checking, virus scanning on upload |
| Rate Limiting | Per-user rate limits on chat messages and file uploads |

### 10.7 Database Considerations

- Full-text search index on task title and description (PostgreSQL tsvector or Elasticsearch)
- Task comments stored with append-only pattern for audit
- Soft deletes for tasks (is_deleted flag)
- Partition TaskComment table by month for performance
- Materialized views for dashboard metrics (refreshed on task change)
- WebSocket connections for real-time chat and board updates
- File attachments stored in object storage (S3-compatible), only metadata in database

---

*Document Version: 1.0*
*Last Updated: 2026-04-14*
*Author: ERP Design Team*
*Review Status: Draft*
