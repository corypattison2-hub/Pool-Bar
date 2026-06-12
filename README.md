# 🍺 The Tap House — Setup Guide

A keg tracker for your home pub. TV scoreboard + phone control. Free forever.

---

## What you're building

- A website at a real URL (like `your-bar-name.netlify.app`)
- Open it on the TV → "Big Screen" mode (live leaderboard, keg levels, etc.)
- Open it on your phone → "Barkeep's Book" (log pours, manage kegs)
- Both update in real-time
- Anyone with the bar code can view; only **you** can log pours

**Cost:** $0/month forever (within free tiers)

---

## Setup overview (about 30–45 minutes the first time)

There are 3 sites to sign up for, all free:

1. **Firebase** — the database that syncs your devices
2. **GitHub** — stores the code
3. **Netlify** — puts the site online

You don't need to write any code. Just follow the steps.

---

## Part 1: Firebase (the database) — ~15 min

### 1.1 Create a Firebase project

1. Go to **https://console.firebase.google.com**
2. Sign in with your Google account
3. Click **"Create a project"** (or "Add project")
4. **Project name:** something like `tap-house` — click Continue
5. **Google Analytics:** turn it OFF (you don't need it). Click "Create project"
6. Wait ~30 seconds for it to finish, then click **Continue**

### 1.2 Add a web app to the project

1. On your project's main page, look for the **`</>` icon** ("Add an app: Web") and click it
2. **App nickname:** `tap-house-web` — click **Register app** (skip the hosting checkbox)
3. You'll see a code block with `const firebaseConfig = { ... }`. **Copy this whole object** — you'll need it in a minute.
4. Click **Continue to console**

### 1.3 Enable Anonymous Authentication

1. In the left sidebar, click **Build → Authentication**
2. Click **Get started**
3. Click the **Sign-in method** tab
4. Click **Anonymous** in the providers list
5. Toggle **Enable** ON, then click **Save**

### 1.4 Create the Firestore database

1. In the left sidebar, click **Build → Firestore Database**
2. Click **Create database**
3. **Start in production mode** (we'll add proper rules in a sec) — click Next
4. Pick a location closest to you (e.g., `nam5 (United States)` or `eur3 (Europe)`) — click Enable
5. Wait ~30 seconds

### 1.5 Add the security rules

1. Once the database is ready, click the **Rules** tab at the top
2. **Delete everything** in the editor
3. Open the `firestore.rules` file from this project, **copy all the contents**
4. **Paste into the rules editor** in Firebase
5. Click **Publish**

✅ Firebase is done.

---

## Part 2: Add your config to the app — ~3 min

1. Open `index.html` in any text editor (Notepad, TextEdit, VS Code — anything)
2. Search for: `PASTE_YOUR_API_KEY`
3. You'll see this block:
   ```js
   const firebaseConfig = window.__FIREBASE_CONFIG__ || {
     apiKey: "PASTE_YOUR_API_KEY",
     authDomain: "PASTE_YOUR_AUTH_DOMAIN",
     ...
   };
   ```
4. Replace ALL the `PASTE_YOUR_...` values with the values from the Firebase config object you copied in step 1.2
5. **Save the file**

> 💡 If you lost the config, go back to Firebase Console → Project Settings (gear icon top-left) → scroll down to "Your apps" → tap your web app → "SDK setup and configuration" → "Config"

---

## Part 3: GitHub (store the code) — ~5 min

1. Go to **https://github.com** and sign up (free) if you don't have an account
2. Click the **+** in the top-right → **New repository**
3. **Repository name:** `tap-house` (or whatever you want)
4. Set it to **Public** (Netlify free tier needs public, or use private with extra steps)
5. Click **Create repository**
6. On the next page, click **"uploading an existing file"** (the link in the middle of the page)
7. **Drag and drop** these files from this project folder:
   - `index.html` (the one you edited with your Firebase config)
   - `netlify.toml`
   - `firestore.rules`
   - `README.md` (this file)
8. Scroll down, click **Commit changes**

✅ Code is on GitHub.

---

## Part 4: Netlify (put it online) — ~5 min

1. Go to **https://netlify.com** and click **Sign up**
2. **Sign up with GitHub** (easiest — just click "GitHub" and authorize)
3. Once signed in, click **"Add new site" → "Import an existing project"**
4. Click **"Deploy with GitHub"**, then authorize Netlify if asked
5. Find and pick your `tap-house` repo
6. Leave all the build settings as-is (publish directory: `.`, build command: empty)
7. Click **Deploy [your-site-name]**
8. Wait ~1 minute. You'll get a URL like `bouncy-pelican-123abc.netlify.app`

### Optional: rename your site

1. In your Netlify dashboard, click your site
2. **Site configuration** → **Change site name**
3. Pick something like `mike-tap-house` → it becomes `mike-tap-house.netlify.app`

✅ **You're live!** Visit the URL — you should see the pub sign.

---

## Part 5: First-time use

### On your phone (you, the bartender):
1. Open your Netlify URL in the browser
2. Tap **"Open a New Bar"** — you become the bartender forever (on this device/browser)
3. You'll see your **bar code** like `TAPHOUSE-X7K2QB` — **screenshot this**
4. Tap **Barkeep's Book**
5. Tap **"Open the Bar"** to start a session
6. Start logging pours

### On the TV:
1. Cast/mirror your laptop browser to the TV (Chromecast, AirPlay, or HDMI cable)
2. Open your Netlify URL in the laptop browser
3. Type the bar code in the join box → **Join**
4. Tap **The Big Screen**
5. Watch it auto-rotate through Leaderboard / On Tap / Hall of Fame / Ledger

### Friends who want to spectate:
1. Share the bar code with them
2. They open the URL, paste the code, join — they get a viewer-only mode

---

## Updating the app later

Want to add a feature or change something? Edit `index.html` on GitHub (or upload a new version):
- Go to your GitHub repo
- Click `index.html` → pencil icon (Edit)
- Make changes → "Commit changes"
- Netlify auto-redeploys in ~30 seconds

---

## Troubleshooting

### "Setup Needed" warning when I open the site
Your Firebase config wasn't pasted correctly. Re-check `index.html` — the values shouldn't say `PASTE_YOUR_...` anymore.

### "Bar not found" when joining
Bar codes are case-sensitive and exact. The format is `TAPHOUSE-XXXXXX`. If you typed just the 6 chars at the end, the app adds the prefix automatically.

### My phone shows "VIEWER" instead of "BARTENDER"
The bartender role is locked to the device/browser that *created* the bar. If you cleared your browser data, you lost it. Workaround: create a new bar.

### TV doesn't update when I pour from phone
- Make sure both devices are on the same bar code
- Check your internet — the database is in the cloud
- Try refreshing both pages

### I want to start over with a fresh bar
- On the splash screen, click "Leave this bar"
- Tap "Open a New Bar"
- The old bar still exists in the database but is orphaned (you won't lose data, just won't see it)

---

## What it costs

All within free tiers:

- **Firebase Firestore:** 50,000 reads/day, 20,000 writes/day, 1GB storage
- **Firebase Auth:** unlimited anonymous sign-ins
- **Netlify:** 100GB bandwidth/month, unlimited deploys
- **GitHub:** unlimited public repos

You'd need a *very* enthusiastic pub to exceed any of these. If you do, Firebase will email you before charging — they don't auto-bill.

---

## Sláinte! 🍻

Built for your home pub. Tweak it, customize it, make it yours.
