---
name: ionic-capacitor
---

### Some notes to help me understand Ionic Capacitor Plugin Development

## Thoughts

 * `npm link` the plugin in development. if you `npm install` it doesn't take your changes.
 * Only `npm run watch` the plugin in development. Don't `npm run build` on plugin while VSCode or `ionic serve` is running because it deletes `dist` and friends and this makes VSCode and other watchers unhappy.

## `Unhandled Rejection: "XXX" plugin is not implemented on android

* Forgot `npm install $plugin` or `ionic cap sync`?
* Does Android Studio show the plugin on the left in the project view?  My newly added `erik-capacitor-plugin-test` didn't show until I `File -> Sync Project with Gradle Files`.
 

## android/app/src/main/assets/capacitor.config.json

This file determines whether to use bundled web assets or serve it from development server.

```
	"bundledWebRuntime": false,
	"server": {
		"url": "http://192.168.18.237:8100"
	}
```

## android/capacitor.settings.gradle

```
include ':capacitor-storage'
project(':capacitor-storage').projectDir = new File('../node_modules/@capacitor/storage/android')

include ':erik-capacitor-plugin-test'
project(':erik-capacitor-plugin-test').projectDir = new File('../../erik-capacitor-plugin-test/android')
```

| android\app\src\main\assets\public | web assets | 
