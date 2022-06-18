I want to fork @capacitor-community/bluetooth-le to add some methods.
My app is "eriks-blue-radar".

The magic word is `npm link`.

```
dev$ git clone https://github.com/capacitor-community/bluetooth-le
Cloning into ‘bluetooth-le’…
...
dev$ cd bluetooth-le
dev/bluetooth-le$ npm i
...
dev/bluetooth-le$ npm run build
...
dev/bluetooth-le$ npm link
C:\Users\erikl\AppData\Roaming\npm\node_modules\@capacitor-community\bluetooth-le -> C:\Users\erikl\Documents\bluetooth-le
dev/bluetooth-le$ cd ..\eriks-blue-radar
dev/eriks-blue-radar$ npm link @capacitor-community/bluetooth-le
C:\Users\erikl\Documents\eriks-blue-radar\node_modules\@capacitor-community\bluetooth-le -> C:\Users\erikl\AppData\Roaming\npm\node_modules\@capacitor-community\bluetooth-le -> C:\Users\erikl\Documents\bluetooth-le
dev/eriks-blue-radar$
```
Voila.