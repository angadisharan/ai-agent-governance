# AI Agent Governance

Reusable security, engineering, and change-management policies for AI coding agents.

## Purpose

This repository defines a shared policy baseline for AI coding agents used across software projects. It covers filesystem access, secrets, remote infrastructure, secure coding, testing, Git operations, dependencies, databases, APIs, AI tool use, and specification-driven development.

**Important:** instruction files are not a security boundary. Enforce critical restrictions with operating-system permissions, isolated workspaces, sandboxing, tool allowlists, network controls, CI checks, and human approval.

## Quick start

1. Review `AGENTS.md` and the policies in `policies/`.
2. Adapt project-specific requirements without weakening mandatory security rules.
3. Put the applicable instruction files in each project repository.
4. Configure the coding agent's filesystem, command, network, and tool permissions.
5. Add secret scanning, dependency scanning, static analysis, and required CI checks.
6. Review exceptions explicitly and record them.

## Repository structure

- `AGENTS.md` — shared baseline instructions for compatible agents.
- `CLAUDE.md` — Claude Code entry point referencing the shared policy.
- `SECURITY.md` — reporting and handling security issues.
- `CONTRIBUTING.md` — policy change process.
- `policies/` — detailed normative policies.
- `checklists/` — repeatable review checklists.
- `templates/` — ADR and change proposal templates.
- `enforcement/` — implementation guidance and optional validation scripts.

## Policy language

- **MUST / MUST NOT**: mandatory requirement.
- **SHOULD / SHOULD NOT**: recommended unless a documented reason justifies otherwise.
- **MAY**: optional.

## Hidden files and project tooling

The default policy denies access to hidden files and directories, including `.git/`, `.github/`, `.env*`, `.ssh/`, `.aws/`, `.config/`, `.claude/`, and `.vscode/`. Some projects need narrowly scoped exceptions for particular configuration files. Such exceptions must be explicit, path-specific, reviewed, and must never expose secrets.

A policy cannot technically stop an agent with unrestricted filesystem or shell access. Use OS-level permissions and sandboxing to enforce the boundary.

## Adoption

This is a starter baseline. Before production use, have your security owner review it, tailor it to your threat model, and verify the actual agent runtime controls. Do not place credentials or production configuration in this repository.
