# Android Notes

## Activities

* `getResources()` will get you reference to a value from app's resources (e.g. colors)
  * Then you can call method for different type of resources (e.g. `getColor()`)
* `Color` class to get color value (e.g. `Color.BLACK`)
* `setContentView()` sets which layout the activity should display
* `startActivity` takes an intent and navigates to another activity
* `getIntent` gets the intent that started the activity

### Life Cycle

* Starting: `onCreate` -> `onStart` -> `onResume`
* Stopping: `onPause` -> `onStop` -> `onDestroy`
  * If user goes back to activity before `onDestroy`, calls `onRestart`

## Manifest File

* `manifest.xml`
* Declares abilities and permissions your app needs to run
* Registers every Activity
  * Activity that shows on app start uses intent filters of action `MAIN` and category `LAUNCHER`
* Registers which launcher icon to use
* Sets app's theme

## Debugging

* Log with `Log.d("TAG", "message")`
  * From `android.util`

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
