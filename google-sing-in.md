# Google Sign-In Setup Guide for Flutter (Android + iOS)

This document explains **all configuration steps required before using the `google_sign_in` Flutter package**. It covers Google Cloud setup, OAuth configuration, and optional Firebase setup for FCM notifications.

---

## 1. Prerequisites

Before starting, ensure you have:

* A Google account
* A Flutter project (Android + iOS)
* Optional: A Firebase project if using FCM (push notifications)

---

## 2. Create or Select a Google Cloud Project

1. Open Google Cloud Console: [https://console.cloud.google.com](https://console.cloud.google.com)
2. Click the project selector at the top.
3. Select an existing project or click **New Project**.
4. Confirm the project is selected and visible in the top navigation bar.

---

## 3. Configure OAuth Consent Screen

1. Go to: **APIs & Services → OAuth consent screen**.
2. Choose **External**.
3. Click **Create**.
4. Fill in the required fields:

   * App name
   * User support email
   * Developer contact email
5. Click **Save and Continue** until finished.
6. On the last screen, click **Back to Dashboard**.

> This step is required before creating a Web OAuth Client ID.

---

## 4. Enable Required APIs

1. Open: **APIs & Services → Library**.
2. Search for and enable these APIs:

   * **Google OAuth API (Google Identity Services)**
   * Optional: **Identity Toolkit API** (only needed for Firebase Auth)

---

## 5. Create OAuth Client IDs

You need three client IDs:

* Web Client ID → Required for Flutter (`serverClientId`)
* Android Client ID
* iOS Client ID

### 5.1 Create Web Client ID (required for Flutter)

1. Go to: **APIs & Services → Credentials**.
2. Click **Create Credentials → OAuth client ID**.
3. Select **Web application**.
4. Name it (example: `myapp-web-client`).
5. Leave Redirect URIs empty.
6. Click **Create**.
7. Copy the **Client ID**.

This is the value you will use as:

```
serverClientId: 'WEB_CLIENT_ID'
```

---

### 5.2 Create Android Client ID

1. Click **Create Credentials → OAuth client ID**.
2. Select **Android**.
3. Fill in:

   * **Package name** (your Android app ID)
   * **SHA-1 fingerprint**
4. Get debug SHA-1 using:

```
keytool -list -v -alias androiddebugkey -keystore ~/.android/debug.keystore
```

Default password: `android`
5. Click **Create**.

---

### 5.3 Create iOS Client ID

1. Click **Create Credentials → OAuth client ID**.
2. Select **iOS**.
3. Fill in:

   * iOS Bundle ID
   * Apple Team ID
4. Click **Create**.

---

## 6. (Optional) Add Firebase for Push Notifications

If you want to use **Firebase Cloud Messaging (FCM)**:

### 6.1 Add your Android app

1. Go to: [https://console.firebase.google.com](https://console.firebase.google.com)
2. Add an Android app with your package name.
3. Download `google-services.json`.
4. Place it into: `android/app/`

### 6.2 Add your iOS app

1. Add an iOS app with your iOS Bundle ID.
2. Download `GoogleService-Info.plist`.
3. Add it to the iOS Runner project.

> Firebase is only needed for FCM. You do **not** need Firebase Auth.

---

## 7. Install Google Sign-In in Flutter

Add to `pubspec.yaml`:

```yaml
dependencies:
  google_sign_in: ^7.2.0
```

Run:

```
flutter pub get
```

---

## 8. Initialize Google Sign-In in Flutter

```dart
final GoogleSignIn _googleSignIn = GoogleSignIn.instance;

Future<void> initGoogle() async {
  await _googleSignIn.initialize(
    serverClientId: 'YOUR_WEB_CLIENT_ID',
  );
}
```

Call `initGoogle()` inside `initState()`.

---

## 9. Start Google Authentication Flow

```dart
onPressed: () async {
  final user = await _googleSignIn.authenticate();
  final auth = await user.authentication;

  print(user.email);
  print(auth.idToken);
}
```

Send `idToken`, `email`, `name`, and `photoUrl` to your Laravel backend.

---

## 10. Summary

| Step | Description                                 |
| ---- | ------------------------------------------- |
| 1    | Create/select Google Cloud project          |
| 2    | Configure OAuth consent screen              |
| 3    | Enable Google OAuth APIs                    |
| 4    | Create Web OAuth client ID (serverClientId) |
| 5    | Create Android & iOS OAuth client IDs       |
| 6    | Optional: configure Firebase for FCM        |
| 7    | Add google_sign_in package                  |
| 8    | Initialize with serverClientId              |
| 9    | Use `authenticate()` in Flutter             |

---

This document provides all required configuration steps before using Google Sign-In in Flutter.
