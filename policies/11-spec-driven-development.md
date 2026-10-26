# Specification-Driven Development

Approved specifications and architecture decisions are authoritative for implementation.

Each requirement should have a unique ID, purpose, priority, dependencies, acceptance criteria, security implications, and verification method.

For each implementation task:
1. Identify approved requirement and specification IDs.
2. Confirm scope and non-goals.
3. Identify relevant interfaces, data models, and dependencies.
4. Record ambiguities rather than inventing requirements.
5. Implement a small vertical slice.
6. Add tests tied to acceptance criteria.
7. Validate contracts and regression behaviour.
8. Update specifications through the approved change process.

Maintain traceability from requirement to specification, ADR, API/event/schema, test, and implementation task. Do not proceed when a material architectural decision is missing or contradictory; ask for clarification.
