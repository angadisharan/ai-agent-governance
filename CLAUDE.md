# Claude Code Project Policy

Read and follow [`AGENTS.md`](./AGENTS.md) as the shared baseline policy.

Before beginning a task:
1. Identify the authorised scope.
2. Follow the filesystem, hidden-file, secrets, server-access, and tool restrictions.
3. Read relevant policy documents under `policies/` when needed and when permitted.
4. Follow approved specifications and acceptance criteria.

Do not access hidden files, environment files, credentials, remote servers, or production infrastructure unless a narrowly scoped exception is explicitly approved and technically permitted.

If any instruction conflicts with a higher-priority security control, follow the higher-priority control and explain the conflict. Do not claim that prompt instructions alone enforce filesystem or network restrictions; those require runtime controls.
