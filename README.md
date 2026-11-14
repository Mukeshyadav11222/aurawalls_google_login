# AuraWalls — Google Login + Video Upload

## Firebase setup (required for real Google login and cloud uploads)
1. Create a project at https://console.firebase.google.com/
2. Add a Web App and copy its Firebase config.
3. Replace the `PASTE_...` values in `index.html`.
4. Firebase Console → Authentication → Sign-in method → enable Google.
5. Authentication → Settings → Authorized domains: add `localhost` and your deployed domain.
6. Firebase Console → Storage → Get started.
7. Set Storage Rules to restrict each user's files to their own account:

rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /users/{userId}/wallpapers/{fileName} {
      allow read: if request.auth != null && request.auth.uid == userId;
      allow write: if request.auth != null && request.auth.uid == userId
        && (request.resource == null ||
          (request.resource.size < 50 * 1024 * 1024 &&
           request.resource.contentType.matches('video/.*')));
    }
  }
}

## Run
Use VS Code Live Server (localhost) or deploy to an HTTPS host such as Firebase Hosting or Vercel. Do not open the module-based page using file://.

## Features
- Google sign-in
- Google profile name, email and photo
- Logout
- Video-only upload (MP4/WebM/MOV, max 50 MB)
- Cloud storage per user
- List, preview and delete uploaded videos

Note: A normal web app can preview videos but cannot set the device's native live wallpaper directly.
