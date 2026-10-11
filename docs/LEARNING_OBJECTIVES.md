# Learning Objectives and Assessment

## Intended audience

Intermediate web application security learners who understand HTTP requests, browser developer tools, authentication, and basic API testing.

## Learning outcomes

After completing the implemented lab, learners should be able to:

1. Explain why a successful WebSocket handshake does not authorize every message sent over that connection.
2. Inspect a WebSocket message flow and identify unauthorized subscriptions or over-broad event delivery.
3. Recognize BOLA/IDOR when an API or socket action accepts an object identifier without an ownership check.
4. Explain why logout or token revocation must also invalidate or restrict existing WebSocket connections.
5. Identify a race condition in a state-changing workflow and explain atomicity and idempotency.
6. Use server-side audit events to reconstruct an event chain.
7. Recommend and validate controls that prevent the same chain in secure mode.

## Assessment rubric

| Area | Evidence of mastery |
|---|---|
| Protocol understanding | Describes handshake, connection lifecycle, messages, and subscriptions accurately |
| Authorization | Distinguishes authentication from channel-, action-, and object-level authorization |
| Session lifecycle | Demonstrates why existing connections must react to revocation |
| Concurrency | Explains duplicate state transitions and verifies atomic/idempotent handling |
| Evidence | Uses request IDs and audit events to support conclusions |
| Remediation | Re-tests the same scenarios in secure mode and explains why they fail safely |

## Suggested learner workflow

1. Read the fictional incident report and map the normal recovery workflow.
2. Establish a baseline using the provided test accounts and synthetic records.
3. Inspect handshake metadata and WebSocket frames in browser developer tools or an approved proxy.
4. Test each stage independently before attempting the combined chain.
5. Capture minimal, redacted evidence for each authorization decision.
6. Switch to secure mode and repeat the tests.
7. Write a short report with impact, root cause, evidence, and remediation.

## Rules of engagement

- Use only the lab's local endpoints and seeded identities.
- Do not scan public IP addresses or unrelated hosts.
- Do not substitute real credentials, cookies, or identifiers.
- Do not attempt resource exhaustion outside the bounded lab.
- Keep proof-of-completion values private to the challenge and do not hard-code them into the UI.