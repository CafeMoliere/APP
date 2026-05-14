# EduLM — Lycée Montaigne

Static web app inspired by Eduka for **Lycée Montaigne**, Beit Chabab.
Lives alongside the existing `Café Molière` site but is fully self-contained
inside this `/edulm` folder.

## Pages

| File             | Role                                                  |
|------------------|-------------------------------------------------------|
| `index.html`     | Public landing + login (élève / parent / enseignant / admin) |
| `student.html`   | Student & parent portal — schedule, grades, homework, announcements |
| `teacher.html`   | Teacher dashboard — classes, attendance, gradebook, homework |
| `admin.html`     | Admin panel — users, classes, announcements, settings |
| `manifest.json`  | PWA manifest                                          |
| `sw.js`          | Service worker for offline support                    |

All pages render with **demo data** out of the box so the UI is visible
without any backend configuration.

## Wiring up Firebase

Each HTML page contains a `firebaseConfig` block with `REPLACE_ME` placeholders.
To connect the app to a real Firebase project:

1. Create a Firebase project at https://console.firebase.google.com/
2. Enable **Authentication** (Email/Password and/or Microsoft) and **Firestore**.
3. Copy your project's web SDK config and paste the values into every page's
   `firebaseConfig` constant:
   ```js
   const firebaseConfig = {
     apiKey:     "AIzaSy…",
     authDomain: "your-project.firebaseapp.com",
     projectId:  "your-project",
   };
   ```
4. Create a Firestore document `users/{uid}` per account with at least:
   ```json
   { "role": "student" | "teacher" | "admin", "firstName": "…" }
   ```
   The login page reads this to route users to the correct portal.

## Firestore collections used

- `users/{uid}` — `{ role, firstName, … }`
- `students/{uid}` — `{ scheduleToday, recentGrades, homework, announcements, stats }`
- `classes` — `{ name, level, teacher, size }`
- `attendance` — `{ classId, date, student, status }`
- `grades` — `{ classId, student, label, grade, max }`
- `homework` — `{ classId, title, due, details }`
- `announcements` — `{ title, body, audience, classId? }`
- `settings/school` — `{ name, city, year }`

## Deploying

This folder is served under the same Firebase Hosting setup as Café Molière.
After deploy it will be reachable at `https://<your-site>/edulm/`.

If you'd rather host EduLM separately, copy the contents of this folder into a
standalone repo, add a `firebase.json` with `"public": "."`, and run
`firebase deploy`.

## Notes

- Built as plain HTML + vanilla JS + Firebase compat SDK 9.23 to match the
  existing Café Molière pages.
- Visual language: deep navy + gold accents, Playfair Display + DM Sans.
- No build step required — open `index.html` in a browser or serve the folder.
