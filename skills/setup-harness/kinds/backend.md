# Kind: Backend / Service

Detect: server frameworks, route handlers, DB drivers/migrations dir, Dockerfile
serving a port, API specs.

## Proof surface

- Layer 1: types, lint, schema/contract checks (OpenAPI, protobuf).
- Layer 2: unit + integration tests against a real or containerized DB.
- Layer 3: request/response probes on running service + log inspection +
  DB-state assertions. A passing unit suite is not system proof.
- features.json verification example:
  `bin/check && curl -sf localhost:PORT/health && <endpoint probe>`

## Safety gates

- **Migrations are one-way doors.** Deploy ≠ migrate: separate steps,
  migration needs explicit owner approval; write the down-path or record
  `forward-only` as a decision before applying.
- Auth/tenancy boundaries get their own tests — a cross-tenant read is a
  release blocker, not a bug ticket.
- API backcompat: breaking a published contract requires a versioned boundary
  or a recorded owner decision.
- Code rollback is cheap; data rollback usually isn't. Anything touching
  persisted data gets a backup-or-reversible plan before it runs.
