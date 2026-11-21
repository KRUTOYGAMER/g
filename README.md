# TG Anime Glass for iPhone

A small SwiftUI iOS shell based on the verified parts of the uploaded `TG-AnimeMod-3.1hx.apk`.

## What the APK analysis confirmed

- Android package: `com.animetka.tg.android.app`
- Android display name: `TG: Anime Mod`
- Version name: `13.0.2`
- A custom `TgappAnimeTab` displays the Anime tab and references `qanimebot`.

The APK is a modified Telegram-derived Android client. It is **not** an anime streaming app. The APK contains Android DEX bytecode and Android-specific native libraries; those cannot be converted directly into an iOS executable.

## What this iOS project currently implements

- **Chats**: Telegram Web K inside a persistent `WKWebView`.
- **Anime**: a route to `@qanimebot` in the same web session.
- **Settings**: native appearance controls and a reload action.
- **Appearance**: SwiftUI Liquid Glass on iOS 26+, with a translucent-material fallback on iOS 17–25.

This is a clean reimplementation of the confirmed chat + Anime-tab flow. It does **not** reproduce every feature, theme, protocol change, or other mod-only behavior of the Android fork. Telegram Web supplies the messaging experience; the app does not contain Telegram's native iOS messaging engine.

## Build an unsigned IPA with GitHub Actions

The repository includes `.github/workflows/build-unsigned-ipa.yml`.

1. Create a GitHub repository and upload/push the contents of this folder to its `main` branch. Do **not** upload the original APK; it is not needed for this iOS build.
2. Open the repository's **Actions** tab.
3. Select **Build unsigned iOS IPA**.
4. Press **Run workflow** (a push to `main` also starts a build).
5. Open the finished workflow run and download the artifact named **TGAnimeGlass-unsigned-IPA**.
6. Extract the artifact ZIP. It contains `TGAnimeGlass-unsigned.ipa`.

The workflow uses GitHub's `macos-26` runner and Xcode, builds for the physical iOS device SDK with code signing disabled, then packages `Payload/TGAnimeGlass.app` as an IPA.

### Important: unsigned does not mean installable yet

The IPA is deliberately unsigned and has no provisioning profile. iOS will not install it as-is. After downloading it, sign it with your own signing method and profile before installing it. No Apple certificate, private key, team ID, or provisioning profile is stored in this repository or used by the workflow.

## Local build on a Mac

Open `TGAnimeGlass.xcodeproj` in Xcode 26 or later. The shared `TGAnimeGlass` scheme is included. To build without signing:

```bash
xcodebuild \
  -project TGAnimeGlass.xcodeproj \
  -scheme TGAnimeGlass \
  -configuration Release \
  -sdk iphoneos \
  -derivedDataPath build \
  CODE_SIGNING_ALLOWED=NO \
  CODE_SIGNING_REQUIRED=NO \
  CODE_SIGN_IDENTITY="" \
  build
```

## Permissions

Camera, microphone and photo-library permission descriptions are declared in `Info.plist`. iOS displays its normal permission prompts when supported web features request access.
