# Plant Waterer app

Your ESP32 dashboard, packaged as an Android app with its own logo. Everything the UI needs is bundled, so it works without internet (only your ESP32 on the same Wi-Fi is required).

## Get the APK (easiest: GitHub builds it for you)

1. Create a free GitHub repository and upload everything in this folder (including the hidden `.github` folder).
2. Open the repo's **Actions** tab, choose **Build Android APK**, click **Run workflow**. (It also runs on every push.)
3. After a few minutes, open the finished run and download **PlantWaterer-APK** under *Artifacts* (a zip containing `app-debug.apk`).
4. Send the APK to your phone, open it, and allow "Install unknown apps" when asked.

## Build it on your own computer instead

Needs Node 18+, Java 17 and Android Studio.

```
npm install
npx cap add android
npx capacitor-assets generate --android
npx cap sync android
npx cap open android      # Android Studio: Build > Build APK(s)
```

## Using the app

- Open the gear icon, enter your ESP32 IP (for example `192.168.1.50`) and tap Save.
- Phone and ESP32 must be on the same Wi-Fi network.
- The app calls `/status`, `/api/water` and `/api/auto` on the ESP32, exactly like the original page. Your ESP32 sketch must send the header `Access-Control-Allow-Origin: *` on those routes (the same requirement as the web version).
- Simulation / Demo Mode in settings lets you try the UI without hardware.

## Files

- `www/` the app itself (`index.html`, prebuilt `app.css`, `manifest.json`, `sw.js`, `icons/`)
- `www/icons/logo.svg` the logo (vector); PNG sizes are alongside it
- `assets/` source images used to generate the Android launcher icon and splash screen
- `capacitor.config.json` app id (`com.plantwaterer.app`), name, and permission for plain-HTTP calls to the ESP32
- `.github/workflows/build-apk.yml` the cloud build

## Notes

- This is a debug-signed APK, fine for your own phone. For the Play Store you would need a release build signed with your own key.
- To add new Tailwind classes to `index.html`, rebuild the CSS as described in `src/input.css`.
- The web version can also be opened as a PWA, but browsers only allow installing PWAs from HTTPS pages, and HTTPS pages cannot call a plain-HTTP ESP32. That is why the Android app is the recommended route.
- iPhone: an iOS app needs a Mac with Xcode (and an Apple developer account to install it permanently).
