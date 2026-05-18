# Pay4Me — Developer Documentation

Welcome to the **Pay4Me** developer guide. This document contains technical setup instructions, architecture overview, and project structure details.

## Project Overview
- **Version:** v1.2.7
- **Package:** `com.kirubas.pay4me`
- **Language:** Java (Android 17)
- **Min SDK:** 24 (Android 7.0)
- **Target SDK:** 34 (Android 14)
- **Architecture:** MVVM (Model-View-ViewModel) + Repository Pattern
- **Threading:** ViewModel with LiveData, WorkManager for background tasks.
- **Security Features:** Device metadata tracking, Mock location detection, Biometric/Credential authentication, Secure session self-healing.
- **Stability & Security Fixes (v1.2.7):**
  - **Splash Update Check:** Integrated `UpdateManager` into `SplashActivity` with callbacks, ensuring critical updates are identified before user session starts.
  - **Silent Listener Service:** Set `NotificationListenerService` importance to `MIN` and renamed notification to "System Sync" to comply with "unobtrusive background" requirements.
  - **Crashlytics Reporting:** Added `FirebaseCrashlytics.recordException` for all OTP lifecycle events (Send, Link, Update) and Firestore transaction failures.
  - **Share Logic Refactor:** Updated `MainActivity` image sharing to support explicit target collection selection (`requests` vs `pairs`).
  - **Branding Sync:** Migrated primary color to `#38B6FF` and updated `ic_launcher` to adaptive vector-based logo.
- **Stability & Security Fixes (v1.2.4):**
  - **Phone Linking:** Improved verification logic to support updating existing linked phone providers using `updatePhoneNumber`.
  - **Firestore Sync:** Added explicit ID token refresh after linking to prevent "Failed to save" errors in Firestore.
  - **UI Polish:** Resolved layout constraints in the Profile verification banner to ensure the "Verify Now" button is legible.
  - **App Lock Polish:** Prevented biometric prompts from appearing during or immediately after logout by adding Firebase auth state checks to `MainActivity` lifecycle methods.
- **Stability & Security Fixes (v1.2.1):** 
  - **Handshake Integrity:** Hardened `pairingTokens` rules to prevent session snatching. Improved `AddFriendViewModel` to reuse existing `pairId` when re-pairing with the same user, preserving chat history.
  - **Transaction Immutability:** Updated Firestore rules to make `amount` and `upiId` immutable after request creation.
  - **App Lock Resume:** Overhauled `MainActivity.onPause` to reset authentication state, ensuring biometric re-auth is required on app resume.
  - **Deleted Account Guard:** Implemented login-time check for `status: "deleted"` profiles.
  - **Firestore Permissions:** Restricted `list` access on sensitive collections (`phoneIndex`, `pairingTokens`) to prevent unauthorized discovery.
- **Stability Fixes (v1.2.0):** 
  - **Observer Lifecycle Fix:** Overhauled `AddFriendViewModel` with proper observer removal and explicit `LOADING` state handling, resolving race conditions that led to "Profile Load Error" during pairing.
  - **Location Refactor:** Migrated from legacy `LocationManager` to `FusedLocationProviderClient`.
- **API Standards:** Intent handling and Back navigation updated to modern Android 14 requirements (OnBackPressedDispatcher, type-safe getParcelableExtra).
- **Trust-Based Tracking:** UI-level warnings implemented for "Mark as Paid" and "Confirm Receipt" actions. **New:** Historical contact snapshots record verified numbers during transactions.
- **Notifications:** Integrated FCM for background and specialized localized system alerts for foreground events. Includes a dedicated **Notification Center** for alert history.
- **Developer Support:** WhatsApp (+91 9092041238), Email (kirubas102@gmail.com). GitHub: [kirubanandem](https://github.com/kirubanandem)

---

## Technical Setup

### 1. Firebase Configuration
The app relies on Firebase for almost all backend functionality.
1. **Project:** Create a project in [Firebase Console](https://console.firebase.google.com).
2. **Android App:** Register `com.kirubas.pay4me` and download `google-services.json`.
3. **Authentication:** Enable **Email/Password** and **Phone** providers.
4. **Firestore:** Enable in Production mode (Region: `asia-south1`).
5. **Realtime Database:** Enable for presence tracking. **IMPORTANT:** The app is hardcoded to use the Singapore region (`asia-southeast1`). Ensure your RTDB instance matches this or update `AppConstants.FIREBASE_RTDB_URL`.
6. **Security Rules:** 
   - Deploy Firestore rules from `firebase/firestore.rules`.
   - Deploy RTDB rules from `firebase/database.rules.json`.
7. **Indexes:** Deploy composite indexes using `firebase deploy --only firestore:indexes`.

### 2. High-Priority Push Alerts
The app uses specialized notification channels for immediate attention:
1. **Notification Channels:** Channels like `pay4me_urgent` and `pay4me_requests` are created with `IMPORTANCE_HIGH`.
2. **Foreground/Minimized:** Handled by `NotificationListenerService`. It uses a silent foreground notification (`IMPORTANCE_MIN`) labeled as "System Sync" to maintain Firestore listeners without distracting the user.
3. **Notification History:** Handled by `NotificationRepository`, allowing users to review delivered alerts later.

### 3. External Image Sharing
The app supports catching images shared from other apps (like UPI transaction receipts):
1. **Manifest:** `MainActivity` is registered with an `<intent-filter>` for `android.intent.action.SEND` with mime-type `image/*`.
2. **Handling:** `MainActivity.handleIntent()` extracts the `Uri`, allows the user to select an active request or a friend, and then uses `RequestDetailViewModel` logic to compress and upload the image.

### 4. Cloudinary (Image Hosting)
Used for profile photo and chat image uploads.
1. Create an account at [Cloudinary](https://cloudinary.com).
2. Obtain **Cloud Name**, **API Key**, and **API Secret**, and **upload_preset**.
3. The app fetches these from Firebase RTDB (`appConfig/cloudinary`) or uses defaults in `AppConstants.java`.

### 5. Assets & Resources
Manual steps required for a fresh build:
- **Fonts (`res/font/`):** Add `poppins_semibold.ttf`, `inter_regular.ttf`, and `roboto_mono_regular.ttf`.
- **Lottie Animations (`res/raw/`):** Add `lottie_splash.json`, `lottie_onboard_1.json` through `lottie_onboard_4.json`, `lottie_success.json`, and `lottie_empty.json`.
- **Help Content (`res/raw/help_content`):** A JSON file defining the FAQ sections and topics.
- **Configuration:** Ensure `app/google-services.json` is present.

---

## Architecture & Structure

### Package Structure
```text
com.kirubas.pay4me/
├── Pay4MeApplication.java   # App initialization, Notification Channel registration
├── core/                    # Shared logic
│   ├── base/                # BaseActivity, BaseFragment (Safe UI handling)
│   ├── constants/           # AppConstants, FirestoreConstants
│   ├── session/             # SessionManager (Session persistence)
│   └── utils/               # Validation, QR, Image, Date utils
├── data/                    # Data Layer
│   ├── local/               # Local persistence (EncryptedSharedPreferences)
│   ├── model/               # POJO models (User, PaymentRequest, ChatMessage)
│   └── repository/          # Firestore/RTDB/Notification repositories
├── ui/                      # Presentation Layer (MVVM)
│   ├── auth/                # Login, Register, Forgot Password
│   ├── main/                # Dashboard Container, External Intent Handling
│   ├── home/                # Main dashboard content
│   ├── friends/             # QR pairing, Friends list (Live Presence)
│   ├── profile/             # Profile view, Edit Profile, Phone Verification
│   ├── requests/            # Sent payment requests
│   ├── payments/            # Received payment requests
│   ├── requestdetail/       # Private Chat, Image Sharing, Request actions
│   ├── scan/                # UPI QR Scanning logic
│   ├── requestmoney/        # Direct request creation
│   ├── notifications/       # Notification History Center
│   ├── settings/            # App preferences & Security settings
│   ├── help/                # In-app FAQ & Support
│   ├── developerstore/      # App Store page for other developer projects
│   ├── common/              # Fullscreen images, cropping
│   ├── splash/              # Animated entry screen
│   └── onboarding/          # Feature introduction slides
├── service/                 # FCM Messaging & Notification Listener
└── worker/                  # WorkManager for request expiry
```

### Key Libraries
- **Firebase:** Auth, Firestore, RTDB, Messaging, Analytics, Crashlytics.
- **Navigation Component:** Single Activity architecture.
- **ViewBinding:** Type-safe layout access.
- **Cloudinary:** Image management and unsigned uploads.
- **Glide:** Image loading with thumbnail optimizations.
- **ZXing:** QR code generation and scanning.
- **Timber:** Enhanced logging.

---

## Build & Release
1. **Gradle Sync:** Ensure all dependencies are resolved.
2. **Keystore:** Ensure you have the `pay4me-keystore.jks` for signed builds.

## Free Tier Limitations
- **Phone Auth:** 10 SMS/day (Firebase Spark Plan).
- **Cloudinary:** 25GB storage/month (Free Tier).
- **Expiry Logic:** Handled via Client-side WorkManager and Server-side Security Rules.
