# M24 - Meeting Room Booking (Phong hop)

**Category:** Digital Office
**Priority:** P4
**Reference Images:** assets/image_16.png, assets/image_60.png
**Dependencies:** M01 (User & Authentication), M03 (Company & Organization), M20 (Internal Chat)
**Generated:** April 14, 2026

---

## 1. Module Overview

### Purpose
The Meeting Room Booking module provides a comprehensive calendar-based system for reserving meeting rooms across multiple buildings and floors. It enables employees to view room availability, book time slots, manage recurring bookings, and avoid scheduling conflicts. The module features a visual calendar grid organized by building, floor, and room with time slots from 08:00 to 17:00, showing both available and booked slots. Rooms are named after international cities (Berlin, London, Madrid, Paris, Prague, Rome, Sofia, Sydney) and may have equipment attributes (Video Conference, Mobile). The system detects and prevents booking conflicts, supports recurring bookings (daily, weekly, monthly), and integrates with the operations dashboard (img_60) for usage statistics. With building and floor navigation, employees can quickly find and reserve the right space for their meetings.

### User Personas
| Role | Description | Access Level |
|------|-------------|-------------|
| Employee | Any staff member who needs to book meeting rooms | Book + view |
| Room Admin | Manages room configuration and equipment | Full (room settings) |
| Department Manager | Views department booking patterns | Read + book |
| Facility Manager | Oversees all room utilization and analytics | Full |
| System Administrator | Configures buildings, floors, and global settings | Full |

### Module Dependencies
- **Requires:** M01 (User & Authentication), M03 (Company & Organization)
- **Used by:** M20 (Internal Chat), M39 (Operations Dashboard), M19 (Workflow)

### Key Business Rules
1. The calendar grid displays time slots from 08:00 to 17:00 in configurable increments (default: 30 minutes).
2. Each room belongs to a specific floor, and each floor belongs to a specific building.
3. Building "N03 T1" has at least Floor 2 (Mezzanine, Floor 4 as shown in img_16).
4. Floor 2 rooms include: Berlin, London, Madrid, Paris (Video Conference), Prague (Mobile), Rome, Sofia, Sydney.
5. Bookings cannot overlap; the system detects and prevents conflicts automatically.
6. A booking can span multiple consecutive time slots (e.g., 09:30-10:30 = 2 slots).
7. Private meetings are shown as "Locked" to non-participants (see Sofia 08:30-10:30 and Madrid 14:00-16:30 in img_16).
8. Recurring bookings support daily, weekly, and monthly patterns with end date or occurrence count.
9. Bookings can be cancelled by the creator up to 15 minutes before start time; after that, only room admins can cancel.
10. Room equipment (Video Conference, Mobile, Projector, Whiteboard) is displayed for each room.
11. Favorite rooms can be pinned by users for quick access in the left navigation.
12. The operations dashboard shows 3 new meetings (img_60).
13. Booking metadata includes: title, organizer, attendees, description, equipment needs, privacy level.

---

## 2. Screen Inventory

### 2.1 Screen: Room Booking Calendar Grid
**Route:** `/rooms/calendar`
**Purpose:** Main calendar view showing room availability across all rooms (img_16).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Building Selector | Dropdown | Select building (e.g., "N03 T1") |
| Date Picker | Calendar input | Select date to view (e.g., "22 August 2019") |
| Left Navigation | Tree | Favorite rooms, Mezzanine, Floor 2 (expanded: Berlin, London, Madrid, Paris, Prague, Rome, Sofia, Sydney), Floor 4 |
| Calendar Grid | Table | Time slots (08:00-17:00) as rows, rooms as columns |
| Booking Blocks | Colored cells | Booked time slots with title overlay |
| Locked Blocks | Gray cells with lock icon | Private meetings not visible to current user |
| Available Cells | White/empty cells | Clickable to create booking |
| Management Settings | Link | Bottom of left nav for admin configuration |

**User Interactions:**
- Select building and date to view room schedules.
- Click an available cell to start booking.
- Click and drag to book multiple consecutive slots.
- Click a booking block to view/edit details.
- Toggle rooms on/off in the left nav.
- Favorite rooms for quick access.

**Data Displayed:**
- Sample bookings:
  - Berlin 09:30-10:30: "PQA_Seminar CMMI 2.0 (GOV)"
  - Paris 10:00-11:30: "BA training for DEV"
  - Sofia 08:30-10:30: [Locked/Private]
  - Madrid 14:00-16:30: [Locked/Private]
  - Sydney 15:00-17:30: "ISO training for new employees"
  - Berlin 16:30-17:30: "Employee evaluation"
- Room labels with equipment indicators (Paris: Video Conference, Prague: Mobile)

### 2.2 Screen: Booking Creation Modal
**Route:** Modal from calendar grid
**Purpose:** Create a new room booking.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Room Display | Card | Selected room name, floor, building, equipment |
| Time Selection | Input | Date, start time, end time (auto-filled from grid selection) |
| Title Input | Text field | Meeting title (required) |
| Description | Textarea | Meeting description/agenda |
| Attendees | Multi-select | Add attendees from company directory |
| Privacy Toggle | Toggle | Public (visible to all) or Private (locked) |
| Equipment Checkboxes | Checkbox group | Video Conference, Projector, Whiteboard, etc. |
| Recurring Options | Dropdown | None, Daily, Weekly, Monthly with end date |
| Save Button | CTA | Confirm booking |

**User Interactions:**
- Fill in meeting details.
- Select privacy level.
- Configure recurring pattern if needed.
- Save to create the booking.

### 2.3 Screen: Booking Detail View
**Route:** `/rooms/bookings/{id}`
**Purpose:** Full booking information and management.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Booking Header | Card | Title, room, date, time, organizer |
| Attendee List | List | All attendees with RSVP status |
| Description | Text | Meeting description/agenda |
| Action Buttons | Button group | Edit, Cancel, Duplicate, Add to Calendar |
| Conflict Warning | Alert | If booking conflicts detected |

**User Interactions:**
- View all booking details.
- Edit booking (if permitted).
- Cancel booking.
- Duplicate booking for similar meetings.
- Export to external calendar (Google, Outlook).

### 2.4 Screen: My Bookings List
**Route:** `/rooms/my-bookings`
**Purpose:** List of all bookings created by or involving the current user.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Filter Tabs | Tabs | Upcoming, Past, Cancelled, All |
| Booking Cards | Card list | Date, time, room, title, status |
| Search | Input | Search by title or room |
| Calendar Export | Button | Export to external calendar |

### 2.5 Screen: Room Management Settings
**Route:** `/rooms/settings`
**Purpose:** Configure buildings, floors, rooms, and equipment.

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Building List | Table | Building name, floors, rooms, actions |
| Floor List | Table | Floor name, rooms, actions |
| Room Form | Form | Room name, floor, capacity, equipment, photo |
| Equipment Tags | Multi-tag | Video Conference, Projector, Whiteboard, Phone, Mobile |
| Booking Rules | Form | Max booking duration, advance booking window, cancellation policy |
| Time Slot Config | Input | Start time (08:00), end time (17:00), slot duration (30min) |

**User Interactions:**
- Add/edit/delete buildings, floors, and rooms.
- Configure room equipment and capacity.
- Set booking rules and time slot configuration.

### 2.6 Screen: Room Utilization Dashboard
**Route:** `/rooms/analytics`
**Purpose:** Analytics on room booking patterns and utilization (img_60).

**UI Components:**
| Component | Type | Description |
|-----------|------|-------------|
| Summary Cards | Metrics | Total bookings today, utilization rate %, most booked room, conflicts |
| Utilization Chart | Bar chart | Room utilization by hour |
| Popular Rooms | Ranking | Most frequently booked rooms |
| Booking Trends | Line chart | Weekly/monthly booking volume |

**Data Displayed:**
- 3 new meetings (from img_60)
- Room utilization percentages
- Peak booking hours
- Department booking patterns

---

## 3. Feature Specifications

### 3.1 Feature: Calendar Grid View
**Description:** Visual calendar showing room availability across multiple rooms with time slots (img_16).

**User Stories:**
- As an employee, I want to see all room bookings for a specific date in a grid, so that I can quickly find available rooms.
- As an employee, I want to see which rooms have video conference equipment, so that I can book the right room for remote meetings.

**Acceptance Criteria:**
- Given a user selects a date and building
- When the calendar grid loads
- Then it shows time slots from 08:00-17:00 with booked and available slots for each room

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Time Slot Duration | 15, 30, or 60 minutes | "Invalid time slot duration" |
| Display Hours | Configurable start/end | N/A |

**Edge Cases:**
- Bookings spanning across the display range (before 08:00 or after 17:00) are shown with overflow indicators.
- Private bookings show as "Locked" to unauthorized users.

### 3.2 Feature: Booking Creation and Conflict Detection
**Description:** Create bookings with automatic conflict detection.

**User Stories:**
- As an employee, I want to book a room by clicking on the calendar, so that the process is intuitive.
- As a system, I want to prevent double-booking, so that there are no scheduling conflicts.

**Acceptance Criteria:**
- Given a user selects room Paris from 10:00-11:30
- When they submit the booking
- Then the booking is created if no conflict exists, or an error is shown if the slot is already taken

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Title | Required, max 200 chars | "Meeting title is required" |
| Start Time | Must be in the future | "Cannot book rooms in the past" |
| End Time | Must be after start time | "End time must be after start time" |
| Max Duration | 4 hours (configurable) | "Booking exceeds maximum duration" |
| Advance Window | Max 30 days in advance | "Booking too far in advance" |

**Edge Cases:**
- Concurrent booking attempts are serialized via database locking.
- If a conflict is detected after the user starts creating, they are shown available alternative rooms.

### 3.3 Feature: Recurring Bookings
**Description:** Book rooms on a recurring schedule (daily, weekly, monthly).

**User Stories:**
- As a team lead, I want to book a room every Monday at 10:00 for the next 3 months, so that I don't have to book manually each week.

**Acceptance Criteria:**
- Given a user creates a weekly recurring booking
- When they set the end date to 3 months from now
- Then individual booking instances are created for each week until the end date

**Validation Rules:**
| Field | Rule | Error Message |
|-------|------|---------------|
| Recurrence End | Max 12 months | "Recurring booking cannot exceed 12 months" |
| Max Occurrences | 52 occurrences | "Too many recurring instances" |

**Edge Cases:**
- If a specific instance conflicts (e.g., room unavailable on a particular date), that instance is skipped with notification.
- Holidays can be excluded from recurring bookings.

### 3.4 Feature: Private Meeting Mode
**Description:** Mark bookings as private, hiding details from non-participants.

**User Stories:**
- As an HR manager, I want to book a room for a sensitive discussion and hide the details, so that other employees don't see confidential meeting topics.

**Acceptance Criteria:**
- Given a booking is marked as private
- When a non-participant views the calendar
- Then the time slot shows as "Locked" without title or attendee details

**Edge Cases:**
- Room admins can always see private booking details.
- Attendees of private meetings can see the details.

### 3.5 Feature: Room Equipment Tracking
**Description:** Track and display room equipment for booking decisions.

**User Stories:**
- As an employee, I want to see which rooms have video conference equipment, so that I can book the right room for my remote meeting.

**Acceptance Criteria:**
- Given rooms are configured with equipment
- When a user views the room list
- Then equipment icons are displayed next to room names

**Equipment Types:**
- Video Conference
- Projector
- Whiteboard
- Phone
- Mobile

### 3.6 Feature: Favorite Rooms
**Description:** Pin frequently used rooms for quick access.

**User Stories:**
- As a regular user of the Paris room, I want to favorite it, so that it appears at the top of my navigation.

**Acceptance Criteria:**
- Given a user favorites the Paris room
- When they view the left navigation
- Then Paris appears under "Favorite rooms" at the top

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+----------------+       +----------------+       +----------------+
|    Building    |       |     Floor      |       |   MeetingRoom  |
+----------------+       +----------------+       +----------------+
| id (PK)        |<----->| id (PK)        |<----->| id (PK)        |
| name           |       | building_id(FK)|       | floor_id (FK)  |
| address        |       | name           |       | name           |
| created_at     |       | sort_order     |       | capacity       |
+----------------+       +----------------+       | equipment      |
                                                  | created_at     |
                                                  +----------------+
                                                         |
                                                  +----------------+
                                                  |  RoomBooking   |
                                                  +----------------+
                                                  | id (PK)        |
                                                  | room_id (FK)   |
                                                  | organizer(FK)  |
                                                  | title          |
                                                  | description    |
                                                  | start_time     |
                                                  | end_time       |
                                                  | is_private     |
                                                  | recurrence_id  |
                                                  | created_at     |
                                                  +----------------+
```

### 4.2 Table Schemas

#### Table: buildings
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL | Building name (e.g., "N03 T1") |
| address | VARCHAR(500) | NULL | Building address |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_buildings_name ON buildings(name);
```

#### Table: floors
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| building_id | BIGINT | NOT NULL, FK -> buildings.id | Parent building |
| name | VARCHAR(100) | NOT NULL | Floor name (e.g., "Floor 2", "Mezzanine") |
| sort_order | INT | NOT NULL, DEFAULT 0 | Display order |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_floors_building_id ON floors(building_id);
CREATE UNIQUE INDEX idx_floors_building_name ON floors(building_id, name);
```

#### Table: meeting_rooms
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| floor_id | BIGINT | NOT NULL, FK -> floors.id | Parent floor |
| name | VARCHAR(100) | NOT NULL | Room name (e.g., "Berlin", "Paris") |
| capacity | INT | NULL | Seating capacity |
| equipment | JSON | NULL | Equipment list (video_conference, projector, etc.) |
| photo_url | VARCHAR(500) | NULL | Room photo URL |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| sort_order | INT | NOT NULL, DEFAULT 0 | Display order |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |

**Indexes:**
```sql
CREATE INDEX idx_meeting_rooms_floor_id ON meeting_rooms(floor_id);
CREATE UNIQUE INDEX idx_meeting_rooms_floor_name ON meeting_rooms(floor_id, name);
```

#### Table: room_bookings
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| room_id | BIGINT | NOT NULL, FK -> meeting_rooms.id | Booked room |
| organizer_id | BIGINT | NOT NULL, FK -> users.id | Booking organizer |
| title | VARCHAR(300) | NOT NULL | Meeting title |
| description | TEXT | NULL | Meeting description |
| start_time | TIMESTAMP | NOT NULL | Booking start |
| end_time | TIMESTAMP | NOT NULL | Booking end |
| is_private | BOOLEAN | NOT NULL, DEFAULT FALSE | Privacy flag |
| recurrence_id | BIGINT | NULL, FK -> recurring_bookings.id | Recurring series |
| status | ENUM('confirmed','cancelled','completed') | NOT NULL, DEFAULT 'confirmed' | Booking status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW(), ON UPDATE | Last update |
| deleted_at | TIMESTAMP | NULL, DEFAULT NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_room_bookings_room_id ON room_bookings(room_id);
CREATE INDEX idx_room_bookings_organizer_id ON room_bookings(organizer_id);
CREATE INDEX idx_room_bookings_start_time ON room_bookings(start_time);
CREATE INDEX idx_room_bookings_status ON room_bookings(status);
CREATE INDEX idx_room_bookings_time_range ON room_bookings(room_id, start_time, end_time);
```

#### Table: room_schedules
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| booking_id | BIGINT | NOT NULL, FK -> room_bookings.id | Parent booking |
| user_id | BIGINT | NOT NULL, FK -> users.id | Attendee |
| rsvp_status | ENUM('pending','accepted','declined','no_response') | NOT NULL, DEFAULT 'pending' | RSVP status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_room_schedules_booking_id ON room_schedules(booking_id);
CREATE INDEX idx_room_schedules_user_id ON room_schedules(user_id);
CREATE UNIQUE INDEX idx_room_schedules_booking_user ON room_schedules(booking_id, user_id);
```

#### Table: recurring_bookings
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| room_id | BIGINT | NOT NULL, FK -> meeting_rooms.id | Target room |
| organizer_id | BIGINT | NOT NULL, FK -> users.id | Organizer |
| title | VARCHAR(300) | NOT NULL | Series title |
| recurrence_pattern | ENUM('daily','weekly','monthly') | NOT NULL | Recurrence type |
| recurrence_interval | INT | NOT NULL, DEFAULT 1 | Interval (every N days/weeks/months) |
| day_of_week | INT | NULL | Day of week for weekly (0=Sun, 6=Sat) |
| day_of_month | INT | NULL | Day of month for monthly |
| start_date | DATE | NOT NULL | Series start date |
| end_date | DATE | NULL, DEFAULT NULL | Series end date |
| max_occurrences | INT | NULL, DEFAULT NULL | Maximum number of occurrences |
| exclude_dates | JSON | NULL | Specific dates to skip |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Record creation |

**Indexes:**
```sql
CREATE INDEX idx_recurring_bookings_room_id ON recurring_bookings(room_id);
CREATE INDEX idx_recurring_bookings_organizer ON recurring_bookings(organizer_id);
CREATE INDEX idx_recurring_bookings_active ON recurring_bookings(is_active, end_date);
```

#### Table: room_equipment
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| name | VARCHAR(100) | NOT NULL, UNIQUE | Equipment name |
| icon | VARCHAR(50) | NULL | Icon identifier |
| description | VARCHAR(200) | NULL | Description |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Active status |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_room_equipment_name ON room_equipment(name);
```

#### Table: booking_conflicts
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| booking_id | BIGINT | NOT NULL, FK -> room_bookings.id | Conflicting booking |
| conflicting_booking_id | BIGINT | NOT NULL, FK -> room_bookings.id | The other booking |
| conflict_type | ENUM('time_overlap','equipment_conflict','capacity_exceeded') | NOT NULL | Conflict type |
| resolved_at | TIMESTAMP | NULL, DEFAULT NULL | Resolution timestamp |
| resolution | ENUM('cancelled','moved','merged') | NULL | Resolution method |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Detection timestamp |

**Indexes:**
```sql
CREATE INDEX idx_booking_conflicts_booking ON booking_conflicts(booking_id);
```

#### Table: user_favorite_rooms
| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Primary key |
| user_id | BIGINT | NOT NULL, FK -> users.id | User |
| room_id | BIGINT | NOT NULL, FK -> meeting_rooms.id | Favorite room |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Favorite timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_user_fav_rooms_user_room ON user_favorite_rooms(user_id, room_id);
CREATE INDEX idx_user_fav_rooms_user_id ON user_favorite_rooms(user_id);
```

### 4.3 All Tables Summary
| Table | Purpose | Row Estimate |
|-------|---------|-------------|
| buildings | Building definitions | 10 |
| floors | Floor definitions | 50 |
| meeting_rooms | Room definitions | 200 |
| room_bookings | Individual booking records | 50,000 |
| room_schedules | Attendee RSVP tracking | 200,000 |
| recurring_bookings | Recurring booking series | 2,000 |
| room_equipment | Equipment type definitions | 20 |
| booking_conflicts | Conflict detection log | 5,000 |
| user_favorite_rooms | User room favorites | 5,000 |

---

## 5. API Specifications

### 5.1 Room Endpoints

#### GET /api/v1/rooms
**Description:** List rooms with building/floor hierarchy

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| building_id | bigint | No | Filter by building |
| floor_id | bigint | No | Filter by floor |
| equipment | string | No | Filter by equipment type |
| capacity_min | int | No | Minimum capacity |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "name": "Berlin",
      "floor": {"id": 2, "name": "Floor 2", "building": {"id": 1, "name": "N03 T1"}},
      "capacity": 10,
      "equipment": ["whiteboard", "phone"],
      "is_active": true
    },
    {
      "id": 4,
      "name": "Paris",
      "floor": {"id": 2, "name": "Floor 2", "building": {"id": 1, "name": "N03 T1"}},
      "capacity": 15,
      "equipment": ["video_conference", "projector"],
      "is_active": true
    }
  ]
}
```

**Permission:** `room:read`

### 5.2 Booking Endpoints

#### GET /api/v1/rooms/{room_id}/bookings
**Description:** Get bookings for a room on a specific date

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date | date | Yes | Date to query |
| include_private | boolean | No | Include private bookings (admin only) |

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": 1,
      "room_id": 1,
      "title": "PQA_Seminar CMMI 2.0 (GOV)",
      "organizer": {"id": 10, "name": "Tran Trung"},
      "start_time": "2019-08-22T09:30:00Z",
      "end_time": "2019-08-22T10:30:00Z",
      "is_private": false,
      "status": "confirmed"
    },
    {
      "id": 2,
      "room_id": 3,
      "title": "[Private Meeting]",
      "start_time": "2019-08-22T08:30:00Z",
      "end_time": "2019-08-22T10:30:00Z",
      "is_private": true,
      "status": "confirmed"
    }
  ]
}
```

**Permission:** `room:booking:read`

#### POST /api/v1/bookings
**Description:** Create a new room booking

**Request Body:**
```json
{
  "room_id": 4,
  "title": "BA training for DEV",
  "description": "Training session for development team",
  "start_time": "2024-04-15T10:00:00Z",
  "end_time": "2024-04-15T11:30:00Z",
  "is_private": false,
  "attendee_ids": [5, 8, 12, 15],
  "recurrence": {
    "pattern": "weekly",
    "interval": 1,
    "day_of_week": 1,
    "end_date": "2024-07-15"
  }
}
```

**Response (201 Created):**
```json
{
  "id": 101,
  "title": "BA training for DEV",
  "room": {"id": 4, "name": "Paris"},
  "start_time": "2024-04-15T10:00:00Z",
  "status": "confirmed",
  "occurrences_created": 13
}
```

**Permission:** `room:booking:create`

#### PUT /api/v1/bookings/{id}
**Description:** Update a booking

**Permission:** `room:booking:update`

#### DELETE /api/v1/bookings/{id}
**Description:** Cancel a booking

**Permission:** `room:booking:cancel`

### 5.3 Calendar Grid Endpoint

#### GET /api/v1/calendar/grid
**Description:** Get calendar grid data for a specific date

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| date | date | Yes | Target date |
| building_id | bigint | No | Filter by building |
| floor_id | bigint | No | Filter by floor |
| room_ids | string | No | Comma-separated room IDs |

**Response (200 OK):**
```json
{
  "date": "2019-08-22",
  "time_slots": ["08:00", "08:30", "09:00", "09:30", "10:00", "...", "17:00"],
  "rooms": [
    {
      "id": 1,
      "name": "Berlin",
      "floor": "Floor 2",
      "bookings": [
        {"id": 1, "title": "PQA_Seminar CMMI 2.0 (GOV)", "start": "09:30", "end": "10:30", "organizer": "Tran Trung"},
        {"id": 6, "title": "Employee evaluation", "start": "16:30", "end": "17:30", "organizer": "HR Manager"}
      ]
    },
    {
      "id": 7,
      "name": "Sofia",
      "floor": "Floor 2",
      "bookings": [
        {"id": 3, "title": null, "start": "08:30", "end": "10:30", "is_private": true}
      ]
    }
  ]
}
```

**Permission:** `room:calendar:read`

### 5.4 All Endpoints Summary
| Method | Endpoint | Description | Permission |
|--------|----------|-------------|------------|
| GET | /api/v1/rooms | List rooms | room:read |
| GET | /api/v1/rooms/{id}/bookings | Room bookings | room:booking:read |
| POST | /api/v1/bookings | Create booking | room:booking:create |
| PUT | /api/v1/bookings/{id} | Update booking | room:booking:update |
| DELETE | /api/v1/bookings/{id} | Cancel booking | room:booking:cancel |
| GET | /api/v1/calendar/grid | Calendar grid | room:calendar:read |

---

## 6. Permissions Matrix

### Roles
| Role | Code | Description |
|------|------|-------------|
| Employee | employee | Book and view rooms |
| Room Admin | room_admin | Manage room configuration |
| Facility Manager | facility_manager | Full booking and analytics access |
| System Admin | sys_admin | Full system access |

### Permission Matrix
| Role | View Rooms | Book Rooms | Cancel Own | Cancel Any | Manage Rooms | View Analytics | Manage Buildings |
|------|-----------|-----------|-----------|-----------|-------------|---------------|-----------------|
| Employee | ✅ | ✅ | ✅ (15min before) | ❌ | ❌ | ❌ | ❌ |
| Room Admin | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Facility Manager | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| System Admin | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

### Row-Level Security
- Private bookings only visible to organizer, attendees, and admins.
- Users can only cancel their own bookings within the allowed window.

---

## 7. State Machine / Workflows

### 7.1 Booking Status Transitions

```
[CONFIRMED] ──start──→ [IN_PROGRESS] ──end──→ [COMPLETED]
     │
     └──cancel──→ [CANCELLED]
```

| From | To | Action | Who | Side Effects |
|------|-----|--------|-----|-------------|
| CONFIRMED | IN_PROGRESS | auto-start | System | Notify attendees |
| IN_PROGRESS | COMPLETED | auto-end | System | Release room, log utilization |
| CONFIRMED | CANCELLED | cancel | Organizer (before start) or Admin | Notify attendees, free time slot |

### 7.2 Recurring Booking Generation

```
[ACTIVE] ──generate-instances──→ [BOOKINGS CREATED] ──end-date-reached──→ [COMPLETED]
     │
     └──cancel-series──→ [CANCELLED]
```

---

## 8. Reports & Analytics

### 8.1 Report: Room Utilization Report
**Type:** Real-time
**Purpose:** Track room booking utilization rates and patterns.

**Dimensions:**
- Room
- Building/Floor
- Time period
- Hour of day

**Metrics:**
- New meetings today: 3
- Utilization rate (%)
- Total bookings per week
- Average booking duration
- Peak hours

**Filters:**
- Date range
- Building
- Floor
- Room

**Chart Type:** Bar chart (utilization by room), Line chart (booking trends), Heat map (hour vs day)

### 8.2 Report: Booking Conflict Analysis
**Type:** Real-time
**Purpose:** Track and analyze booking conflicts.

**Metrics:**
- Total conflicts detected
- Conflicts resolved
- Average resolution time

**Chart Type:** Table

### 8.3 All Reports Summary
| Report | Type | Frequency | Output |
|--------|------|-----------|--------|
| Room Utilization | Real-time | On-demand | Bar chart, heat map |
| Booking Trends | Real-time | On-demand | Line chart |
| Conflict Analysis | Real-time | On-demand | Table |
| Department Usage | Real-time | On-demand | Bar chart |

---

## 9. Integration Points

### 9.1 External APIs Called
| API | Purpose | Method | Auth |
|-----|---------|--------|------|
| Email Service (SMTP) | Booking confirmations, reminders | POST | API Key |
| Calendar Export (Google/Outlook) | Export bookings to external calendars | REST | OAuth2 |
| M01 Notification Service | Push notifications for bookings | REST | JWT |
| M20 Chat Service | Post room availability notifications | REST | JWT |
| M39 Dashboard Service | Feed utilization statistics | REST | JWT |

### 9.2 Webhooks Exposed
| Event | Payload | Subscribers |
|-------|---------|-------------|
| room.booking.created | {booking_id, room_id, organizer_id, start_time, end_time} | M20 Chat, M39 Dashboard |
| room.booking.cancelled | {booking_id, room_id, cancelled_by} | M20 Chat |
| room.conflict.detected | {booking_id, conflicting_id, conflict_type} | M06 Task Management |
| room.utilization.updated | {room_id, utilization_rate, date} | M39 Dashboard |

### 9.3 Events Consumed
| Event | Source | Action |
|-------|--------|--------|
| employee.departed | M01 Auth Service | Cancel all future bookings for departed employee |
| department.restructured | M03 Org Service | Update room access permissions |

---

## 10. Technical Notes

### Performance
- Expected data volume: 50,000+ bookings per year, 200+ rooms
- Query complexity: Medium (calendar grid requires time-range overlap queries)
- Expected response time: <200ms for calendar grid, <100ms for single room bookings
- Conflict detection uses efficient time-range overlap query: `(start1 < end2) AND (end1 > start2)`

### Caching Strategy
- Calendar grid data cached with 1-minute TTL per date/building combination
- Room definitions cached with 1-hour TTL
- Availability status cached with 30-second TTL
- Cache invalidation on booking creation, cancellation, or update

### File Attachments
- Room photos: jpg, png, max 5MB
- Meeting attachments (agenda, materials): pdf, docx, max 10MB
- Storage: S3-compatible object storage

### Localization
- Supported languages: Vietnamese (primary), English
- Time format: HH:mm (24-hour)
- Date format: DD/MM/YYYY
- Room names in English (city names), floor names bilingual

### Mobile Considerations
- Mobile-optimized calendar grid with horizontal scroll for rooms
- Touch-friendly booking creation (tap to select time slot)
- Push notifications for booking reminders (15 min before)
- Quick booking from mobile with minimal form fields
- Room photos displayed for easier identification
- Mobile app icon visible in marketplace (image_06, image_07)

### Conflict Detection Algorithm
- Uses time-range overlap detection: two bookings conflict if `booking1.start < booking2.end AND booking1.end > booking2.start`
- Database-level constraint with exclusion range: `EXCLUDE USING gist (room_id WITH =, tsrange(start_time, end_time) WITH &&)`
- Application-level validation before database insert for better error messages

### Reminder System
- Automated reminders sent 15 minutes and 1 hour before meeting
- Reminders delivered via in-app notification, email, and push notification
- Configurable reminder preferences per user
