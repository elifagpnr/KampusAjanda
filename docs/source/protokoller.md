# Protocols and Interfaces

## 1. Protocols and services

| Protocol / service | Purpose in KampüsAjanda | Screens |
|---|---|---|
| **HTTPS + REST/JSON (Open-Meteo API)** | Gets the weather forecast for outdoor events. The app sends a GET request and reads the JSON response. No API key is needed. | 7.1 |
| **Firebase Authentication** | Registration and login with school e-mail and password. Only `@std.yildiz.edu.tr` addresses are accepted. Keeps the user logged in. | 2, 3 |
| **Cloud Firestore** | Cloud database for users, clubs, codes and events. Real-time listeners update the "For You" list instantly when a club publishes an event. | 4–12 |
| **Hive (local database)** | Saves the schedule and the last loaded events on the phone, so the app works offline. | 5, 6, 8 |
| **Local notifications** | Sends the "1 day before" reminder for joined events. For outdoor events, rain is added to the message. | 7, 9 |

### Example: Open-Meteo request
    GET https://api.open-meteo.com/v1/forecast?latitude=41.02&longitude=28.89&hourly=temperature_2m,precipitation_probability&timezone=Europe/Istanbul

The response is JSON. The app takes the values for the event's hour and shows them as "16°C · Rain 10%".

## 2. Repository interfaces
ViewModels use these interfaces and do not know where the data comes from (Firestore, Hive or the API).

    abstract class AuthRepository {
      Future<UserProfile> register(String name, String email, String password);
      Future<UserProfile> login(String email, String password);
      Future<void> logout();
    }

    abstract class EventRepository {
      Stream<List<Event>> watchEvents();
      Future<void> attend(String eventId, bool reminderOn);
      Future<void> createEvent(Event event);
      Future<void> cancelEvent(String eventId);
    }

    abstract class ScheduleRepository {
      Future<List<CourseSlot>> getSchedule();
      Future<void> saveSchedule(List<CourseSlot> slots);
    }

    abstract class ClubRepository {
      Future<Club> activateWithCode(String code);
      Future<void> addAdmin(String clubId, String email);
      Future<void> removeAdmin(String clubId, String uid);
    }

    abstract class WeatherRepository {
      Future<WeatherInfo> getForecast(DateTime time);
    }

## 3. Error handling and offline mode

| Situation | What the user sees |
|---|---|
| No internet | A banner at the top of "For You"; events loaded earlier are shown from Hive |
| Wrong password | An error message under the password field |
| Invalid or used club code | "This code is invalid or has already been used" on screen 9.1 |
| Missing field when creating an event | The field is marked and "Publish" does not continue |
| Weather API does not respond | The weather box is hidden; the rest of the screen works |
