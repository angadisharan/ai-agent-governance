# Database and Migration Policy

- Never connect to production databases without separate explicit approval.
- Never inspect credentials or private connection strings.
- Use local, synthetic, or explicitly approved sanitised datasets.
- Document schema changes, constraints, indexes, backfills, and data ownership.
- Prefer staged, backward-compatible migrations.
- Define rollback or recovery procedures for material changes.
- Consider locks, table size, transaction duration, concurrency, and deployment compatibility.
- Preserve identifiers, audit history, and financial records.
- Test migration and recovery paths in a safe environment.
- Do not execute destructive operations or live migrations without an approved release workflow.
