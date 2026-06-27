# 5/3 Workout Tracker — Firebase Setup

## What you need
- A Google account
- Node.js installed (`node -v` to check)
- ~10 minutes

---

## 1. Create a Firebase project

1. Go to https://console.firebase.google.com
2. Click **Add project** → name it (e.g. `531-tracker`) → Continue
3. Disable Google Analytics (not needed) → **Create project**

---

## 2. Enable Firestore

1. In the left sidebar: **Build → Firestore Database**
2. Click **Create database**
3. Choose **Start in production mode** → Next
4. Pick any region close to you (e.g. `europe-west1`) → **Enable**

---

## 3. Enable Google Auth

1. In the left sidebar: **Build → Authentication**
2. Click **Get started**
3. Under **Sign-in providers**, click **Google** → toggle **Enable**
4. Set a **Project support email** (your Gmail) → **Save**

Your data is permanently tied to your Google account — works across all devices.

---

## 4. Get your Firebase config

1. In the left sidebar: **Project settings** (gear icon, top left)
2. Scroll to **Your apps** → click the **</>** (Web) icon
3. Register the app (any nickname, e.g. `tracker-web`)
4. Copy the `firebaseConfig` object — it looks like this:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123:web:abc"
};
```

5. Open `public/index.html` and paste your config, replacing the placeholder block near the top of the `<script type="module">` tag.

---

## 5. Update .firebaserc

Open `.firebaserc` and replace `YOUR_PROJECT_ID` with your actual Firebase project ID
(visible in Project Settings, or in the `projectId` field of your config).

---

## 6. Install Firebase CLI & deploy

```bash
# Install Firebase CLI (once)
npm install -g firebase-tools

# Log in
firebase login

# Inside the workout-tracker folder:
cd workout-tracker

# Deploy Firestore rules + hosting
firebase deploy
```

Firebase will print a **Hosting URL** like:
`https://your-project.web.app`

Open that on your phone — it works like a native app.

---

## 7. Add to your phone home screen (optional but recommended)

**iOS Safari:** Share → Add to Home Screen  
**Android Chrome:** Menu (⋮) → Add to Home Screen

---

## Data structure in Firestore

```
users/
  {uid}/
    data/
      state          ← current week, TMs, logged reps
    history/
      {docId}        ← one doc per completed week
        cycleWeek: 3
        cycleNumber: 1
        weekType: "3s"
        tms: { squat: 100, bench: 80, ... }
        reps: { squat: 5, bench: 4, ... }
        completedAt: "2024-01-15T..."
```

---

## Re-deploying after changes

```bash
firebase deploy --only hosting
```

---

## Troubleshooting

**"Missing or insufficient permissions"** — Firestore rules not deployed yet. Run `firebase deploy --only firestore:rules`.

**"Sign-in failed" popup error** — Make sure Google is enabled in Firebase Authentication and your `authDomain` in the config is correct (`your-project.firebaseapp.com`).

**Popup blocked on mobile** — Add the app to your home screen first, then sign in. Home screen apps handle popups more reliably than the browser.
