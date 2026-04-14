# M14 - Recruitment (Tuyen dung)

**Category:** HR  
**Priority:** P3  
**Reference Images:** assets/image_36.png, assets/image_63.png  
**Dependencies:** M01 (Organization), M02 (Employee), M16 (Objectives & KPI), M18 (Labor Relations)  
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Recruitment module manages the full hiring lifecycle from job requisition and posting through candidate sourcing, screening, testing, interviewing, offer letter generation, and onboarding handoff. It provides a Kanban-style pipeline view for tracking candidates through stages, talent pool management for future needs, and analytics on recruitment effectiveness including time-to-fill, source effectiveness, and resignation reason analysis.

### Personas
| Persona | Role | Primary Use |
|---|---|---|
| Recruiter | HR staff | Manage job postings, screen candidates, schedule interviews, track pipeline |
| Hiring Manager | Department manager | Review candidates, conduct interviews, provide feedback, approve hires |
| HR Director | Executive | Monitor recruitment metrics, approve headcount, analyze effectiveness |
| Candidate | External applicant | Apply for positions, track application status (via career portal) |
| Interview Panel | Assessors | Submit interview feedback and scores |
| System Administrator | IT | Configure pipeline stages, email templates, talent pool categories |

### Dependencies
| Dependency | Module | Integration Point |
|---|---|---|
| Organization structure | M01 | Department hierarchy, office locations |
| Employee onboarding | M02 | Convert hired candidate to employee |
| Headcount planning | M16 | Approved headcount from objectives |
| Contract creation | M18 | Labor contract for hired candidates |
| Resignation analysis | M18 | Resignation reason data for recruitment planning |

### Business Rules
1. **Pipeline Stages**: Standard stages: Applied -> Skills Test -> IQ Test -> Interview -> Hired -> Recruited. Stages are configurable per job posting.
2. **Headcount Control**: Each job posting tied to an approved headcount. Cannot hire beyond approved number without additional approval.
3. **Candidate Uniqueness**: Duplicate candidates detected by email + phone matching. Prior application history preserved.
4. **Evaluation Scoring**: Each stage can have scoring criteria. Minimum passing score configurable per stage.
5. **Interview Scheduling**: Supports panel interviews with multiple interviewers. Calendar integration for availability.
6. **Offer Letter**: Generated from approved hire decision. Includes position, salary, start date, terms. Requires HR Director signature.
7. **Talent Pool**: Candidates not hired but qualified are added to talent pools for future matching.
8. **Source Tracking**: Each candidate tagged with source (job board, referral, agency, direct, social media) for effectiveness analysis.
9. **Resignation Reasons**: New hire resignation reasons tracked: Benefits (60), Work environment (35), Adaptation (30), Career change (25), Performance (20) - per image data.
10. **Compliance**: Equal opportunity data collected but stored separately from evaluation data.

---

## 2. Screen Inventory

### 2.1 Recruitment Pipeline (Kanban)
**Route:** `/recruitment/pipeline/:jobId`  
**Purpose:** Kanban board tracking candidates through recruitment stages (img_36).

| UI Component | Type | Description |
|---|---|---|
| Kanban Columns | Board | Applied (2), Skills Test (10), IQ Test (3), Interview (5), Hired (0), Recruited (1) |
| Candidate Cards | Card | Name, position applied, source, rating, stage duration |
| Stage Counters | Badge | Candidate count per stage |
| Headcount Display | Label | e.g., "Android Developer - 45 headcount" |
| Drag & Drop | Interaction | Move candidates between stages |
| Card Detail | Modal | Click card for full candidate profile |
| Filter Bar | Controls | Filter by source, rating, date applied |
| Search | Input | Search candidates by name |

**User Interactions:**
- Drag candidate cards between stages to advance.
- Click card to view full profile and history.
- Add notes and ratings to candidate cards.
- Filter pipeline by various criteria.

**Data Displayed:** Candidate pipeline with stage distribution, candidate details.

### 2.2 Recruitment Overview
**Route:** `/recruitment/overview`  
**Purpose:** Navigation and high-level recruitment metrics.

| UI Component | Type | Description |
|---|---|---|
| Left Navigation | Menu | Overview, Job Postings, Candidates, Calendar, Talent Pools, Tasks, Inbox, Reports, Settings |
| Active Postings | Card | Count of active job postings |
| Pipeline Summary | Cards | Total candidates, in-progress, hired this month |
| Time-to-Fill | KPI | Average days from posting to hire |
| Source Effectiveness | Chart | Candidates by source, conversion rates |
| Upcoming Interviews | List | Scheduled interviews for today/this week |

**User Interactions:**
- Navigate to any recruitment sub-module.
- Click metrics to drill down to detailed views.
- View and manage interview schedule.

**Data Displayed:** Recruitment KPIs, active postings, upcoming activities.

### 2.3 Job Posting Management
**Route:** `/recruitment/job-postings`  
**Purpose:** Create and manage job requisitions and postings.

| UI Component | Type | Description |
|---|---|---|
| Posting List | Table | Title, department, headcount, status, applicants, days open |
| Create Posting | Button | New job requisition |
| Posting Form | Modal | Title, department, headcount, requirements, description, deadline |
| Publish Button | Button | Publish to career portal and job boards |
| Status Toggle | Switch | Active/Paused/Closed |

**User Interactions:**
- Create job posting with details and headcount approval.
- Publish to internal/external channels.
- Pause or close postings.
- View applicant count per posting.

**Data Displayed:** All job postings with status and metrics.

### 2.4 Candidate Management
**Route:** `/recruitment/candidates`  
**Purpose:** View and manage all candidates across all postings.

| UI Component | Type | Description |
|---|---|---|
| Candidate List | Table | Name, position, stage, source, rating, applied date |
| Candidate Profile | Detail view | Resume, contact info, application history, notes |
| Add Candidate | Button | Manually add candidate |
| Import CV | Button | Bulk import from CV files |
| Bulk Actions | Dropdown | Advance stage, reject, send email |
| Filters | Multi-select | Posting, stage, source, rating |

**User Interactions:**
- Search and filter candidate list.
- View detailed candidate profiles.
- Bulk advance or reject candidates.
- Import candidates from CV files.

**Data Displayed:** All candidates with application details.

### 2.5 Interview Calendar
**Route:** `/recruitment/calendar`  
**Purpose:** Schedule and manage interviews.

| UI Component | Type | Description |
|---|---|---|
| Calendar View | Calendar | Interview schedule by day/week/month |
| Schedule Interview | Button | New interview event |
| Interview Form | Modal | Candidate, interviewers, date/time, type, location/link |
| Availability Checker | Tool | Find common available slots |
| Reminder Toggle | Switch | Auto-send reminders to candidate and interviewers |

**User Interactions:**
- Schedule interviews with candidate and interviewer availability.
- Reschedule or cancel interviews.
- Send interview invitations and reminders.

**Data Displayed:** Interview schedule, availability conflicts.

### 2.6 Talent Pools
**Route:** `/recruitment/talent-pools`  
**Purpose:** Manage candidate talent pools for future hiring.

| UI Component | Type | Description |
|---|---|---|
| Pool List | Table | Pool name, candidate count, category, last updated |
| Pool Detail | Detail view | Candidates in pool, skills, notes |
| Create Pool | Button | New talent pool |
| Add to Pool | Button | Add candidate to pool |
| Matching Tool | Tool | Match pool candidates to open postings |

**User Interactions:**
- Create talent pools by skill, role, or category.
- Add qualified but unhired candidates to pools.
- Search pools when new positions open.

**Data Displayed:** Talent pool inventory and candidate lists.

### 2.7 Recruitment Tasks
**Route:** `/recruitment/tasks`  
**Purpose:** Track recruitment-related tasks and to-dos.

| UI Component | Type | Description |
|---|---|---|
| Task List | Table | Task, assignee, due date, status, related candidate/posting |
| Create Task | Button | New recruitment task |
| Task Form | Modal | Description, assignee, due date, priority |
| Status Update | Dropdown | Not started, In progress, Complete |

**User Interactions:**
- Create and assign recruitment tasks.
- Track task completion.
- Link tasks to candidates or postings.

**Data Displayed:** Task list with assignments and status.

### 2.8 Recruitment Inbox
**Route:** `/recruitment/inbox`  
**Purpose:** Communication hub for recruitment correspondence.

| UI Component | Type | Description |
|---|---|---|
| Message List | Table | Sender, subject, date, related candidate, read status |
| Email Templates | Dropdown | Pre-built templates for common communications |
| Compose | Button | Send new message |
| Reply | Button | Reply to message thread |

**User Interactions:**
- View and respond to candidate communications.
- Send templated emails (acknowledgment, rejection, offer).
- Track communication history per candidate.

**Data Displayed:** Email/message threads with candidates.

### 2.9 Recruitment Reports
**Route:** `/recruitment/reports`  
**Purpose:** Analytics on recruitment effectiveness.

| UI Component | Type | Description |
|---|---|---|
| Report Selector | Dropdown | Time-to-fill, source effectiveness, pipeline conversion, resignation reasons |
| Resignation Reasons | Bar chart | Benefits 60, Work environment 35, Adaptation 30, Career change 25, Performance 20 |
| Conversion Funnel | Funnel chart | Applied -> Test -> Interview -> Hired -> Recruited |
| Date Range | Date picker | Report period |
| Export | Button | PDF/Excel export |

**User Interactions:**
- Select and generate recruitment reports.
- Analyze resignation reasons for early-turnover insights.
- Export reports for management review.

**Data Displayed:** Recruitment analytics, resignation analysis.

### 2.10 Recruitment Settings
**Route:** `/recruitment/settings`  
**Purpose:** Configure recruitment pipeline stages, templates, and defaults.

| UI Component | Type | Description |
|---|---|---|
| Stage Configuration | Table | Stage name, order, scoring, auto-advance rules |
| Email Templates | List | Template name, type, content |
| Scoring Criteria | Table | Criteria name, weight, scale |
| Source Categories | List | Job board, referral, agency, direct, social media |

**User Interactions:**
- Customize pipeline stages per posting type.
- Edit email templates.
- Configure scoring weights.
- Manage source categories.

**Data Displayed:** Recruitment system configuration.

---

## 3. Feature Specifications

### 3.1 Job Posting Management
**Description:** Create, publish, and manage job requisitions with headcount tracking.

**User Stories:**
- As a Recruiter, I can create a job posting with title, department, headcount, and requirements.
- As a Recruiter, I can publish a job posting to the career portal and external job boards.
- As a Recruiter, I can pause or close a job posting.
- As a Hiring Manager, I can view all postings for my department.

**Acceptance Criteria:**
1. Job posting has: title, department, position, headcount, requirements, description, deadline, status.
2. Headcount linked to approved headcount from planning (M16).
3. Publishing makes posting visible on career portal and pushes to configured job boards.
4. Status: Draft -> Active -> Paused -> Closed -> Filled.
5. Applicant count tracked per posting.
6. Days open tracked for time-to-fill calculation.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Headcount approval | Approved headcount exists | "No approved headcount for this position" |
| Required fields | All required fields filled | "Missing required field: {field}" |
| Valid deadline | Deadline > today | "Deadline must be in the future" |
| Unique posting | No active posting for same position/dept | "Active posting already exists for this position" |

**Edge Cases:**
- Headcount increased mid-posting: additional hires allowed up to new limit.
- Posting closed with candidates in pipeline: candidates moved to talent pool.
- Multiple postings for same role in different departments: allowed.

### 3.2 Candidate Pipeline (Kanban)
**Description:** Visual Kanban board for tracking candidates through recruitment stages.

**User Stories:**
- As a Recruiter, I can view candidates in a Kanban board organized by recruitment stage.
- As a Recruiter, I can drag candidates between stages to advance their application.
- As a Recruiter, I can see candidate count per stage at a glance.
- As a Hiring Manager, I can filter the pipeline to see candidates for my interviews.

**Acceptance Criteria:**
1. Kanban columns represent recruitment stages in order.
2. Each candidate card shows: name, position, rating, source, days in stage.
3. Drag-and-drop advances candidate to new stage.
4. Stage counters update in real-time.
5. Clicking a card opens candidate detail modal.
6. Pipeline filterable by posting, date range, source.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Stage order | Cannot skip required stages | "Candidate must complete {stage} before advancing" |
| Headcount limit | Hired count <= headcount | "Headcount exceeded for this posting" |
| Stage completion | Required scores submitted | "Cannot advance: pending evaluations for {stage}" |

**Edge Cases:**
- Candidate re-enters pipeline for different position: new application record.
- Candidate rejects offer: moved to "Declined" stage, added to talent pool.
- Candidate withdrawn: moved to "Withdrawn" stage with reason.

### 3.3 Candidate Profile and CV Management
**Description:** Store and manage candidate profiles, resumes, and application history.

**User Stories:**
- As a Recruiter, I can view a candidate's full profile including resume and application history.
- As a Recruiter, I can upload a candidate's CV and supporting documents.
- As a Recruiter, I can add internal notes about a candidate.
- As a system, I can detect duplicate candidates by email/phone matching.

**Acceptance Criteria:**
1. Profile includes: personal info, contact details, resume/CV, application history, notes, ratings.
2. CV stored as PDF or parsed for key fields (skills, experience, education).
3. Application history tracks all postings applied to and stages reached.
4. Internal notes visible only to HR staff, not to candidate.
5. Duplicate detection by email + phone match with manual merge option.
6. Candidate rating system (1-5 stars or custom scale).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Valid email | Email format valid | "Invalid email format" |
| CV file type | PDF, DOC, DOCX only | "Unsupported file type. Use PDF, DOC, or DOCX" |
| File size | < 10MB per file | "File size exceeds 10MB limit" |

**Edge Cases:**
- Candidate applies to multiple postings: single profile, multiple application records.
- CV parsing fails: store original file, manual data entry required.
- Duplicate candidate with different email: manual merge with history preservation.

### 3.4 Testing (Skills Test & IQ Test)
**Description:** Administer and score skills and IQ tests for candidates.

**User Stories:**
- As a Recruiter, I can assign a skills test or IQ test to a candidate.
- As a Candidate, I can complete assigned tests online.
- As an Evaluator, I can score candidate test results.
- As a Recruiter, I can see test scores on the candidate card.

**Acceptance Criteria:**
1. Tests assigned when candidate enters Skills Test or IQ Test stage.
2. Test types: Online assessment, In-person written test, Practical exercise.
3. Scores recorded: numeric score, pass/fail, evaluator notes.
4. Minimum passing score configurable per test type.
5. Failed candidates marked with reason (e.g., "insufficient" per image data).
6. Test results visible on candidate card in pipeline.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Score range | Score within configured range | "Score must be between {min} and {max}" |
| Required evaluations | All evaluators submitted | "Cannot complete stage: {count} evaluations pending" |

**Edge Cases:**
- Candidate requests test extension: deadline extendable by recruiter.
- Test cheating suspected: flag for manual review, retest option.
- Accommodation needed: alternative test format available.

### 3.5 Interview Management
**Description:** Schedule, conduct, and evaluate interviews with candidate and panel management.

**User Stories:**
- As a Recruiter, I can schedule interviews with candidate and interviewer availability.
- As an Interviewer, I can submit interview feedback and scores.
- As a Hiring Manager, I can review all interview feedback for a candidate.
- As a Recruiter, I can send interview invitations and reminders automatically.

**Acceptance Criteria:**
1. Interview has: candidate, interviewers, date/time, type (phone/video/in-person), location/link, status.
2. Calendar integration checks interviewer availability.
3. Panel interviews support multiple interviewers with independent feedback.
4. Feedback form: structured criteria with ratings + free-text comments.
5. Auto-reminders sent 24 hours before interview to all parties.
6. Interview outcomes: Pass, Fail, Hold (for comparison).

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| No conflicts | Interviewer not double-booked | "Interviewer {name} has conflict at this time" |
| Required feedback | All panelists submitted | "Cannot complete: {count} interviewers have not submitted feedback" |
| Valid scheduling | Interview date > today | "Cannot schedule interview in the past" |

**Edge Cases:**
- Interviewer unavailable: reschedule with all parties notified.
- Candidate no-show: mark as no-show, option to reschedule.
- Emergency interview: bypass availability check with override reason.

### 3.6 Offer Letter Generation
**Description:** Generate and manage offer letters for selected candidates.

**User Stories:**
- As a Recruiter, I can generate an offer letter for a candidate who passed all stages.
- As an HR Director, I can review and approve offer letters before sending.
- As a Recruiter, I can track offer status (sent, accepted, declined, expired).

**Acceptance Criteria:**
1. Offer letter includes: position, department, salary, start date, benefits, terms, expiry date.
2. Generated from template with candidate-specific fields.
3. Approval workflow: Recruiter creates -> HR Director approves -> Sent to candidate.
4. Status tracking: Draft -> Approved -> Sent -> Accepted/Declined/Expired.
5. Accepted offer triggers handoff to onboarding (M02/M18).
6. Declined offer reason recorded for analytics.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| All stages passed | Candidate completed all required stages | "Candidate has not completed all required stages" |
| Salary within range | Salary within approved band | "Salary exceeds approved range for this position" |
| Approval required | HR Director approval before sending | "Offer letter requires HR Director approval" |

**Edge Cases:**
- Counter-offer negotiation: revised offer letter with version tracking.
- Offer expired: candidate can request extension (HR Director approval).
- Candidate accepts after declining different offer: reinstate if position still open.

### 3.7 Talent Pool Management
**Description:** Build and search talent pools of qualified candidates for future hiring.

**User Stories:**
- As a Recruiter, I can add qualified but unhired candidates to talent pools.
- As a Recruiter, I can search talent pools when a new position opens.
- As a Recruiter, I can contact talent pool candidates about new opportunities.

**Acceptance Criteria:**
1. Talent pool has: name, category, description, candidate list.
2. Candidates added from: declined offers, pipeline overflow, proactive sourcing.
3. Search by: skills, experience level, location, availability.
4. Contact candidates directly from talent pool interface.
5. Pool member status updated: Available, Hired elsewhere, Unreachable, Not interested.

**Validation Rules:**
| Rule | Condition | Error Message |
|---|---|---|
| Unique pool name | No duplicate pool name | "Talent pool '{name}' already exists" |
| Consent | Candidate consent for pool storage | "Candidate has not consented to talent pool storage" |

**Edge Cases:**
- Pool candidate hired for different role: status updated, remains in pool.
- Pool data stale: periodic outreach to verify continued interest.
- Candidate requests data deletion: comply per data privacy regulations.

### 3.8 Source Tracking and Effectiveness
**Description:** Track candidate sources and analyze recruitment channel effectiveness.

**User Stories:**
- As a Recruiter, I can tag each candidate with their application source.
- As an HR Director, I can analyze which sources produce the best hires.
- As a Recruiter, I can optimize spending based on source effectiveness data.

**Acceptance Criteria:**
1. Source categories: Job board, Employee referral, Recruitment agency, Direct application, Social media, Career fair, Company website.
2. Source captured at application time (auto-detected or manual).
3. Effectiveness metrics: candidates per source, conversion rate, cost per hire, quality of hire.
4. Report shows source distribution and ROI analysis.

**Edge Cases:**
- Multiple sources (candidate saw ad on social media but applied via referral): primary source selected.
- Source deactivated: historical data preserved, no new assignments.

### 3.9 Resignation Reason Analysis
**Description:** Track and analyze reasons for early resignation to improve recruitment quality (img_63).

**User Stories:**
- As an HR Administrator, I can record resignation reasons for departing employees.
- As an HR Director, I can analyze resignation patterns by reason category.
- As a Recruiter, I can use resignation data to refine candidate selection criteria.

**Acceptance Criteria:**
1. Resignation reasons: Benefits (60), Work environment (35), Adaptation (30), Career change (25), Performance (20).
2. Multiple reasons per resignation allowed (ranked).
3. Analysis by: reason category, department, tenure at resignation, source of hire.
4. Early resignation flag (< 6 months) for recruitment quality review.
5. Report shows resignation reason distribution and trends.

**Edge Cases:**
- Resignation reason not in standard categories: "Other" with free-text.
- Post-exit survey adds additional reasons: merged with initial record.
- Anonymous feedback: aggregated only, no individual identification.

---

## 4. Database Design

### ERD (ASCII)
```
+----------------+       +-------------------+       +-------------------+
| JobPosting     |1-----*| Application       |1-----*| Candidate         |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| title          |       | job_posting_id    |       | name              |
| department_id  |       | candidate_id (FK) |       | email             |
| headcount      |       | stage_id (FK)     |       | phone             |
| headcount_used |       | status            |       | resume_url        |
| status         |       | applied_date      |       | source            |
| deadline       |       | rating            |       | rating            |
| description    |       +-------------------+       | created_at        |
| created_at     |               |                   +-------------------+
+----------------+               |
                                 | 1
                    1-----------*|
                                 v
               +-------------------+       +-------------------+
               | RecruitmentStage  |       | Interview         |
               +-------------------+       +-------------------+
               | id (PK)           |       | id (PK)           |
               | posting_id (FK)   |       | application_id    |
               | name              |       | interview_type    |
               | stage_order       |       | scheduled_at      |
               | scoring_required  |       | location          |
               | min_pass_score    |       | status            |
               +-------------------+       +-------------------+
                        |                         |
                        | 1                      *|
                        v                         v
               +-------------------+       +-------------------+
               | TestResult        |       | InterviewFeedback |
               +-------------------+       +-------------------+
               | id (PK)           |       | id (PK)           |
               | application_id    |       | interview_id (FK) |
               | test_type         |       | interviewer_id    |
               | score             |       | score             |
               | result            |       | comments          |
               | evaluator_id      |       | recommendation    |
               | evaluated_at      |       | submitted_at      |
               +-------------------+       +-------------------+

+----------------+       +-------------------+       +-------------------+
| OfferLetter    |       | TalentPool        |       | ResignationReason |
+----------------+       +-------------------+       +-------------------+
| id (PK)        |       | id (PK)           |       | id (PK)           |
| application_id |       | name              |       | category          |
| salary         |       | category          |       | count             |
| start_date     |       | description       |       | percentage        |
| status         |       | created_at        |       +-------------------+
| approved_by    |       +-------------------+
| sent_at        |              |
| response       |       +-------------------+
| response_at    |       | TalentPoolMember  |
+----------------+       +-------------------+
                         | id (PK)           |
                         | pool_id (FK)      |
                         | candidate_id (FK) |
                         | status            |
                         | added_at          |
                         +-------------------+
```

### Table Schemas

#### job_posting
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| title | VARCHAR(200) | NOT NULL | Job title |
| department_id | UUID | FK -> departments.id, NOT NULL | Hiring department |
| position_code | VARCHAR(50) | NOT NULL | Position code |
| headcount | INT | NOT NULL | Approved headcount |
| headcount_used | INT | NOT NULL, DEFAULT 0 | Hired count |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft/active/paused/closed/filled |
| deadline | DATE | NULL | Application deadline |
| description | TEXT | NULL | Job description |
| requirements | TEXT | NULL | Job requirements |
| salary_min | DECIMAL(12,2) | NULL | Minimum salary |
| salary_max | DECIMAL(12,2) | NULL | Maximum salary |
| published_at | TIMESTAMPTZ | NULL | Publish timestamp |
| closed_at | TIMESTAMPTZ | NULL | Close timestamp |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### candidate
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| first_name | VARCHAR(100) | NOT NULL | First name |
| last_name | VARCHAR(100) | NOT NULL | Last name |
| email | VARCHAR(200) | NOT NULL | Email address |
| phone | VARCHAR(20) | NULL | Phone number |
| resume_url | VARCHAR(500) | NULL | Stored resume file |
| source | VARCHAR(30) | NOT NULL | job_board/referral/agency/direct/social/career_fair/website |
| source_detail | VARCHAR(200) | NULL | Specific source (e.g., job board name) |
| rating | INT | NULL | 1-5 star rating |
| skills | TEXT[] | NULL | Skill tags |
| experience_years | INT | NULL | Years of experience |
| education | TEXT | NULL | Education summary |
| notes | TEXT | NULL | Internal notes |
| consent_given | BOOLEAN | NOT NULL, DEFAULT false | Data processing consent |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### application
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| job_posting_id | UUID | FK -> job_posting.id, NOT NULL | Job posting |
| candidate_id | UUID | FK -> candidate.id, NOT NULL | Candidate |
| stage_id | UUID | FK -> recruitment_stage.id, NOT NULL | Current stage |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active/hired/declined/withdrawn |
| applied_date | DATE | NOT NULL | Application date |
| rating | INT | NULL | 1-5 rating |
| rejection_reason | TEXT | NULL | Reason if rejected |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### recruitment_stage
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| job_posting_id | UUID | FK -> job_posting.id, NOT NULL | Associated posting |
| name | VARCHAR(100) | NOT NULL | Stage name (Applied, Skills Test, etc.) |
| stage_order | INT | NOT NULL | Stage sequence order |
| scoring_required | BOOLEAN | NOT NULL, DEFAULT false | Requires scoring |
| min_pass_score | DECIMAL(5,2) | NULL | Minimum passing score |
| description | TEXT | NULL | Stage description |

#### interview
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| application_id | UUID | FK -> application.id, NOT NULL | Application |
| interview_type | VARCHAR(20) | NOT NULL | phone/video/in-person |
| scheduled_at | TIMESTAMPTZ | NOT NULL | Interview date/time |
| duration_minutes | INT | NOT NULL, DEFAULT 60 | Expected duration |
| location | VARCHAR(200) | NULL | Physical location or meeting link |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'scheduled' | scheduled/completed/cancelled/no_show |
| notes | TEXT | NULL | Interview notes |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### interview_feedback
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| interview_id | UUID | FK -> interview.id, NOT NULL | Interview |
| interviewer_id | UUID | FK -> users.id, NOT NULL | Interviewer |
| score | DECIMAL(5,2) | NULL | Interview score |
| comments | TEXT | NULL | Detailed feedback |
| recommendation | VARCHAR(20) | NOT NULL | pass/fail/hold |
| submitted_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Submission timestamp |

#### test_result
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| application_id | UUID | FK -> application.id, NOT NULL | Application |
| test_type | VARCHAR(30) | NOT NULL | skills_test/iq_test/practical |
| score | DECIMAL(5,2) | NULL | Test score |
| max_score | DECIMAL(5,2) | NULL | Maximum possible score |
| result | VARCHAR(20) | NOT NULL | pass/fail/pending |
| failure_reason | TEXT | NULL | Reason if failed |
| evaluator_id | UUID | FK -> users.id, NOT NULL | Evaluator |
| evaluated_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Evaluation timestamp |

#### offer_letter
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| application_id | UUID | FK -> application.id, NOT NULL | Application |
| salary_offered | DECIMAL(12,2) | NOT NULL | Offered salary |
| start_date | DATE | NOT NULL | Proposed start date |
| benefits_summary | TEXT | NULL | Benefits description |
| terms | TEXT | NULL | Offer terms |
| expiry_date | DATE | NOT NULL | Offer expiry deadline |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft/approved/sent/accepted/declined/expired |
| approved_by | UUID | FK -> users.id, NULL | Approver |
| approved_at | TIMESTAMPTZ | NULL | Approval timestamp |
| sent_at | TIMESTAMPTZ | NULL | Sent timestamp |
| response | VARCHAR(20) | NULL | accept/decline/counter |
| response_at | TIMESTAMPTZ | NULL | Response timestamp |
| decline_reason | TEXT | NULL | Reason if declined |
| document_url | VARCHAR(500) | NULL | Generated PDF |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### talent_pool
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| name | VARCHAR(100) | NOT NULL | Pool name |
| category | VARCHAR(50) | NOT NULL | Pool category |
| description | TEXT | NULL | Pool description |
| created_by | UUID | FK -> users.id, NOT NULL | Creator |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### talent_pool_member
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| pool_id | UUID | FK -> talent_pool.id, NOT NULL | Talent pool |
| candidate_id | UUID | FK -> candidate.id, NOT NULL | Candidate |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'available' | available/hired_elsewhere/unreachable/not_interested |
| notes | TEXT | NULL | Pool-specific notes |
| added_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Added timestamp |

#### resignation_reason
| Column | Type | Constraints | Description |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Unique identifier |
| category | VARCHAR(50) | NOT NULL | benefits/work_environment/adaptation/career_change/performance/other |
| employee_id | UUID | FK -> employees.id, NOT NULL | Departing employee |
| tenure_months | INT | NOT NULL | Months employed before resignation |
| source_of_hire | VARCHAR(30) | NULL | How employee was recruited |
| department_id | UUID | FK -> departments.id, NOT NULL | Department |
| recorded_at | TIMESTAMPTZ | NOT NULL, DEFAULT NOW() | Record timestamp |

### All Tables Summary
| Table | Purpose | Key Relationships |
|---|---|---|
| job_posting | Job requisitions and postings | Parent of RecruitmentStage, Application |
| candidate | Candidate profiles | Parent of Application |
| application | Candidate-job link with stage tracking | Links JobPosting, Candidate, Stage |
| recruitment_stage | Pipeline stage definitions | Links JobPosting |
| interview | Interview scheduling | Links Application |
| interview_feedback | Interviewer evaluations | Links Interview, Interviewer |
| test_result | Test scores and results | Links Application |
| offer_letter | Offer management | Links Application |
| talent_pool | Talent pool definitions | Parent of TalentPoolMember |
| talent_pool_member | Pool membership | Links TalentPool, Candidate |
| resignation_reason | Resignation analytics | Links Employee, Department |

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
    "total": 45
  }
}
```

### Error Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Headcount exceeded for this posting",
    "details": [
      { "field": "headcount", "message": "Used: 45, Available: 0" }
    ]
  }
}
```

### Endpoints Summary
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| GET | `/api/recruitment/job-postings` | List job postings | recruitment.posting.read |
| POST | `/api/recruitment/job-postings` | Create job posting | recruitment.posting.create |
| PUT | `/api/recruitment/job-postings/:id` | Update job posting | recruitment.posting.update |
| POST | `/api/recruitment/job-postings/:id/publish` | Publish posting | recruitment.posting.publish |
| POST | `/api/recruitment/job-postings/:id/close` | Close posting | recruitment.posting.update |
| GET | `/api/recruitment/candidates` | List candidates | recruitment.candidate.read |
| POST | `/api/recruitment/candidates` | Create candidate | recruitment.candidate.create |
| GET | `/api/recruitment/candidates/:id` | Get candidate detail | recruitment.candidate.read |
| GET | `/api/recruitment/applications` | List applications | recruitment.application.read |
| PATCH | `/api/recruitment/applications/:id/stage` | Advance application stage | recruitment.application.update |
| GET | `/api/recruitment/pipeline/:jobId` | Get pipeline (Kanban) data | recruitment.pipeline.read |
| GET | `/api/recruitment/interviews` | List interviews | recruitment.interview.read |
| POST | `/api/recruitment/interviews` | Schedule interview | recruitment.interview.create |
| PUT | `/api/recruitment/interviews/:id` | Update interview | recruitment.interview.update |
| POST | `/api/recruitment/interviews/:id/feedback` | Submit interview feedback | recruitment.interview.feedback |
| GET | `/api/recruitment/test-results` | List test results | recruitment.test.read |
| POST | `/api/recruitment/test-results` | Record test result | recruitment.test.create |
| GET | `/api/recruitment/offers` | List offer letters | recruitment.offer.read |
| POST | `/api/recruitment/offers` | Create offer letter | recruitment.offer.create |
| PATCH | `/api/recruitment/offers/:id/approve` | Approve offer letter | recruitment.offer.approve |
| PATCH | `/api/recruitment/offers/:id/respond` | Record candidate response | recruitment.offer.respond |
| GET | `/api/recruitment/talent-pools` | List talent pools | recruitment.pool.read |
| POST | `/api/recruitment/talent-pools` | Create talent pool | recruitment.pool.create |
| POST | `/api/recruitment/talent-pools/:id/members` | Add candidate to pool | recruitment.pool.update |
| GET | `/api/recruitment/resignation-reasons` | Resignation reason analytics | recruitment.report.read |
| GET | `/api/recruitment/reports/source-effectiveness` | Source effectiveness report | recruitment.report.read |
| GET | `/api/recruitment/reports/time-to-fill` | Time-to-fill report | recruitment.report.read |
| GET | `/api/recruitment/reports/conversion-funnel` | Pipeline conversion report | recruitment.report.read |

### Endpoint Details

#### GET /api/recruitment/pipeline/:jobId
**Response:**
```json
{
  "success": true,
  "data": {
    "job_posting": {
      "id": "jp-001",
      "title": "Android Developer",
      "headcount": 45,
      "headcount_used": 1
    },
    "stages": [
      {
        "id": "st-001",
        "name": "Applied",
        "stage_order": 1,
        "candidates": [
          { "id": "app-001", "name": "Nguyen Van A", "rating": 2, "source": "job_board", "days_in_stage": 3, "note": "Unsuitable" },
          { "id": "app-002", "name": "Tran Thi B", "rating": 1, "source": "referral", "days_in_stage": 5, "note": "Unsuitable" }
        ]
      },
      {
        "id": "st-002",
        "name": "Skills Test",
        "stage_order": 2,
        "candidates": [ ... 10 candidates ... ]
      },
      {
        "id": "st-003",
        "name": "IQ Test",
        "stage_order": 3,
        "candidates": [
          { "id": "app-015", "name": "Le Van C", "score": 45, "result": "fail", "note": "Insufficient" }
        ]
      },
      {
        "id": "st-004",
        "name": "Interview",
        "stage_order": 4,
        "candidates": [ ... 5 candidates ... ]
      },
      { "id": "st-005", "name": "Hired", "stage_order": 5, "candidates": [] },
      { "id": "st-006", "name": "Recruited", "stage_order": 6, "candidates": [ ... 1 candidate ... ] }
    ]
  }
}
```

#### POST /api/recruitment/offers
**Request Body:**
```json
{
  "application_id": "app-123",
  "salary_offered": 25000000,
  "start_date": "2026-05-15",
  "benefits_summary": "Standard benefits package",
  "terms": "12-month probation period",
  "expiry_date": "2026-05-01"
}
```

---

## 6. Permissions Matrix

### Roles
| Role | Description |
|---|---|
| HR Director | Full access to all recruitment functions |
| Recruiter | Manage postings, candidates, interviews, offers |
| Hiring Manager | Review candidates, interview, provide feedback for own department |
| Interview Panel | Submit interview feedback only |
| System Administrator | Configuration only (stages, templates, sources) |

### Permission Matrix (CRUD)
| Permission | HR Director | Recruiter | Hiring Mgr. | Interviewer | Sys Admin |
|---|---|---|---|---|---|
| recruitment.posting.read | CRUD | CRUD | R (own dept) | R | R |
| recruitment.posting.create | C | C | - | - | - |
| recruitment.posting.publish | C | C | - | - | - |
| recruitment.candidate.read | CRUD | CRUD | R (own dept) | R (assigned) | R |
| recruitment.candidate.create | CRUD | CRUD | - | - | - |
| recruitment.application.update | CRUD | CRUD | R (own dept) | - | - |
| recruitment.pipeline.read | CRUD | CRUD | R (own dept) | R (assigned) | R |
| recruitment.interview.read | CRUD | CRUD | R (own dept) | R (assigned) | R |
| recruitment.interview.create | CRUD | CRUD | - | - | - |
| recruitment.interview.feedback | CRUD | R | CRUD | C (own) | - |
| recruitment.test.read | CRUD | CRUD | R | R (assigned) | R |
| recruitment.test.create | CRUD | CRUD | - | C (assigned) | - |
| recruitment.offer.read | CRUD | CRUD | R | - | R |
| recruitment.offer.create | CRUD | C | - | - | - |
| recruitment.offer.approve | C | - | - | - | - |
| recruitment.pool.read | CRUD | CRUD | R | - | R |
| recruitment.pool.create | CRUD | CRUD | - | - | - |
| recruitment.report.read | R | R | R | - | R |

### Row-Level Security
| Rule | Description |
|---|---|
| Department scoping | Hiring Managers only see postings and candidates for their department |
| Interviewer scoping | Interviewers only see interviews they are assigned to |
| Confidential notes | Internal candidate notes only visible to HR staff |
| Cross-tenant isolation | All queries scoped to tenant_id |

---

## 7. State Machine / Workflows

### Application Stage Transitions
```
  +---------+   advance()   +-------------+   advance()    +----------+
  | Applied | ------------> | Skills Test | -------------> | IQ Test  |
  +---------+               +-------------+                +----------+
      |                         |                              |
      |                         |                              |
      v                         v                              v
  +----------+            +----------+                   +----------+
  | Rejected |            | Rejected |                   | Rejected |
  +----------+            +----------+                   +----------+
                                                        |
                                                 advance()
                                                        |
                                                        v
                                                  +-----------+  advance()  +-------+
                                                  | Interview | ----------> | Hired |
                                                  +-----------+             +-------+
                                                        |                      |
                                                  reject()                confirm()
                                                        |                      |
                                                        v                      v
                                                  +----------+          +-----------+
                                                  | Rejected |          | Recruited |
                                                  +----------+          +-----------+
```

### Application Status Table
| From Stage | To Stage | Trigger | Required Role | Side Effects |
|---|---|---|---|---|
| Applied | Skills Test | advance() | Recruiter | Send test invitation |
| Applied | Rejected | reject() | Recruiter | Send rejection notice, add to talent pool |
| Skills Test | IQ Test | advance() + pass | Recruiter | Test score >= min_pass_score |
| Skills Test | Rejected | reject() + fail | Recruiter | Record failure reason |
| IQ Test | Interview | advance() + pass | Recruiter | Schedule interviews |
| IQ Test | Rejected | reject() + fail | Recruiter | Record failure reason (e.g., "insufficient") |
| Interview | Hired | advance() + pass all | Hiring Manager | All interview feedback: pass |
| Interview | Rejected | reject() | Hiring Manager | Record rejection reason |
| Hired | Recruited | confirm_start() | HR Admin | Employee onboarded, headcount_used incremented |

### Offer Letter Workflow
```
  +-------+     create()     +---------+    approve()     +------+    send()     +------+
  | DRAFT | ---------------> | APPROVED| ---------------> | SENT | -----------> | OPEN |
  +-------+                  +---------+                  +------+              +------+
                                                                             /    |    \
                                                                      accept   decline  expire
                                                                       /         |        \
                                                                      v          v         v
                                                                 +---------+ +---------+ +--------+
                                                                 |ACCEPTED | |DECLINED | | EXPIRED|
                                                                 +---------+ +---------+ +--------+
```

### Resignation Reason Categories
| Category | Count (per data) | Description |
|---|---|---|
| Benefits | 60 | Compensation, benefits not meeting expectations |
| Work environment | 35 | Culture, management style, workplace conditions |
| Adaptation | 30 | Could not adapt to role, team, or company |
| Career change | 25 | Changed career direction or returned to school |
| Performance | 20 | Performance issues leading to departure |

---

## 8. Reports & Analytics

### 8.1 Time-to-Fill Report
**Dimensions:** Job posting, Department, Position  
**Metrics:** Average days to fill, median days, min/max, postings filled  
**Filters:** Date range, department, position type

### 8.2 Source Effectiveness Report
**Dimensions:** Source category, Job posting  
**Metrics:** Candidates per source, conversion rate, cost per hire, quality rating  
**Filters:** Date range, source category

### 8.3 Pipeline Conversion Report
**Dimensions:** Stage, Job posting  
**Metrics:** Candidates per stage, pass rate, drop-off rate, average days in stage  
**Filters:** Date range, posting

### 8.4 Resignation Reason Report
**Dimensions:** Reason category, Department, Tenure band  
**Metrics:** Count per reason, percentage, early resignation rate (<6 months)  
**Filters:** Date range, department, source of hire

### 8.5 Headcount Utilization Report
**Dimensions:** Department, Position  
**Metrics:** Approved headcount, filled, open, time-to-fill for open  
**Filters:** Current snapshot, department

### 8.6 Interview Success Rate Report
**Dimensions:** Interviewer, Department  
**Metrics:** Interviews conducted, pass rate, candidate satisfaction  
**Filters:** Date range, interviewer

### 8.7 Offer Acceptance Rate Report
**Dimensions:** Department, Position, Salary band  
**Metrics:** Offers sent, accepted, declined, acceptance rate, decline reasons  
**Filters:** Date range, department

### Report Summary Table
| Report | Purpose | Primary Users | Export Formats |
|---|---|---|---|
| Time-to-Fill | Hiring speed analysis | HR Director, Recruiter | PDF, Excel |
| Source Effectiveness | Channel ROI | HR Director, Recruiter | PDF, Excel |
| Pipeline Conversion | Bottleneck identification | Recruiter | PDF, Excel |
| Resignation Reason | Quality of hire analysis | HR Director | PDF, Excel |
| Headcount Utilization | Planning compliance | HR Director, CFO | PDF, Excel |
| Interview Success | Interviewer performance | HR Director | PDF, Excel |
| Offer Acceptance Rate | Competitiveness analysis | HR Director, Recruiter | PDF, Excel |

---

## 9. Integration Points

### External APIs
| Integration | Direction | Purpose | Frequency |
|---|---|---|---|
| Job Board APIs (VietnamWorks, TopCV) | Outbound | Post job openings | On publish |
| Email Service | Outbound | Candidate communications, interview invites | On event |
| Calendar Service | Outbound | Interview scheduling | On schedule |
| SMS Service | Outbound | Interview reminders, status notifications | On event |
| M02 Employee Service | Outbound | Convert hired candidate to employee | On hire confirmed |
| M18 Contract Service | Outbound | Create labor contract for new hire | On hire confirmed |
| M16 Objectives Service | Inbound | Approved headcount data | On posting creation |
| Document Generator | Internal | Offer letter PDF generation | On offer creation |
| CV Parser | Internal | Extract data from uploaded resumes | On CV upload |

### Webhooks
| Event | Trigger | Payload |
|---|---|---|
| application.stage_changed | Candidate advanced | application_id, from_stage, to_stage |
| interview.scheduled | New interview scheduled | interview_id, candidate_id, datetime |
| interview.completed | All feedback submitted | interview_id, result |
| offer.sent | Offer letter sent | offer_id, candidate_id, salary |
| offer.accepted | Candidate accepts | offer_id, candidate_id, start_date |
| offer.declined | Candidate declines | offer_id, candidate_id, reason |
| candidate.added_to_pool | Added to talent pool | candidate_id, pool_id |

### Events Published
| Event | Description | Consumers |
|---|---|---|
| ApplicationStageChanged | Pipeline stage update | Notification Service, Analytics |
| OfferAccepted | New hire confirmed | Employee Service (M02), Contract Service (M18) |
| InterviewReminder | 24h before interview | Email/SMS Service |
| HeadcountFilled | Posting reaches headcount limit | Planning Service (M16) |

---

## 10. Technical Notes

### Performance
- **Pipeline Kanban** loads all candidates for a posting; for postings with 500+ candidates, use virtual scrolling with pagination.
- **CV parsing** runs asynchronously; original file stored immediately, parsed data populated when complete.
- **Interview scheduling** checks availability in <2 seconds for up to 10 interviewers.
- **Database indexing**: Composite indexes on (job_posting_id, stage_id) for application, (candidate_id, email) for duplicate detection.

### Caching
- **Job posting configurations** cached with 1-hour TTL.
- **Talent pool memberships** cached with 5-minute TTL.
- **Resignation reason analytics** cached with 1-hour TTL.
- **Pipeline stage definitions** cached per posting with 30-minute TTL.

### File Handling
- **Resumes/CVs** stored in object storage with signed URLs, 3-year retention.
- **Offer letter PDFs** stored with 5-year retention.
- **File naming convention**: `cv_{candidate_id}_{timestamp}.pdf`, `offer_{application_id}_{version}.pdf`.
- **Maximum file size**: 10MB per document.

### Localization
- **Language**: Vietnamese (primary), English (secondary).
- **Currency**: VND for salary fields.
- **Date format**: DD/MM/YYYY.
- **Job posting templates**: Bilingual support for international postings.

### Mobile Support
- **Candidate career portal** fully responsive on mobile.
- **Interviewer feedback submission** mobile-optimized.
- **Recruiter pipeline view** usable on tablet (not phone due to Kanban layout).
- **Push notifications** for interview reminders, application updates.

### Security
- **Candidate personal data** encrypted at rest (AES-256).
- **Internal notes** segregated from candidate-visible data.
- **GDPR/Vietnam data privacy**: Consent tracking for candidate data storage.
- **Data retention**: Candidate data retained for 2 years after last activity, then anonymized.
- **Access logging**: All candidate profile views logged.

### Email Templates
- **Application acknowledgment**: Auto-sent when candidate applies.
- **Test invitation**: Sent when candidate enters test stage.
- **Interview invitation**: Sent with calendar attachment.
- **Interview reminder**: 24 hours before scheduled interview.
- **Rejection notice**: Sent when candidate rejected (configurable timing).
- **Offer letter**: Sent with PDF attachment and response deadline.

### Duplicate Detection
- **Match criteria**: Email exact match OR phone exact match.
- **Resolution**: Present potential duplicates to recruiter for manual merge.
- **Merge behavior**: Combine application histories, preserve all notes and ratings.
