# Security Policy

## Supported versions

Security fixes are intended for the current version on the repository's default branch. There is no formal long-term support policy at this time.

## Reporting a vulnerability

Please do **not** publish exploitable security details in a public issue.

Use GitHub's **Private vulnerability reporting** or **Security advisories** feature for this repository, if available. If private reporting is not enabled, contact the repository owner through a private channel listed on the owner's public GitHub profile. Do not assume that a public issue is a private report.

Please include, where possible:

- A concise description and potential impact.
- Steps to reproduce or a minimal proof of concept.
- The affected file, behavior, or configuration.
- Any suggested mitigation.

Do not include real users' data, passwords, sign-in tokens, service-account keys, or private exported backups in the report.

## Security considerations

- The Firebase client configuration is public-facing configuration, not a privileged secret.
- Firebase Authentication and restrictive Realtime Database rules are essential. Never use unrestricted public read/write rules for real data.
- Each user should be limited to their own `/users/{uid}` database node.
- Use a dedicated development Firebase project to test changes, imports, and database-rule changes.
- Treat JSON and CSV exports as private: they may contain study notes, timestamps, and other personal data.
- External Firebase SDK files and the optional public curriculum source depend on third-party services and can change or become unavailable.

## Disclosure

Please allow the maintainer a reasonable opportunity to understand and address a report before publicly disclosing details. This is a small open-source project and does not offer a guaranteed response or remediation timeline.
