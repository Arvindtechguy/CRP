# Enable live grievance sync

The portal is ready to use Cloud Firestore for live grievance updates. Complete these steps before publishing on GitHub Pages.

1. Create a Firebase project at [Firebase Console](https://console.firebase.google.com/), then add a **Web app** to it.
2. Copy the web configuration object into `firebase-config.js`, replacing every `REPLACE_WITH_...` value.
3. In **Build → Authentication → Sign-in method**, enable **Anonymous** authentication. The portal uses this only to establish a Firebase session for its live-sync connection.
4. In **Build → Firestore Database**, create a Cloud Firestore database.
5. For a short private test, publish these Firestore rules:

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /portals/mech-sec-d/grievances/{grievanceId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

6. Publish the `outputs` folder through GitHub Pages. Open the site and go to **Grievance Desk**. The status should change from **Local-only** to **Live sync**.

## Important privacy note

The temporary rules above let any anonymously authenticated visitor to your site read and change grievance records. They make the demo work across devices, but they are **not suitable for a real grievance portal**.

Before sharing the portal widely, replace the current browser-only login with Firebase Authentication using real student/staff accounts and use Firestore Security Rules based on authenticated roles. This is necessary to prevent students from reading or editing records they should not access.
