# ThessBus Website

Official website for ThessBus - the modern public transportation app for Thessaloniki, Greece.

## About

This repository hosts the landing page and deep linking infrastructure for [thessbus.gr](https://thessbus.gr). The website serves two main purposes:

1. **App Landing Page**: Information about the ThessBus app with download links to Google Play and Apple App Store
2. **Deep Linking**: Seamless integration between web links and the mobile app - when users have the app installed, web links automatically open in the app

## Features

- 📱 Smart platform detection (Android/iOS)
- 🔗 Universal/App Links integration
- 📄 Privacy policy page
- 🎨 Modern, responsive design
- ⚡ Fast Firebase hosting

## Project Structure

```
├── firebase.json         # Firebase hosting configuration
├── public/               # Web assets
│   ├── .well-known/      # App verification files
│   ├── index.html        # Main landing page
│   ├── privacy.html      # Privacy policy
│   └── beta-*.html       # Beta program pages
└── README.md
```

## Development

### Prerequisites
- Node.js and npm
- Firebase CLI (`npm install -g firebase-tools`)
- Firebase project with custom domain configured

### Setup
1. Clone the repository
2. Login to Firebase: `firebase login`
3. Deploy: `firebase deploy --only hosting`

### Local Development
```bash
firebase serve
```

## Deep Linking

The website includes verification files that allow the ThessBus mobile app to handle `thessbus.gr` links directly:

- **Android**: `/.well-known/assetlinks.json`
- **iOS**: `/.well-known/apple-app-site-association`

When users click a thessbus.gr link:
- ✅ **App installed**: Opens directly in the app
- 🌐 **No app**: Shows website with download options

## Links

- 🌐 **Website**: [thessbus.gr](https://thessbus.gr)
- 📱 **Android App**: [Google Play Store](https://play.google.com/store/apps/details?id=gr.thessbus)
- 🍎 **iOS App**: [Apple App Store](https://apps.apple.com/app/id6479063579)
- 🛡️ **Privacy Policy**: [thessbus.gr/privacy](https://thessbus.gr/privacy)

## About ThessBus

ThessBus is a modern mobile application that provides real-time information for public transportation in Thessaloniki, Greece. Features include:

- Real-time bus arrivals
- Route planning
- Stop information and favorites
- Offline support
- Clean, intuitive interface

---

Built with ❤️ for Thessaloniki's public transportation community
