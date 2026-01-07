# Praxis Vipassana Meditation Tracker

A Flutter mobile application for tracking daily Vipassana meditation practice. The app helps users maintain a consistent meditation schedule by tracking morning and evening sessions, providing meditation timers with audio support, and maintaining a calendar view of practice history.

## Features

- Daily meditation tracking for morning and evening sessions
- Customizable meditation timer with multiple audio options
- Calendar view to visualize meditation history
- Local notifications for daily reminders
- Feed/messages system for motivational content
- Settings management for customizing meditation times and preferences
- Background timer persistence with wakelock support
- Guided meditation audio playback

## Technical Architecture

### Local Storage with Hive Database

This application uses Hive, a lightweight NoSQL database, for all local data storage. All data is stored locally on the device and no data is transmitted to external servers.

The app initializes three Hive boxes at startup:

1. **meditation box**: Stores daily meditation session data

   - Tracks completion status for morning and evening sessions
   - Records session duration in seconds
   - Stores start and end times for each session
   - Maintains completion timestamps
   - Uses ISO8601 date strings as keys for daily entries
   - Automatically resets day-specific times when a new day is detected

2. **settings box**: Stores user preferences and configuration

   - Morning and evening meditation time windows (start/end hours)
   - Timer sound selection
   - Timer background preference
   - Notification enable/disable setting

3. **messages box**: Stores motivational messages and feed content
   - Welcome message initialization
   - User-generated or system messages

### Data Structure

Each meditation day entry in the `meditation` box contains:

- `morningCompleted`: Boolean indicating if morning session was completed
- `eveningCompleted`: Boolean indicating if evening session was completed
- `morningDuration`: Duration in seconds for morning session
- `eveningDuration`: Duration in seconds for evening session
- `morningCompletionTime`: ISO8601 timestamp of morning completion
- `eveningCompletionTime`: ISO8601 timestamp of evening completion
- `morningStartTime`: Start time for morning session
- `morningEndTime`: End time for morning session
- `eveningStartTime`: Start time for evening session
- `eveningEndTime`: End time for evening session

### State Management

The app uses Riverpod for state management, with a `MeditationNotifier` that syncs state with Hive boxes. The notifier listens for external Hive changes and automatically updates the UI when data is modified.

### Day Reset Logic

The app includes automatic day detection that resets day-specific timing data when a new day begins. This ensures that start and end times are cleared each day while preserving completion status and duration data.

## Prerequisites

- Flutter SDK 3.9.0 or higher
- Dart SDK 3.9.0 or higher
- iOS 13.0+ (for iOS builds)
- Android SDK 21+ (for Android builds)
- Xcode (for iOS development)
- Android Studio or VS Code with Flutter extensions

## Installation and Setup

### Clone the Repository

```bash
git clone https://github.com/ZinMK/Praxis-Vipassana-Meditation-Tracker.git
cd Praxis-Vipassana-Meditation-Tracker
```

### Install Dependencies

```bash
flutter pub get
```

### iOS Setup

If building for iOS, navigate to the iOS directory and install CocoaPods dependencies:

```bash
cd ios
pod install
cd ..
```

### Run the Application

For iOS Simulator:

```bash
flutter run
```

For Android Emulator:

```bash
flutter run
```

For a specific device:

```bash
flutter devices
flutter run -d <device-id>
```

### Build for Release

For iOS:

```bash
flutter build ios
```

For Android:

```bash
flutter build apk
```

## Project Structure

```
lib/
├── main.dart                 # App entry point and initialization
├── HiveDb.dart              # Meditation data storage operations
├── SettingsHive.dart        # Settings storage operations
├── HiveMessages.dart        # Messages/feed storage operations
├── TimerPage.dart           # Meditation timer interface
├── calendar.dart            # Calendar view for history
├── feed.dart                # Feed/messages interface
├── Settings.dart            # Settings UI
├── navigationBar.dart       # Bottom navigation
├── Provider/
│   └── meditation_provider.dart  # Riverpod state management
└── services/
    └── notification_service.dart  # Local notifications
```

## Key Dependencies

- `hive` & `hive_flutter`: Local NoSQL database
- `flutter_riverpod`: State management
- `table_calendar`: Calendar widget for meditation history
- `flutter_local_notifications`: Daily reminder notifications
- `audioplayers`: Audio playback for timer sounds and guided meditation
- `wakelock_plus`: Prevents screen from sleeping during meditation
- `google_fonts`: Custom typography
- `path_provider`: File system paths for Hive storage

## Data Persistence

All data is stored locally using Hive. The database files are created in the app's document directory on each platform:

- iOS: App's Documents directory
- Android: App's internal storage

Data persists across app restarts and is only cleared when the app is uninstalled. No cloud sync or external data transmission occurs.

## Development Notes

- The app requires Flutter 3.9.0-dev or higher due to dependency requirements
- Hive boxes are initialized synchronously at app startup
- Timer state persists across app backgrounding using wakelock
- Notifications are scheduled based on user-configured time windows

## Credits

Icon Attributions:

- Settings icons created by Freepik from Flaticon
- Message icons created by Amazona Adorada from Flaticon
- Calendar icons created by Freepik from Flaticon

## License

This project is private and not intended for public distribution.
