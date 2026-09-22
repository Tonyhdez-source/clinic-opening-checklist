# Clinic Opening Checklist

A shared checklist for opening a new Meadows Eye clinic. It has 65 tasks in 8 sections, and each task's due date is counted back from the clinic's target open date. You can set an owner, due date, and notes on each task, mark tasks N/A, add your own tasks, and track several clinics at once.

## Files

| File | What it is |
|---|---|
| `index.html` | GitHub Pages version. Saves to Firebase Firestore and syncs live between everyone using it. |
| `claude-artifact/clinic-launch.html` | Backup of the claude.ai version, which saves to Claude's artifact database. It only saves when opened on claude.ai. |

## Setup (GitHub Pages version)

1. **Firebase config.** In the `SETTINGS` block near the top of `index.html`, paste `apiKey`, `messagingSenderId`, and `appId` from Firebase console → Project settings → Your apps (project `maintenance-tracker-ed411`, the same one the Maintenance Tracker uses). The project ID and domain are already filled in.
2. **Firestore rules.** The checklist stores its data in collections that start with `clinicLaunch_`. Your existing rules need to allow them. For example, add this inside `match /databases/{database}/documents { ... }`:

   ```
   match /clinicLaunch_clinics/{clinicId} {
     allow read, write: if true;
     match /checks/{checkId} {
       allow read, write: if true;
     }
   }
   ```
   Use whatever access condition your maintenance tracker uses in place of `if true`.
3. **Passcode (optional).** Set `PASSCODE = "1234"` (or any code) in the same `SETTINGS` block to add a lock screen.
4. **GitHub Pages.** Settings → Pages → Deploy from branch → `main` / root.

## Data layout

- `clinicLaunch_clinics/{clinicId}`: `{ name, openDate, createdAt }`
- `clinicLaunch_clinics/{clinicId}/checks/{taskId}`: `{ done, doneAt, na, owner, due, note, updatedAt }`. Tasks added by hand also store `{ custom: true, title, section, off }`.

The task template lives in the `SECTIONS` array in `index.html`. Edit it to change the default tasks for every clinic.
