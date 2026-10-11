# Remediation Guide

This document describes the controls the secure version of ECHO//FALL should implement and the checks that should prove they work.

## 1. Protect the handshake

- Use wss:// outside local development.
- Authenticate the connection using a deliberate session/token design.
- Validate the Origin against an explicit allowlist when browser cookies or other ambient credentials are used.
- Do not treat Origin as authentication; non-browser clients can set it.
- Configure cookies with appropriate Secure, HttpOnly, and SameSite attributes.
- Reject unauthorized handshakes and avoid verbose failure details.

**Verify:** missing, invalid, and untrusted origins are rejected according to policy, and unauthenticated clients cannot establish a privileged session.

## 2. Authorize every message

- Separate authentication from authorization.
- Validate every message against a strict schema and allowlist of message types.
- Check permission for each requested action and subscription.
- Re-check resource ownership or assignment for every object-level operation.
- Do not rely on opaque IDs as a substitute for authorization.
- Re-evaluate permissions when roles, assignments, or session state change.

**Verify:** a user cannot subscribe to another user's channel or read/change a record they are not permitted to access.

## 3. Enforce revocation on active connections

- Associate each socket with a server-side session identity.
- On logout, expiry, account disablement, or revocation, invalidate the session and close or downgrade all associated sockets.
- Check session validity for sensitive actions, not only at connection time.
- Define and test heartbeat and idle-timeout behavior.

**Verify:** after revocation, the existing socket cannot perform privileged actions.

## 4. Make workflow transitions atomic and idempotent

- Model the workflow as explicit states and permitted transitions.
- Perform authorization and state transition in a database transaction.
- Use row locks, conditional updates, unique constraints, or equivalent database guarantees as appropriate.
- Use idempotency keys for retryable operations and make duplicate requests return a safe, consistent result.
- Never assume the frontend prevents simultaneous requests.

**Verify:** concurrent approval attempts produce at most one successful state transition and a consistent audit trail.

## 5. Validate input and constrain resources

- Bound message sizes, nesting, collection lengths, and string lengths.
- Reject malformed JSON, unknown fields where appropriate, and unsupported message versions.
- Apply per-user and per-IP connection and message limits.
- Bound outbound queues and simulator event rates.
- Handle disconnects and backpressure safely.
- Avoid reflecting untrusted content into HTML; encode output according to context.

**Verify:** malformed or oversized messages are rejected without crashing the service or exposing internals.

## 6. Log safely

- Record actor/session identifiers, message types, target resource, correlation IDs, and allow/deny decisions.
- Do not log passwords, cookies, bearer tokens, or secrets.
- Use structured logs and redact untrusted content.
- Make audit records useful for investigating the sequence without leaking sensitive data.

**Verify:** an instructor can reconstruct the test flow from audit records without seeing authentication secrets.

## 7. Completion validation

Challenge success must be computed by the server from the intended synthetic workflow state. Do not accept a client-side flag, hidden form field, or arbitrary success message as proof of completion.

## Secure-mode acceptance checklist

- [ ] Unauthenticated handshakes are denied.
- [ ] Untrusted origins are denied when origin validation is required.
- [ ] Unauthorized channel subscriptions are denied.
- [ ] Cross-user object access is denied.
- [ ] Revoked sessions lose access on existing sockets.
- [ ] Duplicate concurrent approvals result in one valid transition.
- [ ] Invalid and oversized messages are rejected safely.
- [ ] Rate limits and connection caps are enforced.
- [ ] Logs omit secrets and contain useful decision context.
- [ ] Completion cannot be forged from the client.