# Choor Cabinet shared counters

The GitHub Pages site uses Firebase Cloud Firestore for shared, live counts. Firebase client configuration is public by design; no GitHub write token or service-account key is included in the page.

## Firebase project

- Project: `choor-share-lavanya-20261011`
- Database: Cloud Firestore Standard, default database, Mumbai (`asia-south1`)
- Database deletion protection: enabled
- Database free tier: 1 GiB stored, 50,000 reads/day, 20,000 writes/day, 20,000 deletes/day, and 10 GiB/month outbound transfer. Quotas are subject to Firebase's current limits. Reaching a free quota can temporarily stop requests; the project is not configured for automatic paid billing.

## Data and protection

Each document in `members/{member-id}` stores the four integer counts: `chappal`, `juta`, `danda`, and `laat`. The page listens for changes so visitors see the same counts across devices.

The rules allow public reads and public writes without sign-in. A write can increase one listed person's one count by exactly 1; clients cannot reduce values or delete documents. The database also has deletion protection enabled.

Because writes are public, automated visitors could inflate counts or use up the free daily quota. The rules limit the shape of each write but cannot prove that a real person clicked.

Old counts previously saved only in individual browsers have not been combined into this database. Only counts added after the shared database connects are shared.

## Deploying rule updates

From this repository root, sign in to Firebase CLI and run:

```sh
firebase deploy --only firestore:rules --project choor-share-lavanya-20261011
```

The live website is published from `index.html` on GitHub Pages.
