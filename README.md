# Puffer

A phone-friendly log of asthma medications. It covers:

- Log each dose with the time given, the amount and an optional note.
- See when the next dose is OK. For as-needed medications this comes from the hours between doses; for twice-a-day medications it shows a morning/night checklist.
- See rescue and controller doses per day for the last 7 days.
- Copy a 30-day summary to send to the doctor.
- Switch between light and dark mode.

**Live app:** https://raysantos.github.io/asthma-log/

On a phone, open the link and choose **Add to Home Screen** so it opens like an app.

## Accounts and approval

With Firebase connected, the log is private and everyone has their own login.

- **Sign up:** people open the app, tap **Create an account**, and enter their name, email and a password. They then see a "Waiting for approval" screen and can't see anything in the log.
- **Admin:** `raysandesigns@gmail.com` is the admin, once its email address is verified. The admin sees a **People** section on the **Medications** tab with everyone who has signed up, and can **Approve**, **Deny** or **Remove** them. Removing someone cuts off access right away.
- **Approved people** all see and add to the same shared log.
- **Forgot password:** the sign-in screen sends a reset link by email.
- **Enforced by the database:** `firestore.rules` refuses every read and change from anyone who isn't approved, so the page can't be edited to get around it.

To change the admin, update the email in both `firestore.rules` and the `ADMIN_EMAIL` line in `index.html`.

Without Firebase, the app saves on one device and uses a single password to log doses. That only keeps out casual visitors.

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
6. **Set up the admin.** Open the app, tap **Create an account**, and sign up with the admin email. Open the verification link Firebase emails you, then tap **I've verified my email**.
7. **Add your authorized domains.** In **Authentication → Settings → Authorized domains**, add every address the app runs on (for example the GitHub Pages and Vercel domains).

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
