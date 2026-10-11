# Development Guide

**Current status:** Initial documentation only. No runnable application is present yet.

## Implementation phases

### Phase 1 — Project scaffold
- Add backend and frontend package manifests with pinned dependencies.
- Add TypeScript configuration, linting, formatting, and environment-variable validation.
- Add Docker Compose for the app and PostgreSQL, bound to localhost for development.
- Add a health endpoint and database migration mechanism.

### Phase 2 — Synthetic domain model
- Define users, sessions, recovery cases, resource assignments, approvals, and audit events.
- Add deterministic seed data with clearly fictional identities and resource IDs.
- Add a documented reset process.

### Phase 3 — HTTP and WebSocket foundations
- Implement authentication and basic HTTP recovery endpoints.
- Define the versioned WebSocket message envelope and strict schemas.
- Add a connection manager and structured audit logging.
- Add connection, message-size, and rate limits.

### Phase 4 — Learning modes
- Implement isolated vulnerable behavior for the intended lesson.
- Implement secure behavior with the remediation controls in docs/REMEDIATION.md.
- Refuse to start vulnerable mode in non-local environments.
- Ensure no external systems or real devices can be reached.

### Phase 5 — Simulator and learner UI
- Generate synthetic telemetry and recovery events.
- Show connection state, event stream, recovery workflow, and audit evidence.
- Keep server authorization authoritative; the UI must not decide access.

### Phase 6 — Automated verification
- Unit tests for authorization and state transitions.
- HTTP integration tests.
- WebSocket integration tests for subscription authorization and revocation.
- Concurrency tests for duplicate approvals.
- End-to-end tests for the full chain and secure-mode rejection.

### Phase 7 — Operational documentation
- Add local startup, reset, troubleshooting, and teardown instructions.
- Add screenshots only after the UI exists.
- Document known limitations and verified test commands.

## Branch and commit conventions

Use focused branches, for example:
- docs/initial-project-docs
- feat/backend-scaffold
- feat/synthetic-domain-model
- feat/websocket-authz
- test/security-regressions

Prefer small commits such as:
- docs: add initial scenario and architecture
- feat: add synthetic recovery case model
- test: cover websocket authorization checks

Do not claim a phase is complete until code, tests, and documentation agree.

## Configuration and secrets

- Commit .env.example only with placeholder values.
- Keep .env, local databases, build outputs, logs, and secrets out of version control.
- Validate required configuration at startup.
- Never use production credentials or copy real user/device data into the lab.

## Pull request checklist

- [ ] Scope is limited and described.
- [ ] Tests cover new behavior and relevant regressions.
- [ ] Secure-mode behavior is tested.
- [ ] No real secrets, user data, or external target dependencies were added.
- [ ] Documentation and configuration examples match the implementation.
- [ ] Vulnerable mode remains local-only and opt-in.