# Pay4Me 💸

**Pay4Me** is a secure, real-time Android application designed to simplify shared payments and peer-to-peer transaction coordination using UPI QR codes.

[![Platform](https://img.shields.io/badge/platform-Android-green.svg)](https://developer.android.com/about/versions/14)
[![Language](https://img.shields.io/badge/language-Java-orange.svg)](https://www.java.com/)

## 🚀 Key Features

- **Instant QR Pairing:** Connect with friends physically using dynamic QR codes that expire in 30 minutes for enhanced security.
- **UPI QR Forwarding:** Scan any UPI QR code and instantly forward it to a paired friend for payment.
- **Real-time Chat:** Built-in messaging system for every payment request to clarify details and share confirmation.
- **Two-Sided Confirmation:** Transparent payment tracking where both the requester and the payer must confirm the transaction.
- **Auto-Expiry:** Payment requests automatically expire after 30 minutes if not fulfilled, keeping your dashboard clean.
- **Push Notifications:** Stay updated with FCM-powered alerts for new requests, chat messages, and pairing status.
- **Secure Authentication:** Robust login system with Email/Password and secondary Phone OTP verification.

## 📱 Screenshots

| Onboarding | Login | QR Pairing |
|:---:|:---:|:---:|
| <img src="art/screen_1.png" width="200"> | <img src="art/screen_2.png" width="200"> | <img src="art/screen_3.png" width="200"> |

| Dashboard | Payment Request | Chat |
|:---:|:---:|:---:|
| <img src="art/screen_4.png" width="200"> | <img src="art/screen_5.png" width="200"> | <img src="art/screen_6.png" width="200"> |

*(Note: Replace images in `art/` folder to update screenshots)*

## 📥 Download APK

You can download the latest stable version of the Pay4Me APK from the Releases section:

[**Download Pay4Me v1.0.6 APK**](https://github.com/kirubas/pay4me/releases/latest)

### Installation Steps:
1. Download the `.apk` file.
2. If prompted, allow "Install from Unknown Sources" in your Android settings.
3. Open the file and tap **Install**.
4. Open the app and verify your phone number to start pairing!

## 🛠 Tech Stack

- **UI:** XML Layouts, Material Design 3, Lottie Animations, Shimmer.
- **Backend:** Firebase (Authentication, Cloud Firestore, Realtime Database).
- **Images:** Cloudinary Android SDK.
- **Utilities:** Google ZXing (QR), Glide (Image Loading), WorkManager.
- **Logging:** Timber.

---

## 👨‍💻 For Developers

If you are looking to build the project from source or contribute, please refer to the **[DEVELOPER.md](./README_DEV.md)** file for detailed setup instructions.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
