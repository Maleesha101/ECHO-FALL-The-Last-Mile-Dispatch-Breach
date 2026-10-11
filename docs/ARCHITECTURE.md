# Proposed Architecture

**Status:** Design proposal; implementation has not yet been verified.

## Components

Learner browser
  | HTTP: login, recovery records, status
  | WebSocket: telemetry, subscriptions, workflow events
  v
Web application / API
  - Authentication and session lifecycle
  - WebSocket connection manager
  - Per-message authorization
  - Recovery workflow service
  - Audit event writer
  v
PostgreSQL <----> Synthetic dispatch simulator

The frontend is a learner interface, not a trusted security boundary. All authorization, workflow transitions, and challenge completion checks must happen on the server.

## Proposed stack

- **Frontend:** React + Vite
- **Backend:** Node.js + TypeScript and an HTTP framework
- **WebSockets:** ws
- **Database:** PostgreSQL
- **Local orchestration:** Docker Compose
- **Tests:** unit, API integration, WebSocket integration, and end-to-end chain tests

Versions and exact libraries should be pinned when implementation begins.

## Trust boundaries

1. **Browser to HTTP API:** Treat all request bodies, IDs, cookies, and headers as untrusted input.
2. **Browser to WebSocket handshake:** Validate authentication and the expected Origin; use a deliberate cookie/session policy.
3. **WebSocket message boundary:** Parse messages with a strict schema and enforce authorization for every action, channel, and object.
4. **Application to database:** Use parameterized queries, constraints, transactions, and least-privilege credentials.
5. **Workflow concurrency boundary:** Make approvals idempotent and atomic; a check followed by an unprotected update is not sufficient.
6. **Simulator boundary:** Accept only documented, validated events and operate on synthetic records.
7. **Challenge validation boundary:** Derive completion from server-side state, not a client-supplied success claim.

## Message contract

The implementation should define a versioned, schema-validated envelope. A conceptual example:

    {
      "version": 1,
      "type": "telemetry.subscribe",
      "requestId": "synthetic-request-id",
      "payload": {
        "resourceId": "locker-demo-001"
      }
    }

This is a conceptual example, not a working endpoint specification. The server should reject unknown message types, malformed payloads, oversized messages, and invalid state transitions. It should avoid returning stack traces or sensitive internals to clients.

## Vulnerable and secure modes

If the lab includes a vulnerable mode, it must be explicit and restricted to the local training environment. Secure mode should be the default for any deployment configuration. Avoid making a public or production deployment intentionally vulnerable.

Suggested controls:
- a single explicit lab-mode configuration value;
- startup refusal when vulnerable mode is requested outside a local development environment;
- synthetic seed data only;
- no real credentials or external API integrations;
- separate automated tests for vulnerable behavior and secure-mode rejection.

## Audit events

Record enough information to explain a decision without logging secrets:
- timestamp and correlation/request ID;
- synthetic actor ID and session status;
- message type and requested resource ID;
- authorization decision and reason code;
- workflow transition result;
- duplicate/idempotent request outcome.

Do not log raw session cookies, bearer tokens, passwords, or full sensitive message bodies.

## Availability controls

Apply connection limits, message-size limits, per-identity rate limits, heartbeat/idle timeouts, bounded queues, and graceful shutdown. The simulator should have fixed resource and event limits so a learner cannot exhaust the host.

## Verification

Tests should cover handshake rejection, cross-user channel access, object ownership checks, revocation of active sockets, malformed messages, concurrent approval attempts, idempotency, rate limits, and secure-mode behavior.