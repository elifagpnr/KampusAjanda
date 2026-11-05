# Data Model and Firestore Structure

The fields below were derived from the information shown on our final mockups.
The "Example" column shows sample values only; real values come from users and clubs.

## 1. Stored data models

### Event
A campus event published by a club (screens 6, 7, 7.1, 11, 12).

| Field | Type | Description | Example |
|---|---|---|---|
| id | String | Unique event ID | (auto-generated) |
| title | String | Event name | Flutter Workshop |
| description | String | Short description | Build your first Flutter app in 75 minutes |
| clubId | String | ID of the club that created the event | (auto-generated) |
| clubName | String | Club name shown on cards | Software Club |
| posterUrl | String? | Poster image (optional) | – |
| startTime | DateTime | Start date and time | 10 Nov 2026, 12:15 |
| endTime | DateTime | End date and time | 10 Nov 2026, 13:30 |
| location | String | Place of the event | KMF B-201 |
| tags | List<String> | Topics used for matching | Technology, Career |
| capacity | int | Maximum number of attendees | 40 |
| attendeeCount | int | Current number of attendees | 34 |
| isOutdoor | bool | True if outdoors; weather is shown on the detail screen | false |

### UserProfile
Every user registers as a student. A student can also become a club admin with a code (screens 3, 4, 9, 9.1).

| Field | Type | Description | Example |
|---|---|---|---|
| uid | String | User ID from Firebase Authentication | (auto-generated) |
| name | String | Full name | Ayşe Yılmaz |
| email | String | School e-mail | name.surname@std.yildiz.edu.tr |
| interests | List<String> | Selected interests (at least 3) | Technology, Career, Games |
| reminderMinutes | int | Default reminder time in minutes | 1440 (1 day) |
| adminClubId | String? | Club the user manages; empty for normal students | – |

### Club
Screens 10 and 10.1.

| Field | Type | Description | Example |
|---|---|---|---|
| id | String | Unique club ID | (auto-generated) |
| name | String | Club name | Music Club |
| isVerified | bool | Shown as "Verified club" on the club panel | true |
| presidentUid | String | User ID of the club president | (auto-generated) |
| adminUids | List<String> | User IDs of the admins (maximum 3) | 2 users |

### ClubCode
The 8-character code that SKS (the student affairs office) gives to a club. A student enters it on the Profile screen to become the club's admin (screen 9.1).

| Field | Type | Description | Example |
|---|---|---|---|
| code | String | The code itself | K7QM4XPA |
| clubId | String | Club the code belongs to | (auto-generated) |
| isUsed | bool | A code can be used only once | false |
| expiresAt | DateTime | The code is valid for 30 days | 5 Dec 2026 |

### CourseSlot
One lecture in the student's weekly schedule (screens 5 and 8).

| Field | Type | Description | Example |
|---|---|---|---|
| day | int | Day of the week (1 = Monday ... 5 = Friday) | 2 |
| startHour | int | Start hour | 9 |
| endHour | int | End hour | 11 |

### Attendance
Stored when a student taps "Katılıyorum" (screens 7 and 8).

| Field | Type | Description | Example |
|---|---|---|---|
| eventId | String | The joined event | (auto-generated) |
| joinedAt | DateTime | When the student joined | 5 Nov 2026, 14:20 |
| reminderOn | bool | The "1 day before" reminder switch | true |

## 2. Data that is not stored

### FreeSlot
A gap between lectures. Calculated from the CourseSlots on the device and used by the matching algorithm (strip on screen 6).

| Field | Type | Description | Example |
|---|---|---|---|
| day | int | Day of the week | 2 |
| start | DateTime | Start of the free time | 12:00 |
| end | DateTime | End of the free time | 14:00 |

### WeatherInfo
Fetched from the Open-Meteo API (REST/JSON) only for outdoor events (screen 7.1). Not stored, because the forecast changes.

| Field | Type | Description | Example |
|---|---|---|---|
| temperature | double | Temperature at the event time (°C) | 16 |
| rainProbability | int | Chance of rain (%) | 10 |

### Calculated values shown on screens
| Screen | Value | Calculated from |
|---|---|---|
| 6, 7 | Match score (%92) and "Why recommended?" | FreeSlots + interests + attendeeCount |
| 9 | Joined events (12) | Number of Attendance records |
| 9 | Free time this week (14 h) | Sum of FreeSlots |
| 10 | Active events (3), total attendance (248) | The club's events |
| 11 | Estimated reach (410 students) | Students who are free at that time and share a tag |

## 3. Firestore structure

    users/{uid}                  → UserProfile
      courses/{slotId}           → CourseSlot
      attending/{eventId}        → Attendance
    clubs/{clubId}               → Club
    clubCodes/{code}             → ClubCode
    events/{eventId}             → Event

- **users:** one document per account. The weekly schedule and joined events are stored as sub-collections, so each user only reads their own data.
- **events:** all events are in one collection, so the "For You" screen can listen to it in real time and new events appear instantly.
- **clubCodes:** the code is the document ID, so the app can check a code with a single read.

## 4. Design decisions
- **No separate club accounts:** club admins log in with their own student account. When the president changes, the new president is added as an admin and no password is shared.
- **Default reminder is 1 day:** in our survey, 79% of students preferred a reminder one day before the event.
- **Calculated values are not stored:** scores, counts and free time are calculated from existing data, so they never get out of sync.

## 5. Local storage (Hive)
The schedule and the last loaded events are also saved on the phone with Hive, so the app still opens and shows events when there is no internet connection.
