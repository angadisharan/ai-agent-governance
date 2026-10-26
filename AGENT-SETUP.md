# AI Agent Setup and Verification Guide

This guide provides a generic checklist for setting up and verifying any AI coding agent (Claude, Cursor, Gemini, OpenAI, Copilot, etc.) in compliance with the governance policies defined in [`AGENTS.md`](./AGENTS.md).

**Before you start:** Identify which AI agent platform(s) your team will use. See [`PLATFORMS.md`](./PLATFORMS.md) for platform-specific setup instructions.

---

## Pre-deployment setup

### 1. Team and leadership alignment

- [ ] Leadership and security stakeholders have reviewed and approved the AI agent for use in this project.
- [ ] Team members have been trained on the governance policy (`AGENTS.md`) and relevant platform guidelines (`PLATFORMS.md`).
- [ ] A security owner or policy maintainer has been assigned.
- [ ] The project README documents which AI agent(s) are approved and for what purposes.

### 2. Configure the agent for least privilege

**General principle:** Agents should have access to only what they need for the authorized task.

- [ ] Configure the agent to read only the necessary files and directories.
  - Include: source code (`src/`, `app/`, `lib/`), documentation, approved configuration.
  - Exclude: hidden files (`.env*`, `.git/`, `.aws/`, `.ssh/`, `.config/`, etc.), secrets, build artifacts, vendor code.
- [ ] Configure the agent to deny access to secrets and credentials (environment files, private keys, API keys, connection strings, etc.).
- [ ] Configure the agent to deny access to production infrastructure, remote servers, and customer data.
- [ ] For shell/command execution: restrict to read-only operations and safe commands. Deny destructive commands (`rm -rf`, `sudo`, `git push`, etc.) by default.
- [ ] For network access: disable if not needed; if enabled, restrict to known endpoints only (e.g., npm registry, GitHub API).
- [ ] Document any exceptions (e.g., "Agent can read `.npmrc` for package authentication") and have them reviewed and signed off.

**Platform-specific configuration:**
- Claude Code: Update `.claude/settings.json` with filesystem and permission restrictions.
- Cursor: Create `.cursorignore` with patterns to exclude.
- Gemini: Configure IDE project structure to exclude sensitive paths.
- OpenAI/Copilot Chat: Establish team policy in project README (enforcement is user-responsibility).
- GitHub Copilot: Configure VS Code workspace settings to exclude paths.

See `PLATFORMS.md` for detailed platform instructions.

### 3. Set up secret scanning and CI/CD checks

- [ ] Configure your CI/CD pipeline (GitHub Actions, GitLab CI, Jenkins, etc.) to scan for secrets before commits are accepted.
  - Tools: `gitleaks`, `TruffleHog`, `detect-secrets`, or platform-native secret scanning (GitHub, GitLab).
- [ ] Add a pre-commit hook to warn developers before committing files that look like secrets.
- [ ] Enable platform-native secret scanning (GitHub secret scanning, GitLab secret detection, etc.) if available.
- [ ] Document the incident process for accidental secret exposure (see `SECURITY.md`).

### 4. Update project documentation

- [ ] Update your project `README.md` with a section: "AI Agent Usage and Governance"
  - List which AI agents are approved.
  - Link to `AGENTS.md` and `PLATFORMS.md` in this governance repo.
  - Include a "do not share secrets" warning with examples.
- [ ] Create or update a `CODE_OF_CONDUCT.md` with a section on responsible AI use.
- [ ] Add a disclaimer in your `CONTRIBUTING.md`: "All code contributions should follow governance policies; review AI-generated suggestions before merging."

### 5. Create a project-specific policy addendum (if needed)

- [ ] If your project has requirements beyond `AGENTS.md` (e.g., only Claude for architecture decisions, no external API calls), document them in `docs/AI-GOVERNANCE.md` or `.github/AI-POLICY.md`.
- [ ] List any approved exceptions and their justification (e.g., "Cursor has read access to `.npmrc` for private package registry authentication").
- [ ] Have exceptions reviewed and approved by your security owner.

---

## Per-agent setup (platform-specific)

### Claude Code

See [`CLAUDE.md`](./CLAUDE.md) and [`PLATFORMS.md#claude-code`](./PLATFORMS.md#claude-code) for detailed instructions.

**Checklist:**
- [ ] Install Claude Code (CLI, web, desktop, or IDE extension).
- [ ] Create or update `.claude/settings.json` with restrictions.
- [ ] Test: Open `.env` file in Claude Code. Verify Claude cannot read it (permission denied or request to allow).
- [ ] Test: Run `ls -la ~/.ssh/` in Claude Code terminal. Verify output is not shown or Bash tool requests approval.
- [ ] Document the configuration in your team wiki or internal docs.

**Example `.claude/settings.json`:**
```json
{
  "permissions": {
    "allow": ["src/**", "tests/**", "docs/**", "*.md", "*.json"],
    "deny": [".env*", ".git/**", ".aws/**", ".ssh/**", "node_modules/**", "dist/**"]
  }
}
```

### Cursor

See [`PLATFORMS.md#cursor`](./PLATFORMS.md#cursor) for detailed instructions.

**Checklist:**
- [ ] Install Cursor IDE.
- [ ] Create `.cursorignore` in project root with exclusion patterns.
- [ ] Test: Try to open `.env` file in Cursor. Verify it is not indexed or searchable in Cursor chat.
- [ ] Test: Try to ask Cursor to edit `.env`. Verify it declines or ignores that file.
- [ ] Set `chmod 600 .env` to enforce OS-level file permissions.

**Example `.cursorignore`:**
```
.env*
.aws/
.ssh/
.config/
.git/
.github/
.idea/
.vscode/
node_modules/
.claude/
```

### Gemini Code Assistance

See [`PLATFORMS.md#gemini-code-assistance`](./PLATFORMS.md#gemini-code-assistance) for detailed instructions.

**Checklist:**
- [ ] In your IDE (Android Studio, IntelliJ IDEA), configure project structure to exclude sensitive paths.
- [ ] Test: Open Gemini chat and ask it to help with a code snippet. Verify it does not auto-include `.env` or `.git/` files.
- [ ] Instruct team members to manually exclude sensitive files from Gemini context.
- [ ] Document in team wiki: "Never paste `.env` files or secrets into Gemini chat."

### OpenAI (ChatGPT API or Web)

See [`PLATFORMS.md#openai`](./PLATFORMS.md#openai) for detailed instructions.

**Checklist:**
- [ ] Create a team policy document in your README: "Do not share secrets, `.env` files, or private keys in ChatGPT."
- [ ] Add a CI/CD secret scan to catch accidentally-committed secrets.
- [ ] For API integration: use environment variables for API keys, never hardcode them.
- [ ] Test: Create a prompt that includes a fake secret. Verify ChatGPT warns about it or team member catches it in code review.

**Example team policy (for README):**
```markdown
## AI Assistance Policy

We use OpenAI's ChatGPT and API for code suggestions and documentation.

**Important:** Do not share secrets, API keys, `.env` files, or customer data in ChatGPT prompts.
All AI suggestions are reviewed in code review before merging.
```

### GitHub Copilot

See [`PLATFORMS.md#microsoft-copilot`](./PLATFORMS.md#microsoft-copilot) for detailed instructions.

**Checklist:**
- [ ] Install GitHub Copilot in VS Code, JetBrains IDE, or other supported editor.
- [ ] Configure `.vscode/settings.json` with `files.exclude` or workspace settings to hide sensitive files.
- [ ] Test: Open Copilot chat and ask a question. Verify it does not auto-include `.env` or `.git/`.
- [ ] Instruct team members to exclude `.env*` and secrets from Copilot context before pasting.
- [ ] Enable GitHub secret scanning on the repository.

**Example VS Code settings (`.vscode/settings.json`):**
```json
{
  "files.exclude": {
    ".env*": true,
    ".aws/**": true,
    ".ssh/**": true,
    "node_modules/**": true
  }
}
```

---

## Verification and testing

### Before the first use in production

1. **Permission test:**
   - [ ] Attempt to read a restricted file (e.g., `.env` file) using the agent. Verify the agent cannot access it or requests approval.
   - [ ] Attempt to run a restricted command (e.g., `sudo`, `rm -rf`). Verify the agent cannot execute it or requests approval.

2. **Secrets test:**
   - [ ] Create a fake `.env` file with a fake API key. Place it in the project.
   - [ ] Ask the agent to generate code or provide suggestions. Verify it does not read or suggest the secret.
   - [ ] Have a team member run the agent. Verify they see the same restriction.

3. **Network test (if agent has network access):**
   - [ ] Verify the agent can only reach approved endpoints (if any). For example, it should not be able to reach arbitrary external services.
   - [ ] Disable network access if it's not needed for the task.

4. **Code review test:**
   - [ ] Ask the agent to generate a code snippet (e.g., "write a function to read a file"). Review the suggestion for security issues.
   - [ ] Have a second team member review the same suggestion. Verify you both catch any potential security flaws.

### Ongoing verification

- [ ] Monthly: Review and rotate any API keys or tokens used for agent integration.
- [ ] Quarterly: Audit the agent's access logs (if available) to ensure it's not accessing restricted resources.
- [ ] After any incident: Review what happened and update the governance policy or agent configuration to prevent it.

---

## Incident response

If the agent accidentally accesses a secret, restricted file, or production system:

1. [ ] Stop the affected operation immediately.
2. [ ] Do not repeat the action or share the output with others.
3. [ ] Follow the incident process in [`SECURITY.md`](./SECURITY.md).
4. [ ] Report the issue to your security owner without disclosing the secret value.
5. [ ] Update the governance policy or agent configuration to prevent a recurrence.

---

## Troubleshooting

### "The agent is asking for access to a file I excluded. What do I do?"

- Decide if the access is necessary for the task.
- If yes: Update the agent's configuration to allow that specific path (e.g., add `.npmrc` to Claude Code's allowed files if needed for package authentication). Document the decision.
- If no: Confirm with the agent's operators (e.g., your team) that the exclusion should remain. The agent may have misconstrued the request.

### "The agent keeps trying to run commands I disabled. How do I enforce it?"

- Ensure the agent's runtime configuration actually denies the command (don't rely on prompts alone).
- For Claude Code: verify `.claude/settings.json` `bashCommandDenyList` is set.
- For Cursor: ensure OS-level permissions are enforced (e.g., `chmod 755` on directories, no execute on files).
- For platforms without built-in enforcement: rely on CI/CD checks and code review to catch unsafe commands before they execute.

### "How do I verify the agent respects file restrictions?"

- Test directly: ask the agent to read a restricted file. It should refuse or request approval.
- Check logs: some agents log access attempts. Review these to see if the agent tried to access restricted files.
- Code review: review all agent-generated code. If it includes hardcoded paths to secrets or attempts to read restricted files, the agent's configuration may need adjustment.

---

## Related documents

- [`AGENTS.md`](./AGENTS.md) — detailed governance policies.
- [`PLATFORMS.md`](./PLATFORMS.md) — platform-specific implementation guides.
- [`CLAUDE.md`](./CLAUDE.md) — Claude Code entry point.
- [`SECURITY.md`](./SECURITY.md) — security reporting and incident process.
- [`CONTRIBUTING.md`](./CONTRIBUTING.md) — policy change process.
- `policies/` — individual policy documents on security, testing, Git, databases, APIs, etc.

---

## Questions?

If you have questions about agent setup or governance, contact your security owner or policy maintainer.
