# MIUI Screen Recorder FPS Unlock

Patches for the built-in MIUI / HyperOS Screen Recorder, for use with [Morphe](https://morphe.software). Record at 120, 144, 165 and 185 fps instead of 90.

**Add to Morphe:** https://morphe.software/add-source?github=FluffyDiscord/miui-screen-recorder-fps-unlock

## Features

- [High frame rates](#what-changes) — up to 185 fps; the stock app stops at 90
- [Real frame timing](#what-changes) — one video frame per screen frame, no duplicates
- [Full-length clips](#what-changes) — still moments at the start and end are kept
- [Separate app](#install) — installs next to the stock recorder, which stays untouched
- [No adb needed](#install) — adds its own tile and asks for every permission on the first tap

## Install

1. Copy the recorder APK off your phone. It's a system app, so it isn't on app stores:

```
adb shell pm path com.miui.screenrecorder
adb pull /product/data-app/MIUIScreenRecorder/MIUIScreenRecorder.apk
```

   Use the path the first command prints.

2. Open Morphe Manager → **Repositories** → add this repo, or tap the link above.
3. Patch the APK with **Unlock high frame rates** and install it.
4. Open **Screen Recorder (high fps)** from the app drawer and tap **Add tile**. Its camera icon has an "H".
5. Set **Frame rate** on the screen that opens. Later, long-press the tile to get back to it.

**The first tap on the tile asks for everything at once:**

1. Phone and notifications. Phone access pauses recording during calls.
2. **Display over other apps** → turn it on, press Back. This is for the floating stop button.
3. **Start recording?** → Start.

### Optional: with adb

Skip the "Start recording?" prompt:

```
adb shell appops set com.miui.screenrecorder.hfr PROJECT_MEDIA allow
```

Hide the stock recorder and its tile:

```
adb shell pm disable-user --user 0 com.miui.screenrecorder
```

Undo with `adb shell pm enable com.miui.screenrecorder`.

## What changes

| | Stock recorder | Patched |
|---|---|---|
| Frame-rate choices | 15–90 fps | 15–185 fps |
| Frame capture | 8 ms timer (`1000 / fps`, rounded) | one encoded frame per new screen frame |
| Frame timestamps | millisecond clock | the screen frame's own nanosecond time |
| Still screen | timer keeps sending frames | last image repeated every 100 ms |
| Start of clip | first keyframe can be lost | keyframe requested until the file writer starts |
| Package | `com.miui.screenrecorder` | `com.miui.screenrecorder.hfr` |

Measured on a POCO F9 Ultra (2 s still, 3 s scrolling, 2 s still):

- Video starts at 0.00 s and covers the whole clip.
- Scrolling → 112–117 fps, every frame a new image.

> **165 and 185 fps only show up in games that switch the screen to those rates.** In normal use the screen tops out at 120 Hz, so that's the most a recording can hold.

## Why a separate app

- **Updates must be signed with the same key as the installed app.** Xiaomi's key isn't public, so the stock recorder can't be replaced.
- **The copy is an ordinary app.** It can't use Xiaomi's system-only screen-capture permission, so Android asks "Start recording?" each time unless you run the adb command above.

## Supported versions

| Screen Recorder | Status |
|---|---|
| 4.15.3.5.0 | tested on HyperOS 3 (Android 16) |
| other versions | experimental; patching fails cleanly if the code doesn't match |

Not covered:

- Choppy playback in Xiaomi Gallery. The same files play smoothly elsewhere.
- Tapping the tile while a recording is running. Stop with the floating button.

## Patches

<!-- PATCHES_START EXPANDED -->

<!-- Do not modify this section by hand. The patch list is generated when release.yml creates a new release. -->

#### The patch list is filled in by the first release.

&nbsp;

<!-- PATCHES_END -->

## Development

Build the patches bundle (needs JDK 17+ and a GitHub token with `read:packages`, see [Morphe setup](https://github.com/MorpheApp/morphe-patcher/blob/main/docs/2_1_setup.md)):

```
./gradlew buildAndroid
```

The bundle lands in `patches/build/libs/patches-*.mpp`.

Commit to `dev` with semantic messages (`feat:`, `fix:`, `chore:`). `release.yml` publishes pre-releases from `dev` and stable releases from `main`.

## License

GPLv3. See [LICENSE](LICENSE) and [NOTICE](NOTICE). Not affiliated with Xiaomi or the Morphe project.
