# Filesystem Access Policy

## Default deny

Do not read, enumerate, search, inspect, copy, modify, or delete hidden files/directories (names beginning with `.`), including `.env*`, `.git/`, `.ssh/`, `.aws/`, `.config/`, `.claude/`, `.github/`, `.vscode/`, and `.idea/`.

Do not access secrets or credential stores regardless of filename.

## Scope

- Establish an approved workspace and allowed paths before access.
- Read only the minimum necessary permitted files.
- Exclude generated files, dependencies, caches, backups, and unrelated files unless approved.
- Do not follow symlinks outside the approved workspace.
- Do not use globs, recursive commands, search tools, metadata tools, or alternate tools to bypass a path restriction.
- Ask for a specific exception if a restricted file is genuinely required.

## Enforcement

Use a sandbox or OS-level permissions. Prompt instructions alone cannot enforce filesystem restrictions. Validate path handling against traversal and symlink attacks.
