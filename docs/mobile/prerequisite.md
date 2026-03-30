# Setup Mobile Application - Prerequisite

Before starting installation, make sure you have all required tools, accounts, and values ready.

## 1) Required Tools

- Latest MegaEstate mobile source code ZIP from CodeCanyon
- **Flutter SDK**
  - Recommended: **3.22.x**
  - Minimum: **3.16+**
- Android Studio + Android SDK
- Java (required for Android builds)
- Xcode (for iOS build, macOS only)

## 2) Required Accounts

- Firebase account (for Android app registration and `google-services.json`)
- Google Cloud Console account (for Google Maps/Places API key)
- Play Console account (for Android release publishing)
- Apple Developer account (for iOS release publishing)

## 3) Required Project Inputs

Keep these values prepared before installation:

### App identity
- App name (display name)
- Android package name (example: `com.example.megaestate`)
- iOS bundle ID (example: `com.example.megaestate`)

### Backend and URLs
- API host URL (example: `https://apimegaestate.megzed.com`)

### API keys
- Google Maps API key
- Google Places API key

### Android release signing
- Keystore file (`.jks`) for release build
- `key.properties` values:
  - `storePassword`
  - `keyPassword`
  - `keyAlias`
  - `storeFile`

## 4) Pre-check Commands

Run these before setup:

```bash
flutter --version
flutter doctor
```

Fix any issues shown by `flutter doctor` before moving to the installation steps.
