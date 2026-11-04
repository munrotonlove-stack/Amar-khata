name: Build Android APK

on:
  push:
    branches: [ main, master ]
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Find project folder
        id: proj
        run: |
          S=$(find . -name 'settings.gradle*' -not -path './.git/*' | head -n 1)
          if [ -z "$S" ]; then
            echo "settings.gradle.kts পাওয়া যায়নি। ফাইল আপলোড ঠিক হয়নি।"
            ls -R | head -60
            exit 1
          fi
          echo "dir=$(dirname "$S")" >> "$GITHUB_OUTPUT"

      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: "17"

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Set up Gradle 8.9
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: "8.9"

      - name: Build debug APK
        working-directory: ${{ steps.proj.outputs.dir }}
        run: gradle assembleDebug --stacktrace

      - name: Collect APK
        run: cp "${{ steps.proj.outputs.dir }}/app/build/outputs/apk/debug/app-debug.apk" AmarHisabKhata.apk

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: AmarHisabKhata-APK
          path: AmarHisabKhata.apk

      - name: Create release with APK
        uses: softprops/action-gh-release@v2
        with:
          tag_name: build-${{ github.run_number }}
          name: Build ${{ github.run_number }}
          files: AmarHisabKhata.apk
