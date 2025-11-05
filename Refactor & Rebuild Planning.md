---
title: Refactor & Rebuild Planning
tags:
  - it-systems-developer
  - rebuild
  - refactor
  - architecture
  - planning
status: Active
last-updated: 11/04/25
---

## 🎯 Objective
Define an evidence-based plan to replace the legacy PHP system with a maintainable, secure, and performant platform. Produce architecture, migration strategy, and an incremental rollout plan that minimizes risk and downtime.

---

## 📍 Scope
**Includes**
- Architecture options (monolith vs modular monolith vs services)
- Tech stack evaluation and decision
- Non-functional requirements (security, reliability, performance)
- Data model strategy and migration plan
- Environments, CI/CD, testing, and observability
- Rollout strategy (strangler pattern, phased cutovers)
- Costing, timelines, and risk management

**Excludes**
- Day-to-day legacy code documentation (see [[Codebase Analysis & Documentation]])
- Greenfield feature ideation (see [[Greenfield Projects & Strategic Visibility]])

---

## 🧭 Decision Framework

### Evaluation Criteria (with example weights)
| Criterion | Description | Weight |
|---|---|---|
| Maintainability | Code clarity, ecosystem maturity, onboarding ease | 0.20 |
| Delivery Speed | Dev velocity, scaffolding, batteries-included | 0.15 |
| Reliability | Operational stability, error handling, maturity | 0.15 |
| Security | First-class security libs & patterns | 0.15 |
| Performance | Throughput/latency characteristics | 0.10 |
| Hiring/Onboarding | Talent pool, learning curve for team | 0.10 |
| Cost | Infra, licensing, hosting | 0.10 |
| Interop | Fit with existing tools, DBs, integrations | 0.05 |

> Adjust weights as we learn more.

### Candidate Stacks (example short list)
- **Modern PHP**: Laravel (full-stack), Symfony (enterprise), Slim (micro)  
- **JVM**: Spring Boot (Java/Kotlin)  
- **Node**: NestJS (TypeScript)  
- **.NET**: ASP.NET Core (C#)

### Scoring Matrix (example)
| Option | Maint | Speed | Reliab | Sec | Perf | Hire | Cost | Interop | Weighted Score |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Laravel | 9 | 9 | 8 | 8 | 8 | 8 | 9 | 9 | **8.45** |
| Symfony | 9 | 7 | 9 | 9 | 8 | 7 | 8 | 9 | 8.30 |
| Spring Boot | 8 | 7 | 9 | 9 | 9 | 8 | 7 | 8 | 8.10 |
| NestJS | 8 | 8 | 8 | 8 | 8 | 9 | 8 | 8 | 8.10 |
| ASP.NET | 8 | 7 | 9 | 9 | 9 | 7 | 7 | 8 | 7.95 |

> Populate with real findings; keep the spreadsheet in `/docs/stack-decision-matrix.xlsx`.

---

## 🧱 Architecture Direction

### Target Shape
- **Preferred:** Modular monolith with clear bounded contexts and internal modules (enables later extraction).
- **Service Boundaries:** Only extract when (a) clear scaling needs, (b) deployment isolation needed, or (c) org boundaries demand it.

### Core Building Blocks
- HTTP API (REST first; GraphQL optional later)
- Background workers / job queues
- Centralized configuration & secrets management
- Repository/Service layers with DTOs
- Validation & authorization as first-class concerns

### C4 Diagrams
- Ctx + Container (mandatory)
- Component diagrams for hot paths
- Sequence diagrams for top flows (Auth, Core CRUD, Reporting)

---

## 🔐 Non-Functional Requirements (NFRs)

### Security (MVP)
- **OWASP ASVS-aligned inputs & outputs**
- Least-privilege DB/service accounts
- Secret rotation policy; no secrets in repo
- CSRF protection, output encoding, rate limits
- Centralized authN/authZ (role/permission model)

### Reliability & Ops
- Health endpoints (liveness/readiness)
- Graceful shutdown for workers
- Error budgets & lightweight SLOs (e.g., 99.5% API availability)
- Rollback plan with versioned artifacts

### Performance
- P95 latency targets per endpoint class
- DB indices for hot queries; slow-query logging
- Caching strategy (app-level + HTTP cache headers)

---

## 🗃️ Data Strategy

### Current → Target Model
- Produce an ERD diff (legacy vs target)
- Identify denormalizations and required cleanup
- Define canonical IDs and data contracts

### Migration Plan
- Prefer **online migrations**:
  1. Add new tables/columns (backfill)
  2. Dual-write (temporary) if needed
  3. Flip reads to new path
  4. Remove old columns/paths
- Establish **idempotent** migration scripts with checkpoints
- Run **restore drills** and data validation checksums

---

## 🧪 Testing Strategy

### Pyramid
- **Unit** (fast, deterministic)
- **Integration** (DB/repo, external contracts)
- **API/Contract** (OpenAPI + CI validation)
- **E2E Smoke** (post-deploy checks)

### Golden Paths & Fixtures
- Curate small golden datasets for reproducible tests
- Snapshot tests for reports where appropriate

---

## 🚀 CI/CD Design (MVP)

- Git strategy: trunk or short-lived feature branches
- CI: lint → unit → integration → artifact
- CD: staged deploy (dev → stage → prod) with approvals
- Blue/green or canary toggles where feasible
- Post-deploy smoke + automated rollback criteria
- Infra as Code for repeatability (even if minimal at first)

---

## 🧪 Proofs of Concept (timeboxed)

Create small, comparable PoCs for top-2 stacks:
- CRUD of a representative domain (forms, validation, auth)
- File upload & background job
- DB migration & rollback demo
- Containerization & deploy script
- Observability hooks (logging/errors)

> Capture setup time, LOC, test ergonomics, and developer experience notes.

---

## 🔄 Rollout Strategy

- **Strangler Fig** approach:
  - Route-by-route replacement behind a reverse proxy
  - Feature flags for gradual exposure
  - Dual-run in read-only scenarios for validation (optional)
- **Cutover Checklist**
  - Data frozen (if needed), backup validated
  - New env healthy, smoke green ??
  - Stakeholder comms & rollback window set
  - On-call assigned and contact sheet posted

---

## 💸 Costing & Timelines

### Rough Order of Magnitude (ROM)
- Infrastructure (compute/storage/network)
- Monitoring/logging/error tracking
- Backups and disaster recovery storage
- Third-party services (email/SMS, auth, etc.)

### Milestones (90–180–270)
- **0–90 days**: PoCs, decision matrix, baseline NFRs, target architecture v1
- **90–180 days**: Build core domains, CI/CD v1, initial migrations, first strangled route live
- **180–270 days**: Complete critical domain cutovers, deprecate legacy routes, stabilize ops

---

## 👥 People & Change Management

- Onboarding guide for new contributors
- Coding standards and PR checklist
- KBs for deploys, incident response, and runbooks
- Stakeholder updates: monthly summary & demo cadence

---

## 🧨 Risks & Mitigations (living)
| ID | Risk | Impact | Likelihood | Mitigation | Status |
|---|---|---:|---:|---|---|
| RR-01 | Unknown data quality issues | High | Med | Early profiling, checksums, dry-run migrations | Open |
| RR-02 | Rollback complexity | High | Med | Versioned artifacts + tested rollback runbook | Open |
| RR-03 | Scope creep | Med | High | Change control + RFC process | Open |
| RR-04 | Skill gap on chosen stack | Med | Med | PoCs + targeted training plan | Open |

---
## 📄 RFC Template (for key decisions)

RFC-XXX: {Title}

Owner: {name}
Status: Draft | Accepted | Rejected
Context: Why this is needed
Options Considered: A, B, C (pros/cons)
Decision: Chosen option + rationale
Impact: Systems, teams, timelines
Migration/Plan: Steps & rollback
Open Questions: ...x

---

## ✅ Next Actions
- [ ] Complete candidate stack PoCs and document results
- [ ] Populate the decision matrix with real scores & weights
- [ ] Draft target C4 Context/Container diagrams
- [ ] Write initial migration approach for one core domain
- [ ] Define SLOs and add post-deploy smoke tests to CI/CD
- [ ] Prepare Cutover Runbook v0
