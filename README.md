# Weather Operations Center Shift Checklist

## Included functionality

- Separate **Day Shift (0600–1800)** and **Night Shift (1800–0600)** checklists.
- Date-specific local browser storage: a checklist is retained on the browser for each selected date and shift.
- Fillable fields for shift supervisor, duty forecasters, and radio checks on 123.225 MHz and 225.05 MHz.
- Radio-check fields for initials, status, and line clarity.
- Completion count and progress bar.
- Client-side **Download Completed PDF** button. The PDF includes all tasks, their complete/open status, entered personnel, radio-check data, notes, generation time, selected date, and selected shift.
- An optional Firebase Realtime Database integration hook for real-time synchronization across multiple users.

## GitHub Pages deployment

1. Create an approved GitHub repository.
2. Upload `index.html` to the repository root.
3. In the repository settings, configure GitHub Pages to publish from the branch/folder that contains `index.html`.
4. Open the published URL and test a Day Shift and a Night Shift checklist.

The site works as a local-only checklist without any additional configuration. Browser-local entries do not appear for other users.

## Enable live multi-user synchronization

GitHub Pages alone is static hosting; it does not persist or broadcast form changes. The supplied page contains an **optional** Firebase Realtime Database client integration.

Before enabling it:

- Obtain organizational approval for GitHub Pages, the chosen database, authentication, access controls, and records handling.
- Do not use a public or unapproved backend for CUI, credentials, or sensitive operational data.
- Configure authentication and restrictive database rules. Do not leave the database in an open test mode.
- Decide who may read/write active shift checklists and what retention/export process is required.

Then edit the `realtimeConfig` block at the top of the script in `index.html`:

```javascript
const realtimeConfig = {
  enabled: true,
  databaseRoot: 'weatherOperationsCenterChecklists',
  firebaseConfig: {
    apiKey: 'YOUR_VALUE',
    authDomain: 'YOUR_VALUE',
    databaseURL: 'YOUR_VALUE',
    projectId: 'YOUR_VALUE',
    appId: 'YOUR_VALUE'
  }
};
```

The page writes each date/shift to a separate database location:

```text
weatherOperationsCenterChecklists/YYYY-MM-DD/day
weatherOperationsCenterChecklists/YYYY-MM-DD/night
```

When enabled and authorized, edits are sent after each input event and connected viewers receive the updated state. If two users edit the **same exact field** concurrently, the later saved value wins. Assigning separate tasks/fields to individual operators minimizes this conflict.

## Operational notes

- The source checklist text was retained in substance. Obvious spelling and punctuation issues were normalized for readability; no unprovided mission links were added.
- Conditional items remain visible every day and are labeled (for example, Wednesday-, Thursday-, Sunday-, and EST-only items). The shift supervisor should determine applicability.
- The PDF download is generated in the user’s browser; a static GitHub Pages site cannot independently archive the file to a server.
- The page uses no externally hosted library for PDF generation. The optional real-time mode loads Firebase scripts only after an approved Firebase configuration is enabled.
