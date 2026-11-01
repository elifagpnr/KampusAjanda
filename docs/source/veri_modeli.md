# Data Model and Firestore Structure

The fields below were derived from the information shown on our screens.
The "Example" column shows sample values only; real values come from users and clubs.

## 1. Data models

### Event
A campus event published by a club.

| Field | Type | Description | Example |
|---|---|---|---|
| id | String | Unique event ID | (auto-generated) |
| title | String | Event name | Flutter Workshop |
| description | String | Short description | Build your first Flutter app |
| clubId | String | ID of the club that created the event | (auto-generated) |
| clubName | String | Club name shown on cards | Software Club |
| posterUrl | String? | Poster image (optional) | – |
| startTime | DateTime | Start date and time | 10 Nov 2026, 12:15 |
| endTime | DateTime | End date and time | 10 Nov 2026, 13:30 |
| location | String | Place of the event | KMF B-201 |
| tags | List<String> | Topics used for matching | Technology, Career |
| capacity | int | Maximum number of attendees | 40 |
| attendeeCount | int | Current number of attendees | 34 |
| isOutdoor | bool | True if outdoors (weather is shown) | false |

### UserProfile
A student or club representative account.

| Field | Type | Description | Example |
|---|---|---|---|
| uid | String | User ID from Firebase Authentication | (auto-generated) |
| name | String | Full name | Ayşe Yılmaz |
| email | String | School e-mail | name.surname@std.yildiz.edu.tr |
| role | String | Account type: "student" or "club" | student |
| interests | List<String> | Selected interests (at least 3) | Technology, Career, Games |
| reminderMinutes | int | Default reminder time in minutes | 30 |
| clubId | String? | Only for club accounts | – |

### Club
| Field | Type | Description | Example |
|---|---|---|---|
| id | String | Unique club ID | (auto-generated) |
| name | String | Club name | Music Club |
| isVerified | bool | Shown as "Verified club" on the club panel | true |

### CourseSlot
One lecture in the student's weekly schedule.

| Field | Type | Description | Example |
|---|---|---|---|
| day | int | Day of the week (1 = Monday ... 5 = Friday) | 2 |
| startHour | int | Start hour | 9 |
| endHour | int | End hour | 11 |

### FreeSlot
A gap between lectures. **Not stored**; it is calculated from the CourseSlots on the device and used by the matching algorithm.

| Field | Type | Description | Example |
|---|---|---|---|
| day | int | Day of the week | 2 |
| start | DateTime | Start of the free time | 12:00 |
| end | DateTime | End of the free time | 14:00 |

## 2. Firestore structure

    users/{uid}                  → UserProfile
      courses/{slotId}           → CourseSlot
      attending/{eventId}        → joinedAt, reminderMinutes
    clubs/{clubId}               → Club
    events/{eventId}             → Event

- **users:** one document per account. The weekly schedule and joined events are stored as sub-collections, so each user only reads their own data.
- **events:** all events are in one collection, so the "For You" screen can listen to it in real time and new events appear instantly.
- **attending:** stores which events the user joined and the reminder time chosen for each one.

## 3. Local storage (Hive)
The schedule and the last loaded events are also saved on the phone with Hive, so the app still opens and shows events when there is no internet connection.
