# Testing and Quality Policy

Use permitted checks appropriate to the change: formatting, linting, static analysis, unit tests, integration tests with synthetic data, contract tests, security tests, and approved end-to-end tests.

For each change:
- Map acceptance criteria to tests.
- Test negative and failure cases, not only happy paths.
- Include tenant-isolation tests for tenant-aware features.
- Test authentication and authorisation boundaries.
- Test retries, duplicate requests, concurrency, and error handling where relevant.
- Do not disable or weaken tests to obtain a pass.
- Report exact checks run and their results.
- Mark checks as not run when unavailable; never infer success.
- Escalate unexplained failures and material coverage gaps.
