# RightPath® Executive Preview

A lightweight, co-branded (**System & Soul × RightPath®**) taster of the RightPath behavioral assessment.
An executive answers 16 quick questions and instantly sees a directional read on the four core RightPath
factors, their closest blended profile, a Change & Drive Index, and a team-mapping view. Facilitators can run
a live team session where multiple people take it and results aggregate on a real-time dashboard.

> **This is a directional preview, not the validated instrument.** The official RightPath® Profile is a
> psychometrically validated assessment delivered through a certified practitioner. RightPath® is a registered
> trademark of RightPath Resources. Get their sign-off before using their mark publicly.

---

## What's in this repo

| File | Purpose |
|------|---------|
| `index.html` | The entire app — HTML, CSS, and JavaScript in one self-contained file. This is all GitHub Pages needs. |
| `README.md` | This file. |

---

## Features

- **16-question preview** scoring the four RightPath factors: Control & Agenda, Interaction, Conflict & Pace, Order & Detail.
- **Nearest of 16 blended profiles**, with a baseline overlay so the user sees how closely they match.
- **Change & Drive Index** (0–100) with Drive and Change-Readiness sub-scores.
- **Team view** — quadrant coverage across the four styles (Prescriptive, Motivating, Methodical, Collaborative).
- **Live team sessions** — share one link; results roll into a live dashboard with style distribution, the team mapped, average factor lean, and blind-spot insights.

---

## Two ways it runs

**1. Local demo mode (default — zero setup)**
Just open `index.html`. Everything works, including team sessions — but a "session" only aggregates people
taking it on the **same device/browser** (use *"+ Add someone on this device"* on the dashboard). Great for a
kiosk, a passed tablet, or quick testing.

**2. Live sync mode (real shared link across devices)**
Connect a free Firebase project (below). Then anyone with the invite link can take it on **their own device**
and the dashboard updates in real time. This requires hosting the file at a URL — that's what GitHub Pages is for.

---

## Publish on GitHub Pages

### Option A — No command line (easiest)

1. Create a free account at [github.com](https://github.com) if you don't have one.
2. Click **+ → New repository**. Name it (e.g. `rightpath-preview`), set it **Public**, and create it.
3. On the repo page, click **Add file → Upload files**. Drag in `index.html` (and `README.md`). Click **Commit changes**.
4. Go to **Settings → Pages**.
5. Under **Build and deployment → Source**, choose **Deploy from a branch**. Set branch to **`main`** and folder to **`/ (root)`**. Click **Save**.
6. Wait ~1 minute, then refresh. Pages will show your live URL:
   `https://YOUR-USERNAME.github.io/rightpath-preview/`

That URL is your shareable link. Done.

### Option B — Command line (git)

```bash
# from the folder that contains index.html and README.md
git init
git add index.html README.md
git commit -m "RightPath executive preview"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/rightpath-preview.git
git push -u origin main
```

Then enable Pages exactly as in steps 4–6 above.

---

## Enable live sync (Firebase) — about 5 minutes

Live sync needs a tiny backend. Firebase Firestore is free, has no server to run, and updates in real time.

1. Go to the [Firebase console](https://console.firebase.google.com) → **Add project** (accept defaults; you can skip Analytics).
2. In the left menu: **Build → Firestore Database → Create database → Start in test mode → Enable.**
3. Click the **gear icon → Project settings**. Scroll to **Your apps**, click the **web** icon `</>`, register an app (any nickname).
4. Firebase shows a `firebaseConfig = { ... }` snippet. Copy the values.
5. Open `index.html`, find `const FIREBASE_CONFIG = {` near the top of the `<script>`, and paste your values in, e.g.:

   ```js
   const FIREBASE_CONFIG = {
     apiKey: "AIza...",
     authDomain: "your-project.firebaseapp.com",
     projectId: "your-project",
     appId: "1:1234567890:web:abc123"
   };
   ```

6. Re-upload `index.html` to GitHub (Add file → Upload, or `git add/commit/push`). Pages redeploys automatically.

When the dashboard shows a green **"Live · syncing across devices"** badge, it's working.

### Lock down your database (recommended before real use)

Test mode leaves the database open and expires after 30 days. Replace the rules in
**Firestore → Rules** with a version that lets people submit and read results but not edit or delete them:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /rp_sessions/{session}/responses/{doc} {
      allow read, create: if true;     // anyone can submit and view the aggregate
      allow update, delete: if false;  // no edits or deletions
    }
  }
}
```

This is fine for a sales demo. For real client data, add Firebase Authentication and tighten further —
responses are otherwise readable by anyone who knows (or guesses) a session name.

---

## How the invite links work

- **Respondent link:** `…/index.html?s=SESSION-CODE` — opens the assessment already joined to a session.
- **Dashboard link:** `…/index.html?s=SESSION-CODE&view=dashboard` — opens the live facilitator dashboard.

The facilitator screen inside the app generates and copies these for you, plus a QR code.

---

## Customizing

- **Branding / colors:** edit the CSS variables in the `:root { … }` block near the top.
- **Booking CTA:** the "Book a team walkthrough" button has a placeholder `alert()` — replace it with your link.
- **Profile baselines:** the `sig:[C,I,P,O]` arrays in the `PROFILES` list are approximations. Drop in
  RightPath's official per-profile factor values when you have them to make the baseline diamonds and the
  matching reflect the real instrument.
- **Questions / scoring:** see `QUESTIONS`, `bestProfile()`, and `changeDrive()` in the `<script>`.

---

*Built with System & Soul. RightPath® is a registered trademark of RightPath Resources.*
