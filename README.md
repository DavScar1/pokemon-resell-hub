# Pokémon Resell Hub

A shared business hub for tracking Pokémon TCG box reselling: sales, inventory,
cash flow, supplier orders and analytics. Both you and your business partner
sign in with your own account and see the same live-synced data.

Single-file app (`index.html`) — no build step, works straight from GitHub Pages.

## 1. Create the GitHub repo

1. Go to https://github.com/new
2. Repository name: `pokemon-resell-hub` (public or private both work with GitHub Pages, but private Pages requires a paid plan — public is simplest)
3. Do **not** initialize with a README (this folder already has one)
4. Create the repo, then run the commands it shows you, or just tell Claude to push once it's created — the remote will be `https://github.com/<your-username>/pokemon-resell-hub.git`

## 2. Enable GitHub Pages

1. In the repo → **Settings → Pages**
2. Source: **Deploy from a branch**, Branch: `main`, folder: `/ (root)`
3. Save. Your site will be live at `https://<your-username>.github.io/pokemon-resell-hub/`

## 3. Create a Firebase project (free tier is plenty)

1. Go to https://console.firebase.google.com/ → **Add project** → name it e.g. `pokemon-resell-hub`
2. Once created, click the **web icon (`</>`)** to register a web app → name it anything → **do not** check "Firebase Hosting"
3. Copy the `firebaseConfig` object it shows you

## 4. Enable Email/Password auth

1. In the Firebase console → **Build → Authentication → Get started**
2. Enable **Email/Password** sign-in provider

## 5. Create Firestore Database

1. **Build → Firestore Database → Create database**
2. Start in **production mode**, pick any region
3. Go to the **Rules** tab and paste the contents of `firestore.rules` from this folder, then **Publish**
   - This restricts read/write to only the two emails baked into the file. Edit that list if your team changes.

## 6. Wire up the config

Open `index.html`, find this block near the bottom (`PASTE YOUR FIREBASE CONFIG HERE`) and replace it with the config you copied in step 3:

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};
```

## 7. Push to GitHub

```
git init
git add -A
git commit -m "Initial Pokémon Resell Hub"
git branch -M main
git remote add origin https://github.com/<your-username>/pokemon-resell-hub.git
git push -u origin main
```

Wait a minute or two for GitHub Pages to build, then visit
`https://<your-username>.github.io/pokemon-resell-hub/`.

## 8. Invite your partner

Send them the site URL. They click **Sign Up** and create an account with
**their own email/password** (must be one of the emails listed in
`firestore.rules`, e.g. `danielliu581@gmail.com`). As soon as they sign up,
they'll see the same shared Sales Log, Inventory, Cash Flow, Supplier
Calendar and Analytics — everything syncs live between you both.

## Notes

- Data is stored in one shared Firestore document (`workspaces/pokemon-business`),
  so there's no per-user data split — you're both editing the same business records.
- Each sale/order records who logged it (`loggedBy`) so you can see who did what.
- Everything also caches to `localStorage`, so the app still works offline and
  syncs the next time you're online.
