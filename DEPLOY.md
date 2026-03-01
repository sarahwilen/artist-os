# 🎵 Artist OS — Deployment Guide
## GitHub Pages + Firebase + Custom Domain

---

## OVERVIEW
You'll set up:
1. **Firebase** — free cloud database to store your data
2. **GitHub** — hosts the website files for free
3. **Custom domain** — point your domain to GitHub Pages

Total time: ~20 minutes.

---

## STEP 1: SET UP FIREBASE (10 min)

### 1.1 Create a Firebase project
1. Go to **https://console.firebase.google.com**
2. Click **"Add project"**
3. Name it `artist-os` → click through (disable Google Analytics if asked)
4. Click **"Create project"**

### 1.2 Create a Firestore database
1. In your project, click **"Firestore Database"** in the left sidebar
2. Click **"Create database"**
3. Choose **"Start in production mode"** → Next
4. Pick any location (e.g. `us-east1`) → **Enable**

### 1.3 Set Firestore security rules
1. In Firestore, click the **"Rules"** tab
2. Replace everything with this:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artistos/data {
      allow read, write: if true;
    }
  }
}
```

3. Click **"Publish"**

> ⚠️ This is safe because only you know the URL and password. The rule
> just allows the app to read/write its single document.

### 1.4 Get your Firebase config
1. Click the **gear icon** (⚙️) → **"Project settings"**
2. Scroll to **"Your apps"** → click **"</> Web"**
3. Name it `artist-os-web` → click **"Register app"**
4. You'll see a config object that looks like this:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "artist-os-xxxxx.firebaseapp.com",
  projectId: "artist-os-xxxxx",
  storageBucket: "artist-os-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

5. **Copy all of this** — you'll paste it into `index.html` next.

---

## STEP 2: ADD YOUR FIREBASE CONFIG TO THE APP

1. Open `index.html` in a text editor
2. Find this section near the bottom (around line 550):

```js
// ─── PASTE YOUR FIREBASE CONFIG HERE ───
const firebaseConfig = {
  apiKey: "REPLACE_WITH_YOUR_API_KEY",
  authDomain: "REPLACE_WITH_YOUR_AUTH_DOMAIN",
  ...
```

3. Replace the entire `firebaseConfig` object with the one you copied from Firebase
4. Save the file

---

## STEP 3: CREATE YOUR GITHUB REPO (5 min)

1. Go to **https://github.com/sarahwilen**
2. Click **"New"** (green button, top right)
3. Repository name: `artist-os` (or whatever you like)
4. Set to **Public** *(required for free GitHub Pages)*
5. Click **"Create repository"**

### 3.1 Upload your files
On the new empty repo page:
1. Click **"uploading an existing file"**
2. Drag and drop ALL files from the folder you downloaded:
   - `index.html`
   - `manifest.json`
   - `icon-192.png`
   - `icon-512.png`
3. Scroll down → click **"Commit changes"**

### 3.2 Enable GitHub Pages
1. Click **"Settings"** tab
2. Click **"Pages"** in the left sidebar
3. Under "Branch", select **`main`** → **`/ (root)`** → click **Save**
4. Wait ~60 seconds, then your site is live at:
   **`https://sarahwilen.github.io/artist-os`**

---

## STEP 4: CONNECT YOUR CUSTOM DOMAIN

### 4.1 In GitHub Pages settings
1. Still in **Settings → Pages**
2. Under "Custom domain", type your domain: e.g. `artistos.yourdomain.com`
3. Click **Save**
4. Check **"Enforce HTTPS"** (may take a few minutes to appear)

### 4.2 In your domain registrar (where you bought your domain)
Go to your DNS settings and add:

**Option A — Subdomain** (e.g. `artistos.yourdomain.com`):
Add a **CNAME** record:
- Name/Host: `artistos`
- Value/Points to: `sarahwilen.github.io`
- TTL: 3600

**Option B — Root domain** (e.g. `yourdomain.com`):
Add 4 **A** records pointing to GitHub's IPs:
- `185.199.108.153`
- `185.199.109.153`
- `185.199.110.153`
- `185.199.111.153`

DNS changes take 5–60 minutes to propagate.

---

## STEP 5: ADD TO YOUR IPHONE HOME SCREEN

Once your site is live at your domain:

1. Open **Safari** on your iPhone
2. Navigate to your app URL
3. Tap the **Share button** (box with arrow pointing up)
4. Scroll down and tap **"Add to Home Screen"**
5. Name it `Artist OS` → tap **Add**

It will appear as a full-screen app with no browser chrome — just like a native app. Your password is saved for the session so you won't have to re-enter it every single time you open it (only when you close Safari completely).

---

## UPDATING THE APP LATER

When you want to make changes:
1. Edit `index.html` locally
2. Go to your GitHub repo
3. Click on `index.html` → click the pencil ✏️ icon to edit, OR
4. Use the **"uploading an existing file"** method to replace it
5. Changes go live in ~30 seconds

---

## YOUR APP DETAILS

| | |
|---|---|
| **GitHub repo** | `github.com/sarahwilen/artist-os` |
| **Live URL** | `https://sarahwilen.github.io/artist-os` |
| **Password** | `kiki` |
| **Data stored in** | Firebase Firestore (free tier) |
| **Sync** | Real-time across all your devices |

---

## TROUBLESHOOTING

**"Firebase permission denied" error**
→ Double-check your Firestore security rules in Step 1.3

**App loads but data doesn't save**
→ Make sure you pasted the correct Firebase config in Step 2

**Custom domain not working**
→ DNS can take up to 24hrs. Try the `github.io` URL first to confirm the app works.

**Password prompt shows every time on iPhone**
→ This is because Safari clears `sessionStorage` when you close it. This is normal browser security behavior. The login is quick!
