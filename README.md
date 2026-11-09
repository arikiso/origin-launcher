# Origin Launcher

A real Android home-screen launcher built from the **OriginWEB** replica of OriginOS 6
(https://github.com/quandz24-ui/OriginWEB, by @sungsamtech). The UI is the original web code,
running in a WebView, so animations, blur / liquid glass, wallpapers, the Dynamic Island, the
control center and the lock screen are identical. A thin layer connects it to Android.

## What is real / what is fake

| Piece | Behaviour |
|---|---|
| Home screen, dock, pages, wallpapers, blur, liquid glass, island, control center | Original replica code |
| App icons | **Only your installed apps.** Icon-pack icon if the pack has that package, otherwise the app's real Android icon |
| Opening an app | Fake OriginOS animation plays; the **real app starts only when the animation is fully expanded** (no system transition) |
| Coming back | Fake "shrink into the icon" animation |
| Lock screen | Fake (the original one, incl. PIN / fingerprint animation). Shown on cold start and after the screen was turned off while the launcher was visible. Android's real lock screen can't be replaced |
| Dynamic Island | Real music (any player), tap = open the player, prev / play-pause / next work. Needs **Notification access** (asked once) |
| Notifications | Real notifications appear in the replica's notification UI (same permission) |
| Battery %, flashlight tile, camera key | Real |
| Launcher "Settings" app | Original replica Settings (animations, wallpaper upload, blur, icon size ...) |

## Build the APK (no Android Studio needed)

1. Create a **new GitHub repo** and upload this whole folder.
2. Go to **Actions -> Build Origin Launcher APK -> Run workflow**.
3. When it is green, open the run and download the artifact **OriginLauncher-apk**, unzip, install the APK.
4. Press Home -> choose **Origin Launcher** -> *Always*.

## Build with Android Studio

```bash
git clone https://github.com/quandz24-ui/OriginWEB upstream
pip install pillow
python3 tools/prepare_assets.py --src upstream --out app/src/main/assets --slim --offline
```
Open this folder in Android Studio and run the `app` configuration.
(Drop `--offline` if you only have no internet; fonts then load from Google when online.)

## Layout of this project

```
launcher/launcher.js     the glue (real apps, fake-open -> real launch, lock, island, battery)
tools/prepare_assets.py  patches + slims the upstream replica into app/src/main/assets
app/.../MainActivity.kt  launcher activity, WebView, asset loader (https://appassets.androidplatform.net/OriginWEB/)
app/.../OriginBridge.kt  JS <-> Android (installed apps, launchApp, flashlight, media)
app/.../SysIcons.kt      serves real app icons to the web UI
app/.../MediaTracker.kt  music -> island        app/.../NotifService.kt  notifications
```

## Known limits (Android, not this project)

* The real lock screen, real status bar and real notification shade can't be replaced by a launcher.
  The replica's lock screen / control center are in-launcher only.
* Real apps open with their own window; the shrink-back animation is a look-alike, not a shared-window morph.
* Cold-starting a heavy app can show the fake expanded window for a moment before the app appears.
* Toggles in the control center other than the flashlight are visual only.
* "OriginOS" is vivo's trademark - use a different name if you ever publish this.
