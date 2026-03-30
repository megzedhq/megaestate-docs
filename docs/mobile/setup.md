# Setup Mobile Application - Installation Steps

This page provides a clear, step-by-step installation flow for the MegaClassify mobile app.

## 1) Unzip and open the project

1. Unzip the downloaded mobile app source package.
2. Open the project folder in your editor/IDE.

## 2) Run initial Flutter commands

From the project root, run:

```bash
flutter clean
flutter pub get
```

## 3) Create Firebase project and app registrations

1. Open [Firebase Console](https://console.firebase.google.com/).
2. Create a **new project**.
3. Add an **Android app** with your package name.
4. Add an **iOS app** with your bundle ID.
5. Download:
   - `google-services.json` (Android)
   - `GoogleService-Info.plist` (iOS)

## 4) Place Firebase files

Copy downloaded files to:

- `android/app/google-services.json`
- `ios/Runner/GoogleService-Info.plist`

## 5) Update app identity, URLs, and keys

Update your project configuration values:

- Android package name
- iOS bundle ID
- App display name
- Host URL / API base URL (example: `https://admin.yourdomain.com/api/v1`)
- Frontend URL
- Google Maps/Places keys

## 6) Update Android/iOS branding assets

- Replace launcher icons
- Replace splash logo (`assets/images/splash_logo.png`)
- Verify app name on both Android and iOS targets

## 7) Configure Android release signing

Create a release keystore and keep it in project:

- `android/keystore/release-key.jks`

Example command:

```bash
keytool -genkey -v -keystore android/keystore/release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias release
```

Update `android/key.properties`:

```properties
storePassword=YOUR_STORE_PASSWORD
keyPassword=YOUR_KEY_PASSWORD
keyAlias=release
storeFile=keystore/release-key.jks
```

## 8) Build release binaries

Android release:

```bash
flutter build apk --release
# or
flutter build appbundle --release
```

iOS release (macOS + Xcode):

```bash
flutter build ios --release
```

## 9) Final real-device validation

Before publishing, test on physical devices:

- Login and registration
- Listing browse/search
- Create listing flow
- Chat/messaging
- Push notifications
- Deep links / app links (if enabled)

---

## Quick checklist

- [ ] `flutter clean` and `flutter pub get` completed
- [ ] Firebase Android + iOS apps created
- [ ] `google-services.json` placed in `android/app/`
- [ ] `GoogleService-Info.plist` placed in `ios/Runner/`
- [ ] API URL points to production backend
- [ ] Package/bundle IDs updated
- [ ] Keystore and `key.properties` configured
- [ ] Release APK/AAB built successfully
- [ ] iOS release build generated (if applicable)
- [ ] Real-device end-to-end tests passed
