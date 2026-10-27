# 🎅 Family Secret Santa

A one-page app for a family Secret Santa you share on WhatsApp.

**How it works**
1. The organiser opens the site, types everyone's names and gets one link.
2. Share the link in the family WhatsApp group.
3. Each person taps their name, sets a 4-digit PIN and adds **3 gifts (max R500 each)**.
4. When *every* wishlist is in, the **Spin the hat** button unlocks.
5. Each person spins once and lands on someone, with their wishlist. It's **locked forever**: reopening the app (even on a new phone, with their PIN) shows the same person.
6. Nobody can draw themselves, and no two people can draw the same person.

---

## Try it first (2 minutes, no setup)

Open `index.html` in a browser. With no Firebase config it runs in **demo mode**: everything stays in that browser, and "Switch person" lets you play every family member to see the full flow.

---

## Go live (about 15 minutes)

### 1. Firebase (the shared database)
1. Go to https://console.firebase.google.com → **Add project** (the free Spark plan is plenty).
2. **Build → Firestore Database → Create database** → choose **Production mode** and the region closest to you.
3. Open the **Rules** tab, replace everything with the contents of `firestore.rules`, then **Publish**.
4. **Project settings** (⚙️) → **Your apps** → click the **`</>`** (Web) icon → give it a name → **Register app**.
5. Copy the `firebaseConfig` values it shows you and paste them into the top of `index.html`:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123...",
  appId: "1:123...:web:abc..."
};
```

> These values are *meant* to be public in web apps — it's normal that they're in the HTML.

### 2. GitHub
1. Create a new repository, e.g. `secret-santa`.
2. Upload `index.html`, `santa.png`, `firestore.rules` and `README.md` (**Add file → Upload files → Commit**).

### 3. Netlify
1. https://app.netlify.com → **Add new site → Import an existing project → GitHub** → pick the repo.
2. Leave **Build command** empty, set **Publish directory** to `.` (or leave blank) → **Deploy**.
3. Optional: **Site configuration → Change site name** to something friendly like `colombo-santa.netlify.app`.

Every time you change a file on GitHub, Netlify redeploys automatically.

**Santa in the WhatsApp preview:** in `index.html`, find `https://YOUR-SITE.netlify.app/santa.png` near the top and replace `YOUR-SITE` with your real Netlify name. Then WhatsApp shows Santa's face when you paste the link.

### 4. Run your draw
Open your Netlify URL → enter everyone's names → **Create** → **Share on WhatsApp**. Done! 🎄

---

## Handy admin bits (Firebase console → Firestore → `groups` → your group)
- **Someone forgot their PIN:** open `participants → pX` and set `pin` to an empty string. They can set a new one.
- **Redo the whole draw:** set every participant's `drawn` to an empty string.
- **Change the budget or number of gifts:** edit `BUDGET` / `ITEMS` near the top of `index.html`.

## Honest note on secrecy 🤫
This is built on family trust. The draw is fair and atomic (Firebase transactions mean two people can never land on the same name), but the results sit in the database, so a determined, tech-savvy relative *could* dig into it with browser dev tools. For a fully peek-proof version, the draw needs to move into a Firebase Cloud Function (requires the pay-as-you-go Blaze plan, which stays free at this size).
