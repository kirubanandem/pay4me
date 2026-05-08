# Pay4Me 💸

**Pay4Me** is a secure, real-time Android application designed to simplify shared payments and peer-to-peer transaction coordination using UPI QR codes.

[![Platform](https://img.shields.io/badge/platform-Android-green.svg)](https://developer.android.com/about/versions/14)
[![Language](https://img.shields.io/badge/language-Java-orange.svg)](https://www.java.com/)
[![Version](https://img.shields.io/badge/version-1.1.7-blue.svg)](https://github.com/kirubanandem/pay4me/releases/latest)

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
  - **Image Sharing:** Send photos directly in chat or **Share from other apps** (like UPI payment receipts) instantly into any active conversation.
- **Two-Sided Confirmation:** Transparent payment tracking where both the requester and the payer must confirm the transaction. Verification warnings ensure users check bank balances before confirming.
- **Smart Notifications & Center:**
  - **Urgent Priority Alerts:** Loud, high-visibility notifications for new requests and chat messages that can bypass "Do Not Disturb".
  - **Notifications Center:** A dedicated history screen to review all past alerts, mark as read, or swipe to delete.
  - **Background Service:** Ensures instant delivery even when the app is minimized. Swipeable notification on newer Android versions with a "Stop" control for older ones.
- **Privacy Controls:** 
  - **Presence Toggle:** Choose whether friends can see your live "Online" status. Turning it off hides your status completely from everyone.
- **Help & Support Center:** Built-in searchable FAQ guide covering pairing, payments, and anti-fraud features.
- **Direct Developer Support:** Contact the developer via **WhatsApp (+91 9092041238)** or **Email (kirubas102@gmail.com)**.
- **Security Hardening (v1.1.7):**
  - **Stability:** Fixed "double image" upload bug when sharing receipts from external apps.
  - **App Lock Integrity:** Prevents lock-screen bypass via notifications or shortcuts.
  - **Ownership Validation:** Strict server-side checks ensure only participants can view request details.
  - **Self-Healing Sessions:** Automatically restores sessions after biometric changes or storage resets.
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
