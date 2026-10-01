# Asthma Log

A phone-friendly log of asthma medications. It covers:

- Log each dose with the time given, the amount and an optional note.
- See when the next dose is OK. For as-needed medications this comes from the hours between doses; for twice-a-day medications it shows a morning/night checklist.
- See rescue and controller doses per day for the last 7 days.
- Copy a 30-day summary to send to her doctor.
- Switch between light and dark mode.

**Live app:** https://raysantos.github.io/asthma-log/

On a phone, open the link and choose **Add to Home Screen** so it opens like an app.

## How saving works

- **Before Firebase is set up**, doses are saved in the browser on each device. They don't sync, and clearing the browser's data erases them.
- **After Firebase is set up**, everyone who signs in with an approved Google account sees the same log, live.

The repo is public, but it holds only code. Doses and medications live in your Firebase project, and only the Google accounts you list in `firestore.rules` can read or write them.

## One-time Firebase setup (about 10 minutes, free)

1. **Create a project.** Go to https://console.firebase.google.com and click **Create a project**. You can name it something like `asthma-log`. Google Analytics isn't needed.
2. **Turn on the database.** Go to **Build → Firestore Database → Create database**. Choose a US location (for example `us-east1`) and **production mode**.
3. **Turn on Google sign-in.**
   - Go to **Build → Authentication → Get started → Sign-in method → Google → Enable**, then **Save**.
   - Open the **Settings** tab, go to **Authorized domains**, click **Add domain** and enter `raysantos.github.io`.
4. **Connect the app.**
   - In Firebase, go to **Project settings** (gear icon) → **Your apps**. Click the web icon `</>`, give it a nickname and register it. Hosting isn't needed.
   - Copy the `firebaseConfig` values it shows.
   - In this repo, open `firebase-config.js` on GitHub and click the pencil to edit.
   - Replace `window.FIREBASE_CONFIG = null;` with your values. The format is in the example in the file.
   - Commit the change. GitHub Pages updates within a minute or two.
5. **Lock it down.**
   - Copy `firestore.rules` from this repo.
   - Replace `parent1@example.com` and `parent2@example.com` with the Gmail addresses that should have access.
   - In Firebase, go to **Firestore Database → Rules**, paste the edited rules and click **Publish**.
   - Don't commit real addresses here; the repo is public.
6. **Sign in and bring over the existing log.** Open the app and sign in with Google. Then do either of these:
   - Open your private import link (the app address ending in `#import=…`).
   - Or go to the **Medications** tab → **Sync & backup** → **Import backup** and paste a backup.

## Import links

A link of the form `https://raysantos.github.io/asthma-log/#import=<data>` adds the medications and doses it carries to the log on whatever device opens it. The part after `#` stays in the browser and is never sent to GitHub. The app removes it from the address bar right after importing. Importing the same link twice doesn't create duplicates. Keep these links private, because anyone with one can read what's in it.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole app (HTML, CSS and JavaScript in one file) |
| `firebase-config.js` | Your Firebase project's web config (`null` = on-device only) |
| `firestore.rules` | Who may read and write the log; paste into the Firebase console |
| `manifest.webmanifest`, `icon.svg` | Home-screen name and icon |

The next-dose times come from the spacing you enter for each medication. They aren't medical advice. Follow the asthma action plan and the doctor's directions.
