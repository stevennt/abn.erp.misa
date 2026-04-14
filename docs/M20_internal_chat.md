# M20 - Internal Chat (Noi bo)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_19.png, assets/image_22.png, assets/image_58.png
**Dependencies:** M01 (User & Authentication), M03 (Company & Organization), M06 (Task Management)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Internal Chat module provides real-time team communication within the organization, supporting group conversations, direct messages, file sharing, task creation from chat, and @mentions. It serves as the primary collaboration hub where employees discuss projects, share files (Figma links, documents, images), coordinate activities, and convert conversations into actionable tasks. The module integrates with the notification system to alert users of new messages, mentions, and task assignments created from chat. Groups support up to 50 members with message history, pinned messages, search functionality, and emoji reactions, creating a Slack-like experience tailored for the enterprise ERP environment.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Chat User | Any employee with messaging needs | Full (own conversations) |
| Group Admin | Group creator or designated admin | Full (group management) |
| Department Manager | Views department-related group activity | Read + participate |
| System Administrator | Manages global chat settings and compliance | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication), M03 (Company & Organization)
- **Used by:** M06 (Task Management), M19 (Workflow), M23 (Social Network), M24 (Meeting Room Booking)

### Key Business Rules
1. A user can belong to a maximum of 50 groups simultaneously.
2. Group chat supports up to 50 members (as shown in "AMIS Chat Design Analysis Group" with 8 members).
3. Direct messages (1-on-1) are created automatically between any two employees in the same company.
4. @mentions trigger real-time notifications to the mentioned user with a link to the message.
5. File attachments in chat support all common types (pdf, docx, png, jpg, figma links, etc.) with a 25MB per-file limit.
6. Messages can be pinned by group admins for easy reference; up to 20 pinned messages per group.
7. Task creation from chat allows converting any message into a task with assignee, due date, and priority.
8. Message edit is allowed within 15 minutes of sending; deletion is allowed within 24 hours for users, unlimited for admins.
9. All messages are retained indefinitely for compliance; soft-deleted messages show as "deleted" with content hidden.
10. Emoji reactions limited to 6 predefined options plus custom emojis configured by admin.
11. Group names must be unique within the organization and max 100 characters.
12. Auto-notifications are generated for system events (room availability, task creation, workflow updates).

---

## 2. Screen Inventory

### 2.1 Screen: Chat Group View
**Route:** `/chat/groups/{id}`
**Purpose:** Main group chat interface with message thread, file sharing, and member list (img_22).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Group Header | Header bar | Group name, member count (8 members), settings icon |
| Message Thread | Scrollable list | Chronological messages with sender avatars, timestamps, reactions |
| Message Composer | Input area | Text input, emoji picker, file attachment button, send button |
| Member Sidebar | Right panel | List of group members with online status |
| File Share Area | Inline | Shared files with preview (Figma links, documents, images) |
| @Mention Dropdown | Autocomplete | Suggests usernames when typing @ |
| Task Creation Modal | Modal popup | Convert message to task form (assignee, due date, priority, subtasks) |

**User Interactions:**
- Type and send messages in the group.
- Share files by dragging or clicking the attachment button.
- @mention colleagues by typing @ followed by their name.
- React to messages with emoji reactions.
- Pin important messages for reference.
- Create tasks from specific messages.
- Search message history.

**Data Displayed:**
- Group name: "AMIS Chat Design Analysis Group"
- Member count: 8 members
- Messages with Figma links, @mentions, auto-notifications
- Shared files and checklist items

### 2.2 Screen: Chat Sidebar / Navigation
**Route:** `/chat`
**Purpose:** Chat navigation showing all conversations, groups, and direct messages.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Search Bar | Input | Search conversations and messages |
| New Chat Button | CTA | Start new group or direct message |
| Conversation List | List | Groups and DMs with last message preview, unread badge |
| Section Headers | Divider | Groups, Direct Messages, Auto-notifications |
| User Status | Indicator | Online, away, busy, offline status |

**User Interactions:**
- Click a conversation to open it.
- Search for conversations or message content.
- Create new groups or start direct messages.
- Pin favorite conversations to top.

**Data Displayed:**
- List of all groups and DMs the user participates in.
- Unread message counts.
- Last message preview with timestamp.

### 2.3 Screen: Direct Message View
**Route:** `/chat/dm/{user_id}`
**Purpose:** One-on-one conversation between two employees.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| DM Header | Header bar | Contact name, online status, call button |
| Message Thread | Scrollable list | Chronological messages |
| Message Composer | Input area | Text input with emoji, file attachment, send |
| Shared Files Tab | Tab panel | Files exchanged in this conversation |

**User Interactions:**
- Exchange direct messages with colleagues.
- Share files privately.
- Convert DM messages to tasks.

### 2.4 Screen: Chat Search
**Route:** `/chat/search`
**Purpose:** Global search across all chat messages, files, and conversations.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Search Input | Full-width | Keyword search with filters |
| Filter Panel | Sidebar | Filter by date, sender, conversation type, file type |
| Results List | Card list | Messages grouped by conversation with highlighted matches |
| File Results | Grid | Shared files matching search criteria |

**User Interactions:**
- Search by keyword across all accessible messages.
- Filter results by date range, sender, or file type.
- Click a result to navigate to that message in context.

### 2.5 Screen: Task Creation from Chat
**Route:** Modal within chat view
**Purpose:** Convert a chat message into an actionable task (img_22).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Source Message | Card | Original message being converted |
| Assignee Selector | Dropdown | Select task assignee from group members |
| Due Date Picker | Date input | Set task deadline |
| Priority Selector | Radio buttons | Low, Medium, High, Urgent |
| Title Input | Text field | Auto-filled from message, editable |
| Subtask List | Repeater | Add subtasks with assignees |
| Description | Rich text | Additional context, pre-filled with message link |
| Create Button | CTA | Create task and link to message |

**User Interactions:**
- Review source message content.
- Fill in task details (assignee, due date, priority).
- Add subtasks if needed.
- Click Create to generate the task in M06 Task Management.

### 2.6 Screen: Group Settings
**Route:** `/chat/groups/{id}/settings`
**Purpose:** Manage group configuration, members, and preferences.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Group Info Form | Form | Name, description, avatar |
| Member Management | Table | Add/remove members, assign admin role |
| Notification Settings | Toggle controls | Mute, customize notification level |
| Pinned Messages | List | View and unpin messages |
| Danger Zone | Buttons | Leave group, delete group (admin only) |

**User Interactions:**
- Edit group name and description.
- Add or remove members.
- Promote/demote group admins.
- Manage pinned messages.
- Configure notification preferences.

---

## 3. Feature Specifications

### 3.1 Feature: Group Chat Messaging
**Description:** Real-time group messaging with rich text, file sharing, emoji reactions, and @mentions.

**User Stories:**
- As a team member, I want to send messages to my group chat, so that everyone sees updates in real-time.
- As a group member, I want to share Figma links and documents, so that the team can review design assets.
- As a user, I want to @mention a colleague, so that they receive a direct notification about the message.

**Acceptance Criteria:**
- Given a user is in a group chat
- When they type a message and press Enter or click Send
- Then the message appears in the thread for all group members in real-time
- Given a user types @ followed by a name
- When they select a user from the autocomplete dropdown
- Then the user is mentioned and receives a notification

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Message Text | Max 10,000 characters | "Message exceeds maximum length of 10,000 characters" |
| File Attachment | Max 25MB per file | "File size exceeds 25MB limit" |
| Group Members | Max 50 members | "Group has reached maximum member capacity" |

**Edge Cases:**
- Real-time delivery using WebSocket; if connection drops, messages queue locally and sync on reconnect.
- Offline users receive notifications when they come online.
- Message ordering guaranteed by server-side timestamp with logical clock for tie-breaking.

### 3.2 Feature: File Sharing in Chat
**Description:** Share files within chat conversations with preview, download, and link generation.

**User Stories:**
- As a designer, I want to share Figma links in chat, so that the team can review designs without leaving the conversation.
- As a user, I want to preview images and documents inline, so that I don't need to download every file.

**Acceptance Criteria:**
- Given a user attaches a file to a message
- When the file is an image
- Then a thumbnail preview appears inline in the message

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| File Type | jpg, png, pdf, docx, doc, xlsx, fig (link) | "File type not supported" |
| File Size | Max 25MB | "File size exceeds 25MB limit" |

**Edge Cases:**
- External links (Figma, Google Drive) are validated for accessibility and shown as rich previews.
- Virus scanning performed on all uploaded files before distribution.

### 3.3 Feature: @Mentions and Notifications
**Description:** Mention users in messages with autocomplete and automatic notification delivery.

**User Stories:**
- As a user, I want to mention someone by typing @name, so that they are directly alerted to the message.
- As a mentioned user, I want to see all my mentions in a dedicated view, so that I can track what requires my attention.

**Acceptance Criteria:**
- Given a user types @ in a message
- When the autocomplete dropdown appears
- Then it shows matching users from the group with their avatar and name

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Mention Target | Must be group member | "Cannot mention users outside the conversation" |

**Edge Cases:**
- Mentioning oneself is allowed but does not trigger a notification.
- Mentioning a user who has muted the group still delivers an @mention notification (override mute).

### 3.4 Feature: Checklist in Chat
**Description:** Create and manage checklists within chat messages for team coordination.

**User Stories:**
- As a team lead, I want to create a checklist in chat, so that team members can track action items.
- As a member, I want to check off items, so that progress is visible to the group.

**Acceptance Criteria:**
- Given a user creates a checklist
- When they add items and send the message
- Then the checklist renders as interactive checkboxes in the message thread

**Edge Cases:**
- Only checklist creator and group admins can add/remove checklist items.
- Any group member can toggle checkbox state.

### 3.5 Feature: Auto-Notifications
**Description:** System-generated messages for events like room availability, workflow updates, and task assignments.

**User Stories:**
- As a team member, I want to receive automatic notifications about room availability changes, so that I know meeting room status.
- As a user, I want system notifications about workflow runs, so that I stay informed without switching apps.

**Acceptance Criteria:**
- Given a meeting room becomes available
- When the room booking system triggers an event
- Then an auto-notification appears in the relevant group chat

**Edge Cases:**
- Auto-notifications are distinguishable from user messages with a system badge.
- Users can configure which auto-notification types they receive.

### 3.6 Feature: Emoji Reactions
**Description:** React to messages with predefined emojis for quick feedback.

**User Stories:**
- As a user, I want to react to a message with a thumbs up, so that I can acknowledge without typing.

**Acceptance Criteria:**
- Given a message is displayed
- When a user clicks the reaction button
- Then they can select from 6 predefined emojis
- And the reaction count increments for that emoji

**Edge Cases:**
- A user can only react once per emoji per message (can toggle on/off).
- Custom emojis can be configured by admin.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|    ChatRoom    |       |  ChatMessage   |       |  ChatMember    |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |<----->| id (PK)        |
| name           |       | room_id (FK)   |       | room_id (FK)   |
| type           |       | sender_id (FK) |       | user_id (FK)   |
| avatar_url     |       | content        |       | role           |
| created_by(FK) |       | parent_id (FK) |       | joined_at      |
| created_at     |       | is_pinned      |       | left_at        |
+----------------+       | created_at     |       +----------------+
                         +----------------+
                                |
                         +----------------+
                         |  ChatMention   |
                         +----------------+
                         | id (PK)        |
                         | message_id(FK) |
                         | mentioned_id   |
                         | created_at     |
                         +----------------+
```

### 4.2 Table Schemas

#### Table: chat_rooms
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL | Group name (NULL for DMs) |
| type | ENUM('group','direct') | NOT NULL | Room type |
| description | TEXT | NULL | Group description |
| avatar_url | VARCHAR(500) | NULL | Group avatar URL |
| created_by | BIGINT | NOT NULL, FK -> users.id | Creator |
| is_archived | BOOLEAN | NOT NULL, DEFAULT FALSE | Archive status |
| last_message_at | TIMESTAMP | NULL, DEFAULT NULL | Last activity timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_chat_rooms_type ON chat_rooms(type);
CREATE INDEX idx_chat_rooms_created_by ON chat_rooms(created_by);
CREATE INDEX idx_chat_rooms_last_message ON chat_rooms(last_message_at DESC);
```

#### Table: chat_members
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| room_id | BIGINT | NOT NULL, FK -> chat_rooms.id | Parent room |
| user_id | BIGINT | NOT NULL, FK -> users.id | Member |
| role | ENUM('admin','member') | NOT NULL, DEFAULT 'member' | Member role |
| is_muted | BOOLEAN | NOT NULL, DEFAULT FALSE | Notification muted |
| joined_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Join timestamp |
| left_at | TIMESTAMP | NULL, DEFAULT NULL | Leave timestamp |
| last_read_at | TIMESTAMP | NULL, DEFAULT NULL | Last message read |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_chat_members_room_user ON chat_members(room_id, user_id);
CREATE INDEX idx_chat_members_user_id ON chat_members(user_id);
CREATE INDEX idx_chat_members_room_id ON chat_members(room_id);
```

#### Table: chat_messages
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| room_id | BIGINT | NOT NULL, FK -> chat_rooms.id | Parent room |
| sender_id | BIGINT | NOT NULL, FK -> users.id | Message sender |
| content | TEXT | NULL | Message text |
| content_type | ENUM('text','file','system','checklist','task_link') | NOT NULL, DEFAULT 'text' | Content type |
| parent_id | BIGINT | NULL, FK -> chat_messages.id | Reply-to message |
| is_pinned | BOOLEAN | NOT NULL, DEFAULT FALSE | Pinned status |
| is_deleted | BOOLEAN | NOT NULL, DEFAULT FALSE | Deletion flag |
| edited_at | TIMESTAMP | NULL, DEFAULT NULL | Last edit timestamp |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_chat_messages_room_id ON chat_messages(room_id);
CREATE INDEX idx_chat_messages_sender_id ON chat_messages(sender_id);
CREATE INDEX idx_chat_messages_created_at ON chat_messages(created_at);
CREATE INDEX idx_chat_messages_is_pinned ON chat_messages(is_pinned);
CREATE INDEX idx_chat_messages_content_type ON chat_messages(content_type);
```

#### Table: chat_mentions
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| message_id | BIGINT | NOT NULL, FK -> chat_messages.id | Parent message |
| mentioned_user_id | BIGINT | NOT NULL, FK -> users.id | Mentioned user |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Mention timestamp |

**Indexes:**
```sql
CREATE INDEX idx_chat_mentions_message_id ON chat_mentions(message_id);
CREATE INDEX idx_chat_mentions_user_id ON chat_mentions(mentioned_user_id);
CREATE INDEX idx_chat_mentions_created_at ON chat_mentions(created_at);
```

#### Table: chat_files
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| message_id | BIGINT | NOT NULL, FK -> chat_messages.id | Parent message |
| file_name | VARCHAR(255) | NOT NULL | Original filename |
| file_type | VARCHAR(50) | NOT NULL | MIME type |
| file_size | BIGINT | NOT NULL | Size in bytes |
| file_url | VARCHAR(500) | NOT NULL | Storage URL |
| thumbnail_url | VARCHAR(500) | NULL | Thumbnail URL |
| uploaded_by | BIGINT | NOT NULL, FK -> users.id | Uploader |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Upload time |

**Indexes:**
```sql
CREATE INDEX idx_chat_files_message_id ON chat_files(message_id);
CREATE INDEX idx_chat_files_uploaded_by ON chat_files(uploaded_by);
CREATE INDEX idx_chat_files_file_type ON chat_files(file_type);
```

#### Table: chat_reactions
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| message_id | BIGINT | NOT NULL, FK -> chat_messages.id | Target message |
| user_id | BIGINT | NOT NULL, FK -> users.id | Reacting user |
| emoji | VARCHAR(20) | NOT NULL | Emoji code |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Reaction timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_chat_reactions_msg_user_emoji ON chat_reactions(message_id, user_id, emoji);
CREATE INDEX idx_chat_reactions_message_id ON chat_reactions(message_id);
```

#### Table: chat_pins
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| room_id | BIGINT | NOT NULL, FK -> chat_rooms.id | Parent room |
| message_id | BIGINT | NOT NULL, FK -> chat_messages.id | Pinned message |
| pinned_by | BIGINT | NOT NULL, FK -> users.id | User who pinned |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Pin timestamp |

**Indexes:**
```sql
CREATE INDEX idx_chat_pins_room_id ON chat_pins(room_id);
CREATE UNIQUE INDEX idx_chat_pins_room_message ON chat_pins(room_id, message_id);
```

#### Table: chat_searches
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | NOT NULL, FK -> users.id | Searching user |
| query | VARCHAR(500) | NOT NULL | Search query |
| filters | JSON | NULL | Applied filters |
| result_count | INT | NULL | Number of results |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Search timestamp |

**Indexes:**
```sql
CREATE INDEX idx_chat_searches_user_id ON chat_searches(user_id);
CREATE INDEX idx_chat_searches_created_at ON chat_searches(created_at);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| chat_rooms | Group and direct message rooms | 5,000 |
| chat_members | Room membership tracking | 50,000 |
| chat_messages | All chat messages | 500,000 |
| chat_mentions | @mention tracking | 100,000 |
| chat_files | Shared file metadata | 200,000 |
| chat_reactions | Emoji reactions | 300,000 |
| chat_pins | Pinned message tracking | 10,000 |
| chat_searches | Search history (optional) | 100,000 |

---

## 5. API Specifications

### 5.1 Chat Room Endpoints

#### GET /api/v1/chat/rooms
**Description:** List all rooms (groups and DMs) the user is a member of

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| type | string | No | Filter: group, direct |
| sort | string | No | Sort by: name, last_message_at |
| order | string | No | asc/desc |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "name": "AMIS Chat Design Analysis Group",
      "type": "group",
      "member_count": 8,
      "last_message": {"content": "Check the Figma link...", "sender": "Nguyen Van A", "created_at": "2024-04-14T10:30:00Z"},
      "unread_count": 3,
      "is_muted": false
    }
  ],
  "meta": {"total": 15}
}
```

**Permission:** `chat:room:read`

#### POST /api/v1/chat/rooms
**Description:** Create a new group chat

**Request Body:**
```json
{
  "name": "Marketing Campaign Discussion",
  "type": "group",
  "description": "Planning Q2 marketing campaign",
  "member_ids": [1, 2, 3, 4, 5, 6, 7, 8]
}
```

**Response (201 Created):**
```json
{
  "id": 16,
  "name": "Marketing Campaign Discussion",
  "member_count": 8,
  "created_at": "2024-04-14T11:00:00Z"
}
```

**Permission:** `chat:room:create`

#### GET /api/v1/chat/rooms/{id}
**Description:** Get room details

**Permission:** `chat:room:read`

#### PUT /api/v1/chat/rooms/{id}
**Description:** Update room settings (name, description, avatar)

**Permission:** `chat:room:update` (group admin only)

#### DELETE /api/v1/chat/rooms/{id}
**Description:** Soft delete / archive a room

**Permission:** `chat:room:delete` (group admin only)

### 5.2 Message Endpoints

#### GET /api/v1/chat/rooms/{room_id}/messages
**Description:** List messages in a room with pagination

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | integer | No | Page number |
| limit | integer | No | Items per page (default: 50, max: 100) |
| before | timestamp | No | Get messages before this time (cursor pagination) |
| pinned | boolean | No | Filter pinned messages only |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "sender": {"id": 10, "name": "Nguyen Van A", "avatar_url": "..."},
      "content": "Check the Figma link for the new design: https://figma.com/file/...",
      "content_type": "text",
      "is_pinned": false,
      "reactions": [{"emoji": "👍", "count": 3, "users": [1, 2, 3]}],
      "mentions": [{"user_id": 5, "user_name": "Tran Thi B"}],
      "files": [],
      "created_at": "2024-04-14T10:30:00Z"
    }
  ],
  "meta": {"total": 250, "has_more": true}
}
```

**Permission:** `chat:message:read`

#### POST /api/v1/chat/rooms/{room_id}/messages
**Description:** Send a new message

**Request Body:**
```json
{
  "content": "Updated the design based on feedback",
  "content_type": "text",
  "mentions": [5, 8],
  "file_ids": [101, 102]
}
```

**Response (201 Created):**
```json
{
  "id": 251,
  "content": "Updated the design based on feedback",
  "created_at": "2024-04-14T10:35:00Z"
}
```

**Permission:** `chat:message:create`

#### PUT /api/v1/chat/messages/{id}
**Description:** Edit a message (within 15 minutes)

**Permission:** `chat:message:update`

#### DELETE /api/v1/chat/messages/{id}
**Description:** Delete/soft-delete a message

**Permission:** `chat:message:delete`

#### POST /api/v1/chat/messages/{id}/pin
**Description:** Pin a message in the room

**Permission:** `chat:message:pin` (group admin)

#### DELETE /api/v1/chat/messages/{id}/pin
**Description:** Unpin a message

**Permission:** `chat:message:pin`

### 5.3 Task Creation Endpoint

#### POST /api/v1/chat/messages/{id}/create-task
**Description:** Convert a chat message into a task

**Request Body:**
```json
{
  "assignee_id": 5,
  "due_date": "2024-04-21",
  "priority": "high",
  "title": "Update design based on chat feedback",
  "subtasks": [
    {"title": "Revise homepage layout", "assignee_id": 5},
    {"title": "Update color scheme", "assignee_id": 8}
  ]
}
```

**Response (201 Created):**
```json
{
  "task_id": 501,
  "title": "Update design based on chat feedback",
  "assignee": {"id": 5, "name": "Tran Thi B"},
  "created_at": "2024-04-14T10:40:00Z"
}
```

**Permission:** `chat:task:create`

### 5.4 Search Endpoint

#### GET /api/v1/chat/search
**Description:** Search across all accessible messages

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| q | string | Yes | Search query |
| room_id | bigint | No | Limit to specific room |
| sender_id | bigint | No | Filter by sender |
| date_from | date | No | Start date |
| date_to | date | No | End date |
| file_type | string | No | Filter by file type |
| limit | integer | No | Results per page (default: 20) |

**Response (200 OK):**
```json
{
  "data": [
    {
      "message_id": 45,
      "room": {"id": 1, "name": "AMIS Chat Design Analysis Group"},
      "sender": {"id": 10, "name": "Nguyen Van A"},
      "content_snippet": "...the <em>Figma</em> link for the new...",
      "created_at": "2024-04-14T10:30:00Z"
    }
  ],
  "meta": {"total": 23, "page": 1}
}
```

**Permission:** `chat:search`

### 5.5 Error Response Format
```json
{
  "error": {
    "code": "ROOM_NOT_FOUND",
    "message": "Chat room with id 999 not found",
    "details": {}
  }
}
```

### 5.6 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/chat/rooms | List rooms | chat:room:read |
| POST | /api/v1/chat/rooms | Create group | chat:room:create |
| GET | /api/v1/chat/rooms/{id} | Get room | chat:room:read |
| PUT | /api/v1/chat/rooms/{id} | Update room | chat:room:update |
| DELETE | /api/v1/chat/rooms/{id} | Delete room | chat:room:delete |
| GET | /api/v1/chat/rooms/{id}/messages | List messages | chat:message:read |
| POST | /api/v1/chat/rooms/{id}/messages | Send message | chat:message:create |
| PUT | /api/v1/chat/messages/{id} | Edit message | chat:message:update |
| DELETE | /api/v1/chat/messages/{id} | Delete message | chat:message:delete |
| POST | /api/v1/chat/messages/{id}/pin | Pin message | chat:message:pin |
| POST | /api/v1/chat/messages/{id}/create-task | Create task | chat:task:create |
| GET | /api/v1/chat/search | Search messages | chat:search |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Chat User | chat_user | Standard employee with messaging access |
| Group Admin | group_admin | Creator or designated admin of a group |
| Department Manager | dept_manager | Can view department group activity |
| System Administrator | sys_admin | Full system-level access |

### Permission Matrix
| Role | Create Room | Send Message | Edit Message | Delete Message | Pin Message | Create Task | Manage Members |
|------|------------|-------------|-------------|---------------|------------|------------|---------------|
| Chat User | ✅ | ✅ | ✅ (15min) | ✅ (24hr) | ❌ | ✅ | ❌ |
| Group Admin | ✅ | ✅ | ✅ (15min) | ✅ (unlimited) | ✅ | ✅ | ✅ |
| Department Manager | ✅ | ✅ | ✅ (15min) | ✅ (24hr) | ❌ | ✅ | ❌ |
| System Administrator | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

### Row-Level Security
- Users can only access rooms they are members of.
- Direct messages require both parties to be active employees.
- Users can only edit/delete their own messages (admins have broader delete rights).
- Group member management restricted to group admins and system admins.

---

## 7. State Machine / Workflows

### 7.1 Message Lifecycle

```
[SENT] ──delivered──→ [DELIVERED] ──read──→ [READ]
   │                      │
   └──edit (15min)────────┘
   │
   └──delete (24hr)──→ [DELETED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| SENT | DELIVERED | deliver | System | Push notification to online members |
| DELIVERED | READ | read | Recipient | Update last_read_at, clear unread badge |
| SENT | EDITED | edit | Sender (15min window) | Set edited_at, mark as edited |
| SENT | DELETED | delete | Sender (24hr) or Admin | Hide content, show "deleted" |

### 7.2 Room Lifecycle

```
[ACTIVE] ──archive──→ [ARCHIVED]
   │
   └──delete──→ [DELETED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Group Activity Summary
**Type:** Real-time
**Purpose:** Track engagement and activity levels in group chats.

**Dimensions:**
- Group name
- Time period

**Metrics:**
- Total messages sent
- Active members count
- Files shared count
- Tasks created from chat

**Filters:**
- Date range
- Group selection

**Chart Type:** Bar chart, summary cards

### 8.2 Report: User Messaging Activity
**Type:** Real-time
**Purpose:** Individual communication patterns and productivity metrics.

**Metrics:**
- Messages sent per day
- Response time (average)
- Groups participated in
- Tasks created

**Chart Type:** Table with trend indicators

### 8.3 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Group Activity Summary | Real-time | On-demand | Bar chart, cards |
| User Messaging Activity | Real-time | On-demand | Table |
| File Sharing Report | Real-time | On-demand | Table |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| WebSocket Server | Real-time message delivery | WS | JWT handshake |
| Push Notification (FCM/APNs) | Mobile push notifications | POST | API Key |
| File Storage (S3) | File upload and serving | REST | IAM Role |
| M06 Task Service | Create tasks from chat messages | REST | JWT |
| M01 User Service | Resolve user profiles and online status | REST | JWT |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| chat.message.sent | {room_id, message_id, sender_id, content} | M06 Task Management, M23 Social Network |
| chat.mention.created | {message_id, mentioned_user_id} | M01 Notification Service |
| chat.task.created | {task_id, message_id, creator_id, assignee_id} | M06 Task Management |
| chat.room.created | {room_id, name, creator_id, member_ids} | M23 Social Network |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| task.assigned | M06 Task Management | Post auto-notification in relevant group |
| workflow.step_assigned | M19 Workflow | Post auto-notification in user's DM or group |
| room_booking.created | M24 Meeting Room | Post availability notification in group |
| employee.onboarded | M01 Auth Service | Auto-add to department group chat |

---

## 10. Technical Notes

### Performance
- Expected data volume: 500,000+ messages per year, growing continuously
- Query complexity: Low for single room, Medium for cross-room search
- Expected response time: <100ms for message delivery (WebSocket), <500ms for search
- Real-time message delivery via WebSocket; HTTP polling fallback

### Caching Strategy
- Room membership cached with 5-minute TTL (Redis)
- Recent messages (last 100 per room) cached with 1-minute TTL
- User online status cached with 30-second TTL
- Full-text search index (Elasticsearch) for cross-room message search
- Cache invalidation on new message, member change, or room update

### File Attachments
- Supported types: pdf, docx, doc, jpg, png, xlsx, csv, figma (links), mp4, gif
- Max size: 25MB per file
- Storage: S3-compatible object storage with CDN for thumbnails
- Virus scanning via ClamAV on upload

### Localization
- Supported languages: Vietnamese (primary), English
- Date format: DD/MM/YYYY
- Time display: HH:mm (24-hour format)
- Emoji rendering consistent across platforms via Unicode standard

### Mobile Considerations
- Real-time message sync via WebSocket or FCM data messages
- Offline message caching for last 100 messages per room
- Image thumbnail preloading for fast scrolling
- Push notifications for @mentions and direct messages even when app is backgrounded
- File download progress indicator for large attachments
- Mobile app icon visible in app marketplace (image_06, image_07)

### Security
- End-to-end encryption not required (enterprise trust model); TLS in transit
- Messages encrypted at rest (AES-256)
- File URLs signed and expire after 24 hours for non-members
- Rate limiting: 60 messages per minute per user to prevent spam
- Content moderation: configurable keyword filters for compliance
- Audit trail: all message edits and deletions logged with before/after content

### WebSocket Protocol
- Connection established with JWT authentication
- Heartbeat every 30 seconds to maintain connection
- Message ordering guaranteed by server-side sequence number
- Reconnection with message sync using last_received_sequence_id
- Presence protocol for online/offline status broadcasting
