# Enforcement Guidance

Repository instructions communicate policy; they do not enforce access boundaries by themselves. Enforcement must be implemented in the agent runtime, operating system, CI/CD, and infrastructure.

## Recommended controls

1. **Workspace isolation:** run agents in a dedicated workspace containing only permitted source files.
2. **Filesystem controls:** deny access to secrets and hidden paths using OS permissions or sandbox policy. Ensure the agent cannot escape through symlinks, parent directories, mounts, or alternate tools.
3. **Tool allowlists:** expose only required tools; disable SSH, cloud consoles, remote shells, deployment tools, and destructive commands by default.
4. **Network egress:** restrict outbound network access and require approved destinations.
5. **Credential isolation:** do not mount developer or production credentials into the agent environment.
6. **Command review:** require approval for destructive, privileged, external, or high-impact commands.
7. **CI controls:** run secret scanning, dependency scanning, licence checks, static analysis, tests, and policy validation.
8. **Human approval:** require review for security-sensitive changes, migrations, permission changes, releases, and production operations.
9. **Auditability:** record agent identity, task, approved tools, changes, checks, and approvals without logging secrets.

## Hidden-file policy caveat

A broad rule denying every hidden file also blocks common repository metadata and CI configuration such as `.git/` and `.github/`. Create only narrow, path-specific exceptions when needed. Never use an exception to permit `.env*` or credential stores.

## Validation scripts

Any script added here must be reviewed before use. It should operate only on explicitly supplied paths, avoid reading file contents, avoid following symlinks, and fail safely. Do not treat a simple filename scan as proof that filesystem access is technically blocked.
