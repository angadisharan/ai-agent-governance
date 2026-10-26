# AI Agent Governance

Reusable security, engineering, and change-management policies for AI coding agents.

## Purpose

This repository defines a shared policy baseline for **any AI coding agent** used across software projects—whether Claude, Cursor, Gemini, OpenAI, Copilot, or other platforms. It covers filesystem access, secrets, remote infrastructure, secure coding, testing, Git operations, dependencies, databases, APIs, AI tool use, and specification-driven development.

**Important:** instruction files are not a security boundary. Enforce critical restrictions with operating-system permissions, isolated workspaces, sandboxing, tool allowlists, network controls, CI checks, and human approval.

## Quick start

1. Review `AGENTS.md` and the policies in `policies/`.
2. Review `PLATFORMS.md` to understand how your specific AI agent platform maps to this governance.
3. Adapt project-specific requirements without weakening mandatory security rules.
4. Put the applicable instruction files in each project repository.
5. Configure the coding agent's filesystem, command, network, and tool permissions at the platform level.
6. Add secret scanning, dependency scanning, static analysis, and required CI checks.
7. Review exceptions explicitly and record them.

## Repository structure

**Start here based on your role:**

- **I'm setting up an AI agent:** [`AGENT-SETUP.md`](./AGENT-SETUP.md) — step-by-step guide for any platform.
- **I'm using Claude Code:** [`CLAUDE.md`](./CLAUDE.md) — Claude-specific entry point.
- **I need platform-specific guidance:** [`PLATFORMS.md`](./PLATFORMS.md) — implementation guides for Claude, Cursor, Gemini, OpenAI, Copilot, etc.
- **I need to understand the full policy:** [`AGENTS.md`](./AGENTS.md) — foundational governance for all agents.
- **I need an overview of the framework:** [`GOVERNANCE-OVERVIEW.md`](./GOVERNANCE-OVERVIEW.md) — structure, navigation, and principles.

**Reference documents:**

- `policies/` — detailed normative policies (security, testing, Git, databases, APIs, etc.), all platform-independent.
- `checklists/` — repeatable code-review and verification checklists.
- `templates/` — templates for architecture decisions and change proposals.
- `enforcement/` — implementation guidance and optional validation scripts.
- `SECURITY.md` — reporting and handling security issues.
- `CONTRIBUTING.md` — policy change process.

## Policy language

- **MUST / MUST NOT**: mandatory requirement (applies to all agents and platforms).
- **SHOULD / SHOULD NOT**: recommended unless a documented reason justifies otherwise.
- **MAY**: optional.

## Hidden files and project tooling

The default policy denies access to hidden files and directories, including `.git/`, `.github/`, `.env*`, `.ssh/`, `.aws/`, `.config/`, `.claude/`, `.vscode/`, and `.idea/`. Some projects need narrowly scoped exceptions for particular configuration files. Such exceptions must be explicit, path-specific, reviewed, and must never expose secrets.

**Note:** Enforcement mechanisms vary by platform. Claude Code uses instruction-based restrictions; Cursor uses `.cursorignore`; Gemini, OpenAI, and other agents rely on runtime constraints in their hosting environment. A policy instruction cannot technically stop an agent with unrestricted filesystem or shell access. Use OS-level permissions, sandboxing, platform-specific allowlists, and CI checks to enforce the boundary.

## Adoption and verification

This is a starter baseline. Before production use:

1. Have your security owner review it and tailor it to your threat model.
2. Verify the actual agent runtime controls at your platform (Claude Code permissions, Cursor config, deployment sandbox, etc.).
3. Test that the agent cannot access restricted files or systems in your environment.
4. Do not place credentials or production configuration in this repository.
5. Use PLATFORMS.md to verify your chosen agent's compliance path.
