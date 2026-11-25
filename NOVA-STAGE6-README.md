# NOVA – Stage 6 (Voice Settings Foundation)

This stage continues the supplied Stage 5.2 project without rebuilding its architecture.

## Implemented
- Added a dedicated Arabic voice-settings screen.
- Added Arabic Android TTS voice discovery and selection (only voices whose locale language is `ar` are listed).
- Added a persistent speech-rate setting (0.60x–1.40x, default 1.00x).
- Added a spoken preview sentence.
- Applied the selected Android TTS voice and speech rate to `VoiceCommandService`.
- Added a direct on-device SpeechRecognizer test on Android 12+ when the device reports local recognition availability.
- Preserved the existing Stage 5 speech-recognition fallback behavior; no Vosk replacement was made.

## Dii embedded voice
- Added a local Dii female Arabic voice using sherpa-onnx VITS/Piper.
- The GitHub Actions workflow downloads the INT8 ONNX weights plus the required tokens and eSpeak-NG data before building the APK. This keeps the large model files out of the source ZIP/repository.
- The model is licensed CC BY-NC-SA 4.0. Redistribution is non-commercial only; the build pins the source revisions and adds attribution/license information to the packaged assets. Do not use this model in a commercial release without a different license. See `DII-MODEL-LICENSE.txt`.
- Dii speech is synthesized and played locally; it does not require network access after installation.
- If the model cannot initialize, NOVA falls back to Android TTS and reports the fallback in its notification.

## Offline recognition test
- The settings screen checks `SpeechRecognizer.isOnDeviceRecognitionAvailable()`.
- The test button creates `SpeechRecognizer.createOnDeviceSpeechRecognizer()` and performs a short Arabic recognition session.
- A reported available engine does not guarantee that an Arabic offline language pack is installed; the actual test result on the target phone is decisive.
- The existing Stage 5 `NovaSpeechLoop` behavior remains unchanged: local recognizer first when available, Android default recognizer otherwise.

## Files changed
- `app/src/main/java/com/example/callhelper/NovaVoiceSettingsActivity.java` (new)
- `app/src/main/java/com/example/callhelper/DiiTtsEngine.java` (new)
- `app/build.gradle` (sherpa-onnx dependency and uncompressed ONNX assets)
- `build.gradle` (JitPack repository)
- `.github/workflows/android-apk.yml` (downloads the pinned Dii model before building)
- `DII-MODEL-LICENSE.txt` (model attribution and licensing notes)
- `app/src/main/java/com/example/callhelper/MainActivity.java`
- `app/src/main/java/com/example/callhelper/VoiceCommandService.java`
- `app/src/main/AndroidManifest.xml`

## Validation
- Android manifest and resource XML parsed successfully.
- Java structural delimiter checks passed.
- Static source and workflow review was performed. Full Gradle/Android compilation and device playback tests still need to run in GitHub Actions and on a physical Android phone.
