<div align="center">

# ML Kit Pose Detection (Kotlin)

Real-time human pose detection on Android, drawing the skeleton on a canvas overlay and computing body joint angles live from the camera feed.

[![GitHub stars](https://img.shields.io/github/stars/niccolofanton/mlkit-pose-detection-kotlin?style=social)](https://github.com/niccolofanton/mlkit-pose-detection-kotlin/stargazers)

<img src="https://github.com/niccolofanton/mlkit-pose-detection-kotlin/blob/main/14649.jpg?raw=true" width="32%" alt="Pose detection sample 1" />
<img src="https://github.com/niccolofanton/mlkit-pose-detection-kotlin/blob/main/14652.jpg?raw=true" width="32%" alt="Pose detection sample 2" />

</div>

## What it does

This is an Android app (Kotlin) that uses [Google ML Kit Pose Detection](https://developers.google.com/ml-kit/vision/pose-detection) together with [CameraX](https://developer.android.com/training/camerax) to detect a person's pose in real time from the rear camera.

For every analyzed frame it:

- Runs ML Kit's **accurate** pose detector in stream mode to locate body landmarks (shoulders, elbows, wrists, hips, knees, ankles, eyes, ears, hands and feet).
- Draws the detected skeleton as connected lines on a transparent custom `View` (`RectOverlay`) layered over the live camera preview.
- Computes and displays joint angles relevant to ergonomic posture assessment (REBA): neck, torso and legs, for both the left and right sides.

The project is set up around the REBA (Rapid Entire Body Assessment) use case: it surfaces the neck/torso/leg angles needed to evaluate working posture.

## Screenshots

The repository ships two sample captures showing the skeleton overlay and the on-screen angle readout:

<div align="center">
<img src="https://github.com/niccolofanton/mlkit-pose-detection-kotlin/blob/main/14649.jpg?raw=true" width="40%" alt="Sample capture 1" />
<img src="https://github.com/niccolofanton/mlkit-pose-detection-kotlin/blob/main/14652.jpg?raw=true" width="40%" alt="Sample capture 2" />
</div>

## Quick start

You need [Android Studio](https://developer.android.com/studio) and a physical Android device (the app uses the camera). Minimum SDK is **API 26 (Android 8.0)**.

```bash
git clone https://github.com/niccolofanton/mlkit-pose-detection-kotlin.git
cd mlkit-pose-detection-kotlin
```

Then either open the project in Android Studio, let Gradle sync, and run it on a connected device, or build from the command line:

```bash
./gradlew assembleDebug
```

On first launch the app requests camera permission and starts streaming pose detection immediately.

## How it works

<details>
<summary>Pipeline overview</summary>

- **Camera** — CameraX binds a `Preview` and an `ImageAnalysis` use case to the activity lifecycle, using the default back camera.
- **Analysis** — each `ImageProxy` frame is converted to an ML Kit `InputImage` and passed to an `AccuratePoseDetectorOptions` detector configured for `STREAM_MODE`.
- **Overlay** — on a successful detection, `RectOverlay` clears its bitmap-backed canvas and redraws the skeleton by connecting the relevant `PoseLandmark` positions.
- **Angles** — `getAngle()` and `getNeckAngle()` use `atan2` to derive neck, torso and leg angles, which are rendered into a `TextView` over the preview.

</details>

## Features

- Real-time pose detection from the rear camera using ML Kit's accurate model in stream mode.
- Full-body landmark coverage (head, arms, hands, torso, legs and feet).
- Skeleton rendering on a transparent canvas overlaid on the live preview.
- Live joint-angle readout for neck, torso and legs (left and right).
- Runtime camera-permission handling.

## Tech stack

- **Language:** Kotlin
- **Pose detection:** Google ML Kit `pose-detection` / `pose-detection-accurate` (17.0.1-beta3)
- **Camera:** AndroidX CameraX (camera2, lifecycle and view)
- **UI:** AndroidX AppCompat, Material Components, ConstraintLayout, custom `View` canvas drawing
- **Build:** Gradle (Android Gradle Plugin), `compileSdk` 30 / `minSdk` 26

## License

No license file is currently included in this repository. If you intend to reuse the code, please contact the author to clarify the terms.

## Credits

Built by [niccolofanton](https://github.com/niccolofanton) on top of Google ML Kit and CameraX.
