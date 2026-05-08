# Pay4Me — Developer Documentation

Welcome to the **Pay4Me** developer guide. This document contains technical setup instructions, architecture overview, and project structure details.

## Project Overview
- **Version:** v1.1.7
- **Package:** `com.kirubas.pay4me`
- **Language:** Java (Android)
- **Min SDK:** 24 (Android 7.0)
- **Target SDK:** 34 (Android 14)
- **Architecture:** MVVM (Model-View-ViewModel) + Repository Pattern
- **Threading:** ViewModel with LiveData, WorkManager for background tasks.
- **Security Features:** Device metadata tracking, Mock location detection, Biometric/Credential authentication, Secure session self-healing.
- **Stability Fixes (v1.1.7):** Optimized LiveData observer management in `MainActivity` to prevent race conditions and duplicate message broadcasts during external intent handling.
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
2. **Foreground/Minimized:** Handled by `NotificationListenerService`. It uses a persistent foreground notification. For Android 14+, it is swipeable; for older versions, it includes a "Stop" button.
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
│   ├── constants/           # AppConstants, NotificationConstants
│   ├── session/             # SessionManager (Session persistence)
│   └── utils/               # Validation, QR, Image, Date utils (Calendar-based)
├── data/                    # Data Layer
│   ├── model/               # POJO models (User, PaymentRequest, ChatMessage)
│   └── repository/          # Firestore/RTDB logic (Caching in UserRepository)
├── ui/                      # Presentation Layer (MVVM)
│   ├── auth/                # Login, Register (+91 preset), Forgot Password
│   ├── main/                # Home, External Intent Handling
│   ├── friends/             # QR pairing, Friends list (Live Presence)
│   ├── requestdetail/       # Private Chat, Typing Indicators, Image Sharing
│   ├── payments/            # Received requests management
│   ├── notifications/       # Dedicated notification history & management
│   └── help/                # In-app FAQ and support documentation
├── service/                 # Background & Messaging Services
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
