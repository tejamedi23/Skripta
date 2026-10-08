# Android app (PWA -> APK)

Done in this repo: web app manifest, service worker, 192/512 and maskable icons.

After the Vercel deploy works (needs HTTPS):
1. Open https://www.pwabuilder.com, paste your Vercel URL, Start.
2. Check the manifest/service-worker/icon items are green.
3. Package for Android. PWABuilder returns a signed APK/AAB and a `assetlinks.json`.
4. Put `assetlinks.json` at `.well-known/assetlinks.json` in this repo, push, redeploy (removes the browser address bar in the app).
5. Install the APK on a phone to test. Keep the signing key PWABuilder gives you; Play Store updates need it.

Offline: the app shell opens offline; generating notes needs the network.
