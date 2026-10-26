# 👋🧩 MIUI Screen Recorder FPS Unlock

Morphe patches for the MIUI / HyperOS Screen Recorder.

## ❓ About

Record at 120, 144, 165 and 185 fps instead of 90. The patched recorder installs next to the stock one.

### How to use these patches

Click here to add these patches to Morphe: https://morphe.software/add-source?github=FluffyDiscord/miui-screen-recorder-fps-unlock

The recorder is a system app, so pull its APK off the phone first:

```
adb shell pm path com.miui.screenrecorder
adb pull /product/data-app/MIUIScreenRecorder/MIUIScreenRecorder.apk
```

## 🩹 Patches list

<!-- PATCHES_START EXPANDED -->

<!-- Do not modify this section by hand. The patch list is generated when release.yml creates a new release. -->

#### The patch list is filled in by the first release.

&nbsp;

<!-- PATCHES_END -->

### 🛠️ Building locally

- Run `./gradlew buildAndroid`
- The built patches .mpp file is found in `patches/build/libs/patches-*.mpp`
- Patch the mpp file using [Morphe-Desktop](https://github.com/MorpheApp/morphe-desktop)
  like any other patch bundle.

See the [Morphe documentation](https://github.com/MorpheApp/morphe-documentation) for more information.

## 📜 License

FluffyDiscord Screen Recorder patches are licensed under the [GNU General Public License v3.0](LICENSE)
