---
title: Codebase Analysis & Documentation
tags:
  - it-systems-developer
  - codebase
  - documentation
  - php
  - discovery
status: Active
last-updated: 11/04/25
version: "1.0"
---
## 🎯 Objective
Establish a clear, accurate picture of the current PHP system (architecture, flows, data, deployments), identify technical debt and risks, and produce living documentation/KBs that enable safe maintenance now and an informed refactor/rebuild later.

---

## 📍 Scope
**Includes**
- PHP application structure, module ownership, dependencies
- HTTP flows, background jobs, integrations, auth/session model
- DB schemas, data flow, migrations, seeders, backups/restore
- CI/CD pipelines and environments (dev/stage/prod), secrets handling
- Observability: logging, metrics, error handling, alerting
- Operational runbooks and KBs

**Excludes (for now)**
- Rewriting/refactoring (tracked in [[Refactor & Rebuild Planning]])
- Greenfield features (tracked in [[Greenfield Projects & Strategic Visibility]])

---

## 📄 Deliverables
- 🗺️ **System Overview Diagram** (C4: Context + Container; optional Component where helpful)
- 🧱 **Module Map** (ownership, purpose, dependencies, maturity)
- 🗃️ **Database Map** (ERD + critical tables/indices + data contracts)
- 🚚 **CI/CD Map** (build, test, deploy, rollback, env vars/secrets)
- 📚 **KB Set**:
  - `KB/App/Overview`
  - `KB/App/Deployment`
  - `KB/App/Config & Secrets`
  - `KB/DB/Schema & Migrations`
  - `KB/Runbooks/Incident-001 <topic>`
  - `KB/Playbooks/<recurring-task>`
- 🧪 **Test Surface Catalog** (what exists, test gaps, risk-ranked priorities)
- 🧨 **Risk Register** (fail points, single points of failure, mitigations)
- 🧰 **Ops Tooling Inventory** (scripts, cron, monitors, access paths)

---

## ⏱️ Milestones (30–60–90 for this sub-thread)
### Days 1–30 (Discovery & Baseline)
- [ ] Read-only pass: clone repo(s), map directories, skim entrypoints (`index.php`, routers, controllers)
- [ ] Inventory composer packages; note custom forks or pinned versions
- [ ] Trace 3–5 critical user journeys end-to-end (HTTP → DB → side effects)
- [ ] Export DB schema; generate ERD; identify missing FKs/indices
- [ ] Document current deploy path (who presses what, from where, to where)
- [ ] Create initial **System Overview Diagram** (C4-Ctx/Container)
- [ ] Start **Risk Register** and **Module Map**

### Days 31–60 (Deep Dives & Runbooks)
- [ ] CI/CD deep dive: build steps, tests (if any), artifact, release strategy, rollback
- [ ] Secrets/config audit: where stored, rotated, backed up; standardize `.env` handling
- [ ] Logging/error handling: log format, sinks, retention; confirm PII handling
- [ ] Add **Runbooks** for top 3 recurring incidents + **Playbooks** for routine ops
- [ ] Draft **Test Surface Catalog**; propose minimum smoke suite

### Days 61–90 (Stabilize & Prep for Rebuild)
- [ ] Close documentation gaps; add component diagrams for hairy areas
- [ ] Prioritized **Tech Debt List** with impact/effort estimates
- [ ] Baseline observability (basic request logs + error rate + slow query log)
- [ ] Produce **Current State Report** to feed [[Refactor & Rebuild Planning]]

---

## 🧭 Work Plan

### 1) Repo & Structure Inventory
- [ ] List repos and purposes
- [ ] Identify app entrypoints (web, CLI), routers, controllers, services, helpers
- [ ] Note global state, singletons, and cross-cutting utilities
- [ ] Composer deps + any autoloading quirks

### 2) Runtime Flows
- [ ] Sequence diagrams for: Login, Order/Request lifecycle, Reporting, Admin actions
- [ ] Identify synchronous vs async work (cron/queues); failure modes

### 3) Data & Persistence
- [ ] Dump schema (with column types, nullability, defaults)
- [ ] ERD for core domains; list orphan/mysterious tables
- [ ] Index review (slow queries? composite indices needed?)
- [ ] Backups: schedule, location, restore drill notes

### 4) CI/CD & Environments
- [ ] Build steps, test commands, artifact details
- [ ] Deployment targets and method (ssh/rsync, FTP, container, etc.)
- [ ] Rollback procedure (document and test)
- [ ] Env diffs (dev/stage/prod), feature flags

### 5) Config, Secrets, Access
- [ ] Inventory secrets (DB creds, API keys); storage & rotation policy
- [ ] Privilege review for service accounts
- [ ] Audit who-can-deploy, who-has-prod-DB

### 6) Observability & Reliability
- [ ] Log format, sink(s), retention
- [ ] Centralized error tracking? (if not, propose minimum viable)
- [ ] Uptime checks, health endpoints
- [ ] SLO-ish basics: request latency/error rate dashboards (even manual first)

---

## 🧪 Test Surface Catalog (skeleton)
| Area | Current Coverage | Risk | Proposed Minimum Tests |
|------|-------------------|------|------------------------|
| Auth/session | None/Partial | High | Login happy-path, lockout, session expiry |
| Critical CRUD A | Unknown | High | Create/read/update/delete + validation |
| Reports | Manual | Medium | Deterministic sample snapshot compare |
| Integrations | Unknown | High | Contract tests w/ mock or sandbox |

---

## 🧨 Risk Register (living)
| ID | Area | Risk | Impact | Likelihood | Mitigation | Owner | Status |
|----|------|------|--------|------------|------------|-------|--------|
| R-001 | Deploy | No rollback | High | Med | Add artifact versioning + rollback doc | You | Open |
| R-002 | DB | No backups tested | High | Med | Monthly restore drill | You | Open |

---

## 🧱 Module Map (example structure)
| Module | Purpose | Entrypoint(s) | Depends On | Owner | Notes |
|--------|---------|---------------|------------|-------|-------|
| Auth | Login/session | `/auth/*` | DB, Email | You | Global state usage |
| Orders | Core CRUD | `/orders/*` | Auth, DB | You | Large controllers |

---

## 🧰 KB Templates

### Template: `KB/App/Overview`
- **Context:** What the app does, who uses it, critical paths  
- **Entrypoints:** web, CLI, cron  
- **Major Modules:** summary + links  
- **Integrations:** external services, APIs  
- **Diagrams:** Context + Container  

### Template: `KB/App/Deployment`
- **Branches & Versioning:** naming, protection  
- **Build Steps:** commands, artifacts  
- **Deploy Steps:** target(s), approvals, expected duration  
- **Rollback:** exact commands, verification steps  
- **Post-Deploy Checks:** health endpoints, smoke tests  

### Template: `KB/DB/Schema & Migrations`
- **ERD + Key Tables**  
- **Migrations Process:** tooling, ordering, rollback  
- **Data Contracts:** inputs/outputs shared with other systems  
- **Backup/Restore:** where, how, test logs  

### Template: `KB/Runbooks/Incident-XXX`
- **Symptom**  
- **Triage Checklist**  
- **Root Cause Patterns**  
- **Fix Steps**  
- **Prevention/Follow-up**

---

## 🔎 Audit Checklists

### Code Health
- [ ] Long files / God classes
- [ ] Mixed concerns (controller does SQL + rendering)
- [ ] Global state / singletons
- [ ] Input validation missing/inconsistent
- [ ] Direct SQL vs abstraction
- [ ] Hardcoded secrets/paths
- [ ] Duplicate logic (copy-paste)

### Security Basics
- [ ] AuthN/AuthZ boundaries
- [ ] CSRF for forms
- [ ] Output encoding (XSS)
- [ ] SQL injection surfaces
- [ ] Password policy & storage method
- [ ] File upload handling

### Performance
- [ ] N+1 queries in hot paths
- [ ] Missing indices for filters/joins
- [ ] Heavy work on request thread
- [ ] Large payloads/unbounded queries

---

## 🧭 Decisions / Next Steps
- [ ] Confirm authoritative repos and environments
- [ ] Choose diagramming convention (C4 + simple sequence diagrams)
- [ ] Standardize KB locations/naming
- [ ] Schedule a **read-only demo** of current deploy + a **restore drill** of DB backup

---

## ✅ Today / This Week (quick start)
- [ ] Clone repo; generate dependency and route maps
- [ ] Identify top 3 user journeys and trace them
- [ ] Export schema; draft ERD
- [ ] Write `KB/App/Overview` (stub) and `KB/App/Deployment` (stub)