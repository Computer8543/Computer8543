# ADR-0000: [Short Noun-Phrase Title of Decision]

* **Status:** Proposed | Accepted | Superseded by ADR-XXXX
* **Date:** YYYY-MM-DD
* **Security Domain:** Identity & Access | Supply Chain & CI/CD | Network Telemetry | Data Isolation

## Context & Operational Constraints

[Describe the system state, attack surface, and real-world constraints—such as compliance mandates, micro-budget limits, or multi-contributor drift—that forced a decision.]

## Decision & Enforced Invariant

[State the architectural choice and the exact system invariant that must hold true regardless of client behavior or developer error.]

## Alternatives Considered & Rejected

* **[Option A]:** [Why it was rejected—e.g., relies on client-side enforcement, introduces unacceptable blast radius, or breaks budget/latency ceilings.]
* **[Option B]:** [Why it was rejected.]

## Consequences & Verification Gate

* **Positive:** [Security guarantee or operational resilience gained.]
* **Trade-off:** [Added complexity, migration discipline, or runtime cost accepted.]
* **Verification Gate:** [Automated test, CI check, or SQL assertion proving the invariant cannot regress.]
