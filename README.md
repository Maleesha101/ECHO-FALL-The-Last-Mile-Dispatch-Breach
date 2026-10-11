# ECHO//FALL — The Last-Mile Dispatch Breach

**ECHO//FALL** is a planned, locally hosted cybersecurity training lab about chained WebSocket security failures in a fictional last-mile logistics platform. Learners investigate how weak message-level authorization, object-level access control, session revocation, and concurrency controls can combine into a larger business-logic compromise.

> **Project status:** Documentation-first scaffold. The application, simulator, and automated challenge checks are not implemented yet. Do not treat the attack chain below as a claim that a working lab already exists.

## Scenario

A fictional delivery operator uses a real-time dispatch console to coordinate simulated smart lockers and delivery robots. The console uses HTTP APIs for account and recovery workflows and WebSockets for live telemetry and recovery events. A flaw in the way the platform authorizes WebSocket messages may expose another team's recovery metadata. Additional weaknesses in resource ownership checks and session lifecycle handling can allow a learner to reach a simulated dispatch-approval race.

All organizations, users, device identifiers, telemetry, and recovery records in the intended lab are synthetic. The lab must never connect to real delivery services or physical devices.

## Planned learning topics

- WebSocket handshake and Origin validation
- Authentication versus per-message authorization
- Unauthorized channel subscription and information disclosure
- Broken Object Level Authorization (BOLA / IDOR)
- Session revocation and stale WebSocket connections
- Race conditions in state-changing workflows
- Secure event validation, logging, and remediation

## Intended technology

The proposed implementation uses Node.js with TypeScript, an HTTP framework, the ws WebSocket library, React with Vite, PostgreSQL, and Docker Compose. These are design targets, not implemented dependencies yet.

## Repository map

- docs/SCENARIO.md — narrative, roles, and intended challenge chain
- docs/ARCHITECTURE.md — proposed components and trust boundaries
- docs/LEARNING_OBJECTIVES.md — skills and learner outcomes
- docs/REMEDIATION.md — defensive guidance and verification ideas
- docs/DEVELOPMENT.md — implementation phases and local development expectations

## Planned local setup

Setup instructions will be added once the application scaffold and Docker Compose configuration exist. Until then, there is no runnable lab command.

## Safety boundaries

- Use only on systems you own or are explicitly authorized to test.
- Keep the eventual challenge stack bound to localhost by default.
- Use synthetic data and a deterministic simulator; do not control real devices.
- Do not include real credentials, production tokens, or external target URLs.
- Keep intentionally vulnerable behavior isolated to the training environment.
- Provide a secure-mode comparison and tests for the corresponding fixes.

## Current roadmap

1. Establish scenario, architecture, and learning objectives.
2. Build the HTTP/WebSocket service and synthetic seed data.
3. Add isolated vulnerable and secure modes.
4. Implement the simulator and learner-facing UI.
5. Add automated tests for the chain and defensive controls.
6. Document setup, reset, troubleshooting, and teardown.

## Contribution approach

Follow the phased plan in docs/DEVELOPMENT.md. Keep changes small and reviewable, add tests for behavior, and never introduce real-world targets or secrets.

## License

No license has been selected yet. Unless and until a license is added, all rights remain with the copyright holder.