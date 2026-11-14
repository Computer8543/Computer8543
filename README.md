## Hi, I'm Cynthia Ingersoll — Security SRE & Systems-First Builder

I approach cybersecurity as a **systems engineering and resilience discipline**, not a spectator sport. Instead of treating security as an afterthought or isolated audit checklist, I build deterministic guardrails, least-privilege data boundaries, and automated CI/CD verification directly into the infrastructure and codebases I ship.

---

### Core Engineering & Security Focus

* **Database & Application Invariants (`Breadth of View`):** Designing metadata-immune PostgreSQL Row-Level Security (RLS) policies, schema-qualified cryptographic checks (`extensions.digest`), and COPPA-compliant data isolation for youth-facing mobile and WebGL platforms on Fly.io and Supabase.
* **Network Telemetry & CI/CD Governance (`LAN Atlas`):** Serving as Founding Platform Lead and Trusted Committer for [Cloud Security Office Hours — LAN Atlas](https://github.com/CloudSecurityOfficeHours/LANAtlas), maintaining Docker/Codespaces environment parity and enforcing automated GitHub Actions `Super-Linter` merge gates.
* **Systems Foundations & Operational Resilience (`WGU B.S. CS` + Field Ops):** Pairing Computer Science fundamentals and **LPI Linux Essentials** POSIX hardening with real-world incident triage—including multi-site network outage recovery, 5-day SLA fleet patching across 40+ endpoints, and post-ransomware EDR remediation.

---

### Architectural Principles I Build By

1. **Invariants Over Hope:** Enforce security at the state-machine and database layer so entire classes of privilege escalation and configuration drift cannot occur silently.
2. **Reproducibility as a Security Control:** Eliminate "works on my machine" drift with containerized dev environments, deterministic builds, and protected Git branch governance.
3. **Resilience Under Real Constraints:** Architect for micro-budgets, strict youth-privacy mandates, and clean single-engineer handover continuity.

---

### Stack & Verification Tooling

* **Languages & Data:** Python, SQL (PostgreSQL / RLS), C++, Swift, Bash, HTML/CSS
* **Infrastructure & Containers:** Linux (POSIX hardening, cgroups, systemd), Docker, Podman, Fly.io, Supabase
* **Governance & CI/CD:** Git, GitHub Actions, Super-Linter, automated invariant harnesses, Architecture Decision Records (ADRs)
