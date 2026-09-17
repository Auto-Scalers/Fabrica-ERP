# Fabrica ERP — Tasks

> Single source of truth for Odoo ERP implementation. The Roadmap (`Fabrica-ERP-roadmap.md`) tracks high-level phases — this file owns execution details.
>
> **Principle:** We implement standard Odoo 19 modules. We do NOT develop custom modules from scratch.

---

## What This Project Is

Self-hosted Odoo 19 ERP instance configured for our use case. The foundation for all Fabrica business operations. Humans manage work through Odoo's web UI. Agents manage work through the JSON-2 API.

**Tech stack:** Odoo 19 Community + PostgreSQL, installed directly on Windows.

### Data Model

```
Entity (Company / Business Unit / Brand)
 └── has many Projects
      └── has many Tasks
```

- **Entity** — top-level org unit. Each entity owns its own projects and has isolated data.
- **Project** — belongs to one entity.
- **Task** — belongs to one project. Has stages, assignees, deadlines.

---

## Odoo Integration (For Agents)

| Field | Value |
|-------|-------|
| API Endpoint | `http://localhost:8069` |
| API Protocol | JSON-2 (`POST /json/2/<model>/<method>`) |
| Database | Set during Odoo setup |
| Auth | Bearer token (API key per bot user) |
| Bot Users | One per agent role, created in Phase E |

### How Agents Use This

1. Read this file's **Checkpoint** table first.
2. Call `POST /json/2/project.task/search_read` to fetch tasks for your project.
3. Pick highest-priority task from Backlog or To Do.
4. Call `POST /json/2/project.task/write` to move stage to "In Progress".
5. Do the work.
6. Call `POST /json/2/project.task/write` to move stage to "Done" + update description.

---

## High-Level Goals

1. **Entity hierarchy.** Each entity (company/brand) owns its own projects. Full isolation between entities.
2. **Odoo 19 running locally.** Installed on Windows, database created, web UI accessible.
3. **Project module configured.** 8 task stages matching our workflow. Dependencies, sub-tasks, milestones enabled.
4. **Global project structure.** Fabrica-Global + per-sub-project Odoo projects, each with independent boards.
5. **Agent API access.** Bot users, API keys, verified end-to-end JSON-2 API calls.
6. **Standard modules configured.** CRM, Sales, Accounting, Helpdesk, HR installed and ready.

---

## Rollup

| Metric | Value |
|---|---|
| Total tasks | 0 |
| ✅ DONE | 0 |
| 🔶 IN_PROGRESS | 0 |
| 👀 VERIFY | 0 |
| ⬜ TODO | 0 |
| 🚫 BLOCKED | 0 |
| ❌ CANCELLED | 0 |
| Completion | 0% |

---

## Checkpoint (Current State)

| Field | Value |
|---|---|
| **Current Group** | — |
| **Current Task** | — |
| **Last Action** | Planning complete. All tasks moved to roadmap. |
| **Next Action** | Refine roadmap phases |
| **Blockers** | None |

---

## Dependencies & Coordination Rules

- Only the orchestrator creates sessions in this ledger
- Workers are released after review
- Never leave orphaned sessions
- Each task belongs to exactly one group

---

## Session Ledger

| Handle | Type | Task ID | Status | Created |
|---|---|---|---|---|
| — | — | — | — | — |

---

*Created: 2026-09-16*
*Last updated: 2026-09-16*
