name: Flutter Android Build

# Trigger the build when code is pushed to the main branch
on:
  push:
    branches: [ "gallery" ]
  pull_request:
    branches: [ "gallery" ]

jobs:
  build:
    name: Build Release APK
    runs-on: ubuntu-latest # Uses a Linux virtual machine

    steps:
      # 1. Get the code from the repository
      - name: Checkout Code
        uses: actions/checkout@v3

      # 2. Set up Java (Required for Android build)
      - name: Set up Java
        uses: actions/setup-java@v3
        with:
          distribution: 'zulu'
          java-version: '17' # Use Java 17 for newer Flutter versions

      # 3. Set up Flutter environment
      - name: Set up Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.19.0' # You can change this to your specific version
          channel: 'stable'

      # 4. Get Flutter dependencies
      - name: Install Dependencies
        run: flutter pub get

      # 5. Build the Android APK in release mode
      - name: Build APK
        run: flutter build apk --release

      # 6. Upload the generated APK so you can download it from GitHub
      - name: Upload APK Artifact
        uses: actions/upload-artifact@v3
        with:
          name: release-apk
          path: build/app/outputs/flutter-apk/app-release.apk
