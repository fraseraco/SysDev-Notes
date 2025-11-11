---
tags:
status: Active
last-updated: 11/05/25
version: "1.0"
---

**Purpose:** Maintain consistent identifiers and naming across all files (RFCs, KBs, SOPs, etc.) to improve traceability, cross-linking, and automation.

---

## 📘 General Guidelines

| Principle | Description |
|------------|--------------|
| **Readable > Short** | Prefer clarity (e.g., `Refactor_Plan` over `RFP_Plan`). |
| **Date Format:** | Always use ISO format `YYYY-MM-DD` (sortable). |
| **Separators:** | Use underscores `_` for internal separators, hyphens `-` for IDs. |
| **Case:** | Use PascalCase for titles (`Codebase_Analysis.md`), UPPERCASE for identifiers (`RFC-GF-001`). |
| **Prefix Hierarchy:** | `Type-Area-ID_Description.md` (e.g., `RFC-GF-002_Metrics_Dashboard.md`). |
| **Tags (optional):** | Add `#tags` inline or in YAML metadata instead of filenames. |

---

## 🧩 Document Type Reference

| Type | ID Format | Example | Description |
|------|------------|----------|-------------|
| **RFC** (Request for Comments) | `RFC-<Area>-<###>_<Title>` | `RFC-GF-001_Asset_Tracker_App.md` | Formal proposals for new systems, greenfield projects, or architecture changes. |
| **ADR** (Architecture Decision Record) | `ADR-<###>_<Topic>` | `ADR-001_TechStack_Selection.md` | Architectural choices and rationale; version-controlled. |
| **KB** (Knowledge Base) | `KB/<Category>/<Topic>` | `KB/App/Deployment.md` | Technical how-tos, reference material, and SOPs. Stored in KB folder. |
| **SOP** (Standard Operating Procedure)** | `SOP-<Area>-<###>_<Title>` | `SOP-OPS-002_Password_Reset.md` | Repeatable, procedural tasks. Often referenced from KB. |
| **Incident / Runbook** | `Incident-<###>_<Topic>` | `Incident-003_DB_Outage.md` | Incident-specific response docs. |
| **Report** | `Report_<Type>_<YYYY-MM-DD>` | `Report_Weekly_2025-11-07.md` | Weekly/monthly summaries or performance reports. |
| **Meeting Notes** | `Meeting_<Person/Team>_<YYYY-MM-DD>` | `Meeting_CTO_2025-11-05.md` | Meeting summaries; link to tasks or follow-ups. |
| **Learning Notes** | `Learn_<Topic>` | `Learn_PHP_Collections.md` | Learning reflections, code experiments, key takeaways. |
| **Script** | `verb_action.ext` | `reset_password.php` | CLI or automation scripts, grouped under `/scripts/`. |
| **Diagram** | `<system>-<level>.<ext>` | `app-context.puml`, `db-erd.drawio` | PlantUML or Draw.io diagrams. |
| **Template** | `<Type>_Template.md` | `RFC_Template.md` | Reusable structure for new docs. |
| **Checklist** | `Checklist_<Topic>.md` | `Checklist_Deployment_Validation.md` | Pre/post tasks for routine operations. |

---

## 📚 Folder-Level Naming

| Folder | Contents | Convention |
|---------|-----------|-------------|
| `/03_Projects/IT Systems Developer/RFCs/` | Architecture & feature proposals | `RFC-<Area>-<###>_<Title>.md` |
| `/03_Projects/IT Systems Developer/KB/` | Knowledge base content | Category subfolders (`App/`, `DB/`, `Runbooks/`) |
| `/03_Projects/IT Systems Developer/Docs/` | Architecture, diagrams, decision logs | `Topic_Area.md` |
| `/03_Projects/IT Systems Developer/Notes/` | Scratchpad and ad hoc thinking | `Topic_<YYYY-MM-DD>.md` |
| `/repo/docs/decisions/` | ADRs (version-controlled) | `ADR-###_Title.md` |
| `/repo/scripts/` | Automation scripts | `verb_action.ext` |
| `/repo/resources/sql/` | Queries, migrations | `purpose_table.sql` |
| `/repo/tests/` | Organized by test type | `unit/`, `integration/`, `smoke/` |

---

## 🧭 ID Prefix Reference

| Prefix | Area | Example |
|---------|-------|----------|
| `GF` | Greenfield projects | `RFC-GF-002_Metrics_Dashboard.md` |
| `RB` | Rebuild/Refactor | `RFC-RB-001_Laravel_Stack.md` |
| `OPS` | IT Operations | `SOP-OPS-004_Backup_Verification.md` |
| `APP` | Application-level docs | `KB-APP-005_Login_Module.md` |
| `DB` | Database | `KB-DB-003_Schema_Migrations.md` |
| `SEC` | Security | `KB-SEC-001_API_Auth.md` |
| `DEV` | Development / CI-CD | `KB-DEV-002_Pipeline_Structure.md` |
| `ARCH` | Architecture | `ADR-ARCH-001_Modular_Monolith.md` |

---

## 🧰 Examples in Context

**Example Workflow:**
1. You create an RFC for a new greenfield project idea:  
   → `RFC-GF-003_Internal_Request_Portal.md`  
2. Once accepted, you open an ADR in `/repo/docs/decisions/`:  
   → `ADR-004_Stack_Laravel.md`  
3. During development, you write a KB entry:  
   → `KB/App/Deployment.md`  
4. You automate part of IT workflow:  
   → `SOP-OPS-006_User_Onboarding.md` + `create_user_template.sh`  
5. Finally, you summarize the quarter:  
   → `Report_Quarterly_2025-Q1.md`  

All of these link back to one another in Obsidian using `[[WikiLinks]]`.

---

## 🧩 Metadata Block Template (for each doc)
Always include YAML frontmatter at the top:

---
title: RFC-GF-003 Internal Request Portal
type: RFC
status: Draft
author: Fraser Coyle
last-updated: {{date}}
tags: [greenfield, proposal, internal-tools]
related:
  - [[Greenfield Projects & Strategic Visibility]]
  - [[Refactor & Rebuild Planning]]
---
