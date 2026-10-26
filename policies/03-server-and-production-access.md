# Server and Production Access Policy

No remote or production access is permitted by default.

Without separate explicit approval, do not SSH, log into cloud consoles, connect to remote databases/clusters, inspect remote logs/processes, execute remote commands, upload/download production files, change infrastructure, or deploy/release/rollback.

Approval must identify:
- Environment and target.
- Exact permitted operations and purpose.
- Authorised identity and credential method.
- Time window and expiry.
- Expected side effects and rollback/recovery plan.
- Required human confirmation.

Do not request credentials in chat or embed them in source code. Prefer short-lived, least-privilege identities and audited access. Production changes must follow an established change-management process.
