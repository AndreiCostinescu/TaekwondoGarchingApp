# 🚀 Complete Deployment Guide - Taekwondo Garching App

This guide covers building and deploying standalone mobile apps for iOS (App Store) and Android (Google Play Store) with emphasis on **FREE unlimited local builds**.

---

## ⚠️ IMPORTANT: Understanding Build Limits

### Expo Free Tier Limits (Cloud Builds)
- **30 builds per month total**: 15 Android + 15 iOS
- **Resets monthly** (not a lifetime limit)
- These limits apply **only to cloud builds** (builds on Expo's servers)

### Local Builds (RECOMMENDED)
- **UNLIMITED** builds forever
- **FREE** - no cost
- Builds on your own computer
- **Android**: Works on Windows, Mac, or Linux
- **iOS**: Requires macOS with Xcode

### Our Strategy: Preserve Cloud Builds
**Use local builds for development/testing, cloud builds only for final store submissions.**

Expected monthly usage:
- Development testing: 100+ times → **Expo Go** (unlimited)
- Feature testing builds: 5-20 times → **Local builds** (unlimited)
- Production releases: 1-2 times → **Cloud builds** (uses 2-4 of your 30)

**Result**: You'll typically use only 4-8 cloud builds per month, leaving plenty of buffer!

---

## 📋 Prerequisites

### Required Accounts
- **Expo Account**: Free - https://expo.dev (for both local and cloud builds)
- **Apple Developer Account**: $99/year - https://developer.apple.com/programs/ (for iOS App Store)
- **Google Play Console**: $25 one-time - https://play.google.com/console/ (for Android Play Store)

### Required Tools
```bash
# Install globally
npm install -g eas-cli

# For iOS local builds (Mac only)
# Install Xcode from Mac App Store

# For Android local builds
# Docker will be automatically used by EAS
```

---

## 🎬 First-Time Setup

### 1. Install Dependencies
```bash
cd TaekwondoGarchingApp
npm install
```

### 2. Login to Expo
```bash
eas login
```
Create a free account at expo.dev if you don't have one.

### 3. Initialize EAS (if needed)
```bash
eas build:configure
```
This is already done in this repo, but run it if you encounter issues.

---

## 🏠 Local Builds (UNLIMITED - Use These for Development!)

### Why Use Local Builds?
✅ **UNLIMITED** builds - build as many times as you want  
✅ **FREE** forever  
✅ **Faster** - no waiting in queue  
✅ **Full control** - build on your machine  
✅ **Preserves your 30 monthly cloud builds** for production

### Android Local Build (Works on Any OS)

```bash
# Using npm script (recommended)
npm run build:android:local

# Or directly
eas build --platform android --profile preview --local
```

**What you get**: An `.apk` file in your current directory that you can:
- Install directly on Android devices
- Share with testers
- Upload to Play Store (for internal testing track)

**Build time**: 10-20 minutes (first build slower, then cached)

### iOS Local Build (Requires macOS with Xcode)

```bash
# Using npm script (recommended)
npm run build:ios:local

# Or directly
eas build --platform ios --profile preview --local
```

**Requirements**:
- macOS with Xcode installed
- Apple Developer account
- EAS will manage certificates for you

**What you get**: An `.ipa` file that you can upload to TestFlight

**Build time**: 15-30 minutes (first build slower, then cached)

### Build Both Platforms Locally

```bash
npm run build:all:local
```

---

## ☁️ Cloud Builds (LIMITED - Save for Production!)

**⚠️ WARNING: These count against your 15 builds per platform per month!**

Use cloud builds **ONLY** for:
- Final production releases
- When you don't have the right hardware (e.g., building iOS without a Mac)
- Store submissions

### Production Android Build

```bash
# Using npm script (recommended)
npm run build:android:cloud

# Or directly
eas build --platform android --profile production
```

**Uses**: 1 of your 15 monthly Android builds

### Production iOS Build

```bash
# Using npm script (recommended)
npm run build:ios:cloud

# Or directly
eas build --platform ios --profile production
```

**Uses**: 1 of your 15 monthly iOS builds

### Production Build - Both Platforms

```bash
# Using npm script (recommended)
npm run build:all:cloud

# Or directly
eas build --platform all --profile production
```

**Uses**: 2 cloud builds (1 Android + 1 iOS)

### Monitoring Cloud Builds

Track your usage at: https://expo.dev/accounts/[your-username]/projects/taekwondo-garching-app/builds

You can see:
- Builds remaining this month
- Build history
- Download links for completed builds

---

## 📦 Submitting to App Stores

### Android (Google Play Store)

#### First Time Setup:
1. **Create app in Play Console**: https://play.google.com/console/
   - Click "Create app"
   - Fill in app details
   - Choose app category (Health & Fitness or Sports)

2. **Create Service Account** (for automated submission):
   - Go to: Play Console → Setup → API access
   - Create new service account
   - Grant "Release manager" role
   - Download JSON key file
   - Save as `secrets/play-store-service-account.json` (local only, don't commit!)

3. **Update eas.json** with your service account path (already configured)

#### Submit to Play Store:

```bash
# Using npm script
npm run submit:android

# Or directly
eas submit --platform android
```

**Note**: This uses the build you just created, not a new cloud build!

#### Manual Submission Alternative:
1. Go to: https://play.google.com/console/
2. Select your app
3. Navigate to: Production → Create new release
4. Upload the `.aab` file from your build
5. Fill in release notes
6. Submit for review

**Review time**: Typically 1-3 days

---

### iOS (Apple App Store)

#### First Time Setup:

1. **Create app in App Store Connect**: https://appstoreconnect.apple.com/
   - Click "+" → "New App"
   - Fill in app information
   - Note your App Store Connect App ID

2. **Create App Store Connect API Key**:
   - Go to: Users and Access → Keys → App Store Connect API
   - Click "+" to create new key
   - Give it "Admin" or "App Manager" role
   - Download the `.p8` file
   - Note: Key ID, Issuer ID

3. **Update eas.json** with your App Store details:
   ```json
   "ios": {
     "ascAppId": "YOUR_APP_ID_HERE",
     "appleId": "your-email@example.com",
     "appleTeamId": "YOUR_TEAM_ID"
   }
   ```

#### Submit to App Store:

```bash
# Using npm script
npm run submit:ios

# Or directly
eas submit --platform ios
```

**Note**: This uses the build you just created, not a new cloud build!

#### Manual Submission Alternative:
1. Download Apple's **Transporter** app from Mac App Store
2. Upload your `.ipa` file using Transporter
3. Go to App Store Connect
4. Fill in app metadata, screenshots, privacy info
5. Submit for review

**Review time**: Typically 1-3 days (can be longer for first submission)

---

## 🔄 Recommended Workflow (Minimize Cloud Build Usage)

### Daily Development (UNLIMITED)
```bash
# Test changes instantly with Expo Go
npm start
# Scan QR code with Expo Go app on your phone
```

**Frequency**: 10-100 times per day  
**Cost**: FREE, unlimited  
**Use for**: UI changes, feature development, quick testing

---

### Weekly Feature Testing (UNLIMITED)
```bash
# Build locally for thorough testing
npm run build:android:local

# Install APK on real devices
# Test all functionality
```

**Frequency**: 5-20 times per week  
**Cost**: FREE, unlimited  
**Use for**: Pre-release testing, QA, beta testing

---

### Production Release (1-2x per month)

```bash
# Step 1: Bump version
npm run version:patch  # or minor/major

# Step 2: Commit version bump
git add package.json
git commit -m "chore: bump version to X.X.X"
git push

# Step 3: Build for production (USES CLOUD BUILDS!)
npm run build:all:cloud
# ⚠️ Uses 2 of your 30 monthly cloud builds

# Step 4: Wait for builds to complete (check Expo dashboard)

# Step 5: Submit to stores (does NOT use additional builds)
npm run submit:all
```

**Frequency**: 1-2 times per month  
**Cloud builds used**: 2-4 per month (well within 30 limit!)  
**Use for**: Final production releases to App Store/Play Store

---

## 🎨 Customizing App Icons and Splash Screen

### Required Assets:

1. **Icon** (`assets/icon.png`):
   - Size: 1024×1024px
   - Format: PNG with transparency
   - Your Taekwondo Garching logo

2. **Splash Screen** (`assets/splash.png`):
   - Size: 1284×2778px (iPhone 14 Pro Max size)
   - Format: PNG
   - Centered logo with white background

3. **Adaptive Icon** (`assets/adaptive-icon.png`):
   - Size: 1024×1024px
   - Format: PNG with transparency
   - Android uses this with different shapes

4. **Favicon** (`assets/favicon.png`):
   - Size: 48×48px
   - Format: PNG
   - For web version

### Generate Icons Easily:

**Online tools**:
- https://www.appicon.co/ - Upload one image, get all sizes
- https://easyappicon.com/ - Similar service
- https://icon.kitchen/ - Expo's official tool

**After generating**:
- Replace files in `assets/` directory
- Rebuild app
- Icons will update automatically

---

## 🤖 GitHub Actions Automation

### What's Automated (Safely):

The repository includes GitHub Actions workflows, but they're configured to **preserve your cloud build quota**:

✅ **Manual trigger only** - workflows run only when you click "Run workflow"  
✅ **No automatic builds on push** - saves your cloud builds  
✅ **Optional cloud builds** - you choose when to use them

### Available Workflows:

1. **Build Android App** (`.github/workflows/build-android.yml`)
   - Manual trigger only
   - Choose preview (local) or production (cloud)

2. **Build iOS App** (`.github/workflows/build-ios.yml`)
   - Manual trigger only
   - Choose preview (local) or production (cloud)

3. **Build All Platforms** (`.github/workflows/build-all.yml`)
   - Manual trigger only
   - Option to auto-submit after building

4. **Auto Version Bump** (`.github/workflows/auto-version-bump.yml`)
   - Automatically increment version numbers
   - Doesn't use builds

### Setup GitHub Actions (Optional):

If you want to use automated builds:

1. Go to: https://github.com/AndreiCostinescu/TaekwondoGarchingApp/settings/secrets/actions

2. Add secret:
   - Name: `EXPO_TOKEN`
   - Value: Get from https://expo.dev/accounts/[username]/settings/access-tokens

3. Trigger manually:
   - Go to Actions tab
   - Select workflow
   - Click "Run workflow"
   - Choose options

**Note**: Even with GitHub Actions, prefer local builds to save cloud quota!

---

## 📊 Monitoring Your Build Usage

### Check Remaining Builds:

Visit: https://expo.dev/accounts/[your-username]/projects/taekwondo-garching-app/builds

You'll see:
- Builds used this month
- Builds remaining
- Reset date (1st of next month)

### Track Usage:

| Activity | Method | Builds Used |
|----------|--------|-------------|
| Development testing | Expo Go | 0 (unlimited) |
| Feature testing | Local build | 0 (unlimited) |
| Production Android | Cloud build | 1 of 15 Android |
| Production iOS | Cloud build | 1 of 15 iOS |
| Store submission | Submit command | 0 (uses existing build) |

---

## 🐛 Troubleshooting

### "Build failed: Missing credentials"

**For Android**:
```bash
eas credentials --platform android
# Follow prompts to create keystore
```

**For iOS**:
```bash
eas credentials --platform ios
# Follow prompts to set up certificates
```

EAS will guide you through credential setup.

---

### "Local build failed: Docker error" (Android)

```bash
# Make sure Docker is installed and running
# EAS will download it automatically if needed

# On Windows/Mac: Install Docker Desktop
# On Linux: Install Docker Engine

# Restart the build
npm run build:android:local
```

---

### "Xcode errors" (iOS local builds)

```bash
# Update Xcode to latest version
# Open Xcode and accept license
sudo xcodebuild -license accept

# Install command line tools
xcode-select --install

# Try building again
npm run build:ios:local
```

---

### "Ran out of cloud builds this month"

**Solution 1: Use local builds** (recommended)
```bash
npm run build:android:local
npm run build:ios:local
```

**Solution 2: Wait for monthly reset** (1st of next month)

**Solution 3: Upgrade to paid plan** ($29/month for unlimited builds)

---

## 🔐 Security Best Practices

### Secrets to NEVER Commit:

Create a `secrets/` directory (already in `.gitignore`):

```bash
mkdir secrets

# Store sensitive files here:
secrets/
├── play-store-service-account.json  # Android
├── app-store-connect-key.p8         # iOS
└── .env                              # API keys
```

These files are automatically ignored by git!

---

## 📱 Testing Your Builds

### Android APK Testing:

1. **Build locally**:
   ```bash
   npm run build:android:local
   ```

2. **Transfer APK to device**:
   - Email to yourself
   - Upload to Google Drive
   - Use `adb install app.apk`

3. **Install on device**:
   - Enable "Install from unknown sources" in Settings
   - Tap APK file to install
   - Test all features!

---

### iOS TestFlight Testing:

1. **Build for production**:
   ```bash
   npm run build:ios:cloud  # Uses 1 cloud build
   ```

2. **Submit to App Store Connect**:
   ```bash
   npm run submit:ios
   ```

3. **Set up TestFlight**:
   - Go to App Store Connect
   - Click "TestFlight" tab
   - Add internal testers (up to 100)
   - They'll receive email with TestFlight link

4. **Testers install via TestFlight app**

---

## 🚀 Release Checklist

### Before Each Release:

- [ ] Test thoroughly with Expo Go
- [ ] Build locally and test on real devices
- [ ] Update version number: `npm run version:patch`
- [ ] Update CHANGELOG.md (if you have one)
- [ ] Create git tag: `git tag v1.0.1`
- [ ] Push changes: `git push && git push --tags`

### Android Release:

- [ ] Build production: `npm run build:android:cloud` (uses 1 cloud build)
- [ ] Test the AAB file
- [ ] Update Play Store listing (if needed)
- [ ] Submit: `npm run submit:android` (uses 0 builds)
- [ ] Monitor for crashes in Play Console
- [ ] Gradually roll out: 10% → 50% → 100%

### iOS Release:

- [ ] Build production: `npm run build:ios:cloud` (uses 1 cloud build)
- [ ] Submit: `npm run submit:ios` (uses 0 builds)
- [ ] Update App Store listing (if needed)
- [ ] Add screenshots (required sizes)
- [ ] Submit for review
- [ ] Monitor for crashes in App Store Connect

---

## 💰 Cost Summary

### Current Setup (Free Tier):

| Item | Cost | Limits |
|------|------|--------|
| Expo account | FREE | 30 cloud builds/month |
| Local builds | FREE | UNLIMITED |
| Expo Go testing | FREE | UNLIMITED |
| Development | FREE | UNLIMITED |

### Paid Options (If Needed):

| Plan | Cost | Benefits |
|------|------|----------|
| Expo Production | $29/month | Unlimited cloud builds, priority queue |
| Expo Enterprise | Custom | Advanced features, SLAs |

**Recommendation**: Stay on free tier using local builds! Only upgrade if you:
- Need to build iOS without a Mac
- Want faster build queues
- Need advanced features

---

## 📚 Additional Resources

- **Expo Documentation**: https://docs.expo.dev/
- **EAS Build Docs**: https://docs.expo.dev/build/introduction/
- **EAS Submit Docs**: https://docs.expo.dev/submit/introduction/
- **Local Builds Guide**: https://docs.expo.dev/build-reference/local-builds/
- **React Native Docs**: https://reactnative.dev/

---

## 💡 Pro Tips

1. **Always use local builds for testing** - save cloud builds for production
2. **Use Expo Go for 90% of development** - fastest way to test
3. **Build locally before cloud builds** - catch issues early
4. **Version bump before each release** - keeps everything organized
5. **Use git tags for releases** - easy to track versions
6. **Test on real devices** - simulators don't catch everything
7. **Gradual rollouts on Android** - Start with 10%, increase if stable
8. **Monitor crash reports** - Fix issues quickly after release

---

## 🆘 Need Help?

1. **Check build logs**: 
   - Local builds: Terminal output
   - Cloud builds: https://expo.dev dashboard

2. **Expo Discord**: https://chat.expo.dev/
3. **GitHub Issues**: Open an issue in this repository
4. **Stack Overflow**: Tag with `expo` and `react-native`

---

## 📈 Success Metrics

With this setup, you should achieve:

- ✅ **UNLIMITED development iterations** (Expo Go)
- ✅ **UNLIMITED test builds** (local builds)
- ✅ **4-8 cloud builds used per month** (well within 30 limit)
- ✅ **$0 monthly cost** (stay on free tier)
- ✅ **Fast iteration cycles** (no waiting for cloud builds)

---

**Happy building! 🥋📱**