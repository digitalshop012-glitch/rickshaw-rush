# Rickshaw Rush — Play Store build guide (no installs needed)

Everything runs free on GitHub's servers.

## A. One-time setup
1. github.com → sign up / log in → **New repository** → name `rickshaw-rush` → Public → Create.
2. **Upload this whole folder** (use "Add file → Upload files"; include the hidden `.github` folder — if the web uploader skips it, create the two workflow files manually via "Add file → Create new file" with the paths `.github/workflows/1-create-keystore.yml` and `.github/workflows/2-build-aab.yml`).
3. Repo → **Settings → Pages** → Source: "Deploy from a branch" → Branch `main`, folder `/docs` → Save.
   Your Privacy Policy will be at `https://YOUR-USERNAME.github.io/rickshaw-rush/` (open `docs/index.html`, replace `YOUR_EMAIL_HERE` first).

## B. Create the signing key (once, keep it forever)
1. Repo → **Settings → Secrets and variables → Actions → New repository secret**
   Name `KEYSTORE_PASSWORD`, value: any strong password you will remember (6+ chars, letters+numbers). Write it down.
2. **Actions** tab → "1 - Create signing key" → **Run workflow**.
3. When done, open the run → download the artifact **KEYSTORE-SAVE-THIS-SAFELY**. Keep `upload.jks` in a safe place (Google Drive + pen drive). If you lose it you cannot update your app normally.
4. Open `upload.jks.base64.txt`, copy all text → make a second secret named `KEYSTORE_BASE64` and paste it.

## C. Build the AAB
1. **Actions** → "2 - Build Play Store AAB" → **Run workflow** (takes ~5-10 min).
2. Download artifact **rickshaw-rush-AAB** → unzip → `rickshaw-rush.aab`. This is the file for Play Store.
3. If the run fails (red ✗), open it, copy the red error text and send it to me.

## D. Test first (important)
Play Console → your app → **Testing → Internal testing → Create release** → upload the AAB → add your Gmail as tester → open the opt-in link on your phone → install and play. Test ads show with Google test IDs.

## E. Turn on real ads (before Production)
1. admob.google.com → Apps → Add app (Android, "published? No" for now) → copy **App ID** (`ca-app-pub-xxx~yyy`).
2. Create two ad units: **Rewarded** and **Interstitial** → copy their IDs (`ca-app-pub-xxx/zzz`).
3. GitHub → Settings → Secrets and variables → Actions → **Variables** tab → New variable `ADMOB_APP_ID` = your App ID.
4. Edit `www/index.html` top: set `IS_TEST = false` and paste your two Ad Unit IDs.
5. Run workflow 2 again → upload the new AAB. Never click your own ads.
6. After the app is live, link it in AdMob (Apps → App settings → Link to Google Play) and add `app-ads.txt` if AdMob asks.

## F. Publish
Use `store-assets/store-listing.md` for text and form answers, `icon-512.png` and `feature-graphic-1024x500.png` for graphics, and your own phone screenshots.
Play Console → Production → Create release → upload AAB → Send for review.
New personal developer accounts may need a closed test (several testers for ~14 days) before Production — Play Console will show this.

## Notes
- Package name is `com.rickshawrush.game` (in `capacitor.config.json`). It must be unique on Play Store and can't change after first upload; change it before the first build if you want.
- Each workflow run raises versionCode automatically, so every new build can be uploaded as an update.
- The launcher icon inside the phone is the default one for now; the store icon is `icon-512.png`.
