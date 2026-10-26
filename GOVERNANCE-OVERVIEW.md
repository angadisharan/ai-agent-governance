# Governance Framework Overview

This document explains the structure of the AI Agent Governance framework and how to navigate it for your specific AI agent platform.

---

## Quick navigation

**I'm using...**

- **Claude Code** → Start with [`CLAUDE.md`](./CLAUDE.md), then refer to [`PLATFORMS.md#claude-code`](./PLATFORMS.md#claude-code)
- **Cursor** → Start with [`PLATFORMS.md#cursor`](./PLATFORMS.md#cursor)
- **Gemini Code Assistance** → Start with [`PLATFORMS.md#gemini-code-assistance`](./PLATFORMS.md#gemini-code-assistance)
- **OpenAI / ChatGPT** → Start with [`PLATFORMS.md#openai`](./PLATFORMS.md#openai)
- **GitHub Copilot** → Start with [`PLATFORMS.md#microsoft-copilot`](./PLATFORMS.md#microsoft-copilot)
- **Any other AI agent** → Start with [`AGENTS.md`](./AGENTS.md), then find your platform in [`PLATFORMS.md`](./PLATFORMS.md)

**I need to...**

- **Set up an agent** → [`AGENT-SETUP.md`](./AGENT-SETUP.md)
- **Understand the full policy** → [`AGENTS.md`](./AGENTS.md)
- **Understand how different platforms enforce governance** → [`PLATFORMS.md`](./PLATFORMS.md)
- **Report a security issue** → [`SECURITY.md`](./SECURITY.md)
- **Propose a policy change** → [`CONTRIBUTING.md`](./CONTRIBUTING.md)
- **Review a code change** → See `checklists/` for review templates
- **Write an architecture decision** → See `templates/` for ADR template

---

## Core framework

### Policies (platform-independent)

**[`AGENTS.md`](./AGENTS.md)** — The foundational policy document.

Defines security and engineering principles that apply to **all AI agents**, regardless of platform. Covers:

- Priority and scope of restrictions
- Filesystem access and secrets handling
- Server and infrastructure access
- Command and tool restrictions
- Privacy and confidentiality
- Coding and architecture standards
- Multi-tenancy requirements
- Database and migration policies
- Dependencies and supply chain
- Git and change management
- Testing and verification
- Specification-driven development
- Incident handling

This policy is **not** Claude-specific. All agents (Claude, Cursor, Gemini, OpenAI, Copilot, etc.) are expected to follow these principles.

### Policies (domain-specific)

Under `policies/`:

- `01-security-and-privacy.md` — Secrets, encryption, authentication, data handling
- `02-filesystem-access.md` — File access restrictions and permitted operations
- `03-server-and-production-access.md` — Remote access and infrastructure policies
- `04-coding-standards.md` — Code quality, architecture, and best practices
- `05-testing-and-quality.md` — Testing requirements and validation
- `06-git-and-change-management.md` — Git operations, commits, and change control
- `07-dependencies-and-supply-chain.md` — Dependency management and security
- `08-database-and-migrations.md` — Database access and data integrity
- `09-api-and-integration-security.md` — API security and external integrations
- `10-ai-tool-use.md` — AI tool execution and side effects
- `11-spec-driven-development.md` — Specification traceability and architecture

All these policies are **platform-independent**. They describe what must or must not be done, not how each platform enforces it.

---

## Platform guides

### [`PLATFORMS.md`](./PLATFORMS.md) — Platform implementation guide

Explains how each AI agent platform maps the shared governance policies to its specific runtime controls.

**Covers:**

- Claude Code (permission model, `.claude/settings.json`)
- Cursor (`.cursorignore`, VS Code integration)
- Gemini Code Assistance (IDE project settings)
- OpenAI / ChatGPT (prompt-based, CI/CD enforcement)
- Microsoft Copilot (workspace settings, GitHub integration)
- GitHub Copilot (VS Code, JetBrains, platform-specific controls)
- Other chat-based and IDE-integrated agents

For each platform:
- How it enforces file access, secrets, shell commands, and network access
- Setup checklist specific to that platform
- Example configuration files
- Platform-specific notes and caveats

### [`CLAUDE.md`](./CLAUDE.md) — Claude Code entry point

Claude-specific guidance for teams using Claude Code (CLI, web, desktop, IDE extensions). References the shared [`AGENTS.md`](./AGENTS.md) baseline and points to [`PLATFORMS.md#claude-code`](./PLATFORMS.md#claude-code) for detailed setup.

---

## Setup and verification

### [`AGENT-SETUP.md`](./AGENT-SETUP.md) — Generic setup guide

A practical checklist for setting up and verifying **any AI agent** in your project. Covers:

- Team alignment and approvals
- Least-privilege configuration (platform-independent principles)
- Secret scanning and CI/CD setup
- Documentation and policy announcements
- Platform-specific checklists (with links to [`PLATFORMS.md`](./PLATFORMS.md))
- Verification tests (permission, secrets, network tests)
- Incident response
- Troubleshooting

Use this guide regardless of which platform you're setting up.

---

## Checklists and templates

### Checklists (`checklists/`)

- `code-change.md` — Review checklist for AI-generated code changes
- `security-review.md` — Security review checklist for sensitive changes
- `release-readiness.md` — Pre-release verification checklist

### Templates (`templates/`)

- `architecture-decision-record.md` — Template for documenting architectural decisions
- `change-proposal.md` — Template for proposing policy or configuration changes

---

## Key principles

### 1. Platform-independent policy, platform-specific enforcement

The **policy** (what you must/must not do) is the same for all agents. The **enforcement** (how that policy is technically implemented) varies by platform.

For example:
- **Policy:** "MUST NOT read `.env` files."
- **Claude Code enforcement:** Permission-based (`.claude/settings.json` denies access).
- **Cursor enforcement:** Pattern-based (`.cursorignore` excludes the file).
- **OpenAI enforcement:** User-responsibility (team policy in README, code review, CI scanning).

### 2. Multi-layer enforcement

No single mechanism (prompt, config file, or instruction) is sufficient. Use multiple layers:

- **Policy layer:** Clear, written governance (`AGENTS.md`).
- **Platform configuration:** Built-in controls (permissions, allowlists, workspace settings).
- **Operating system:** File permissions (chmod, ACLs, SELinux).
- **CI/CD layer:** Automated scanning (secret scanning, linting, dependency checks).
- **Human layer:** Code review, team training, incident response.

### 3. Trust but verify

- Treat all agent output as untrusted. Review and test before merging.
- Test that agent restrictions actually work in your environment.
- Audit agent access and behavior (logs, code changes, commits).
- Update the governance framework based on real-world experience.

### 4. Clear scope and boundaries

Before using an agent, clearly define:
- What files and systems it can access
- What commands it can execute
- What external services it can reach
- What decisions it can make autonomously vs. requiring approval

Document any exceptions and have them reviewed.

---

## Governance structure in this repository

```
ai-agent-governance/
├── README.md                           # Purpose, quick start, adoption guidance
├── GOVERNANCE-OVERVIEW.md              # This file — structure and navigation
├── AGENTS.md                           # Foundational policy (all agents)
├── PLATFORMS.md                        # Platform-specific implementation guides
├── AGENT-SETUP.md                      # Generic setup checklist (any agent)
├── CLAUDE.md                           # Claude Code entry point
├── SECURITY.md                         # Security reporting and incident process
├── CONTRIBUTING.md                     # Policy change process
├── policies/                           # Domain-specific policy documents
│   ├── 01-security-and-privacy.md
│   ├── 02-filesystem-access.md
│   ├── 03-server-and-production-access.md
│   ├── 04-coding-standards.md
│   ├── 05-testing-and-quality.md
│   ├── 06-git-and-change-management.md
│   ├── 07-dependencies-and-supply-chain.md
│   ├── 08-database-and-migrations.md
│   ├── 09-api-and-integration-security.md
│   ├── 10-ai-tool-use.md
│   └── 11-spec-driven-development.md
├── checklists/                         # Repeatable review and verification checklists
│   ├── code-change.md
│   ├── security-review.md
│   └── release-readiness.md
├── templates/                          # Document templates
│   ├── architecture-decision-record.md
│   └── change-proposal.md
└── enforcement/                        # Implementation guidance and scripts
    └── README.md
```

---

## How to use this framework

### For a new project

1. Copy all files from this repository into your project (e.g., `.github/ai-governance/` or `docs/governance/`).
2. Review [`AGENTS.md`](./AGENTS.md) to understand the policies.
3. Choose which AI agent(s) your team will use.
4. Follow the platform-specific setup in [`PLATFORMS.md`](./PLATFORMS.md) or [`AGENT-SETUP.md`](./AGENT-SETUP.md).
5. Configure your CI/CD pipeline with secret scanning and any required checks.
6. Update your project README to reference the governance framework.

### For an existing project

1. Review your current AI agent setup and security practices.
2. Identify gaps against [`AGENTS.md`](./AGENTS.md) policies.
3. Update platform configuration (e.g., add `.cursorignore`, configure Claude Code permissions, update team policies).
4. Add this governance framework to your `.github/` or `docs/` directory.
5. Announce the policy to your team and schedule a review session.
6. Update your CI/CD pipeline and code-review process.

### For teams with multiple agents

1. Define which agent is used for what purpose (e.g., "Claude for architecture, Cursor for daily development, ChatGPT for documentation").
2. Configure each agent separately following [`PLATFORMS.md`](./PLATFORMS.md).
3. Document the choices and reasoning in your project README.
4. Ensure all agents follow the same [`AGENTS.md`](./AGENTS.md) baseline policies.
5. In code review, clarify which agent was used for which change.

---

## Continuous improvement

This framework is not static. As your team gains experience with AI agents, you may discover:

- New security risks or edge cases.
- Platform-specific features that can improve enforcement.
- Policies that are overly restrictive or too permissive.
- New agents or integrations to support.

Follow the process in [`CONTRIBUTING.md`](./CONTRIBUTING.md) to propose updates. All policy changes should:

- Be reviewed and approved by your security owner.
- Be tested in a low-risk environment before full rollout.
- Be communicated to all team members.
- Be documented in the change history of the affected files.

---

## Questions and support

- **Security question:** See [`SECURITY.md`](./SECURITY.md) for incident reporting.
- **Policy question:** See [`AGENTS.md`](./AGENTS.md) or the relevant policy file in `policies/`.
- **Platform setup question:** See [`PLATFORMS.md`](./PLATFORMS.md) or the platform-specific section in [`AGENT-SETUP.md`](./AGENT-SETUP.md).
- **Policy change request:** Follow the process in [`CONTRIBUTING.md`](./CONTRIBUTING.md).

Contact your security owner or policy maintainer for guidance specific to your organization.
