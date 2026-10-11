# Choor Cabinet shared counters

The GitHub Pages site uses Firebase Cloud Firestore for shared live counts and reports. Firebase client configuration is public by design; no GitHub write token or service-account key is included in the page.

## Firebase project

- Project: `choor-share-lavanya-20261011`
- Database: Cloud Firestore Standard, default database, Mumbai (`asia-south1`)
- Database deletion protection: enabled
- Database free tier: usage quotas apply; reaching a free quota can temporarily stop requests. The project is not configured for automatic paid billing. See [Firestore pricing and quotas](https://firebase.google.com/docs/firestore/pricing).

## Data model

- `members/{member-id}` stores the four shared integer totals: `chappal`, `juta`, `danda`, and `laat`.
- `messageStats/{sha256-id}` stores a daily aggregate for one person, weapon, and displayed message. Repeated identical hits on the same UTC day increment that record.
- The Analysis screen combines those daily records into message/person/weapon totals and a 30-day daily line chart. Message and daily reporting begins when this feature was deployed; previous shared counts remain in the all-time person and weapon totals, but have no message or date history.

## Access and limitations

The rules allow public reads and public writes without sign-in. Each accepted member count write increments exactly one weapon count by one. Message records can only be created at count 1 or incremented by 1; message text is limited to 90 characters. Clients cannot reduce values or delete records. The app writes the member total and message aggregate together in a Firestore transaction.

Because writes are public, automated visitors could inflate counts, add many unique messages, or use up the free quota. Rules limit each record's shape and each increment, but cannot confirm that a real person clicked. These rules are a prototype and should be reviewed and hardened before sharing widely.

Custom message text and the “Show each hit” preference are saved in the current browser. The exact caption displayed on each new hit is recorded in Firestore for shared reports. Leaving the custom-message option off records the comic sound-effect caption shown for that hit.

## Deploying rule updates

From this repository root, sign in to Firebase CLI and run:

```sh
firebase deploy --only firestore:rules --project choor-share-lavanya-20261011
```

The live website is published from `index.html` on GitHub Pages.