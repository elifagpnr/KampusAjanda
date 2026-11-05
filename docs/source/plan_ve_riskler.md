# Features, Weekly Plan and Risks

## 1. Core features and course topics

| # | Feature | Screens | Course topics |
|---|---|---|---|
| 1 | Login and registration with school e-mail | 1, 2, 3 | Firebase (Authentication), navigation |
| 2 | Onboarding: interests and weekly schedule | 4, 5 | Local DB (Hive), MVVM |
| 3 | "For You" list ranked by free time and interests | 6, 13 | Firebase (Firestore, real-time), MVVM, responsive layout |
| 4 | Event detail, joining and 1-day reminder | 7, 7.1, 8 | Navigation, animation (card → detail), local notifications |
| 5 | Weather for outdoor events | 7.1 | API/JSON (Open-Meteo REST API) |
| 6 | Club panel: activate with code, create events, manage admins | 9.1, 10, 10.1, 11, 12 | Firebase, navigation, form validation and error handling |

## 2. Weekly plan until the final demo (Week 13)
Each member builds their own features end to end (View, ViewModel and Repository), so the workload is equal.

| Week | Elif: login, onboarding, profile | Zeynep: student main flow | Nisa: club flow and infrastructure |
|---|---|---|---|
| 8 | Flutter setup, clone the repository | Theme and shared widgets | Project skeleton (MVVM folders, Riverpod, go_router), Firebase setup, data models |
| 9 | Screens 1–3, Firebase Authentication | Screen 6 (For You), live event list | Screens 10–11, event creation and validation, demo data |
| 10 | Screens 4–5, schedule with Hive, free-time calculation | Screens 7 and 7.1, joining, card → detail animation | Matching algorithm, screen 12 |
| 11 | Screen 9, 1-day reminder notifications | Screen 8 (calendar), Open-Meteo weather | Screens 9.1 and 10.1 (club code, admins) |
| 12 | Offline mode, error messages | Tablet layout (13), visual polish | Firestore security rules, error messages, first APK |
| 13 | Testing on a real phone, README | Demo video (5 min) and backup video (2 min) | Final APK and submission |

Every member commits their own work with their own GitHub account each week.

## 3. Risks and solutions

| Risk | Effect | Solution |
|---|---|---|
| No team member has used Firebase before | Login and data features may be delayed | Firebase setup is done first (Week 8); official FlutterFire guides are followed |
| Midterms and other courses reduce working time | Weekly tasks may slip | Tasks are small and weekly; Week 12 has lighter tasks as a buffer |
| Matching algorithm is more complex than expected | "For You" list may not work well | First version uses a simple score (free time + interests); it is improved later |
| Merge conflicts when three people edit the same files | Lost work | Each member works in their own feature folder; small, frequent commits |
| Not enough real event data for the demo | Empty screens in the demo | We add sample events of real YTÜ clubs to Firestore before the demo |
| Weather API does not respond | Detail screen error | The weather box is hidden; the rest of the screen works |
