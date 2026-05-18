# Pay4Me 💸

**Pay4Me** is a secure, real-time Android application designed to simplify shared payments and peer-to-peer transaction coordination using UPI QR codes.

[![Platform](https://img.shields.io/badge/platform-Android-blue.svg)](https://developer.android.com/about/versions/14)
[![Language](https://img.shields.io/badge/language-Java-orange.svg)](https://www.java.com/)
[![Version](https://img.shields.io/badge/version-1.2.7-skybue.svg)](https://github.com/kirubanandem/pay4me/releases/latest)

## 🚀 Key Features

- **Secure Physical Pairing:** Connect with friends using dual-proximity verification. Pairing requires both **GPS Location** and **Bluetooth BLE** confirmation to ensure you are physically standing next to each other. Remote pairing via screenshots or photos is strictly blocked.
- **UPI QR Forwarding:** Scan any UPI QR code and instantly forward it to a paired friend for payment.
- **Transaction Localization & History:** 
  - Automatically captures GPS coordinates during payment requests.
  - **Historical Contact Snapshots:** Every transaction records the verified mobile numbers of both parties at the time of the request, ensuring a permanent "receipt" even if users change their numbers later.
- **Real-time Chat & Communication:**
  - Private chat room for every payment request.
  - **General Chat:** Start a conversation with any friend directly from their profile.
  - **Typing Indicators:** See when your friend is actively composing a message.
  - **Image Sharing:** Send photos directly in chat or **Share from other apps** (like UPI payment receipts) instantly into any active conversation. Target selection allows choosing between specific transactions or general friend chat.
- **Two-Sided Confirmation:** Transparent payment tracking where both the requester and the payer must confirm the transaction. Verification warnings ensure users check bank balances before confirming.
- **Developer App Store:** Explore and download other applications by the developer directly from within the app. Fetches dynamic app lists from a remote repository.
- **Easy Sharing:** Built-in "Share App" feature to invite friends via WhatsApp, Mail, or even offline via Bluetooth (direct APK file sharing).
- **Smart Notifications & Center:**
  - **Urgent Priority Alerts:** Loud, high-visibility notifications for new requests and chat messages that can bypass "Do Not Disturb".
  - **Notifications Center:** A dedicated history screen to review all past alerts, mark as read, or swipe to delete.
  - **Silent Background Service:** "System Sync" service ensures instant delivery without cluttering your notification tray.
- **Privacy Controls:** 
  - **Presence Toggle:** Choose whether friends can see your live "Online" status. Turning it off hides your status completely from everyone.
- **Help & Support Center:** Built-in searchable FAQ guide covering pairing, payments, and anti-fraud features.
- **Direct Developer Support:** Contact the developer via **WhatsApp (+91 9092041238)** or **Email (kirubas102@gmail.com)**.
- **Security Hardening & Polish (v1.2.7):**
  - **New Branding:** Rebranded to a modern Sky Blue theme with a refreshed "Envelope + Rupee" logo for better visibility.
  - **Instant Update Checks:** Moved update verification to the Splash screen to ensure users are always running the latest version before they even log in.
  - **Silent Background Ops:** Optimized the background listener to run silently as a "System Sync" process, removing tray clutter while maintaining real-time speed.
  - **Crashlytics Integration:** Implemented non-fatal error reporting for OTP failures and Firestore permission issues to ensure rapid bug fixing.
  - **Targeted Sharing:** Fixed image sharing logic to correctly distinguish between Transaction Chats and General Chats.
  - **Phone Linking Self-Healing:** Added support for updating linked phone numbers even if a provider is already linked to the Firebase user.
  - **Verification Reliability:** Hardened the verification flow with forced token refreshes and improved error handling for "credential-already-in-use".
  - **UI Polishing:** Fixed layout issues in profile banners where the "Verify Now" button text was clipped or invisible.
  - **Handshake Integrity:** Hardened physical pairing handshake in Firestore rules to prevent session hijacking.
  - **Transaction Protection:** Made core payment fields (Amount, UPI ID) immutable in Firestore rules to prevent post-request tampering.
  - **Role Enforcement:** Enforced status transitions in security rules—only the Payer can mark as "Paid", and only the Requester can "Confirm".
  - **App Lock Hardening:** Fixed a bypass where the app stayed unlocked when resumed from the background; now re-prompts for biometric on every resume if App Lock is enabled.
  - **Deleted Account Protection:** Prevents users from logging into accounts marked as "deleted".
  - **Privacy:** Disabled list permissions on phone index and pairing tokens to prevent data scraping.
  - **Screenshot Protection:** Disabled screenshots and screen recording on sensitive payment and pairing screens.
  - **Logical Handshake Improvements:** Added detailed error reporting for handshake failures and fixed "Failed to update pairing session" errors by improving observer management.
  - **Persistent Friendships:** Re-pairing with the same person now reuses the existing Pair ID, preserving chat history and continuing the relationship instead of creating duplicates.
- **Anti-Fraud Protections:** 
  - **Screen Security:** Screenshots and screen recordings are disabled on sensitive payment and pairing screens.
  - **Device Fingerprinting:** Records device model and ID for every request to detect unauthorized account usage.
  - **Anti-Spoofing:** Blocks mock location (Fake GPS) apps during pairing and payment requests.
- **Robust Verification:** Mandatory sequential verification (Email → Phone OTP) locks core features until a verified number is linked.

## 📱 Screenshots

| Onboarding | Login | QR Pairing |
|:---:|:---:|:---:|
| <img src="art/screen_1.svg" width="200"> | <img src="art/screen_2.svg" width="200"> | <img src="art/screen_3.svg" width="200"> |

| Dashboard | Payment Request | Chat |
|:---:|:---:|:---:|
| <img src="art/screen_4.svg" width="200"> | <img src="art/screen_5.svg" width="200"> | <img src="art/screen_6.svg" width="200"> |

*(Note: Replace these template SVGs in the `art/` folder with actual screenshots to update the visual guide.)*

## 📥 Download APK

You can download the latest stable version of the Pay4Me APK from the Releases section:

[**Download Pay4Me Latest Release APK**](https://github.com/kirubanandem/pay4me/releases/latest)

### Installation Steps:
1. Download the `.apk` file.
2. If prompted, allow "Install from Unknown Sources" in your Android settings.
3. Open the file and tap **Install**.
4. Open the app and verify your email and phone number to start pairing!

## 🛠 Tech Stack

- **UI:** XML Layouts, Material Design 3, Lottie Animations, Shimmer.
- **Backend:** Firebase (Authentication, Cloud Firestore, Realtime Database).
- **Proximity:** GPS Location + Bluetooth Low Energy (BLE) scanning.
- **Images:** Cloudinary Android SDK.
- **Utilities:** Google ZXing (QR), Glide (Image Loading), WorkManager.
- **Logging:** Timber.

---

## 👨‍💻 For Developers

If you are looking to build the project from source or contribute, please refer to the **[DEVELOPER.md](./README_DEV.md)** file for detailed setup instructions.

## 📄 License

This project is licensed under the MIT License.
