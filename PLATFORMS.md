# AI Agent Platforms and Governance Implementation

This document explains how different AI agent platforms (Claude, Cursor, Gemini, OpenAI, Copilot, and others) implement the shared governance policies defined in [`AGENTS.md`](./AGENTS.md).

**Key principle:** The governance rules themselves are platform-independent. The *enforcement mechanism* varies by platform. All agents are expected to follow the same security and engineering principles; the practical controls differ.

---

## Overview table

| Aspect | Claude Code | Cursor | Gemini Code Assistance | OpenAI (ChatGPT) | Microsoft Copilot | GitHub Copilot |
|--------|-----------|--------|------------------------|------------------|------------------|-----------------|
| **Agent type** | Dedicated agent / IDE extension | AI IDE with file integration | IDE plugin (Google IDEs) | Chat-based with plugins | IDE-integrated assistant | IDE autocomplete + chat |
| **File access control** | Permission-based (prompt controls + settings) | `.cursorignore` file + editor rules | Project-scoped read-only | No direct file access (manual paste) | Workspace-scoped access | Limited to context window |
| **Secret scanning** | Via `.claude/settings.json`, user config | `.cursorignore`, environment variable masking | User-responsible | User-responsible | Environment variable filtering | User-responsible |
| **Shell command execution** | Conditional (Bash tool, with approve prompts) | Integrated terminal (restricted via settings) | N/A (no shell access) | User copies commands (manual execution) | Via terminal integration | N/A (suggestions only) |
| **Network access** | Tool-based (WebFetch, MCP, API tools) | Integrated HTTP client (via settings) | Via IDE runtime | User-responsible | Via IDE runtime | N/A (suggestions only) |
| **Deployment scope** | Local, CLI, web, desktop, IDE | Local IDE + cloud sync | Local IDE + cloud services | Cloud-based | Cloud-based with local workspace | Cloud-based |
| **Recommended enforcement** | Enforce via permissions + CI/CD checks | Enforce via `.cursorignore` + OS permissions | Enforce via OS permissions + IDE isolation | Enforce via prompt + human review + CI/CD | Enforce via workspace settings + CI/CD | Enforce via CI/CD + Git hooks |

---

## Detailed platform guidance

### Claude Code

**Platform URL:** https://claude.ai/code (web), local CLI, desktop app, VS Code extension

**Governance entry point:** [`CLAUDE.md`](./CLAUDE.md)

#### How Claude enforces governance

- **File access:** Permission system in Claude Code settings (`project/.claude/settings.json`). Users configure `permissions` to allow/deny filesystem operations. Default: restrictive.
- **Hidden files:** Instruction-based (policies say "do not read"), enforced by runtime permissions. Users can override per `.claude/settings.json`.
- **Secrets:** Environment variable masking in Claude Code settings; instruction enforcement via prompt.
- **Shell commands:** Bash tool requires approval prompts in interactive mode. Allowlists configured in project settings.
- **Network:** WebFetch, MCP servers, API integration tools gated by permissions. Can be restricted or disabled.

#### Setup checklist

1. Copy `AGENTS.md`, policy files, and checklists into your project.
2. In Claude Code settings, configure `.claude/settings.json` with:
   - File access restrictions (deny hidden files, secrets by default).
   - Command allowlists (restrict harmful Bash commands).
   - Tool restrictions (disable network tools if not needed).
3. Run security checklist before any first use.

#### Example `.claude/settings.json` snippet

```json
{
  "permissions": {
    "allow": ["src/**", "docs/**"],
    "deny": [".env*", ".git/**", ".aws/**", "node_modules/**"]
  },
  "restrictions": {
    "bashCommandAllowList": ["ls", "find", "grep", "cat", "head"],
    "bashCommandDenyList": ["rm -rf", "sudo", "chmod 777"]
  }
}
```

---

### Cursor

**Platform URL:** https://www.cursor.sh (local IDE, built on VS Code)

**Governance entry point:** Platform-specific configuration in `.cursorignore` and settings.

#### How Cursor enforces governance

- **File access:** `.cursorignore` file (similar to `.gitignore`). Patterns in `.cursorignore` prevent Cursor from reading or suggesting edits to those files.
- **Hidden files:** Convention-based (put patterns in `.cursorignore`). Not enforced by default; requires project setup.
- **Secrets:** Use `.cursorignore` to exclude `.env*`, `.aws/**`, etc. Cursor also masks common env var patterns in context.
- **Shell commands:** Integrated terminal in Cursor editor; governed by OS-level shell permissions.
- **Network:** Cursor does not have direct outbound HTTP; suggestions and completions run locally or via Cursor's backend.

#### Setup checklist

1. Create `.cursorignore` in project root with patterns from `AGENTS.md` (example below).
2. Configure Cursor editor settings (`.vscode/settings.json` or Cursor-specific config) to enable file restrictions.
3. Use OS-level file permissions to enforce hidden-file access restrictions (e.g., `chmod 600 .env`).
4. Verify in Cursor that excluded patterns do not appear in completions or chat suggestions.

#### Example `.cursorignore` template

```
# Secrets and environment
.env*
.aws/
.ssh/
.config/

# Version control
.git/
.github/
.gitignore

# IDE and build artifacts
node_modules/
.vscode/
.idea/
dist/
build/

# Sensitive local configuration
.claude/
.cursor/
```

#### Cursor-specific notes

- `.cursorignore` is the primary enforcement mechanism. File exclusion patterns prevent Cursor from accessing those paths.
- Cursor's AI suggestions run locally or via Cursor's cloud backend (managed by Cursor, not your repo).
- Terminal commands in Cursor's integrated terminal follow your shell's permissions and OS rules.
- Verify that `.cursorignore` patterns match your threat model (e.g., exclude production config, customer data, credentials).

---

### Gemini Code Assistance

**Platform:** Google IDEs (Android Studio, IntelliJ IDEA, VS Code with extension)

**Governance entry point:** IDE project settings and user configuration.

#### How Gemini enforces governance

- **File access:** Primarily read-only context window access. Gemini reads the files you select or those in the active project scope.
- **Hidden files:** IDE configuration controls which files are indexed. Users configure this in IDE settings.
- **Secrets:** No automatic masking; user must manually exclude sensitive files from Gemini's context.
- **Shell commands:** Gemini provides code suggestions but does not execute commands. User copies and runs manually.
- **Network:** Gemini does not have outbound network access from the IDE; suggestions are generated on Google's backend.

#### Setup checklist

1. In your IDE (Android Studio, IntelliJ), configure project structure to exclude `.env*`, `.git/`, `.aws/`, etc.
2. Copy relevant policies and checklists into your repository.
3. Instruct team members to manually exclude sensitive files from Gemini context before pasting code snippets into Gemini chat.
4. Verify via IDE settings that hidden files are not auto-indexed.

#### Gemini-specific notes

- Gemini is suggestion and chat-based; it does not modify files directly in most cases.
- Exclude sensitive information manually by not including those files in your prompts.
- Treat all Gemini suggestions as untrusted code—review and test before committing.

---

### OpenAI (ChatGPT, API)

**Platform URL:** https://chatgpt.openai.com (web), API integration in code

**Governance entry point:** Prompt instructions + human review + organizational policy.

#### How OpenAI enforces governance

- **File access:** ChatGPT web interface has no direct filesystem access. Users paste code manually or upload files.
- **Hidden files:** No enforcement by platform. Governed by what users paste into ChatGPT.
- **Secrets:** User-responsible. ChatGPT will suggest masking or removal but does not enforce it.
- **Shell commands:** ChatGPT suggests commands; users copy and run manually in their shell.
- **Network:** OpenAI's API and web interface do not directly access your infrastructure. Network calls are made by users or integrations they control.

#### Setup checklist

1. Establish a team policy: "Do not paste secrets, `.env` files, or private keys into ChatGPT."
2. Include governance policies in your project's documentation and Code of Conduct.
3. Use CI/CD checks to scan commits for accidentally-committed secrets.
4. For API integration, authenticate via environment variables (not hardcoded keys), use API keys scoped to minimal permissions.
5. Review ChatGPT suggestions for security issues before merging.

#### OpenAI-specific notes

- Treat ChatGPT as an assistant, not an autonomous agent. Users control what information is shared.
- OpenAI's API has usage policies; review them for any restrictions on your use case (e.g., cannot generate code for accessing other users' accounts).
- Consider that prompts and completions sent to OpenAI may be logged for abuse detection. Do not send customer data or proprietary information.

---

### Microsoft Copilot

**Platform:** GitHub Copilot integration (VS Code, JetBrains, Vim, etc.) + Copilot Chat

**Governance entry point:** IDE configuration + `.github/copilot-config.yml` (where available)

#### How Microsoft Copilot enforces governance

- **File access:** GitHub Copilot reads files in your editor and workspace context. Scope is controlled by IDE settings and what you open.
- **Hidden files:** Not automatically excluded. Use IDE workspace settings or `.copilotignore` (if supported) to exclude patterns.
- **Secrets:** GitHub's secret scanning checks commits before push, but Copilot itself does not enforce secret masking. User-responsible in chat.
- **Shell commands:** Copilot suggests completions and code, but does not execute. User copies and runs manually.
- **Network:** Copilot suggestions are generated by GitHub's backend; no direct outbound access from your machine.

#### Setup checklist

1. Use VS Code's workspace settings to exclude sensitive patterns from Copilot's context (e.g., via `files.exclude` or `.vscode/settings.json`).
2. Configure GitHub's secret scanning to detect accidental commits of secrets.
3. Include a `.gitignore` and governance instructions in your repository README.
4. Instruct team members to exclude `.env*` and secrets from Copilot chat.
5. Review Copilot suggestions before committing; do not auto-merge large Copilot-generated blocks.

#### Copilot-specific notes

- GitHub Copilot is trained on public code; suggestions may reflect common patterns but are not verified for security.
- Treat all Copilot suggestions as untrusted—review for correctness and security before merging.
- Microsoft scans commits in public repos for secrets; private repos are not scanned by default.

---

### GitHub Copilot X and Other Chat-Based AI Assistants

**Platform:** Copilot Chat (VS Code, GitHub.com), Claude API, Anthropic Workbench, etc.

**Governance entry point:** Prompt instructions + CI/CD enforcement + human review.

#### How chat-based assistants enforce governance

- **File access:** Typically no direct access; users provide code snippets or file paths manually.
- **Hidden files:** User-responsible—user decides what to share.
- **Secrets:** No automatic detection by the assistant. Users must manually verify they're not sharing secrets.
- **Shell commands:** Suggested by the assistant; user copies and runs manually.
- **Network:** Assistants run on vendor servers (GitHub, Anthropic, OpenAI); no direct access to your infrastructure.

#### Setup checklist

1. Establish clear team guidelines: "No secrets, `.env` files, or customer data in AI chat."
2. Use `git diff` to review changes before committing; CI/CD scans for secrets and sensitive patterns.
3. Include governance and security policies in your project README and Code of Conduct.
4. Use commit message templates to remind developers to avoid sharing sensitive info.
5. Periodically audit your git history for accidentally-committed secrets (tools: `gitleaks`, `TruffleHog`, GitHub secret scanning).

#### Chat-based notes

- Chat assistants are more transparent about limitations—they will refuse to help with clearly harmful requests.
- Always review suggestions for correctness, security, and alignment with your architecture.
- Be explicit in prompts about your security and compliance requirements.

---

## Cross-platform compliance checklist

Use this checklist to verify that any AI agent on your project complies with `AGENTS.md`:

### Before using the agent in production

- [ ] Project README documents which AI agent(s) are permitted and why.
- [ ] Agent is configured to deny access to hidden files (`.env*`, `.git/`, `.aws/`, `.ssh/`, etc.).
- [ ] Agent is configured to deny access to credentials, private keys, or production configuration.
- [ ] Shell/command execution is restricted to safe commands (read-only preferred; no `rm -rf`, `sudo`, etc.).
- [ ] Network access is disabled or restricted to approved endpoints (no free-form HTTP if unnecessary).
- [ ] CI/CD pipeline includes secret scanning on all commits.
- [ ] Team has signed off on the governance policy and platform choice.

### During agent use

- [ ] Users follow the "do not share secrets" policy.
- [ ] Code review process includes verification that no secrets were accidentally introduced.
- [ ] Agent output is treated as untrusted code—review and test all suggestions.
- [ ] Incident reporting process is documented for accidental secret exposure.

### Platform-specific verifications

- **Claude Code:** Verify `.claude/settings.json` restrictions are in place; test that agent cannot read `.env` files.
- **Cursor:** Test `.cursorignore` patterns; verify Cursor does not suggest edits to excluded files.
- **Gemini:** Verify IDE project structure excludes sensitive files; instruct users to manually exclude secrets from chat.
- **OpenAI/Copilot Chat:** Educate users to not paste secrets; rely on CI/CD scanning and code review.
- **GitHub Copilot:** Verify workspace settings exclude sensitive files; test that Copilot does not complete with secrets.

---

## Questions and customization

**Q: Can I use multiple AI agents on the same project?**  
A: Yes, with caveats. Each agent must be separately configured to enforce the same restrictions. Document which agents are approved and for what purposes. For example: "Cursor for day-to-day coding, Claude Code for complex architecture decisions, ChatGPT for documentation and learning."

**Q: How do I update this policy for a new platform?**  
A: Follow the template above. Document the platform's enforcement mechanism, add a setup checklist, list platform-specific notes, and ensure the checklist aligns with `AGENTS.md`. Submit changes via the process in `CONTRIBUTING.md`.

**Q: What if a platform does not support file-level restrictions?**  
A: Rely on OS-level permissions (file mode bits, SELinux, Windows ACLs), CI/CD checks (secret scanning, linting), and human code review. Trust cannot be placed solely in the agent's promises.

**Q: How do I report a security issue with a platform's enforcement?**  
A: See `SECURITY.md`.
