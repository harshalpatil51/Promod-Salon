# SalonKhata

SalonKhata is a Marathi, mobile-first salon ledger. Firebase Authentication uses email and password; Cloud Firestore stores each signed-in owner's shop profile and ledger entries. Vercel builds and hosts the static Vite app.

## 1. Create and configure Firebase

1. Create a Firebase project and register a **Web app** in Project settings.
2. In Authentication, enable **Email/Password** sign-in.
3. Create a Cloud Firestore database.
4. In Project settings, copy the Web app config values into a local `.env.local` file based on `.env.example`. The `VITE_FIREBASE_*` values are public client configuration; access control is enforced by Firebase Authentication and Firestore rules, not by hiding these values.
5. Publish the included Firestore rules:

   ```sh
   npx firebase-tools login
   npx firebase-tools use --add
   npx firebase-tools deploy --only firestore:rules
   ```

Owner photo uploads are currently disabled because Firebase Storage requires upgrading this project from the Spark plan. The rest of the app uses Authentication and Firestore without Storage.

## 2. Run locally

```sh
npm install
cp .env.example .env.local
# Fill .env.local with the Firebase Web app configuration.
npm run dev
```

Use the local URL printed by Vite. Create a test account from the app's sign-in page.

## 3. Deploy to Vercel

1. Push this project to a Git repository and import it into Vercel. Set the project root to `Promod-Salon` if importing the parent repository.
2. Add every `VITE_FIREBASE_*` variable from `.env.example` in Vercel's Environment Variables for Preview and Production.
3. Deploy. `vercel.json` configures the Vite build (`npm run build`) and `dist` output.
4. Add the deployed Vercel domain to Firebase Authentication's Authorized domains.

The Firebase Web config is compiled into the client bundle. Never add a Firebase service-account key or Admin SDK credential to this frontend or to a `VITE_` variable.

## Data access

Each Firebase Auth user can access only their own `users/{uid}` profile and `incomes`, `expenses`, and `credits` documents. The included rules validate ownership and basic record fields. Review and deploy the rules before entering real customer information.
