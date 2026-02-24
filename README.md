# QuestExplorer – React Native App for Meta Quest 3

[![Expo](https://img.shields.io/badge/Expo-000000?logo=expo&logoColor=white)](https://expo.dev)
[![React Native](https://img.shields.io/badge/React%20Native-20232A?logo=react&logoColor=61DAFB)](https://reactnative.dev)
[![Meta Horizon](https://img.shields.io/badge/Meta%20Horizon-1A73E8?logo=meta&logoColor=white)](https://developers.meta.com/horizon/)

A complete, ready-to-run **React Native** example optimized for **Meta Quest 3 / Quest 3S** using the official `expo-horizon-core` plugin (announced February 2026).

Runs as a beautiful resizable 2D floating window in VR with perfect controller/hand tracking support. Zero browser/WebXR – fully native on Horizon OS.

## ✨ Features

- Large, high-contrast VR-friendly UI (48+ dp touch targets, 24+ px fonts)
- Runtime Horizon OS detection (`expo-horizon-core`)
- Pre-configured window size: 1024×640 dp (perfect for Quest)
- Supports Quest 3 & Quest 3S only
- Instant hot reloading via Expo Go on Quest
- Full native builds (`questDebug` / `questRelease` variants)
- TypeScript + New Architecture enabled
- One-click GitHub deployment instructions

## 📱 Live Demo (on your Quest)

1. Put on your Quest 3  
2. Open **Expo Go** (install from Horizon Store)  
3. Scan the QR code below (or use the link)  
   → [Open in Expo Go](https://expo.dev/@yourusername/quest-explorer) *(replace with your published link after EAS build)*

## 🚀 Quick Start (Windows 11 Pro)

### 1. Prerequisites
- Node.js **v20 or v22 LTS** (https://nodejs.org)
- Git for Windows (https://git-scm.com)
- Meta Quest 3 with **Developer Mode** enabled
- **Expo Go** installed on your Quest from the Horizon Store
- (Optional) Meta Quest Developer Hub (MQDH)

### 2. Create the project

```powershell
npx create-expo-app@latest QuestExplorer --template blank-typescript
cd QuestExplorer
npm install expo-horizon-core
```

### 3. Get your Horizon App ID
1. Go to https://developers.meta.com/horizon/
2. Create a new App → copy the **App ID**
3. For testing you can use the demo ID: `DEMO_APP_ID`

### 4. Update `app.json` (or `app.config.ts`)

```json
{
  "expo": {
    "name": "QuestExplorer",
    "slug": "quest-explorer",
    "version": "1.0.0",
    "orientation": "default",
    "plugins": [
      [
        "expo-horizon-core",
        {
          "horizonAppId": "YOUR_HORIZON_APP_ID_HERE",
          "defaultHeight": "640dp",
          "defaultWidth": "1024dp",
          "supportedDevices": "quest3|quest3s",
          "disableVrHeadtracking": false
        }
      ]
    ],
    "newArchEnabled": true
  }
}
```

### 5. Replace `App.tsx`

Copy the full `App.tsx` from the [src/App.tsx](src/App.tsx) file in this repository (or see the code block at the bottom of this README).

### 6. Prebuild

```powershell
rmdir /s /q android 2>$null   # Windows PowerShell
npx expo prebuild --clean
```

### 7. Test on Quest 3 (recommended)

```powershell
npx expo start
```

→ Open Expo Go on Quest → Scan QR code → **Instant VR app!**

**Hot reload** works perfectly while wearing the headset.

### 8. Full Native Quest Build

```powershell
npm run quest          # debug build
npm run quest:release  # production build
```

(Connect Quest via USB-C and allow debugging when prompted.)

## 📂 Project Structure

```
QuestExplorer/
├── app.json                  # Horizon plugin config
├── App.tsx                   # Main VR-optimized component
├── assets/
├── package.json
└── android/                  # generated after prebuild
```

## 🔧 Available Scripts

```json
"scripts": {
  "start": "expo start",
  "android": "expo run:android",
  "quest": "expo run:android --variant questDebug",
  "quest:release": "expo run:android --variant questRelease"
}
```

## 🛠️ Optimization Tips for Quest

- Use font sizes ≥24 px
- Touch targets ≥48 dp
- High contrast colors
- Check `ExpoHorizon.isHorizonDevice` for Quest-only features
- Add `expo-three` for 3D immersive experiences

## 📤 Deploy to GitHub (from Windows)

```powershell
git init
git add .
git commit -m "Initial commit – Quest 3 React Native app"
git branch -M main
git remote add origin https://github.com/YOURUSERNAME/quest-explorer.git
git push -u origin main
```

## 📝 Full `App.tsx` Code

```tsx
import React, { useState } from 'react';
import { View, Text, TouchableOpacity, StyleSheet, Alert, Platform } from 'react-native';
import * as ExpoHorizon from 'expo-horizon-core';

export default function App() {
  const [count, setCount] = useState(0);
  const isQuest = ExpoHorizon.isHorizonDevice;
  const isHorizonBuild = ExpoHorizon.isHorizonBuild;
  const appId = ExpoHorizon.horizonAppId ?? 'Not set';

  const handlePress = () => {
    setCount(c => c + 1);
    if (isQuest) {
      Alert.alert('Quest Detected!', 'Running natively on Meta Horizon OS 🎉');
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Quest Explorer</Text>
      
      <Text style={styles.info}>Platform: {Platform.OS}</Text>
      <Text style={styles.info}>Horizon Device: {isQuest ? '✅ Yes (Quest 3)' : 'No'}</Text>
      <Text style={styles.info}>Build Type: {isHorizonBuild ? 'Quest Build' : 'Standard'}</Text>
      <Text style={styles.info}>App ID: {appId}</Text>

      <Text style={styles.counter}>Count: {count}</Text>

      <TouchableOpacity style={styles.button} onPress={handlePress}>
        <Text style={styles.buttonText}>Tap / Controller Select</Text>
      </TouchableOpacity>

      <Text style={styles.hint}>
        Resize this window in VR • Large targets = best Quest UX
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#0a0a0a', alignItems: 'center', justifyContent: 'center', padding: 40 },
  title: { fontSize: 48, fontWeight: 'bold', color: '#00ffcc', marginBottom: 30 },
  info: { fontSize: 24, color: '#ffffff', marginVertical: 8, textAlign: 'center' },
  counter: { fontSize: 60, fontWeight: 'bold', color: '#ffffff', marginVertical: 40 },
  button: { backgroundColor: '#00ffcc', paddingHorizontal: 60, paddingVertical: 24, borderRadius: 16, marginVertical: 20 },
  buttonText: { fontSize: 28, fontWeight: '600', color: '#000000' },
  hint: { fontSize: 20, color: '#aaaaaa', textAlign: 'center', marginTop: 40 },
});
```

## 🎉 Ready to build the future of VR!

Star this repo ⭐, fork it, and start building your own Quest 3 experiences with React Native today.

Made with ❤️ for the React Native + Meta Horizon community.

---

**Questions?** Open an issue or find me on X [@faethflex](https://x.com/faethflex)