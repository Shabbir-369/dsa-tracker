# DSA Journey Tracker

<p align="center">
  <strong>Your personal learning log for Data Structures & Algorithms.</strong><br />
  Track the time you invest, the problems you practise, and the progress you make — one session at a time.
</p>

<p align="center">
  <a href="LICENSE"><img alt="MIT License" src="https://img.shields.io/badge/License-MIT-22c55e.svg"></a>
  <img alt="Vanilla JavaScript" src="https://img.shields.io/badge/JavaScript-Vanilla-f7df1e.svg?logo=javascript&logoColor=111111">
  <img alt="Firebase" src="https://img.shields.io/badge/Backend-Firebase-ffca28.svg?logo=firebase&logoColor=111111">
  <img alt="Static web app" src="https://img.shields.io/badge/Deployment-Static%20Web%20App-0ea5e9.svg">
</p>

DSA Journey Tracker is a responsive, browser-based workspace for building a consistent problem-solving habit. It combines an editable DSA curriculum, a study timer, a searchable learning history, progress visualizations, and cloud synchronization through your own Firebase project.

**The goal is simple:** make your learning visible, keep a trustworthy record of the work you do, and make it easier to decide what to study next.

> **Open source:** Use, study, modify, and redistribute this project under the [MIT License](LICENSE). Keeping the original copyright and license notice is required by the license. A visible credit and a link back to the original project are also greatly appreciated.

---

## Contents

- [Highlights](#highlights)
- [Screens and workflows](#screens-and-workflows)
- [Technology](#technology)
- [How data is stored](#how-data-is-stored)
- [Quick start](#quick-start)
- [Firebase setup](#firebase-setup)
- [Configure the app](#configure-the-app)
- [Run locally](#run-locally)
- [Deploy](#deploy)
- [Everyday usage](#everyday-usage)
- [Backups and data recovery](#backups-and-data-recovery)
- [Privacy and security](#privacy-and-security)
- [Troubleshooting](#troubleshooting)
- [Project structure](#project-structure)
- [Contributing](#contributing)
- [Credits and attribution](#credits-and-attribution)
- [License](#license)

## Highlights

### Progress dashboard

- Study time for today, the current week, and all time.
- Current and longest study streaks.
- A 26-week activity heatmap.
- Daily study-time bars for the current week and a 12-week learning-velocity view.
- Curriculum coverage, topic progress, latest problem statuses, recent activity, and a **Revisit Later** queue.

### Editable curriculum

- Organize material into topics, sub-topics, and individual problems.
- Add, edit, delete, and reorder topics, sub-topics, and problems.
- Store difficulty, practice links, tags, and personal notes.
- Search across the curriculum and start a session directly from a problem.
- Edit the curriculum in the built-in JSON editor for bulk changes.
- Load or synchronize the Take U Forward / Striver A2Z catalogue from the configured public JSON source, when available. The app has a starter curriculum as a fallback for initial setup.

### Focus timer and journey log

- Start a timed session for a specific problem or a general study task.
- Pause, resume, complete, or discard a live session.
- Manually record past sessions with a date, time, and duration.
- Attach notes and a status to a session.
- Edit previous entries or move them to Trash and restore them later.
- Filter and search history by text, topic, session type, status, and date range.
- Preserve an in-progress timer across refreshes; a local browser copy is kept as a recovery aid.

### Settings and data tools

- Customize problem-status labels.
- Export the curriculum and session data as JSON, or session history as CSV.
- Import a compatible JSON export. A safety snapshot is downloaded before the import is applied.
- Rebuild the latest problem statuses from recorded session history.
- Run a data-integrity audit for common consistency issues.
- Use global search with **Ctrl+K** (or **Cmd+K** on macOS) or **/**.

### Account and sync

- Sign in using Google through Firebase Authentication.
- Keep each account's app records under its own Firebase user ID.
- Synchronize curriculum, sessions, timer state, and settings through Firebase Realtime Database.
- See the current database connection state in the interface.

## Screens and workflows

| Area | What it is for |
|---|---|
| **Overview** | Review consistency, study time, coverage, and work to revisit. |
| **Curriculum** | Manage topics, sub-topics, problems, links, tags, and notes. |
| **Journey Log** | Search, filter, review, edit, restore, or move session entries to Trash. |
| **Live Session** | Track a focused study session and save it to the log. |
| **Settings & Backup** | Manage statuses, synchronize the A2Z catalogue, import/export data, rebuild statuses, and audit data. |

## Technology

| Technology | Role |
|---|---|
| HTML5 | Application markup |
| CSS | Responsive layout and styling |
| Vanilla JavaScript | Application logic and rendering |
| Firebase Authentication | Google sign-in |
| Firebase Realtime Database | Per-user cloud data and synchronization |
| Firebase Web SDK compat CDN (`10.13.1`) | Browser integration with Firebase |
| Public A2Z JSON source | Optional curriculum initialization and synchronization |

The current implementation is a **static web app**. It does not require a Node.js server, bundler, package installation, or compilation step. Firebase SDK files are loaded from Google's CDN.

## How data is stored

The app uses the signed-in Firebase user's UID as its data boundary. Its database root is:

```text
users/{uid}/
├── curriculum/        # Topics, sub-topics, problem metadata
├── sessions/          # Study history and entry status
├── activeTimer/       # In-progress timer state
├── settings/
│   ├── statuses/      # Custom problem-status labels
│   └── statusSemantics/
└── meta/               # Curriculum initialization/source metadata
```

The exact records evolve with the application. The active timer also uses local browser storage for recovery. **That local timer copy is not a complete offline database or a substitute for exporting a backup.**

## Quick start

### What you need

- A modern browser with JavaScript and internet access enabled.
- A Firebase project you control.
- Google sign-in enabled in Firebase Authentication.
- A Firebase Realtime Database with owner-only rules configured.
- A local static web server for development (for example, Python's built-in HTTP server).

### Steps

1. Put the application file in the repository root and name it `index.html` (or adjust the instructions to match your actual filename).
2. Complete the [Firebase setup](#firebase-setup) below.
3. Copy your own web-app configuration into the `firebaseConfig` object as described in [Configure the app](#configure-the-app).
4. Publish the [Realtime Database security rules](#4-publish-realtime-database-rules) before using real data.
5. [Run locally](#run-locally), sign in, create a test session, reload the page, and test export/restore before using it for your main study history.

## Firebase setup

The application uses **your Firebase project**, not a shared project. You control its database, access rules, authentication settings, and usage/billing.

### 1. Create a Firebase project and web app

1. Open the [Firebase Console](https://console.firebase.google.com/).
2. Create a project or select a separate project for this app.
3. Open **Project settings → General**.
4. Under **Your apps**, register a **Web app** if one is not already registered.
5. Keep the SDK configuration from the web app handy for the next section.

### 2. Enable Google Authentication

1. In the Firebase Console, open **Authentication**.
2. Open **Sign-in method** or **Sign-in providers** (the label may vary).
3. Enable **Google** and provide a support email if prompted.
4. Open Authentication settings and find **Authorized domains**.
5. Make sure `localhost` and the hostname where you will deploy the app are authorized.

For example, if your site is `https://yourname.github.io/dsa-journey-tracker/`, the authorized hostname is `yourname.github.io` — not the full URL with the path. Use the actual hostname of your own deployment.

### 3. Create a Realtime Database

1. Open **Realtime Database** in the Firebase Console.
2. Create the database and select the appropriate location.
3. Copy the full database URL exactly as shown by Firebase. The host format depends on your Firebase database and region.
4. Do not leave the database using open test rules for a public deployment.

### 4. Publish Realtime Database rules

The app stores records underneath `/users/{uid}`. In **Realtime Database → Rules**, review and publish rules equivalent to the following:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        ".write": "auth != null && auth.uid === $uid"
      }
    }
  }
}
```

These rules let a signed-in user read and write only their own `/users/{uid}` node. They are an important starting policy for the data layout in this project; review and test them if you change the database structure or add new access patterns.

**Never use public rules such as `".read": true` or `".write": true` for personal data.** Client-side Firebase configuration is not a replacement for database security rules.

### 5. Register local and deployed domains

In Firebase Authentication's **Authorized domains**, add every hostname you use, including `localhost` for local development and your deployed hostname. Save the settings and allow a short time for configuration changes to take effect.

## Configure the app

Open `index.html` and find the `firebaseConfig` object near the start of the main JavaScript block. Replace the example strings with values copied from **Firebase Console → Project settings → General → Your apps → SDK setup and configuration**.

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  databaseURL: "YOUR_DATABASE_URL_FROM_FIREBASE_CONSOLE",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET_FROM_FIREBASE_CONSOLE",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
  // measurementId is optional for this app; add it only if Firebase provides it.
};
```

Important notes:

- Use the values from **your own** Firebase web app. Do not invent the database URL or project identifiers.
- Keep the syntax valid: quoted strings, commas between properties, and a closing `};`.
- The application needs `apiKey`, `authDomain`, `databaseURL`, `projectId`, and `appId` to initialize correctly.
- `measurementId` is optional for the current app. If you add it, include a valid quoted value and ensure the object remains valid JavaScript.
- Firebase's web-app configuration is expected to be present in a browser application. It identifies the project; it is **not** a server credential. Never commit Firebase service-account JSON, private keys, OAuth client secrets, or other privileged credentials.

> **Before publishing:** check that the `firebaseConfig` block in your current `index.html` contains valid JavaScript. An unfinished value, such as `measurementId: "`, prevents the script from parsing and the application from starting.

## Run locally

Run commands from the folder containing `index.html`.

### Option A: Python

```bash
python -m http.server 5500
```

Open [http://localhost:5500](http://localhost:5500) in your browser. Keep the terminal running while you test the app.

### Option B: Visual Studio Code

1. Open the project folder in VS Code.
2. Start a static web server using an extension such as Live Server.
3. Open the local URL it provides.

Using a local HTTP server is recommended over opening the file with a `file://` URL, especially for authentication and origin handling.

## Deploy

The current application can be hosted by GitHub Pages, Firebase Hosting, Netlify, or another static-file host. You do not need to run a backend server for the app itself.

### GitHub Pages

1. Commit `index.html`, `README.md`, `LICENSE`, and the other project files to your GitHub repository.
2. Open **Settings → Pages** in the repository.
3. Choose the desired deployment source and branch. If `index.html` is in the repository root, choose the root folder.
4. Save and wait for GitHub Pages to publish the site.
5. Add the site's hostname to Firebase Authentication's **Authorized domains**.
6. Visit the published website and test Google sign-in, creating/saving a session, reloading, and exporting data.

A typical GitHub Pages URL looks like `https://USERNAME.github.io/REPOSITORY/`. Replace both parts with your own account and repository names; this project does not assume a particular repository URL.

### Firebase Hosting

1. Follow the official [Firebase Hosting documentation](https://firebase.google.com/docs/hosting) to install and configure the Firebase CLI.
2. Set the public directory to the folder containing `index.html`.
3. Deploy the static files using the commands generated by the Firebase setup flow.
4. Add the deployed hostname to Firebase Authentication's authorized domains and test the full sign-in/data flow.

For every hosting provider, confirm that the Realtime Database rules are secure and that the configuration points to the Firebase project you intended to use.

## Everyday usage

1. **Sign in:** Select **Continue with Google** and use the Google account you want associated with this study history.
2. **Choose a problem:** Open Curriculum, search for the problem, and start a session from its row — or use **Start Session** to log general study.
3. **Focus:** Pause or resume the timer as needed. When finished, stop and save the session, review the details, and add notes about what you learned.
4. **Review:** Use Overview to check trends and the Journey Log to find earlier work, edit entries, or mark items to revisit.
5. **Protect your data:** Export JSON backups regularly, particularly before large edits or imports.

### Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+K` / `Cmd+K` | Open global search |
| `/` | Open global search when focus is not in a text field |
| `N` | Open the new live-session flow |
| `J` | Open Journey Log |
| `C` | Open Curriculum |
| `T` | Open Live Session |

Shortcuts that use letter keys are ignored while typing in input, textarea, and select controls.

## Backups and data recovery

### Export a backup

Open **Settings & Backup**:

- **Export JSON** includes curriculum, sessions, and status settings.
- **Export CSV** exports the session history in a spreadsheet-friendly format.

Keep the downloaded files somewhere private and safe. JSON backups can contain personal notes and a detailed record of your study habits.

### Import a JSON backup

1. Open **Settings & Backup → Import JSON**.
2. Select a compatible app export.
3. Read the confirmation message and check which records will be merged.
4. Confirm only when you are ready. The app downloads a snapshot of the current data before applying the import.

Import is a **merge**, not a clean replacement of all existing records. Matching IDs can be overwritten field-by-field, so keep the safety snapshot and inspect the imported result.

### Trash and history

Deleting a journey entry from the interface moves it to Trash, where it can be restored. This is useful protection against accidental deletion, but it does not replace a separate backup. Curriculum deletions do not automatically erase historical journey entries.

## Privacy and security

- **Use your own Firebase project.** You control the cloud storage, database location, authentication, rules, and billing settings.
- **Keep database rules restrictive.** The included rules limit each authenticated UID to its own data node. Re-check them whenever database paths change.
- **Do not publish private backups.** JSON/CSV exports may include personal notes, timestamps, durations, and the problems you have studied.
- **Never commit privileged credentials.** Service-account keys and OAuth client secrets must not be placed in the repository or client-side code.
- **Treat the web config correctly.** Firebase web configuration is public-facing client configuration. Its presence alone does not grant safe access; security depends on Authentication, rules, and project settings.
- **Understand offline limits.** The app preserves timer state locally as a recovery aid, but cloud data still requires network access. Do not assume all actions work offline.
- **Review third-party dependencies.** The app loads Firebase SDK scripts from Google's CDN and can retrieve A2Z catalogue data from an external public repository. Those services have their own availability and policies.
- **Use a development project for experiments.** Test rules, imports, and code changes with non-sensitive data before using your primary study history.

This project is provided as-is. The repository's owner cannot access or administer Firebase projects created by users of the project, and cannot recover a user's database or backups.

## Troubleshooting

| Issue | What to check |
|---|---|
| The app reports an incomplete Firebase configuration | Check that the configuration object is valid JavaScript and contains `apiKey`, `authDomain`, `databaseURL`, `projectId`, and `appId`. |
| The page is blank or the app does not start | Open the browser developer console. Check for syntax errors, especially in the `firebaseConfig` object. |
| Google sign-in reports `auth/unauthorized-domain` | Add the current hostname under Firebase Authentication → Settings → Authorized domains. Add `localhost` for local development. |
| A sign-in popup is blocked or fails on mobile | Allow popups or retry. The app attempts redirect sign-in for certain popup-related failures. |
| Realtime Database reports `PERMISSION_DENIED` | Check the active account, database URL, published rules, and whether the rule allows only the matching `/users/{uid}` node. |
| The app stays offline or changes do not sync | Check the internet connection, Firebase project, exact database URL, browser console, and database rules. |
| The A2Z catalogue does not load | Check connectivity and the configured public JSON source. Initial setup can fall back to the built-in starter curriculum; the catalogue sync can be retried later. |
| Data looks different after signing into another account | Each UID has a separate data location. Use the account that created the data, or import a backup intentionally. |
| A timer remains but other data does not load | The local timer state is only a recovery aid. Restore connectivity and verify the session is saved to Firebase. |
| An import produces unexpected changes | Stop making further changes, keep the downloaded pre-import snapshot, and inspect the imported IDs. Import merges data and may overwrite matching records. |

When opening a public issue, include the browser and a description of the steps to reproduce the problem. **Do not include private exports, sign-in tokens, service-account keys, or personal notes.**

## Project structure

The minimal runtime can be as simple as:

```text
.
├── index.html
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── CITATION.cff
└── .gitignore
```

The HTML file currently contains the markup, styles, and application JavaScript. Add folders and files as the project grows, and keep this tree aligned with the actual repository contents.

## Contributing

Bug reports, thoughtful feature requests, documentation improvements, and pull requests are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) and follow the community standards in [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

In general, please:

1. Explain the problem and the behavior you expect.
2. Keep changes focused and preserve the current web-first architecture unless a change is discussed first.
3. Test affected flows with your own Firebase development project.
4. Do not commit credentials, Firebase service-account files, private exports, or personal notes.
5. Include screenshots or reproduction steps when they clarify a UI change or bug.

## Credits and attribution

This project is authored and maintained as an open-source learning tool by **Shabbir Ezzy**.

If you use or build on this project, the most helpful credit is a mention such as:

> Based on **DSA Journey Tracker** by [Shabbir Ezzy](https://github.com/Shabbir-369).

A link to the original repository in your README, project credits, or release notes is appreciated. The MIT License requires retaining the original copyright and permission notice when redistributing copies or substantial portions, but it does not require a separate visible in-app credit.

The A2Z catalogue is fetched from an external public JSON source configured in the application. The catalogue, external practice links, and any third-party names or marks remain subject to their respective owners' terms. They are not relicensed by the MIT license for this application.

## License

This project is released under the [MIT License](LICENSE). You may use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the software, subject to the license conditions.

Please keep the license and copyright notice with copies or substantial portions of the project. A clear credit and link back to the original project are appreciated and help others discover the source.

---

<p align="center">
  Made for the long game — show up, solve problems, and keep your progress visible.
</p>
