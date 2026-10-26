# Claude Code Project Policy

**Platform:** Claude Code (including Claude Code CLI, web, desktop app, and IDE extensions)

**Policy baseline:** [`AGENTS.md`](./AGENTS.md) — shared policy for all AI agents.

**Platform setup:** See [`PLATFORMS.md`](./PLATFORMS.md#claude-code) for Claude Code–specific configuration, permissions, and enforcement details.

## Claude Code-specific guidance

Before beginning a task in Claude Code:

1. Identify the authorised scope.
2. Follow the filesystem, hidden-file, secrets, server-access, and tool restrictions defined in `AGENTS.md`.
3. Read relevant policy documents under `policies/` when needed and when permitted.
4. Follow approved specifications and acceptance criteria.
5. Do not attempt to bypass Claude Code's permission prompts or override security controls.

Do not access hidden files, environment files, credentials, remote servers, or production infrastructure unless a narrowly scoped exception is explicitly approved and technically permitted **at the platform level** (e.g., Claude Code permission settings, project `.claude/settings.json`).

If any instruction conflicts with a higher-priority security control (Claude Code's built-in restrictions, your OS permissions, or your organization's policies), follow the higher-priority control and explain the conflict.

**Remember:** prompt instructions and GitHub repository files are not a security boundary. The actual enforcement comes from Claude Code's runtime permission model (file access, shell command allowlists, tool restrictions, etc.). Verify your configuration in Claude Code settings before trusting any policy exemption.
