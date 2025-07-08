# ThessBus Web Fallback & Deep Link Host

This repository contains the web assets and Firebase Hosting configuration for `thessbus.gr`. Its primary purpose is to facilitate deep linking (App Links for Android, Universal Links for iOS) into the native ThessBus mobile application and provide a user-friendly web fallback experience if the app is not installed.

## Overview

When a user clicks a link pointing to `https://thessbus.gr/...`:

1.  **If the ThessBus app is installed:** The mobile operating system (Android or iOS) intercepts the link and opens the app directly, passing the full URL (including the path like `/stopId/123`) to the app for internal routing.
2.  **If the ThessBus app is NOT installed:** The user's browser opens the link, and Firebase Hosting serves the `index.html` page from this repository. This page:
    *   Provides basic information about the ThessBus app.
    *   Detects the user's operating system (Android/iOS).
    *   Displays a prominent "Download App" button linking to the appropriate App Store (Google Play Store or Apple App Store).
    *   Includes a link to the Privacy Policy page.
    *   Retains the original path (`/stopId/123`) in the browser's URL bar and makes it accessible via JavaScript (for potential future use like deferred deep linking).

This setup relies on Firebase Hosting to serve the necessary verification files (`assetlinks.json` for Android, `apple-app-site-association` for iOS) required by the mobile operating systems to validate the domain association.

## Features

*   **Deep Link Verification:** Hosts `.well-known/assetlinks.json` and `.well-known/apple-app-site-association` with correct `Content-Type` headers.
*   **Web Fallback Page:** Simple `index.html` served when the app isn't installed.
*   **OS Detection:** JavaScript in `index.html` detects Android/iOS to provide the correct store link.
*   **App Store Redirection:** Directs users to Google Play or Apple App Store.
*   **Path Preservation:** Handles arbitrary URL paths (e.g., `thessbus.gr/stopId/123`) by rewriting to `index.html`, keeping the path available.
*   **Privacy Policy Page:** Separate `privacy.html` page.
*   **Firebase Hosting:** Leverages Firebase for reliable hosting, SSL, and configuration.

## Project Structure

```
.
├── .firebaserc           # Firebase project configuration
├── firebase.json         # Firebase Hosting rules (rewrites, headers)
└── public/               # Web root directory served by Firebase Hosting
    ├── .well-known/
    │   ├── assetlinks.json  # Android App Links verification file
    │   └── apple-app-site-association # iOS Universal Links verification file (NO extension)
    ├── index.html          # Main fallback landing page
    ├── privacy.html        # Privacy Policy content
    └── (optional: css/, js/, images/ folders for styling and assets)
```

## Key Files Explained

*   **`public/index.html`**: The main landing page shown to users who don't have the app installed. Contains OS detection logic and links to app stores/privacy policy.
*   **`public/privacy.html`**: Contains the application's privacy policy.
*   **`public/.well-known/assetlinks.json`**: Required by Android to verify that your app has permission to handle links from `thessbus.gr`. **Must be customized** with your app's package name and SHA-256 fingerprint.
*   **`public/.well-known/apple-app-site-association`**: Required by iOS to verify that your app has permission to handle links from `thessbus.gr`. **Must be customized** with your Apple Team ID and app Bundle ID.
*   **`firebase.json`**: Configures Firebase Hosting. Crucially, it sets the correct `Content-Type` header for `apple-app-site-association` and includes a rewrite rule (`"source": "**", "destination": "/index.html"`) to ensure all non-file paths load the `index.html` fallback page.
*   **`.firebaserc`**: Associates the local project directory with a specific Firebase project.

## Setup & Configuration

**Prerequisites:**

*   Node.js and npm/yarn installed.
*   Firebase CLI installed (`npm install -g firebase-tools`).
*   A Firebase project created on the [Firebase Console](https://console.firebase.google.com/).
*   The custom domain `thessbus.gr` registered and connected to your Firebase Hosting project via the Firebase Console.

**Steps:**

1.  **Clone the Repository:**
    ```bash
    git clone <your-repo-url>
    cd <repo-directory>
    ```
2.  **Login to Firebase:**
    ```bash
    firebase login
    ```
3.  **Associate with Firebase Project:**
    *   If `.firebaserc` is not pre-configured or you need to change projects:
        ```bash
        firebase use --add
        ```
    *   Select your Firebase project when prompted.
4.  **Customize Verification Files:**
    *   **`public/.well-known/assetlinks.json`**: 
        *   Replace `YOUR_ANDROID_PACKAGE_NAME` with your app's package name (e.g., `gr.thessbus.app`).
        *   Replace `YOUR_SHA256_CERT_FINGERPRINT` with the SHA-256 fingerprint of your app's signing certificate (obtainable via `keytool` or Google Play Console).
    *   **`public/.well-known/apple-app-site-association`**: 
        *   Replace `YOUR_TEAM_ID.YOUR_BUNDLE_ID` with your Apple Developer Team ID and your app's Bundle Identifier (e.g., `ABCDE12345.gr.thessbus.app`).
5.  **Customize App Store Links in `index.html`:**
    *   Open `public/index.html`.
    *   Find the JavaScript section.
    *   Replace `'https://play.google.com/store/apps/details?id=YOUR_ANDROID_PACKAGE_NAME'` with your actual Google Play Store URL.
    *   Replace `'https://apps.apple.com/app/idYOUR_APP_STORE_ID'` with your actual Apple App Store URL.
6.  **Update Privacy Policy:**
    *   Edit `public/privacy.html` with your complete and accurate privacy policy information.

## Deployment

After configuration, deploy the site to Firebase Hosting:

```bash
firebase deploy --only hosting
```

Wait for the deployment to complete. Changes should be live within a few minutes.

## Verification

After deployment, ensure the verification files are served correctly:

1.  **Android:** Navigate to `https://thessbus.gr/.well-known/assetlinks.json`. You should see the raw JSON content.
2.  **iOS:** Navigate to `https://thessbus.gr/.well-known/apple-app-site-association`. You should see the raw JSON content. Use browser developer tools (Network tab) to confirm the `Content-Type` header is `application/json`.

*Note: It can sometimes take time for Apple/Google servers to fetch and cache these files after deployment.*

## Mobile App Configuration (Reminder)

This web setup is only half of the deep linking equation. Ensure your native mobile apps are configured correctly:

*   **Android:**
    *   Add the appropriate `<intent-filter>` with `android:autoVerify="true"` to your app's `AndroidManifest.xml`.
*   **iOS:**
    *   Add the appropriate `applinks` entitlements to your app's `Entitlements.plist`.
    *   Ensure your app's `Info.plist` includes the correct `Associated Domains`.
