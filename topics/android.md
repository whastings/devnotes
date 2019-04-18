# Android Notes

## adb

* Needs to be added to `$PATH`
  * Should live at `~/Library/Android/sdk/platform-tools/`

### Commands

* Install an APK over USB: `adb install path/to/apk`

## Emulator

### `emulator` Command

* Needs to be added to `$PATH`
  * Should live at `~/Library/Android/sdk/emulator`

#### Commands

* Start AVD: `emulator -avd nameOfDevice`
* List all AVDs: `emulator -list-avds`
