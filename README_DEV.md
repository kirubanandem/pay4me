# Pay4Me — Developer Documentation

Welcome to the **Pay4Me** developer guide. This document contains technical setup instructions, architecture overview, and project structure details.

## Project Overview
- **Package:** `com.kirubas.pay4me`
- **Language:** Java (Android)
- **Min SDK:** 24 (Android 7.0)
- **Target SDK:** 34 (Android 14)
- **Architecture:** MVVM (Model-View-ViewModel) + Repository Pattern
- **Threading:** ViewModel with LiveData, WorkManager for background tasks.
- **Security Features:** Device metadata tracking, Mock location detection, Biometric/Credential authentication.
- **Notifications:** Integrated FCM for background and localized system alerts for foreground events.

---

## Technical Setup

### 1. Firebase Configuration
The app relies on Firebase for almost all backend functionality.
1. **Project:** Create a project in [Firebase Console](https://console.firebase.google.com).
2. **Android App:** Register `com.kirubas.pay4me` and download `google-services.json`.
3. **Authentication:** Enable **Email/Password** and **Phone** providers.
4. **Firestore:** Enable in Production mode (Region: `asia-south1`).
5. **Realtime Database:** Enable for presence tracking (Region: `asia-southeast1`).
6. **Security Rules:** 
   - Deploy Firestore rules from `firebase/firestore.rules`.
   - Deploy RTDB rules from `firebase/database.rules.json`.
7. **Indexes:** Deploy composite indexes using `firebase deploy --only firestore:indexes`.
8. **Crashlytics:** Enable in Firebase Console. To receive email alerts, opt-in via "Alerts" settings in the Firebase Console dashboard.

### 2. SMS Alerts (Blaze Plan)
To enable SMS alerts for payment requests:
1. Upgrade Firebase project to the **Blaze (Pay-as-you-go)** plan.
2. Deploy a Cloud Function or Firebase Extension to listen for new documents in the `notifications` collection.
3. Verify the recipient's `smsAlertsEnabled` field in their user profile before sending.

### 2. Cloudinary (Image Hosting)
Used for profile photo uploads.
1. Create an account at [Cloudinary](https://cloudinary.com).
2. Obtain **Cloud Name**, **API Key**, and **API Secret**, and **upload_preset**.
3. The app fetches these from Firebase RTDB (`appConfig/cloudinary`) or uses defaults in `AppConstants.java`.
4. Ensure an "Unsigned Upload Preset" is created in Cloudinary settings.

### 3. Assets & Resources
Manual steps required for a fresh build:
- **Fonts (`res/font/`):** Add `poppins_semibold.ttf`, `inter_regular.ttf`, and `roboto_mono_regular.ttf`.
- **Lottie Animations (`res/raw/`):** Add `lottie_splash.json`, `lottie_onboard_1.json` through `lottie_onboard_4.json`, `lottie_success.json`, and `lottie_empty.json`.
- **Help Content (`res/raw/`):** Add `help_data.json` for the searchable help center.
- **Configuration:** Ensure `app/google-services.json` is present.

---

## Architecture & Structure

### Package Structure
```text
com.kirubas.pay4me/
├── Pay4MeApplication.java   # App initialization, Firebase/Cloudinary config
├── core/                    # Shared logic
│   ├── base/                # BaseActivity, BaseFragment, BaseViewModel
│   ├── constants/           # AppConstants, NotificationConstants
│   ├── session/             # SessionManager (EncryptedSharedPreferences)
│   └── utils/               # Validation, QR, Image, Date utils
├── data/                    # Data Layer
│   ├── model/               # POJO models (User, Request, Message)
│   └── repository/          # Firestore/RTDB logic
├── ui/                      # Presentation Layer (MVVM)
│   ├── auth/                # Login, Register, Forgot Password
│   ├── main/                # Home, Dashboard
│   ├── friends/             # QR pairing, Friends list
│   ├── requests/            # Payment request flows
│   ├── chat/                # Real-time chat inside requests
│   ├── profile/             # Profile management, Account deletion
│   └── help/                # In-app FAQ and support documentation
├── service/                 # FCM Messaging Service
└── worker/                  # WorkManager for request expiry
```

### Key Libraries
- **Firebase:** Auth, Firestore, RTDB, Messaging, Analytics, Crashlytics.
- **Navigation Component:** Single Activity architecture.
- **ViewBinding:** Type-safe layout access.
- **Cloudinary:** Image management.
- **Glide:** Image loading.
- **ZXing:** QR code generation and scanning.
- **Timber:** Enhanced logging.
- **Lottie:** Vector animations.

---

## Build & Release
1. **Gradle Sync:** Ensure all dependencies are resolved.
2. **Lint:** Run `./gradlew lint` to check for issues.
3. **ProGuard:** Enabled in release builds to obfuscate and shrink APK.
4. **Keystore:** Ensure you have the `release-keystore.jks` and matching credentials in `local.properties` for signed builds.

## Free Tier Limitations
- **Phone Auth:** 10 SMS/day (Firebase Spark Plan).
- **Cloudinary:** 25GB storage/month (Free Tier).
- **Expiry Logic:** Handled by a combination of Client-side WorkManager and Server-side Security Rules (since no Cloud Functions are used in the free tier).
