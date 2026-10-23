# Contributing to DSA Journey Tracker

Thank you for considering a contribution. Useful bug reports, documentation fixes, accessibility improvements, and focused code changes are welcome.

## Before you start

- Check existing issues and pull requests to avoid duplicating work.
- For a substantial feature or architectural change, open an issue first to discuss the problem and approach.
- Be respectful and follow the project's [Code of Conduct](CODE_OF_CONDUCT.md).

## Report a bug

Please include:

1. A clear summary of what happened and what you expected.
2. Steps to reproduce the issue.
3. Browser and operating-system details, if relevant.
4. Console errors with private information removed.
5. Screenshots or a short recording when they help explain a UI issue.

Never attach Firebase service-account keys, authentication tokens, private JSON/CSV backups, or personal study notes to a public issue. For a security concern, follow [SECURITY.md](SECURITY.md) instead.

## Suggest a feature

Describe the problem you are trying to solve, who benefits, and what a useful solution would look like. Focus on user outcomes rather than only a proposed implementation.

## Development setup

1. Fork or clone the repository.
2. Create a separate Firebase project for development and configure Google Authentication and Realtime Database.
3. Apply the owner-only database rules documented in the README.
4. Put your web app's configuration into the local `firebaseConfig` object.
5. Run a local static server from the directory containing `index.html`, for example:

   ```bash
   python -m http.server 5500
   ```

6. Open `http://localhost:5500` and test the changed behavior using non-sensitive test data.

The current implementation is a static HTML/CSS/vanilla-JavaScript app. It has no required compile or package-install step.

## Pull request checklist

- [ ] The change is focused and has a clear purpose.
- [ ] The app loads without JavaScript syntax errors.
- [ ] Relevant UI flows have been tested in a local browser.
- [ ] Firebase sign-in and data access still follow the expected user-isolation rules, if affected.
- [ ] Import/export and backup behavior has been tested, if affected.
- [ ] Responsive layout and keyboard interactions have been considered, if affected.
- [ ] Documentation is updated where behavior or setup changes.
- [ ] No service-account keys, secrets, personal backups, or sensitive user data are included.
- [ ] Third-party data or code has appropriate attribution and license compatibility.

## Code and design guidelines

- Preserve the web-first architecture unless an alternative is agreed upon before implementation.
- Prefer straightforward, readable browser JavaScript over unnecessary dependencies.
- Validate and safely render user-provided text.
- Keep Firebase access limited to the signed-in user's own data.
- Do not weaken database rules to make a feature appear to work.
- Explain meaningful trade-offs and manual test steps in the pull request description.

## License of contributions

By submitting a contribution, you agree that it may be distributed under the project's MIT License. You retain ownership of your contribution; please submit only work you have the right to contribute.
