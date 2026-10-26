# Security Policy

## Reporting a security concern

Report security concerns privately to the repository owner or designated security contact. Do not publish secrets, exploit details, personal data, or production identifiers in public issues.

## Scope

Report issues involving policy bypasses, accidental secret exposure, unsafe agent permissions, tenant isolation failures, supply-chain risks, or insecure automation.

## Handling

1. Stop the affected activity where safe.
2. Preserve necessary evidence without copying or exposing secrets.
3. Report the affected component and impact using the minimum necessary detail.
4. Do not attempt unauthorised remediation or access systems outside your authority.
5. Coordinate remediation and validation with the authorised owner.

## Secrets

Never commit credentials to this repository. If a credential is exposed, arrange revocation/rotation through the authorised owner. Do not paste the value into an issue or chat.

## Important limitation

This document and other repository instructions do not technically prevent a capable agent from accessing resources available to its process. Use operating-system permissions, sandboxing, tool restrictions, network controls, and CI enforcement.
