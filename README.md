# Anti-Snooze

A native alarm clock app for Android (and iOS) built with [Expo](https://expo.dev) / React Native that fights the snooze button. When the alarm rings you have to complete a challenge — like shaking your phone — before it will shut up.

## Features

- **Alarm scheduling** with `expo-notifications` + a native alarm layer (`@notifee/react-native`) for Android
- **Wake-up challenges** — verify you are actually awake (e.g. shake detection via `expo-sensors`) before dismissing the alarm
- **Reliable alarms on Android** — exact alarms (`SCHEDULE_EXACT_ALARM`), full-screen intent, `WAKE_LOCK`, and boot-completed rescheduling
- **Multi-ringtone library** with bundled sounds and sound-effect feedback
- **Ringtone picking** from local audio files (`expo-document-picker`)
- **Stats** — track how long it takes you to fully wake up and dismiss alarms
- **EAS Update** (`expo-updates`) with channel-based over-the-air updates
- **Custom material UI** — Plus Jakarta Sans typography, glassmorphism components, custom tab bar (no design system dep)

## Tech Stack

- [Expo](https://expo.dev) SDK 57 · React Native 0.86 · React 19
- [expo-router](https://docs.expo.dev/router/introduction/) file-based navigation (typed routes)
- [expo-notifications](https://docs.expo.dev/versions/latest/sdk/notifications/) · [@notifee/react-native](https://notifee.app/react-native)
- [expo-sensors](https://docs.expo.dev/versions/latest/sdk/sensors/) (shake challenge) · [expo-audio](https://docs.expo.dev/versions/latest/sdk/audio/)
- [expo-updates](https://docs.expo.dev/versions/latest/sdk/updates/) + [EAS Build/Update](https://expo.dev/eas)
- TypeScript

> Note: this project ships a locally-built Notifee core artifact because `notifee.app/maven` is no longer available; the shared Gradle setup lives in `android/` (see `android/settings.gradle`).

## Getting Started

```bash
npm install
npx expo start
```

Run on a device/emulator (native build):

```bash
npm run android   # expo run:android
npm run ios       # expo run:ios
```

Lint / type-check:

```bash
npm run lint      # tsc --noEmit
```

### Config

Project config lives in `app.json` (name, slug, icon, permissions, notifications sounds, EAS project id). Environment/signing secrets are **not** committed — see `.gitignore` (`*.jks`, `*.keystore`, `*.p8`, `*.p12`, `.env.*`).

## Project Structure

```
expo/
├── app.json              # Expo + EAS + permissions config
├── eas.json              # EAS Build/Update profiles & channels
├── plugins/
│   └── with-alarm-activity.js   # Expo config plugin for alarm native activity
├── assets/               # images & bundled alarm sounds
├── android/ ios/         # native projects (managed by expo prebuild)
└── src/
    ├── app/              # expo-router screens
    │   ├── (tabs)/       # index (alarms), stats, settings
    │   ├── alarm-form.tsx
    │   ├── active-alarm.tsx   # ringing alarm screen
    │   ├── challenge.tsx      # wake-up challenge screens
    │   └── permissions.tsx
    ├── components/       # glass UI, time wheel, tab bar, etc.
    ├── hooks/
    ├── services/         # nativeAlarm, alarm-ringer, storage, sound-effects, feedback
    └── (module)          # AlarmNativeModule (see /modules)
```

## Building & OTA Updates

Builds are managed with **EAS Build**. Channels (`preview`, `production`, …) are configured in `app.json` (via `updates.requestHeaders`) and `eas.json`; releases land through `expo-updates`.

```bash
eas build --profile preview --platform android
```

> **Signing:** keep your release `.jks`/`.keystore` safe and outside the repo. If you use a locally-built keystore and it is lost, you cannot resign future releases under the same identity.

## License

Private project.