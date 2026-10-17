# Royack Hub (GitHub Pages wrapper)

A one-page wrapper that shows the Hub (a Google Apps Script web app) full-screen in an iframe.
Embedded this way, Google's grey "This application was created by a Google Apps Script user"
banner is not shown.

## Files

| File | What it is |
|---|---|
| `index.html` | Full-viewport iframe of the Hub `/exec` URL, dark background, safe-area padding for notched phones. If the Hub hasn't loaded after ~15 s, an **Open Hub directly** link appears. |
| `manifest.json` | Lets the page be added to the home screen as "Hub" (standalone, dark theme). |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | Home-screen icons. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is. |

## Requirements on the Apps Script side

`doGet()` in `Code.gs` must return its page with
`.setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)` or Google refuses to be framed.
The deployment stays "Execute as: me / Who has access: Only myself", so the app is still owner-only.
The wrapper adds no access; it just frames the same URL.

## Publish

1. Create an empty repo on GitHub (e.g. `royack-hub`) under the nickroyack account. Don't add a README.
2. Push this folder:
   ```
   git remote add origin https://github.com/<user>/royack-hub.git
   git push -u origin main
   ```
3. On GitHub, open **Settings → Pages**, set **Source: Deploy from a branch**, **Branch: `main` / root**, and save.
4. After a minute the site is live at `https://<user>.github.io/royack-hub/`. Open it on the phone, then **Share → Add to Home Screen**.

## If it shows a Google sign-in page or a blank frame

Google's login page can't be shown inside an iframe. Some browsers also block the Google cookies
when script.google.com is framed by another site. Safari on iPhone does this by default, and
Chrome may too, depending on its third-party cookie setting. If that happens:

- Tap **Open Hub directly** and sign in once, then reopen the wrapper.
- Or allow cookies for `script.google.com` / `google.com` in the browser settings.
- With several Google accounts signed in, the Hub's owner account should be the default (first) account.

## Changing the Hub URL

If the Apps Script deployment ID ever changes, replace the `/exec` URL in both places in `index.html`
(the iframe `src` and the fallback link).
