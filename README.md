# Asthma Log

A phone-friendly log of asthma medications. It covers:

- Log each dose with the time given, the amount and an optional note.
- See when the next dose is OK. For as-needed medications this comes from the hours between doses; for twice-a-day medications it shows a morning/night checklist.
- See rescue and controller doses per day for the last 7 days.
- Copy a 30-day summary to send to her doctor.
- Switch between light and dark mode.

**Live app:** https://raysantos.github.io/asthma-log/

On a phone, open the link and choose **Add to Home Screen** so it opens like an app.

## Password lock

Anyone can view the log. To log a dose, delete one or change medications, you enter the family password once on each device.

- **With Firebase connected (the shared log):** the password is checked by Firebase's sign-in service, and the database's security rules (`firestore.rules`) refuse any change that doesn't come from the family account. The page itself can't be edited to get around it. The first unlock creates the family account, so enter the password on the live app right after Firebase is connected.
- **Without Firebase:** the page checks a hash of the password. That only keeps out casual visitors.

To lock a device again, go to **Medications → Sync & backup → Lock this device**.

## How saving works

- **Without Firebase**, doses are saved in the browser on each device. They don't sync between phones.
- **With Firebase**, there is one shared log that's saved online. Every phone that opens the app sees the same doses, live.

## One-time Firebase setup (free)

Your Google account needs 2-Step Verification turned on before Firebase will let you in.

1. **Create a project.** Go to https://console.firebase.google.com and click **Create a project** (for example `asthma-log`). Google Analytics isn't needed.
2. **Turn on the database.** Go to **Build → Firestore Database → Create database**. Choose a US location and **production mode**.
3. **Turn on password sign-in.** Go to **Build → Authentication → Get started → Sign-in method → Email/Password → Enable**, then **Save**.
4. **Lock the database.** In Firestore Database → **Rules**, paste the contents of `firestore.rules`, then click **Publish**.
5. **Connect the app.**
   - Go to **Project settings → Your apps → Web (`</>`)**, register the app, and copy the `firebaseConfig` values.
   - Put them in `firebase-config.js`.
6. **Unlock once.** Open the app, enter the family password on the **Dosage** tab, then open your import link to load the existing log.

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
