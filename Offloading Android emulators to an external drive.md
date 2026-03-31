---
created: 2026-03-31
stage: published
---

At [WTMG](/projects/welcome-to-my-garden) we're almost releasing our mobile app, built with Capacitor. A beta tester in our community recently reported an issue, which turned out to be specific to Android 9. Previously, I also struggled to enable edge-to-edge screen filling, due to safe-area inset bugs in old Android webview versions.

Ensuring wide compatibility of our app required different Android devices to reproduce issues and test fixes. More realistically though, it required many emulators. Unfortunately those emulators take up storage space, which I didn't have.

It's not the first time I spent significant time developing a mobile app. In fact, I bought my current Macbook Pro with 32 GB RAM in 2022 for the main purpose of running multiple simulators and emulators at the same time. The laptop I used then, a 16 GB Intel MBP from 2017, struggled with this in terms of CPU and RAM, let alone that it had a 256 GB SSD. Unfortunately, my then-desired MBP model with a 1 TB SSD had bad availability. Which meant I settled for 512 GB version, and so here I was, again trying to figure out how I could offload some emulators to an external SSD.

Back then, I figured it out thanks to [a dev.to blog post](https://dev.to/oscherler/move-android-studio-virtual-devices-on-a-mac-5ej5) (on which I even wrote some comments).

The last days, I've compiled a more comprehensive yet simple "switch script" which allows me to still have some emulators locally, and many more externally.

If the SSD is plugged in and I run the command, then it switches both the target Android SDK and AVD folders from my local disk to the external SSD. It does this for:
- For my shell's environment (`~/.zshrc`)
- Using `launchctl env` for the AVD path in Android Studio
- For the SDK path, two spots in Android Studio's configuration need to be changed, one global and one project-specific.

If the SSD paths are not available, it does the reverse.

The reason I'm also including the SDK, is that it includes the `system-images` folder, which AFAIK can't be easily extracted. This folder also grows significantly in size as you install different Android versions.

Currently, my external SSD has 5 images (21.3 GB) and 5 virtual devices (22.27 GB), for a total of 43.57 GB. My local disk only has one image (4.58 GB) and one device (6.15 GB), leading to almost 33 GB of saved space (and the option to keep adding more virtual devices without worrying about space).

## The script

Was researched and typed out mostly by Claude Code/Sonnet 4.6, some edits by me.

Note that this script assumes:
- macOS `sed`
- that you use `zsh`
- that you already have `ANDROID_HOME` and `ANDROID_AVD_HOME` path variables defined in your `~/.zshrc`, two of each for both the local disk and the external disk.
- that you also edit the script with said path definitions, on top

It needs to be adjusted for different environments (Linux, `bash` etc).

Android Studio needs to be shut down for the switch to work without hiccups. 

```sh
#!/usr/bin/env bash
# android-env-switch.sh
# Switches Android Studio SDK and AVD paths between external SSD and local Mac.
# Checks if the external SSD paths exist; uses them if so, otherwise falls back to local.
# Updates: launchctl env vars, ~/.zshrc (comment/uncomment), and each Android Studio
# installation's android.sdk.path.xml (the source of truth for the IDE's SDK picker),
# as well as the sdk path in a given project's local.properties

set -euo pipefail

ZSHRC="$HOME/.zshrc"

# --- Path definitions ---
# Paths on your external drive, when it's connected
EXT_SDK=/${YOUR_PATH}/sdk
EXT_AVD=/${YOUR_PATH}/avd
# Paths on your local disk
LOCAL_SDK=/${YOUR_PATH}/sdk
LOCAL_AVD=/${YOUR_PATH}/avd
# This file is in the root of your Android project
LOCAL_PROPS=/${YOUR_PATH}/android/local.properties
STUDIO_PREFS_BASE="$HOME/Library/Application Support/Google"

# --- Decide which paths to activate ---
if [[ -d "$EXT_SDK" && -d "$EXT_AVD" ]]; then
    ACTIVE_SDK="$EXT_SDK"
    ACTIVE_AVD="$EXT_AVD"
    INACTIVE_SDK="$LOCAL_SDK"
    INACTIVE_AVD="$LOCAL_AVD"
    MODE="external SSD"
else
    ACTIVE_SDK="$LOCAL_SDK"
    ACTIVE_AVD="$LOCAL_AVD"
    INACTIVE_SDK="$EXT_SDK"
    INACTIVE_AVD="$EXT_AVD"
    MODE="local Mac"
fi

echo "Switching Android environment to: $MODE"

# --- 1. Set launchctl env vars ---
launchctl setenv ANDROID_HOME "$ACTIVE_SDK"
launchctl setenv ANDROID_AVD_HOME "$ACTIVE_AVD"
echo "  launchctl: ANDROID_HOME=$ACTIVE_SDK"
echo "  launchctl: ANDROID_AVD_HOME=$ACTIVE_AVD"

# --- 2. Update ~/.zshrc ---
# Paths are assumed unquoted. Uses '|' as sed delimiter to avoid escaping '/'.
# macOS sed requires -i '' for in-place edits.

zshrc_uncomment() {
    local var="$1" path="$2"
    # Capture group handles any whitespace between '#' and 'export'
    sed -i '' "s|^#[[:space:]]*\(export ${var}=${path}\)$|\1|" "$ZSHRC"
}

zshrc_comment() {
    local var="$1" path="$2"
    sed -i '' "s|^export ${var}=${path}$|# export ${var}=${path}|" "$ZSHRC"
}

zshrc_uncomment ANDROID_HOME     "$ACTIVE_SDK"
zshrc_uncomment ANDROID_AVD_HOME "$ACTIVE_AVD"
zshrc_comment   ANDROID_HOME     "$INACTIVE_SDK"
zshrc_comment   ANDROID_AVD_HOME "$INACTIVE_AVD"

echo "  ~/.zshrc: uncommented $ACTIVE_SDK, $ACTIVE_AVD"
echo "  ~/.zshrc: commented   $INACTIVE_SDK, $INACTIVE_AVD"

# --- 3. Update android.sdk.path.xml for every installed Android Studio version ---
# This is the file backing Languages & Frameworks -> Android SDK in the IDE.
# Must be done while Studio is closed; Studio overwrites it on exit.


for sdk_xml in "$STUDIO_PREFS_BASE"/AndroidStudio*/options/android.sdk.path.xml; do
    [[ -f "$sdk_xml" ]] || continue
    sed -i '' "s|androidSdkAbsolutePath\" value=\"[^\"]*\"|androidSdkAbsolutePath\" value=\"${ACTIVE_SDK}\"|" "$sdk_xml"
    echo "  android.sdk.path.xml: $sdk_xml -> $ACTIVE_SDK"
done

# --- 4. Update local.properties in the project directory ---

if [[ -f "$LOCAL_PROPS" ]]; then
    sed -i '' "s|^sdk\.dir=.*|sdk.dir=${ACTIVE_SDK}|" "$LOCAL_PROPS"
    echo "  local.properties: $LOCAL_PROPS -> $ACTIVE_SDK"
else
    echo "  local.properties: not found at $LOCAL_PROPS (skipping)"
fi

echo ""
echo "Done. Open a new shell or run the 'source' line below for env vars to take effect."
echo "To apply in current shell: export ANDROID_HOME=\"$ACTIVE_SDK\" && export ANDROID_AVD_HOME=\"$ACTIVE_AVD\""
echo "Note: android.sdk.path.xml & local.properties changes take effect on next Android Studio launch (close Studio first)."

```
