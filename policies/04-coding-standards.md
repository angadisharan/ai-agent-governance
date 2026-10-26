# Coding Standards

- Follow existing approved project conventions and architecture.
- Keep changes focused and avoid unrelated refactoring.
- Use explicit interfaces and cohesive responsibilities.
- Validate inputs at trust boundaries and encode outputs appropriately.
- Enforce authentication and authorisation server-side.
- Handle errors safely without leaking internal details or secrets.
- Use structured logs with correlation IDs and privacy-conscious fields.
- Protect transaction integrity, concurrency invariants, and idempotency.
- Avoid hardcoded secrets, environment-specific infrastructure values, and unnecessary dependencies.
- Document non-obvious decisions and public contracts.
- Preserve backward compatibility unless a breaking change is approved.
- Add tests for changed behaviour and regressions.
