# Static APK analysis

Inspected file: `TG-AnimeMod-3.1hx.apk` (55 MB). The APK was not executed.

## Identified metadata

- Android package: `com.animetka.tg.android.app`
- Display label in DEX strings: `TG: Anime Mod`
- Version name: `13.0.2`
- Minimum Android SDK: 24
- Target Android SDK: 36

## Confirmed custom Anime entry

The DEX contains `org.telegram.ui.Components.TgappAnimeTab`. Its UI setup includes the Russian label `Аниме`; its `openBotProfile` method references `qanimebot`. The iOS project reproduces that observed route by opening `@qanimebot` in the same Telegram Web session.

## Package composition

The APK includes four DEX bytecode files, Telegram-related native libraries (`libtmessages.49.so`, `libtgappsec.so`), a large `libgojni.so` library, localization assets, themes and media resources. This is a modified Telegram-derived Android client, not an anime streaming app.

## Permission caution

The Android manifest declares permissions including `SEND_SMS`, `READ_CALL_LOG`, `READ_PHONE_NUMBERS`, `READ_PHONE_STATE` and `CALL_PHONE`, among many standard messaging permissions. A manifest declaration alone does not prove that the app uses each permission, but these are sensitive capabilities and are a reason not to blindly transplant or trust the Android package.

## Limits of this analysis

The APK was inspected statically; it was not run. The available environment did not include JADX/apktool or an Android SDK decoder, so this report does not claim a complete reverse-engineering of every mod-specific feature. The iOS app is a clean reimplementation of the confirmed chat + Anime-tab flow, not a direct conversion of Android bytecode.
