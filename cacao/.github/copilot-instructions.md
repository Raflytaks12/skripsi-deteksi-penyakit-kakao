<!--
Short project-specific guidance for AI coding agents working on the `dagsap_absensi` Flutter app.
Keep this file concise: reference real files, commands, conventions, and where to look for details.
-->
# Copilot instructions for dagsap_absensi

**Purpose**: Help AI agents be immediately productive editing and extending this Flutter app (mobile + desktop). Focus on where code lives, how model/assets are integrated, and the commands to build/run/test.

**Big picture**:
- **App type**: Flutter app targeting Android, Windows (desktop), and web scaffolding exists. Native Windows runner is present under `windows/`.
- **ML integration**: A TensorFlow Lite model is shipped at `assets/images/models/cacao_model.tflite` and is loaded via `tflite_flutter` (see `pubspec.yaml` and `windows/flutter/generated_plugins.cmake`). The inference UI lives in `lib/deteksi_screen.dart`.
- **Routing / UX**: Screens are top-level widgets in `lib/` named `*_screen.dart`. `lib/main.dart` declares a `RouteObserver` (`routeObserver`) and registers it on `MaterialApp` for navigation tracking.

**Key files to read first**:
- `pubspec.yaml` — dependency list, assets (model path), and Flutter SDK constraints.
- `lib/main.dart` — app entry, routes, and `RouteObserver` registration.
- `lib/deteksi_screen.dart` — TFLite usage / image-picking and inference flow.
- `lib/splash_screen.dart`, `lib/home_screen.dart`, `lib/about_screen.dart` — common screen patterns and navigator usage.
- `lib/widget.dart` and `lib/tex_box.dart` — shared UI building blocks used across screens.
- `assets/images/models/cacao_model.tflite` — the actual model file; when editing model behaviour, update asset path in `pubspec.yaml` if moved.
- `windows/flutter/generated_plugins.cmake` — confirms native plugin wiring for `tflite_flutter` on Windows.

**Project-specific conventions & patterns**:
- Screen files use the suffix `_screen.dart` and expose a single `StatefulWidget`/`StatelessWidget` representing the page.
- Shared widgets are consolidated in `lib/widget.dart` and `lib/tex_box.dart` rather than many small files.
- Named routes are used in `main.dart` (e.g. `'/deteksi_screen'`) — prefer re-using these names for navigation consistency.
- Keep asset paths exact as declared in `pubspec.yaml` (note: `assets/images/` and the specific tflite path are listed).

**Build / run / test commands (PowerShell examples)**:
```
# fetch packages
flutter pub get

# run on connected Android device or emulator
flutter run -d android

# run on Windows desktop
flutter run -d windows

# build Android release
flutter build apk

# build Windows release
flutter build windows

# run tests
flutter test

# (optional) run Gradle directly for Android if needed
cd android; .\gradlew.bat assembleRelease
```

**TFLite / native plugin notes**:
- The project depends on `tflite_flutter` (see `pubspec.yaml`). On Windows the plugin is wired in `windows/flutter/generated_plugins.cmake` — when adding native plugin changes, regenerate and verify the plugin registration.
- If updating the `.tflite` model, ensure `pubspec.yaml` lists the new path and run `flutter pub get` so assets are bundled.

**Debugging hints specific to this repo**:
- To inspect navigation events, look at `routeObserver` declared in `lib/main.dart` and implement `RouteAware` on screens you want to trace.
- Image picking and camera flows use `image_picker`; check platform permissions in `android/app/src/main/AndroidManifest.xml` (camera, storage) when debugging device issues.
- For inference issues, add logging around model load and input preprocessing in `lib/deteksi_screen.dart`.

**Editing guidelines for AI agents**:
- Prefer localized changes: modify only the screen/widget file involved rather than global app wiring unless the task explicitly requires it.
- Do not move the TFLite model file without updating `pubspec.yaml` and confirming asset bundling.
- When adding dependencies, update `pubspec.yaml` and run `flutter pub get`; mention required native setup steps in the PR if a plugin requires manual native changes.
- Keep style consistent with existing files (single-file screens; no large refactors without owner's consent).

**Where to look if uncertain**:
- Start at `lib/main.dart` to understand app structure and routing.
- Open `pubspec.yaml` to verify dependencies and assets.
- Search for `DeteksiScreen` (detection flow) when working with ML or image input.

If anything here is unclear or you'd like the instructions expanded (examples for common edits, more command variants, or additional file references), tell me which area to iterate on.
