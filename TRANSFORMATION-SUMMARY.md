# Governance Transformation: Claude-Specific to Universal AI Agent

## Summary

The AI Agent Governance framework has been transformed from a **Claude Code-specific** set of policies to a **universal framework that applies to any AI agent** (Claude, Cursor, Gemini, OpenAI, Copilot, and others).

The core policies and principles remain the same. The change is in **scope and presentation**: the governance is now explicitly multi-platform, with clear guidance on how different agents implement the shared security and engineering standards.

---

## What changed

### 1. **Core policy is now platform-agnostic**

**Before:**
- `AGENTS.md` existed but had implicit Claude assumptions.
- `CLAUDE.md` was the entry point for all users.
- No explicit guidance for other platforms.

**After:**
- `AGENTS.md` is now explicitly labeled as applying to "all AI coding agents."
- Clear statement: "The governance rules are the same for all agents; enforcement mechanisms vary by platform."
- All policies in `policies/` have been reviewed to remove Claude-specific language.

### 2. **New platform-specific implementation guide**

**Added:** `PLATFORMS.md`

A comprehensive guide explaining how each AI agent platform enforces the shared governance policies:

- **Claude Code:** Permission model, `.claude/settings.json` configuration.
- **Cursor:** `.cursorignore` patterns, VS Code workspace settings.
- **Gemini:** IDE project structure configuration.
- **OpenAI / ChatGPT:** Team policy, CI/CD scanning, code review.
- **Microsoft Copilot:** Workspace settings, GitHub secret scanning.
- **GitHub Copilot:** Editor integration, workspace rules.
- **Generic chat-based agents:** Prompt instructions + human oversight.

Each platform section includes:
- How that platform technically enforces restrictions (file access, secrets, commands, network).
- Step-by-step setup checklist.
- Configuration file examples.
- Platform-specific notes and limitations.

### 3. **Generic agent setup guide**

**Added:** `AGENT-SETUP.md`

A practical, platform-independent checklist for setting up and verifying **any AI agent**:

- Team alignment and security training.
- Configuring least-privilege access (principles, not platform-specific).
- Secret scanning and CI/CD integration.
- Documentation and policy announcements.
- Links to platform-specific instructions for each major agent.
- Verification and testing procedures.
- Incident response guidelines.

This guide works whether you're setting up Claude Code, Cursor, Gemini, or any other agent.

### 4. **Framework overview and navigation**

**Added:** `GOVERNANCE-OVERVIEW.md`

Explains the governance framework structure, key principles, and how to navigate it:

- Quick-start navigation: "I'm using [Platform] → Start here."
- Explanation of policy vs. enforcement separation.
- Multi-layer enforcement model (policy, platform config, OS, CI/CD, human).
- How to use the framework for new and existing projects.

### 5. **Updated README**

**Modified:** `README.md`

- Explicitly states the framework applies to "any AI coding agent."
- Added explanation of enforcement differences (Claude Code, Cursor, Gemini, etc.).
- Updated quick start to reference new documents.
- Clarified the repository structure with role-based navigation.

### 6. **Enhanced Claude entry point**

**Modified:** `CLAUDE.md`

- Clarified this is Claude Code–specific guidance.
- References the shared `AGENTS.md` baseline.
- Links to `PLATFORMS.md#claude-code` for detailed setup.
- Explains that enforcement comes from Claude Code's permission model, not prompt instructions alone.

---

## Key principles of the transformation

### 1. Separate policy from implementation

**Policy (universal, applies to all agents):**
```
- MUST NOT read `.env` files.
- MUST NOT execute destructive shell commands without approval.
- MUST NOT access remote servers without explicit authorization.
```

**Implementation (varies by platform):**
- Claude Code: Enforces via `.claude/settings.json` permission system.
- Cursor: Enforces via `.cursorignore` exclusion patterns.
- Gemini: Enforces via IDE project settings.
- OpenAI: Enforces via team policy + code review + CI/CD scanning.

### 2. Multi-layer enforcement

The framework acknowledges that **no single enforcement mechanism is sufficient**. All implementations include:

1. **Policy layer:** Clear, written governance (`AGENTS.md`).
2. **Platform configuration:** Built-in controls (permissions, allowlists, settings).
3. **OS layer:** File permissions (chmod, ACLs, SELinux).
4. **CI/CD layer:** Automated scanning (secret detection, linting).
5. **Human layer:** Code review, training, incident response.

### 3. Platform-specific, not agent-agnostic

The framework is **platform-aware**, not platform-agnostic. It acknowledges that:

- Different platforms have different capabilities (some have file-level restrictions, some don't).
- Enforcement mechanisms vary (Claude uses permission prompts, Cursor uses config files, OpenAI uses team policy).
- Users need specific guidance for their chosen platform.

---

## What stayed the same

### Core policies

All policies in `policies/` remain unchanged:

- `01-security-and-privacy.md`
- `02-filesystem-access.md`
- `03-server-and-production-access.md`
- `04-coding-standards.md`
- `05-testing-and-quality.md`
- `06-git-and-change-management.md`
- `07-dependencies-and-supply-chain.md`
- `08-database-and-migrations.md`
- `09-api-and-integration-security.md`
- `10-ai-tool-use.md`
- `11-spec-driven-development.md`

These policies apply to all agents. No changes to the rules themselves, only clarification that they're universal.

### Checklists and templates

- `checklists/code-change.md`
- `checklists/security-review.md`
- `checklists/release-readiness.md`
- `templates/architecture-decision-record.md`
- `templates/change-proposal.md`

These remain platform-independent and reusable.

### Security and contribution processes

- `SECURITY.md` — unchanged (applies to all agents).
- `CONTRIBUTING.md` — unchanged (process for policy updates).

---

## Migration guide

### For teams currently using Claude Code

**You don't need to change anything immediately.** The governance is backwards-compatible:

1. Continue using Claude Code as before.
2. Gradually review `PLATFORMS.md#claude-code` to ensure your setup matches the recommended configuration.
3. If you later adopt other agents (Cursor, Copilot), follow the platform-specific guidance.
4. Ensure your team understands that the governance is now multi-platform.

### For teams considering other agents

1. Read `GOVERNANCE-OVERVIEW.md` to understand the framework.
2. Review `AGENTS.md` for the baseline policies (applies to all agents).
3. Go to `PLATFORMS.md` and find your platform.
4. Follow the platform-specific setup checklist (or use `AGENT-SETUP.md` as a template).
5. Verify the setup by running the tests in `AGENT-SETUP.md`.

### For teams with multiple agents

1. Define which agent is used for what purpose (documented in project README).
2. Configure each agent separately using the relevant section of `PLATFORMS.md`.
3. Ensure all agents follow the same `AGENTS.md` baseline.
4. In code review, clarify which agent was used (so reviewers can apply the right mental model).
5. Apply the same CI/CD checks to all changes, regardless of which agent generated them.

---

## Benefits of the transformation

1. **Clearer governance for heterogeneous teams:** Teams can use multiple AI agents while maintaining consistent security standards.
2. **Easier onboarding for new tools:** When a new AI agent emerges, the framework clearly shows how to evaluate and integrate it.
3. **Platform-specific without losing generality:** Guidance is tailored to each platform's unique capabilities and constraints.
4. **Better enforceability:** By separating policy from implementation, each team can adapt the enforcement to their specific environment (OS, CI/CD, cloud provider, etc.).
5. **Future-proof:** The framework doesn't assume any particular platform will dominate. It's ready for Claude, Cursor, Gemini, OpenAI, Copilot, and any future agents.

---

## New documents in this repo

| File | Purpose |
|------|---------|
| `GOVERNANCE-OVERVIEW.md` | Framework overview, navigation, and key principles. |
| `PLATFORMS.md` | Platform-specific implementation guides (Claude, Cursor, Gemini, OpenAI, Copilot, etc.). |
| `AGENT-SETUP.md` | Generic setup and verification checklist for any AI agent. |
| `TRANSFORMATION-SUMMARY.md` | This file—explains what changed and why. |

---

## Next steps for your team

1. **Read:** Start with `GOVERNANCE-OVERVIEW.md` to understand the new structure.
2. **Choose:** Decide which AI agent(s) your team will use.
3. **Setup:** Follow the platform-specific guidance in `PLATFORMS.md` or use `AGENT-SETUP.md` as a template.
4. **Verify:** Run the verification tests to ensure restrictions are actually enforced.
5. **Document:** Update your project README to reference the governance framework and clarify which agents are approved.
6. **Train:** Brief your team on the governance policy and their platform's specific setup.
7. **Review:** Use the checklists in `checklists/` during code review.

---

## Questions?

- **General framework questions:** See `GOVERNANCE-OVERVIEW.md`.
- **Platform-specific setup:** See `PLATFORMS.md`.
- **Generic setup help:** See `AGENT-SETUP.md`.
- **Detailed policies:** See `policies/` and `AGENTS.md`.
- **Security incident:** See `SECURITY.md`.
- **Policy change:** See `CONTRIBUTING.md`.

Contact your security owner or policy maintainer for guidance.
