things

```
sdkmanager --install "ndk;21.1.6352462"
sdkmanager --install "build-tools;28.0.3"
sdkmanager --install "platforms;android-28"
sdkmanager --install "platforms;android-29"

sdkmanager "system-images;android-29;google_apis;x86_64"

avdmanager create avd -n pixel -k "system-images;android-29;google_apis;x86_64" -d "17"

mkdir «android sdk path»/platform-tools

«sdk»/emulator/emulator -avd pixel

zig build keystore
zig build run -Dsdk-root=/path/to/sdk/root # TODO; make this configurable
```

the password for running is "passwd456" (defined in build.zig)

if the emulator sigsev's, edit `~/.android/avd/pixel.avd/config.ini` and add `hw.gpu.mode = swiftshader_indirect`

# Android Apps in Zig

This repository contains a example on how to create a minimal Android app in Zig.

## State of the project
This project contains a really small app skeleton in `src/main.zig` which initializes OpenGL and renders a color cycle. Touchscreen events will be displayed as small circles beneath the fingers that will fade as soon as no event for the same finger will happen again.

The code contains some commented examples on how to interface with the JNI to use advanced features of the `ANativeActivity`.

It has no dependencies to C code except for the android libraries, so it can be considered a pure Zig app.

Please note that `build.zig` and `libc/` have hardcoded paths right now and will be changed to a configurable version later.

## Presentation

There is a [FOSDEM Talk](https://fosdem.org/2021/schedule/event/zig_android/) you can watch here:
- [MP4 Video](https://video.fosdem.org/2021/D.zig/zig_android.mp4)
- [WebM Video](https://video.fosdem.org/2021/D.zig/zig_android.webm)

## What's missing
- Configuration management example
- Save/load app state example

## Requirements & Build

You need the [Android SDK](https://developer.android.com/studio#command-tools) installed together with the [Android NDK](https://developer.android.com/ndk).

You also need [adb](https://developer.android.com/studio/command-line/adb) and a Java SDK installed (required for `jarsigner`), as well as the Unix tools `zip` and `unzip`.

To build the APK, you need to adjust the paths in `build.zig` and `libc/` to your local system installation so the build runner can find all required tools and libraries.

Now you need to generate yourself a keystore to sign your apps. For debugging purposes, the build script contains a helper. Just invoke `zig build keystore` to generate yourself a debug keystore that can be used with later build invocations. 

If all of the above is done, you should be able to build the app by running `zig build`.

There are convenience options with `zig build push` (installs the app on a connected phone) and `zig build run` (which installs, then runs the app).

## Credits
Huge thanks to [@cnlohr](https://github.com/cnlohr) to create [rawdrawandroid](https://github.com/cnlohr/rawdrawandroid) and making this project possible!
