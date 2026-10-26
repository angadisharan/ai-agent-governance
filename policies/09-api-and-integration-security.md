# API and Integration Security

- Authenticate and authorise every protected operation server-side.
- Enforce tenant and resource ownership independently of client-supplied identifiers.
- Validate inputs, constrain payload size, and use safe output encoding.
- Apply rate limits, quotas, timeouts, and appropriate abuse controls.
- Protect webhooks with signatures, replay protection, and idempotent handling.
- Do not log credentials, access tokens, payment secrets, or unnecessary personal data.
- Use explicit API versions and backward-compatible contract evolution.
- Validate third-party callbacks and external data as untrusted.
- Keep secrets in approved secret-management systems, never in source or documentation.
- Require approval before calling external services with real customer data or causing external side effects.
