# Ledger — Daily Expense Tracker

A single-page, static expense tracker that stores your data as a JSON file in a
GitHub repository, so it's available from any device. No backend, no build step —
just one HTML file hosted on GitHub Pages.

## How it works

- The **site** (this `index.html`) can live in any public repo — it contains no
  personal data, so it's safe for it to be public.
- Your **expense data** is stored separately, as `data.json` in a repo of your
  choice. The site reads/writes that file directly through the GitHub API, using
  a Personal Access Token (PAT) you provide once in Settings. The token is saved
  only in your browser's local storage — it is never written into any file or
  committed anywhere.

**Use a private repo for your data.** The site repo can stay public (GitHub Pages
on the free plan requires a public repo), but point Settings at a *separate,
private* repo for `data.json` so your spending isn't publicly readable.

## 1. Create the data repo

1. On GitHub, create a new **private** repository, e.g. `ledger-data`. You can
   leave it empty — the app will create `data.json` in it on first save.

## 2. Create a Personal Access Token

1. Go to https://github.com/settings/personal-access-tokens/new (fine-grained token).
2. Under **Repository access**, choose "Only select repositories" → pick `ledger-data`.
3. Under **Permissions → Repository permissions**, set **Contents: Read and write**.
4. Generate the token and copy it — you'll paste it into the site's Settings panel.

## 3. Deploy the site

1. Create (or reuse) a **public** repository for the site, e.g. `ledger`.
2. Upload `index.html` (and this `README.md`, optional) to the repo root.
3. In the repo, go to **Settings → Pages**, set Source to "Deploy from a branch",
   pick `main` and `/ (root)`, save.
4. Your site will be live at `https://<your-username>.github.io/<repo-name>/`
   within a minute or two.

```
git init
git add index.html README.md
git commit -m "Initial ledger site"
git branch -M main
git remote add origin https://github.com/<your-username>/ledger.git
git push -u origin main
```

## 4. Connect the site to your data repo

1. Open the deployed site, click the ⚙ settings icon.
2. Fill in:
   - **GitHub username** — your username or org
   - **Data repository name** — `ledger-data`
   - **Branch** — `main`
   - **File path** — `data.json`
   - **Personal Access Token** — the token from step 2
   - **Monthly budget** — optional, in ₹
3. Click **Save & connect**. The app will create `data.json` in your data repo
   on the first expense you add (or immediately, if the repo already has one).

From then on, every add/delete commits an update to `data.json` in your data
repo, so opening the site from your phone, laptop, or anywhere else and
re-entering the same settings will show the same data.

## Notes

- If GitHub is unreachable (offline, token expired, etc.), the app keeps working
  against a local browser cache and retries syncing on the next change — use the
  ⇩ download button any time for a manual JSON backup.
- Token scope: keep it limited to the one data repo and "Contents: Read and
  write" only — no need for broader scopes.
- All amounts are in Indian Rupees (₹), formatted with Indian digit grouping.

## Installing it as an app (PWA)

The site is now an installable Progressive Web App — it can sit on your phone's
home screen or in your desktop app dock like a native app, and the interface
loads instantly even with no signal (your data still needs network to sync, but
the app itself opens offline).

**Files this adds**, all of which need to be uploaded to the `ledger` repo
alongside `index.html`:
- `manifest.json` — app name, colors, icons
- `sw.js` — service worker that caches the app shell for offline loading
- `icons/` folder — app icons in the sizes iOS/Android/desktop expect

**To install:**
- **Android / Chrome desktop**: open the site, click the **📲 Install app**
  button in the header (or the install icon in Chrome's address bar) → Install.
- **iOS Safari**: open the site → Share button → **Add to Home Screen**. iOS
  doesn't support the automatic install prompt, so this manual step is required.
- **Desktop (Chrome/Edge)**: same install icon appears in the address bar, or
  use the in-app **📲 Install app** button.

Once installed, it opens in its own window without browser chrome, and the
service worker keeps the last-loaded version cached so it opens instantly even
with the network off — your entries made offline are still saved locally and
sync automatically next time you're online, same as using it in a regular
browser tab.

