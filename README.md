# Taekwondo Garching App

A React Native mobile application that displays the Taekwondo Garching website (https://taekwondogarching.ddns.net) in a WebView.

## Features

- Cross-platform support (iOS, Android)
- Full website interaction within the app
- Loading indicator
- Safe area support for modern devices

## Prerequisites

- Node.js (v18 or later)
- npm or yarn
- Expo CLI
- For iOS: macOS with Xcode
- For Android: Android Studio

## Installation

1. Clone the repository:
```bash
git clone https://github.com/AndreiCostinescu/TaekwondoGarchingApp.git
cd TaekwondoGarchingApp
```

2. Install dependencies:
```bash
npm install
```

## Running the App

### Development with Expo Go

1. Start the development server:
```bash
npm start
```

2. Scan the QR code with:
   - **iOS**: Camera app
   - **Android**: Expo Go app

### Running on Specific Platforms

```bash
# iOS
npm run ios

# Android
npm run android

# Web
npm run web
```

## Building for Production

### Android (APK)

```bash
# Install EAS CLI
npm install -g eas-cli

# Login to Expo
eas login

# Configure the project
eas build:configure

# Build APK
eas build --platform android --profile preview
```

### iOS (IPA)

```bash
# Build for iOS
eas build --platform ios
```

## Project Structure

```
TaekwondoGarchingApp/
├── App.js              # Main application component with WebView
├── app.json            # Expo configuration
├── package.json        # Dependencies and scripts
├── babel.config.js     # Babel configuration
└── README.md           # This file
```

## Technologies Used

- **React Native**: Cross-platform mobile framework
- **Expo**: Development platform for React Native
- **react-native-webview**: WebView component for displaying web content

## License

MIT
