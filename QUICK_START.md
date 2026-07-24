# ⚡ Quick Start - 30 Seconds

## What This Does

Your app will verify it's genuine before allowing multiplayer:

```
┌────────────────────────────────────────────────────────┐
│ 1. Check APK Signature (Local)          │
│    - Verifies app hasn't been modified  │
│ 2. Backend Verification                 │
│    - Confirms with your server          │
│ 3. PlayFab Authentication               │
│    - Creates/identifies player          │
│ 4. Photon Connection                    │
│    - Joins multiplayer game             │
└────────────────────────────────────────────────────────┘
```

## 3-Step Setup

### Step 1: Get Your IDs (5 minutes)

```
1. Photon App ID
   → https://dashboard.photonengine.com
   → Create app, copy ID

2. PlayFab Title ID
   → https://developer.playfab.com
   → Create game, copy Title ID

3. Your Backend URL
   → Deploy Flask app from /backend
   → Get API key and URL
```

### Step 2: Configure (2 minutes)

```
1. Open /config/attestation-config.json
2. Update these values:
   - EXPECTED_SIGNATURE_HASH: Your APK hash
   - BACKEND_URL: Your server
   - API_KEY: Your API key

3. Open /config/multiplayer-config.json
4. Update:
   - PHOTON_APP_ID
   - PLAYFAB_TITLE_ID
```

### Step 3: Import to Unity (2 minutes)

```
1. Create new Unity project
2. Import Photon PUN 2 plugin
3. Import PlayFab SDK
4. Copy /unity-client/Assets/Scripts to your project
5. Copy prefabs to your project
```

## Full Setup Guide

→ Read [COMPLETE_TUTORIAL.md](COMPLETE_TUTORIAL.md)

## How They Work Together

→ Read [PHOTON_PLAYFAB_GUIDE.md](PHOTON_PLAYFAB_GUIDE.md)

## Architecture

→ Read [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)

## Now What?

✅ All code is ready to copy
✅ All configs are templates
✅ All docs are complete
✅ Example projects included

**Just follow the complete tutorial and you're done! 🚀**
