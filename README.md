# SPM Project

A production-grade Flutter application that integrates secure authentication, real-time video conferencing, voice management, and an educational quiz module. The project follows a modular architecture with dependency injection, robust routing, and clear separation of concerns to enable maintainability and scalability.

- Completed branch: int_1
- Primary platform target: Android (Flutter cross-platform ready)

## Goals

- Provide a unified platform for learning and collaboration with:
  - Secure multi-provider authentication (Email/Password, Google, Facebook)
  - Real-time video meetings powered by Agora
  - Voice features (Speech-to-Text, Text-to-Speech, audio recording/playback)
  - Local quiz management with CRUD operations using SQLite
- Ensure clean architecture with DI, feature-based modules, and declarative routing

## Key Use Cases

- Instructors host a class via video conference, share materials, and run quizzes
- Students authenticate via Firebase, join rooms, and participate in quizzes
- Users leverage voice features for accessibility and productivity (record notes, STT/TTS)

## Core Features

- Authentication
  - Email/Password, Google Sign-In, Facebook Login via Firebase Auth
  - Sign-out flows and error-handling
- Video Conference
  - Join/start rooms using Agora RTC Engine
  - UI components for room controls and participant management
- Educational Quiz
  - Create, update, view, and delete multiple-choice questions
  - Persist questions locally via SQLite (sqflite)
- Voice Management
  - Speech-to-Text (speech_to_text)
  - Text-to-Speech (flutter_tts)
  - Audio record/playback (record, audioplayers)
  - Haptic feedback (vibration)
- Utilities
  - PDF generation/open (pdf, open_file)
  - Sharing content (share_plus)
  - Internationalization helpers (intl)
  - HTTP client (http)
  - Permission handling (permission_handler)

## Technologies and Stack

- Flutter SDK with Dart (see [pubspec.yaml](spm_project/pubspec.yaml))
- Firebase: Core, Auth, Firestore
- Social Auth: Google Sign-In, Facebook Auth
- State and Architecture:
  - Dependency Injection: get_it, injectable
  - Code Generation: build_runner, injectable_generator
  - Routing: go_router
  - Optional: flutter_bloc (project-ready)
- Media/Voice: agora_rtc_engine, agora_uikit, speech_to_text, flutter_tts, record, audioplayers, vibration
- Data & Utilities: sqflite, path_provider, path, http, intl, pdf, open_file, share_plus
- Logging: logger

## Architecture Overview

- Entry point and App wiring:
  - [main()](spm_project/lib/main.dart:6) initializes Flutter and Firebase, configures DI, and boots the app router
  - [class MyApp](spm_project/lib/main.dart:13) hosts the MaterialApp.router and binds [GoRouter](spm_project/lib/di/go_router_module.dart)
- Authentication:
  - [abstract class AuthService](spm_project/lib/core/services/auth_service.dart:7)
  - [FirebaseAuthService](spm_project/lib/core/services/auth_service.dart:15) implements email/password, Google, and Facebook login
  - [signInWithGoogle()](spm_project/lib/core/services/auth_service.dart:34), [signInWithFacebook()](spm_project/lib/core/services/auth_service.dart:54), [signOut()](spm_project/lib/core/services/auth_service.dart:102)
- Educational Quiz:
  - [DatabaseHelper](spm_project/lib/features/educational_quiz/database_helper.dart:6) encapsulates SQLite setup and CRUD
  - Feature screens and navigation in [quiz_routes.dart](spm_project/lib/features/educational_quiz/quiz_routes.dart)
- Video Conference:
  - Agora service and screens: [my_agora_service.dart](spm_project/lib/features/videoConference/services/my_agora_service.dart), [video_conf_page.dart](spm_project/lib/features/videoConference/screens/video_conf_page.dart), [start_page.dart](spm_project/lib/features/videoConference/screens/start_page.dart), [room_options_page.dart](spm_project/lib/features/videoConference/screens/room_options_page.dart)
- Voice Management:
  - Controller and routes: [voice_controller.dart](spm_project/lib/features/voice_management/voice_controller.dart), [note_routes.dart](spm_project/lib/features/voice_management/services/note_routes.dart)
- Routing and DI:
  - DI modules: [injectable.dart](spm_project/lib/di/injectable.dart), [injectable.config.dart](spm_project/lib/di/injectable.config.dart), [go_router_module.dart](spm_project/lib/di/go_router_module.dart)
  - Services modules: [agora_module.dart](spm_project/lib/di/agora_module.dart), [firebase_auth_module.dart](spm_project/lib/di/firebase_auth_module.dart), [firestore_module.dart](spm_project/lib/di/firestore_module.dart), [stt_module.dart](spm_project/lib/di/stt_module.dart), [user_service_module.dart](spm_project/lib/di/user_service_module.dart), [logger_module.dart](spm_project/lib/di/logger_module.dart)

## Project Structure

- Entry & config
  - [lib/main.dart](spm_project/lib/main.dart)
  - [lib/firebase_options.dart](spm_project/lib/firebase_options.dart)
- Core
  - Pages: [home_page.dart](spm_project/lib/core/pages/home_page.dart), [login_page.dart](spm_project/lib/core/pages/login_page.dart)
  - Models: [user_model.dart](spm_project/lib/core/models/user_model.dart)
  - Services: [auth_service.dart](spm_project/lib/core/services/auth_service.dart), [route_service.dart](spm_project/lib/core/services/route_service.dart), [speech_to_text_service.dart](spm_project/lib/core/services/speech_to_text_service.dart), [user_service.dart](spm_project/lib/core/services/user_service.dart), [auth_routes.dart](spm_project/lib/core/services/auth_routes.dart)
- Features
  - Educational quiz: [lib/features/educational_quiz/](spm_project/lib/features/educational_quiz/)
  - Video conference: [lib/features/videoConference/](spm_project/lib/features/videoConference/)
  - Voice management: [lib/features/voice_management/](spm_project/lib/features/voice_management/)
- DI modules: [lib/di/](spm_project/lib/di/)
- Assets (images, fonts, sounds): [assets/](spm_project/assets/)

## Setup and Installation

Prerequisites:
- Flutter SDK (stable)
- Dart SDK >= 3.5.1
- A configured Firebase project

Steps:
1) Clone and checkout the completed branch
   ```
   git clone <repo-url>
   cd spm_project
   git checkout int_1
   ```
2) Install dependencies
   ```
   flutter pub get
   ```
3) Generate DI code (injectable)
   ```
   dart run build_runner build --delete-conflicting-outputs
   ```
4) Firebase configuration
   - Ensure FlutterFire is set up and [firebase_options.dart](spm_project/lib/firebase_options.dart) exists
   - Android: keep [google-services.json](spm_project/android/app/google-services.json) in place
   - Confirm [firebase_core](spm_project/pubspec.yaml) and related packages are in dependencies

5) Permissions
   - The app requests microphone/storage permissions for recording, STT/TTS, and conferencing
   - For Android, verify AndroidManifest configurations and runtime grant via permission_handler

## Run

- Development run:
  ```
  flutter run
  ```
- Platform-specific build (example Android):
  ```
  flutter build apk
  ```

## Configuration Notes

- Routing is centralized via GoRouter and injected into the app at startup ([main()](spm_project/lib/main.dart:6), [class MyApp](spm_project/lib/main.dart:13))
- DI uses get_it + injectable; add services to DI modules and regenerate with build_runner
- Ensure audio devices and network permissions are granted for Agora sessions

## Branch Strategy

- The int_1 branch contains the completed and stable implementation
- Use feature branches for new modules and merge into int_1 after review

## License

Proprietary or internal use unless specified otherwise. Update this section based on your organizational policy.

## Acknowledgements

- Flutter team for the framework
- Firebase for backend services
- Agora for reliable real-time engagement SDKs

For dependency versions and assets, refer to [pubspec.yaml](spm_project/pubspec.yaml).
