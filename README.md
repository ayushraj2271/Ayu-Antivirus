# AYU ANTIVIRUS
**SCAN • PROTECT • SECURE**

A native Kotlin/Jetpack Compose Android security application designed around local-first analysis, explicit user actions, and Android platform limits.

## Current implementation
- Native Android/Compose project, package `com.ayu.antivirus`.
- Streaming SHA-256 utility and deterministic local threat matching.
- Harmless EICAR test-pattern detection path (no real malware samples).
- APK metadata/signing/permission/component/native-library analysis using `PackageManager` and ZIP inspection.
- AES-GCM quarantine using an Android Keystore AES key.
- Room-backed scan/event history with 7-day retention support.
- WorkManager hooks for install monitoring and charging-aware scheduled maintenance.
- Notification channels.
- Offline-first architecture; network helpers are optional and non-core.
- GitHub Actions build workflow.

## Android limitations
Android does not grant ordinary apps unrestricted access to other applications' private data, a universal filesystem watcher, or decryption of arbitrary HTTPS application traffic. Ayu therefore uses SAF for user-selected files/folders, PackageManager for inspectable installed-app metadata, package-install broadcasts where available, and optional VPN-based network filtering only as a separately consented capability. It never claims kernel/root access.

## Build
Requires JDK 17 and Android SDK/API 35. The repository contains a self-contained Gradle bootstrap at `gradle/wrapper/gradle-wrapper.jar` plus wrapper properties. In this environment the public Gradle host was unreachable, so a standard Gradle wrapper JAR could not be downloaded; the bootstrap downloads Gradle 8.9 on a connected build machine such as GitHub Actions. Run:

```bash
./gradlew clean testDebugUnitTest assembleDebug
```

## GitHub Actions
Push the repository to GitHub, open **Actions**, and run **Build Ayu Antivirus APK**. The workflow validates the expected root layout, builds/tests with JDK 17, verifies the APK, and uploads `ayu-antivirus-debug-apk`.

## Testing
Use the EICAR test pattern only for antivirus behavior. The app must not contain real malware samples. APK analysis is static and never executes an APK.

## Privacy
No login, ads, analytics SDK, or required API key. Core scanning is local. Network reputation/database updates are optional and should be treated as online features. Security history is designed for seven days. Quarantine is app-private and encrypted.

## Threat database
The local feed is data, not executable code. Feed installation should verify an out-of-band SHA-256/signature before activation, write atomically, and retain the last known-good feed. The bundled database is deliberately small and does not claim millions of detections.

## License
Add licenses for any third-party threat feeds/rules before distributing them. This repository does not bundle proprietary malware databases.
