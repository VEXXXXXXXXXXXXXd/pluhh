# 📚 Complete Tutorial: APK Attestation System

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Part 1: Initial Setup](#part-1-initial-setup)
3. [Part 2: Backend Deployment](#part-2-backend-deployment)
4. [Part 3: Unity Client Setup](#part-3-unity-client-setup)
5. [Part 4: Configuration](#part-4-configuration)
6. [Part 5: Testing Locally](#part-5-testing-locally)
7. [Part 6: Testing on Meta Quest](#part-6-testing-on-meta-quest)
8. [Part 7: Production Deployment](#part-7-production-deployment)
9. [Part 8: Troubleshooting](#part-8-troubleshooting)

---

## Prerequisites

Before starting, make sure you have:

### Required Software
- ✅ **Unity 2020 LTS or newer** - Download from [unity.com](https://unity.com)
- ✅ **Python 3.8+** - Download from [python.org](https://python.org)
- ✅ **Git** - Download from [git-scm.com](https://git-scm.com)
- ✅ **Android SDK** - Install via Android Studio
- ✅ **adb (Android Debug Bridge)** - Install via Android Studio

### Required Accounts (Free)
1. **Photon Cloud** - https://dashboard.photonengine.com
2. **PlayFab** - https://developer.playfab.com
3. **Meta Developer** - https://developer.oculus.com

---

# Part 1: Initial Setup

## Step 1: Clone the Repository

```bash
cd ~/Documents
git clone https://github.com/hauntedbyxre/apk-attestation-gtag.git
cd apk-attestation-gtag
ls -la
```

## Step 2: Create Accounts

### Create Photon Account
```
1. Go to https://dashboard.photonengine.com
2. Click "Sign Up"
3. Create account
4. Create new app
5. COPY YOUR APP ID - Save it!
```

### Create PlayFab Account
```
1. Go to https://developer.playfab.com
2. Click "Sign Up"
3. Create account
4. Create new game title
5. COPY YOUR TITLE ID - Save it!
```

## Step 3: Get Your APK Signature Hash

### On Windows:
```bash
keytool -list -v -keystore %USERPROFILE%\.android\debug.keystore -alias androiddebugkey -storepass android -keypass android | find "SHA256"
```

### On Mac/Linux:
```bash
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android | grep SHA256
```

You'll see: `SHA256: AA:BB:CC:DD:EE:FF:..."`

**Remove colons:** `aabbccddeeeff...`

**SAVE THIS - Very important!**

---

# Part 2: Backend Deployment

## Step 1: Install Python Dependencies

```bash
cd backend
pip install -r requirements.txt
```

## Step 2: Configure Backend

Edit `backend/config/config.json`:
```json
{
  "expected_signature_hash": "aabbccddeeeff...",
  "versions": {
    "1": {"version_name": "1.0.0", "released": true}
  },
  "min_version": 1,
  "max_version": 1
}
```

## Step 3: Test Backend Locally

```bash
python attestation_backend.py
# You should see: Running on https://127.0.0.1:5000
```

Test endpoint in another terminal:
```bash
curl https://127.0.0.1:5000/health
```

✅ **Backend is working!**

---

# Part 3: Unity Client Setup

## Step 1: Create New Unity Project

```
1. Open Unity Hub
2. Click "Create Project"
3. Select "3D (URP)"
4. Name: "GorillaTagAttestation"
5. Create
```

## Step 2: Import Plugins

### Photon PUN 2
```
1. Window > Asset Store
2. Search "Photon PUN 2"
3. Click Import
4. Wait for import
```

### PlayFab SDK
```
1. Window > Asset Store
2. Search "PlayFab"
3. Click Import
4. Wait for import
```

## Step 3: Copy Scripts

Copy entire `unity-client/Assets/Scripts/` folder to your project's `Assets/Scripts/`

---

# Part 4: Configuration

## Step 1: Update AttestationConfig.cs

Edit `Assets/Scripts/Config/AttestationConfig.cs`:

```csharp
public class AttestationConfig
{
    public const string BACKEND_URL = "https://127.0.0.1:5000";
    public const string API_KEY = "your_api_key_here";
    public const string EXPECTED_SIGNATURE_HASH = "aabbccddeeeff...";
    public const string PACKAGE_NAME = "com.example.gorillatagattestation";
    public const long VERSION_CODE = 1L;
    public const string VERSION_NAME = "1.0.0";
}
```

## Step 2: Update MultiplayerConfig.cs

Edit `Assets/Scripts/Config/MultiplayerConfig.cs`:

```csharp
public class MultiplayerConfig
{
    public const string PHOTON_APP_ID = "your_photon_id_here";
    public const string PLAYFAB_TITLE_ID = "your_playfab_title_id";
    public const string PHOTON_REGION = "us";
}
```

---

# Part 5: Testing Locally

## Step 1: Open Project in Unity

```
1. In Unity, open your project
2. Make sure backend is running (from Part 2)
3. Press Play
```

## Step 2: View Logs

In Console window, you should see:
```
✓ Signature hash verified
✓ APK checksum verified
✓ Backend verification passed
✓ PlayFab authenticated
✓ Photon connected
```

---

# Part 6: Testing on Meta Quest

## Step 1: Enable Developer Mode

```
1. Put on Quest
2. Settings > System > Developer
3. Enable "Developer Mode"
```

## Step 2: Connect Quest

```bash
adb devices
# Should show your device
```

## Step 3: Build and Install

In Unity:
```
1. File > Build And Run
2. Choose location
3. Click "Build And Run"
4. Unity installs and launches automatically
```

---

# Part 7: Production Deployment

## Step 1: Create Release Keystore

```bash
keytool -genkey -v -keystore release.keystore \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -alias release_key \
  -storepass your_password \
  -keypass your_password
```

## Step 2: Build Release APK

In Unity:
```
1. File > Build Settings
2. Select Android
3. Build
```

## Step 3: Sign APK

```bash
jarsigner -keystore release.keystore \
  app-release.apk release_key
zipalign -v 4 app-release.apk app-final.apk
```

## Step 4: Upload to Meta Quest Store

```
1. Go to https://developer.oculus.com
2. My Apps > Your App
3. Build > Upload APK
4. Select app-final.apk
5. Fill in store listing
6. Submit for review
```

---

# Part 8: Troubleshooting

## Backend Connection Failed
```
1. Backend running? python attestation_backend.py
2. Port correct? Check localhost:5000
3. Config correct? Check config.json
```

## Signature Hash Mismatch
```
1. Get your hash: keytool -list -v -keystore ~/.android/debug.keystore | grep SHA256
2. Convert by removing colons
3. Update AttestationConfig
4. Rebuild
```

## Photon Connection Failed
```
1. App ID correct? https://dashboard.photonengine.com
2. Region available? Try "us"
3. Check internet connection
```

## PlayFab Auth Failed
```
1. Title ID correct? https://developer.playfab.com
2. Check PlayFab status
3. Try again
```

---

✅ **Congratulations! You now have a fully working attestation system!**

Next: Deploy to Meta Quest Store!
