---
name: ionic-capacitor
---

### Some notes to help me understand how Ionic Capacitor turns into an APK

## Thoughts

 * App + plugins should show on the left, but my newly added `erik-capacitor-plugin-test` didn't show until I `File -> Sync Project with Gradle Files`.
 * `npm link` the plugin in development. if you `npm install` it doesn't take your changes.

## android/capacitor.settings.gradle

```
include ':capacitor-storage'
project(':capacitor-storage').projectDir = new File('../node_modules/@capacitor/storage/android')

include ':erik-capacitor-plugin-test'
project(':erik-capacitor-plugin-test').projectDir = new File('../../erik-capacitor-plugin-test/android')
```

| android\app\src\main\assets\public | web assets | 
