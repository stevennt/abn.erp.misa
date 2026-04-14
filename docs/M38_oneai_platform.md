# M38 - OneAI Platform (Nen tang AI Thong nhat)

**Category:** Platform
**Priority:** P1
**Reference Images:** assets/image_03.png, assets/image_66.png, assets/image_72.png
**Dependencies:** M01 (Authentication), M02 (Roles & Permissions), M03 (Company & Organization)
**Generated:** April 14, 2026

---

## 1. Module Overview

The OneAI Platform integrates advanced AI models into a single unified product within the ABN ERP system, providing enterprise-grade artificial intelligence capabilities to all users across the organization. The platform consolidates multiple leading AI models -- including ChatGPT (OpenAI), Gemini (Google), Grok (xAI), DeepSeek, Claude (Anthropic), and 10+ additional models -- into a single access point with centralized quota management, cost tracking, and usage reporting. The core value proposition is simple: "Buy once for the team" -- organizations purchase AI credits centrally and allocate them across team members, eliminating the need for individual subscriptions and enabling unified governance of AI usage.

The platform dashboard (image_03) presents a comprehensive view of available AI models with their status, capabilities, and access points. The model selection interface (image_66) allows users to choose between different AI models based on task type, with smart Q&A, data research, document processing, summarization, content creation, and planning capabilities. The member management screen (image_72) provides quick search functionality for team members, quota allocation and recovery controls, usage monitoring, and cost tracking per user. The platform claims to save 80% of time on knowledge work tasks through AI-assisted workflows. The mobile application extends AI capabilities to mobile users with optimized interfaces for on-the-go interactions. Key benefits include: centralized management (allocate, recover, and adjust quotas), detailed usage reports, and team-wide access from a single purchase. The platform serves as the AI backbone for the entire ERP system, providing intelligent capabilities to AI Marketing (M31), CRM (M32), and all other modules that require AI-powered features.

### User Personas

| Role | Description | Access Level |
|------|-------------|--------------|
| Platform Administrator | Manages AI model configuration, quota allocation, and cost monitoring | Full access to all platform settings and usage data |
| Team Manager | Allocates AI quotas to team members, monitors usage and costs | Full access to team usage data, quota management |
| AI User (Employee) | Uses AI models for daily work tasks, content creation, and research | Access to assigned models within quota limits |
| Finance Manager | Monitors AI costs, budget utilization, and ROI analysis | Read access to all cost and usage data |
| Executive | Views platform-wide AI adoption metrics and productivity gains | Read-only access to aggregated reports |

### Module Dependencies

| Dependency Module | Relationship |
|-------------------|--------------|
| M01 - Authentication | User identity and session management |
| M02 - Roles & Permissions | Model access control by role and department |
| M03 - Company & Organization | Organization-level quota and billing |
| M31 - AI Marketing | AI content generation for marketing campaigns |
| M19 - Workflow | AI-assisted document processing and summarization |
| M06 - Task Management | AI task assistance and planning |

### Key Business Rules

1. Each user is assigned a monthly token quota that determines how many AI interactions they can perform; exceeding quota requires manager approval or quota purchase.
2. Token consumption varies by model and input/output length; premium models (GPT-4, Claude 3) consume more tokens per interaction than standard models.
3. Quota allocation is hierarchical: organization quota -> department quota -> individual user quota; unused quota can be recovered and reallocated.
4. AI conversation history is preserved for 90 days by default; users can save important conversations to their personal library indefinitely.
5. All AI interactions are logged for cost tracking, usage analysis, and compliance purposes; logs are immutable.
6. Sensitive data must not be sent to external AI model APIs; a data classification filter checks outgoing prompts for PII, financial data, and confidential information.
7. AI-generated content must be reviewed and approved by the user before being used in official documents or communications; the system marks AI-generated content with appropriate attribution.
8. Cost allocation follows the quota assignment: costs are charged to the department that allocated the quota, not the individual user.
9. AI model availability is monitored in real-time; if a model becomes unavailable, users are automatically redirected to an alternative model with similar capabilities.
10. Mobile AI usage is tracked separately from desktop usage for cost analysis purposes, but shares the same token quota.

---

## 2. Screen Inventory

### 2.1 Screen: OneAI Platform Dashboard

**Route:** `/oneai/dashboard` | **Purpose:** Display AI platform overview with model availability, usage summary, and quick access.

| UI Component | Description |
|--------------|-------------|
| Model Availability Grid | Cards for each model: ChatGPT, Gemini, Grok, DeepSeek, Claude, +10 more with status indicators |
| Usage Summary | Total conversations this month, tokens consumed, remaining quota |
| Cost Overview | Total AI cost this month, cost per user, cost trend |
| Quick Start | Popular AI tasks: Quick search, Smart Q&A, Data research, Document processing, Summarization, Content creation, Planning |
| Time Saved Metric | "Saves 80% time" indicator with breakdown by task type |
| Recent Conversations | List of recent AI conversations with model used and topic |

**User Interactions:**
- Click on any model card to start a conversation with that model
- View usage summary and remaining quota
- Click quick start options to launch pre-configured AI tasks
- Browse recent conversations to resume or review

**Data Displayed:**
- Available AI models and their status
- Token usage and remaining quota
- Cost breakdown and trends
- Time savings metrics

### 2.2 Screen: AI Chat Interface

**Route:** `/oneai/chat` | **Purpose:** Interact with AI models through a conversational interface.

| UI Component | Description |
|--------------|-------------|
| Model Selector | Dropdown to choose AI model (ChatGPT, Gemini, Grok, DeepSeek, Claude, etc.) |
| Chat Window | Conversation history with user messages and AI responses |
| Input Area | Text input with formatting options, file attachment, code block support |
| Token Counter | Estimated tokens for current input and response |
| Conversation Actions | Save conversation, export, share, clear, new conversation |
| Prompt Templates | Quick-access template library for common tasks |

**User Interactions:**
- Type message and send to AI model
- Switch models mid-conversation to compare responses
- Attach documents for processing
- Save conversation to library
- Export conversation as PDF or text
- Use prompt templates for structured tasks

**Data Displayed:**
- Conversation history with timestamps
- Model being used
- Token consumption per message
- Response generation time

### 2.3 Screen: Model Selection & Comparison

**Route:** `/oneai/models` | **Purpose:** Browse available AI models, compare capabilities, and select appropriate model for task.

| UI Component | Description |
|--------------|-------------|
| Model Cards | Each model: name, provider, capabilities, strengths, token cost multiplier |
| Capability Comparison | Table comparing models by task type (writing, coding, analysis, translation) |
| Cost Comparison | Token cost per model relative to baseline |
| Availability Status | Real-time model availability with uptime percentage |
| Recommendation Engine | Suggests best model based on task description |

**User Interactions:**
- Browse model cards to understand capabilities
- Compare models side by side
- Select recommended model for specific task
- View availability and performance history

**Data Displayed:**
- Model capabilities and strengths
- Token cost multipliers
- Availability status and uptime
- Task-model recommendations

### 2.4 Screen: Member Management & Quota Allocation

**Route:** `/oneai/members` | **Purpose:** Manage team members, allocate AI quotas, and monitor individual usage.

| UI Component | Description |
|--------------|-------------|
| Quick Search | Search members by name, email, department |
| Member List | Name, department, assigned quota, used tokens, remaining, cost |
| Quota Allocation | Slider/input to assign tokens to individual members |
| Quota Recovery | Reclaim unused quota from members |
| Quota Adjustment | Increase/decrease quota for individual members |
| Usage Charts | Per-member usage trends and patterns |

**User Interactions:**
- Search for specific team members
- Allocate token quotas using sliders
- Recover unused quotas from inactive members
- Adjust quotas based on changing needs
- View individual usage charts and patterns

**Data Displayed:**
- Member list with department and role
- Assigned, used, and remaining tokens
- Cost per member
- Usage trends over time

### 2.5 Screen: Usage Reports & Analytics

**Route:** `/oneai/reports` | **Purpose:** Detailed AI usage reports with filtering, charting, and export capabilities.

| UI Component | Description |
|--------------|-------------|
| Report Filters | Date range, department, model, user, task type |
| Usage Chart | Tokens consumed over time by model and user |
| Cost Breakdown | Cost by model, department, and user |
| Top Users | Most active AI users with usage patterns |
| Model Adoption | Usage distribution across models |
| Export Options | PDF, Excel, CSV export |

**User Interactions:**
- Apply filters to generate custom reports
- Switch between chart types (bar, line, pie)
- View top user rankings and usage patterns
- Export reports for sharing

**Data Displayed:**
- Token consumption trends
- Cost allocation by dimension
- Model adoption rates
- User activity rankings

### 2.6 Screen: AI Template Library

**Route:** `/oneai/templates` | **Purpose:** Browse and use pre-built AI prompt templates for common tasks.

| UI Component | Description |
|--------------|-------------|
| Template Categories | Content creation, Data research, Document processing, Summarization, Planning, Quick search, Smart Q&A |
| Template Cards | Name, description, model recommendation, usage count |
| Template Editor | Create and customize templates with variables |
| Template Sharing | Share templates with team members |

**User Interactions:**
- Browse templates by category
- Use template to start a new conversation
- Create custom templates with variable placeholders
- Share templates with team

**Data Displayed:**
- Template categories and descriptions
- Usage counts per template
- Recommended models per template

### 2.7 Screen: AI Settings & Configuration

**Route:** `/oneai/settings` | **Purpose:** Configure platform-wide AI settings, model access, and data protection rules.

| UI Component | Description |
|--------------|-------------|
| Model Access Control | Enable/disable models per role or department |
| Quota Configuration | Set default quotas, token cost multipliers |
| Data Protection | Configure sensitive data filtering rules |
| Retention Policy | Set conversation history retention period |
| Cost Alerts | Configure budget threshold notifications |
| API Configuration | Manage external AI model API keys and endpoints |

**User Interactions:**
- Configure model access by role
- Set default quotas and pricing
- Configure data protection filters
- Set retention policies
- Configure cost alert thresholds

**Data Displayed:**
- Current configuration values
- API connection status
- Data protection filter rules

### 2.8 Screen: Mobile AI Interface

**Route:** `/oneai/mobile` | **Purpose:** Mobile-optimized AI chat interface for on-the-go interactions.

| UI Component | Description |
|--------------|-------------|
| Chat Interface | Simplified mobile chat with model selector |
| Voice Input | Voice-to-text for hands-free interaction |
| Quick Actions | Preset prompts for common mobile tasks |
| Conversation History | Access saved conversations |
| Usage Status | Remaining quota and token usage |

**User Interactions:**
- Chat with AI models on mobile
- Use voice input for hands-free operation
- Access quick action presets
- View usage status

**Data Displayed:**
- Conversation history
- Token usage and remaining quota
- Quick action options

---

## 3. Feature Specifications

### 3.1 Feature: Multi-Model AI Access
**Description:** Access multiple AI models through a unified interface with model selection, switching, and comparison.

**User Stories:**
- As an AI User, I want to choose between different AI models, so that I can use the best model for each task.
- As a Team Manager, I want to see which models my team is using, so that I can optimize cost-effectiveness.

**Acceptance Criteria:**
- Given a user starts a conversation, when they select a model, then all interactions use that model until changed.
- Given a user switches models mid-conversation, when the switch is made, then the new model continues the conversation context.

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Model selection | Must be available and accessible | Selected model is currently unavailable |
| Token quota | Must have remaining tokens | You have exceeded your monthly AI token quota |
| Input length | Max 50,000 characters per message | Message exceeds maximum length |

**Edge Cases:**
- Model unavailability triggers automatic fallback to recommended alternative
- Token quota exceeded prompts user to request quota increase from manager
- Very long conversations may be truncated for context window limits

### 3.2 Feature: Token Quota Management
**Description:** Allocate, monitor, and adjust AI token quotas at organization, department, and individual levels.

**User Stories:**
- As a Team Manager, I want to allocate token quotas to my team members, so that they can use AI within budget.
- As a Platform Administrator, I want to recover unused quotas from inactive users, so that tokens are not wasted.

**Acceptance Criteria:**
- Given a manager allocates quota to a user, when the quota is set, then the user can consume tokens up to the allocated amount.
- Given a user has unused quota at month end, when the recovery runs, then unused tokens are returned to the department pool.

**Edge Cases:**
- Quota increases during the month draw from department reserve or require organization-level approval
- Emergency quota requests bypass normal allocation with manager approval
- Quota consumption is tracked in real-time with 5-second update delay

### 3.3 Feature: Conversation History & Management
**Description:** Preserve, search, and manage AI conversation history with retention policies.

**User Stories:**
- As an AI User, I want to access my conversation history, so that I can reference previous AI responses.
- As a Platform Administrator, I want to set retention policies, so that storage costs are controlled.

**Acceptance Criteria:**
- Given a conversation is completed, when the user views their history, then the conversation is listed with model, date, and topic.
- Given a conversation exceeds the retention period, when the cleanup runs, then it is archived or deleted per policy.

**Edge Cases:**
- Saved conversations are preserved beyond retention period
- Shared conversations are visible to all shared recipients
- Sensitive conversations flagged by data filter are quarantined for review

### 3.4 Feature: Data Protection & Sensitive Content Filtering
**Description:** Filter outgoing prompts for sensitive data to prevent data leakage to external AI providers.

**User Stories:**
- As a Platform Administrator, I want to prevent sensitive data from being sent to AI models, so that data privacy is maintained.
- As a Finance Manager, I want financial data to be filtered, so that confidential numbers are not exposed.

**Acceptance Criteria:**
- Given a prompt contains sensitive data patterns (PII, financial numbers, confidential markers), when the filter processes it, then the data is masked or the prompt is blocked.
- Given a prompt is blocked, when the user is notified, then they receive a clear explanation of what was detected.

**Edge Cases:**
- False positives (legitimate data that looks sensitive) can be overridden with user confirmation
- Custom sensitive data patterns can be configured per organization
- Filtering adds minimal latency (< 100ms) to prompt processing

### 3.5 Feature: AI-Powered Task Templates
**Description:** Provide pre-built prompt templates for common business tasks with variable substitution.

**User Stories:**
- As an AI User, I want to use pre-built templates for common tasks, so that I don't have to write prompts from scratch.
- As a Team Manager, I want to create team-specific templates, so that AI usage is standardized across the team.

**Acceptance Criteria:**
- Given a user selects a template, when the template loads, then variable placeholders are highlighted for input.
- Given a template is used, when the conversation starts, then the prompt is sent to the recommended model.

**Edge Cases:**
- Templates with missing required variables cannot be submitted
- Custom templates can be created and shared within the team
- Template usage is tracked for popularity analytics

### 3.6 Feature: Cost Tracking & Budget Management
**Description:** Track AI costs at all levels (user, department, organization) with budget alerts and reporting.

**User Stories:**
- As a Finance Manager, I want to see AI costs by department, so that I can monitor budget utilization.
- As a Team Manager, I want cost alerts when approaching budget limits, so that I can prevent overspending.

**Acceptance Criteria:**
- Given tokens are consumed, when the cost is calculated, then the model-specific rate is applied and charged to the allocating department.
- Given spending reaches a configured threshold, when the check runs, then notifications are sent to configured recipients.

**Edge Cases:**
- Cost calculations use current model pricing, not historical pricing
- Refunds for failed API calls are credited back to user quota
- Monthly cost reports include year-to-date totals and trends

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|    AIModel     |       |AIConversation  |       |   AIPrompt     |
+----------------+       +----------------+       +----------------+
| id (PK)        |       | id (PK)        |<----->| id (PK)        |
| name           |       | user_id(FK)    |       | conversation_id|
| provider       |       | model_id(FK)   |       | role           |
| capabilities   |       | title          |       | content        |
| token_cost_mult|       | status         |       | token_count    |
| is_active      |       | created_at     |       | created_at     |
| config_json    |       +----------------+       +----------------+
+----------------+              |
        |                       v
        v                +----------------+
+----------------+       |  AITemplate    |
|     AIUser     |       +----------------+
+----------------+       | id (PK)        |
| id (PK)        |       | name           |
| user_id(FK)    |       | category       |
| quota_assigned |       | prompt_template|
| quota_used     |       | model_recommend|
| quota_remaining|       | usage_count    |
| cost_this_month|       | created_by(FK) |
+----------------+       +----------------+
        |
        v                   +----------------+
+----------------+          |    AIUsage     |
|   AIQuota      |          +----------------+
+----------------+          | id (PK)        |
| id (PK)        |          | user_id(FK)    |
| org_total      |          | model_id(FK)   |
| dept_json      |          | token_count    |
| user_json      |          | cost           |
| recovery_date  |          | task_type      |
+----------------+          | date           |
                            +----------------+
        |                           |
        v                           v
+----------------+          +----------------+
|    AICost      |          |AIConvHistory   |
+----------------+          +----------------+
| id (PK)        |          | id (PK)        |
| period         |          | conversation_id|
| total_cost     |          | snapshot_data  |
| by_model_json  |          | archive_date   |
| by_dept_json   |          +----------------+
+----------------+
```

### 4.2 Table Schemas

#### Table: ai_model
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL | Model display name |
| provider | VARCHAR(100) | NOT NULL | Provider: OpenAI, Google, xAI, DeepSeek, Anthropic |
| model_code | VARCHAR(50) | NOT NULL, UNIQUE | Internal model code |
| capabilities_json | JSON | NOT NULL | Supported capabilities (writing, coding, analysis, etc.) |
| token_cost_multiplier | DECIMAL(5,2) | DEFAULT 1.0 | Cost multiplier relative to baseline |
| max_context_length | INT | DEFAULT 4096 | Maximum context window |
| is_active | TINYINT(1) | DEFAULT 0 | Availability flag |
| api_endpoint | VARCHAR(500) | NULL | API endpoint URL |
| uptime_percent | DECIMAL(5,2) | DEFAULT 100 | Uptime percentage |
| config_json | JSON | NULL | Provider-specific configuration |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_ai_model_code ON ai_model(model_code);
CREATE INDEX idx_ai_model_provider ON ai_model(provider);
CREATE INDEX idx_ai_model_active ON ai_model(is_active);
```

#### Table: ai_conversation
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | FK -> users.id, NOT NULL | Conversation owner |
| model_id | BIGINT | FK -> ai_model.id, NOT NULL | AI model used |
| title | VARCHAR(300) | NULL | Auto-generated or user-set title |
| status | VARCHAR(30) | NOT NULL, DEFAULT 'active' | Status: active, archived, deleted |
| token_count | INT | DEFAULT 0 | Total tokens consumed |
| message_count | INT | DEFAULT 0 | Number of exchanges |
| is_saved | TINYINT(1) | DEFAULT 0 | Saved to library flag |
| shared_with_json | JSON | NULL | Shared user IDs |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| expires_at | TIMESTAMP | NULL | Retention expiry |

**Indexes:**
```sql
CREATE INDEX idx_ai_conv_user ON ai_conversation(user_id);
CREATE INDEX idx_ai_conv_model ON ai_conversation(model_id);
CREATE INDEX idx_ai_conv_status ON ai_conversation(status);
CREATE INDEX idx_ai_conv_created ON ai_conversation(created_at);
CREATE INDEX idx_ai_conv_saved ON ai_conversation(is_saved);
```

#### Table: ai_prompt
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| conversation_id | BIGINT | FK -> ai_conversation.id, NOT NULL | Parent conversation |
| role | VARCHAR(20) | NOT NULL | Role: user, assistant, system |
| content | LONGTEXT | NOT NULL | Message content |
| token_count | INT | NOT NULL | Token count for this message |
| filtered_content | TEXT | NULL | Content after sensitive data filtering |
| response_time_ms | INT | NULL | AI response generation time |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_ai_prompt_conversation ON ai_prompt(conversation_id);
CREATE INDEX idx_ai_prompt_role ON ai_prompt(role);
CREATE INDEX idx_ai_prompt_created ON ai_prompt(created_at);
```

#### Table: ai_template
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Template name |
| category | VARCHAR(50) | NOT NULL | Category: content_creation, research, summarization, planning, quick_search, smart_qa, doc_processing |
| description | TEXT | NULL | Template description |
| prompt_template | TEXT | NOT NULL | Prompt with variable placeholders |
| model_recommendation | BIGINT | FK -> ai_model.id | Recommended model |
| variables_json | JSON | NULL | Variable definitions |
| usage_count | INT | DEFAULT 0 | Total usage count |
| is_public | TINYINT(1) | DEFAULT 0 | Public vs private |
| created_by | BIGINT | FK -> users.id | Template creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_ai_template_category ON ai_template(category);
CREATE INDEX idx_ai_template_public ON ai_template(is_public);
CREATE INDEX idx_ai_template_creator ON ai_template(created_by);
```

#### Table: ai_user
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | FK -> users.id, NOT NULL | System user |
| quota_assigned | INT | DEFAULT 0 | Monthly token quota |
| quota_used | INT | DEFAULT 0 | Tokens consumed |
| quota_remaining | INT | DEFAULT 0 | Remaining tokens |
| cost_this_month | DECIMAL(18,2) | DEFAULT 0 | Cost this month |
| last_active_at | TIMESTAMP | NULL | Last AI usage timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_ai_user_user ON ai_user(user_id);
CREATE INDEX idx_ai_user_quota ON ai_user(quota_remaining);
CREATE INDEX idx_ai_user_active ON ai_user(last_active_at);
```

#### Table: ai_quota
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| period_month | INT | NOT NULL | Quota period month |
| period_year | INT | NOT NULL | Quota period year |
| org_total | INT | NOT NULL | Organization total quota |
| dept_allocation_json | JSON | NOT NULL | Department allocations |
| user_allocation_json | JSON | NOT NULL | Individual user allocations |
| recovery_date | DATE | NULL | Last recovery execution date |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_ai_quota_period ON ai_quota(period_month, period_year);
```

#### Table: ai_usage
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | FK -> users.id, NOT NULL | User |
| model_id | BIGINT | FK -> ai_model.id, NOT NULL | Model used |
| conversation_id | BIGINT | FK -> ai_conversation.id | Associated conversation |
| token_count | INT | NOT NULL | Tokens consumed |
| cost | DECIMAL(18,4) | NOT NULL | Cost incurred |
| task_type | VARCHAR(50) | NULL | Task category |
| usage_date | DATE | NOT NULL | Usage date |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_ai_usage_user ON ai_usage(user_id);
CREATE INDEX idx_ai_usage_model ON ai_usage(model_id);
CREATE INDEX idx_ai_usage_date ON ai_usage(usage_date);
CREATE INDEX idx_ai_usage_task ON ai_usage(task_type);
```

#### Table: ai_cost
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| period_month | INT | NOT NULL | Cost period month |
| period_year | INT | NOT NULL | Cost period year |
| total_cost | DECIMAL(18,2) | NOT NULL | Total cost for period |
| by_model_json | JSON | NULL | Cost breakdown by model |
| by_dept_json | JSON | NULL | Cost breakdown by department |
| by_user_json | JSON | NULL | Cost breakdown by user |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_ai_cost_period ON ai_cost(period_month, period_year);
```

#### Table: ai_conversation_history
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| conversation_id | BIGINT | FK -> ai_conversation.id, NOT NULL | Archived conversation |
| snapshot_data | LONGTEXT | NOT NULL | Full conversation snapshot |
| archive_reason | VARCHAR(50) | DEFAULT 'retention_expiry' | Reason for archiving |
| archive_date | TIMESTAMP | NOT NULL, DEFAULT NOW() | Archive timestamp |

**Indexes:**
```sql
CREATE INDEX idx_ai_history_conversation ON ai_conversation_history(conversation_id);
CREATE INDEX idx_ai_history_date ON ai_conversation_history(archive_date);
```

#### Table: ai_bot
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(200) | NOT NULL | Bot display name |
| description | TEXT | NULL | Bot description |
| system_prompt | TEXT | NOT NULL | Bot system prompt / personality |
| model_id | BIGINT | FK -> ai_model.id | Default model |
| avatar_url | VARCHAR(500) | NULL | Bot avatar image |
| is_public | TINYINT(1) | DEFAULT 0 | Public availability |
| created_by | BIGINT | FK -> users.id | Bot creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_ai_bot_public ON ai_bot(is_public);
CREATE INDEX idx_ai_bot_creator ON ai_bot(created_by);
```

#### Table: ai_settings
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| setting_key | VARCHAR(100) | NOT NULL, UNIQUE | Setting identifier |
| setting_value | TEXT | NULL | Setting value (JSON for complex) |
| description | TEXT | NULL | Setting description |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| updated_by | BIGINT | FK -> users.id | Last updater |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_ai_settings_key ON ai_settings(setting_key);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| ai_model | AI model configurations | 20 |
| ai_conversation | Conversation records | 500,000 |
| ai_prompt | Individual messages | 5,000,000 |
| ai_template | Prompt templates | 200 |
| ai_user | User quota tracking | 5,000 |
| ai_quota | Quota allocations | 100 |
| ai_usage | Usage log entries | 10,000,000 |
| ai_cost | Cost aggregation | 500 |
| ai_conversation_history | Archived conversations | 200,000 |
| ai_bot | Custom AI bots | 50 |
| ai_settings | Platform settings | 50 |

---

## 5. API Specifications

### 5.1 AI Conversation Endpoints

#### POST /api/v1/oneai/conversations
**Description:** Start a new AI conversation

**Request Body:**
```json
{
  "model_id": 1,
  "title": "Marketing copy for product launch",
  "initial_prompt": "Write a marketing email for our new product..."
}
```

**Response (201 Created):**
```json
{
  "id": 5001,
  "model_id": 1,
  "title": "Marketing copy for product launch",
  "token_count": 45,
  "created_at": "2026-04-14T10:00:00Z"
}
```

**Permission:** `oneai:conversation:create`

#### POST /api/v1/oneai/conversations/{id}/messages
**Description:** Send a message in an existing conversation

**Permission:** `oneai:conversation:message`

#### GET /api/v1/oneai/conversations
**Description:** List user's conversations

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page |
| model_id | integer | No | Filter by model |
| status | string | No | Filter by status |
| saved | boolean | No | Filter saved only |

**Permission:** `oneai:conversation:read`

### 5.2 Model Endpoints

#### GET /api/v1/oneai/models
**Description:** List available AI models

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "name": "ChatGPT (GPT-4)",
      "provider": "OpenAI",
      "capabilities": ["writing", "coding", "analysis", "translation"],
      "token_cost_multiplier": 2.0,
      "is_active": true,
      "uptime_percent": 99.9
    }
  ]
}
```

**Permission:** `oneai:model:read`

### 5.3 Quota Endpoints

#### GET /api/v1/oneai/quota
**Description:** Get current quota status

**Response (200 OK):**
```json
{
  "quota_assigned": 100000,
  "quota_used": 35000,
  "quota_remaining": 65000,
  "cost_this_month": 175000,
  "last_active_at": "2026-04-14T09:30:00Z"
}
```

**Permission:** `oneai:quota:read`

#### PUT /api/v1/oneai/quota/allocate
**Description:** Allocate quota to user (manager only)

**Permission:** `oneai:quota:allocate`

### 5.4 Template Endpoints

#### GET /api/v1/oneai/templates
**Description:** List AI templates

**Permission:** `oneai:template:read`

#### POST /api/v1/oneai/templates/{id}/use
**Description:** Use a template to start conversation

**Permission:** `oneai:template:use`

### 5.5 Usage Report Endpoints

#### GET /api/v1/oneai/reports/usage
**Description:** Get usage report

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date_from | string | Yes | Start date |
| date_to | string | Yes | End date |
| department_id | integer | No | Filter by department |
| model_id | integer | No | Filter by model |

**Permission:** `oneai:report:read`

### 5.6 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| POST | /api/v1/oneai/conversations | Start conversation | oneai:conversation:create |
| POST | /api/v1/oneai/conversations/{id}/messages | Send message | oneai:conversation:message |
| GET | /api/v1/oneai/conversations | List conversations | oneai:conversation:read |
| GET | /api/v1/oneai/models | List models | oneai:model:read |
| GET | /api/v1/oneai/quota | Get quota | oneai:quota:read |
| PUT | /api/v1/oneai/quota/allocate | Allocate quota | oneai:quota:allocate |
| GET | /api/v1/oneai/templates | List templates | oneai:template:read |
| POST | /api/v1/oneai/templates/{id}/use | Use template | oneai:template:use |
| GET | /api/v1/oneai/reports/usage | Usage report | oneai:report:read |
| GET | /api/v1/oneai/members | List members | oneai:member:read |
| GET | /api/v1/oneai/reports/cost | Cost report | oneai:report:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Platform Administrator | platform_admin | Full access to all AI platform features |
| Team Manager | team_manager | Manage team quotas, view team reports |
| AI User | ai_user | Use AI models within quota |
| Finance Manager | finance_manager | View cost reports and budget data |
| Executive | executive | Read-only aggregated reports |

### Permission Matrix
| Role | Use AI Models | Manage Quota | View Reports | Manage Templates | Configure Settings | View All Costs |
|------|-------------|-------------|-------------|-----------------|-------------------|---------------|
| Platform Administrator | Yes | Yes | Yes | Yes | Yes | Yes |
| Team Manager | Yes | Team only | Team only | Yes | No | Team only |
| AI User | Yes | No | Own only | Create own | No | No |
| Finance Manager | No | No | All costs | No | No | Yes |
| Executive | Yes | No | Aggregated | No | No | Aggregated |

### Row-Level Security
- AI Users can only view their own conversations, quota, and usage data.
- Team Managers can view and manage data for their direct reports only.
- Platform Administrators have full access across all users and departments.
- Finance Managers have read access to all cost data but cannot view conversation content.

---

## 7. State Machine / Workflows

### 7.1 Conversation Status Transitions

```
[ACTIVE] ──archive──→ [ARCHIVED] ──delete──→ [DELETED]
   │
   └──expire──→ [ARCHIVED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| ACTIVE | ARCHIVED | archive | User | Conversation moved to archive |
| ACTIVE | ARCHIVED | expire | System | Retention period exceeded |
| ARCHIVED | DELETED | delete | System/User | Permanent deletion after archive retention |

### 7.2 Quota Lifecycle

```
[ALLOCATED] ──consume──→ [PARTIALLY_USED] ──consume──→ [EXHAUSTED]
     │                        │
     └──recover──→ [RECOVERED]└──increase──→ [PARTIALLY_USED]
```

### 7.3 Model Availability

```
[AVAILABLE] ──outage──→ [UNAVAILABLE] ──restore──→ [AVAILABLE]
     │
     └──deactivate──→ [DISABLED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Usage Overview
**Type:** Real-time
**Purpose:** Display AI usage by model, user, and department

**Metrics:**
- Total conversations, tokens consumed, cost
- Usage by model
- Usage by department

**Chart Type:** Bar chart, Pie chart

### 8.2 Report: Cost Analysis
**Type:** Real-time
**Purpose:** Track AI costs across all dimensions

**Metrics:**
- Total cost by model, department, user
- Cost trend over time
- Cost per token by model

**Chart Type:** Line chart, Table

### 8.3 Report: Model Adoption
**Type:** Real-time
**Purpose:** Monitor adoption rates across available models

**Metrics:**
- Usage distribution by model
- Active user count per model
- Task type by model

**Chart Type:** Pie chart, Bar chart

### 8.4 Report: Productivity Impact
**Type:** Real-time
**Purpose:** Measure time saved through AI assistance

**Metrics:**
- Estimated time saved (80% claim)
- Tasks completed with AI
- User satisfaction scores

**Chart Type:** Bar chart, Table

### 8.5 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Usage Overview | Real-time | Weekly | Chart, CSV |
| Cost Analysis | Real-time | Monthly | Line Chart, Excel |
| Model Adoption | Real-time | Monthly | Pie Chart |
| Productivity Impact | Real-time | Quarterly | Bar Chart |
| Quota Utilization | Real-time | Monthly | Table |
| Template Usage | Real-time | Monthly | Table |
| User Activity | Real-time | Weekly | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| OpenAI API | ChatGPT model access | REST | API Key |
| Google AI API | Gemini model access | REST | API Key |
| xAI API | Grok model access | REST | API Key |
| DeepSeek API | DeepSeek model access | REST | API Key |
| Anthropic API | Claude model access | REST | API Key |
| Additional Model APIs | 10+ other model providers | REST | API Key |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| ai.conversation.started | {conversation_id, user_id, model_id} | Usage tracking |
| ai.quota.exceeded | {user_id, quota_assigned, quota_used} | User, Manager notification |
| ai.cost.threshold_reached | {threshold, current_cost, period} | Finance Manager |
| ai.model.unavailable | {model_id, error} | Platform admin, fallback routing |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| user.created | Auth (M01) | Initialize AI user quota |
| user.departed | HR (M05) | Trigger quota recovery |
| department.changed | Organization (M03) | Reallocate department quota |

---

## 10. Technical Notes

### Performance
- Expected data volume: 500,000+ conversations, 5,000,000+ messages, 10,000,000+ usage records
- Query complexity: Medium (aggregations for reports, conversation retrieval)
- Expected response time: Model list < 200ms, Conversation load < 500ms, Message send < 30s (depends on model API)
- Token counting: < 50ms per message using tiktoken or equivalent
- Quota check: < 10ms per request

### Caching Strategy
- Redis cache for model configurations, template library, and user quota status
- TTL: 1 hour for model configs, 5 minutes for quota status, 24 hours for templates
- Conversation messages cached individually with 30-minute TTL
- Cache invalidation on quota change, model status change, or template update

### File Attachments
- Supported types: TXT, PDF, DOCX, XLSX, CSV (for document processing)
- Max size: 10 MB per file
- Files processed by AI models are temporarily stored and deleted after processing
- Storage: Encrypted S3-compatible object storage

### Localization
- Supported languages: Vietnamese (primary), English, +10 languages supported by AI models
- Date format: DD/MM/YYYY
- AI models handle multi-language input/output natively
- UI translated to Vietnamese and English

### Mobile Considerations
- Mobile app provides full AI chat experience optimized for small screens
- Voice input supported via device speech-to-text
- Push notifications for conversation responses (optional)
- Offline mode: recent conversations cached locally
- Mobile usage tracked separately for cost analysis

### Security
- Sensitive data filtering prevents PII, financial data, and confidential information from reaching external AI APIs
- All AI API communications use TLS 1.2+
- Conversation content encrypted at rest using AES-256
- API keys for external model providers stored in encrypted vault
- Audit trail logs all AI interactions with user, model, timestamp, and token count
- Cost data accessible only to authorized roles; conversation content never exposed to finance roles

### Scalability
- Streaming responses from AI models for real-time display
- Conversation processing uses async queue for high-volume periods
- Usage logging is batched and written asynchronously
- Quota checks use fast in-memory counters with periodic database sync
- Multi-region deployment for low-latency access to external AI APIs
