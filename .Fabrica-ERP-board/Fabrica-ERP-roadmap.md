# Fabrica ERP — Roadmap

> Central command for Odoo ERP implementation. Vision/identity lives in the task file descriptions. Execution details live in `Fabrica-ERP-tasks.md`. This file tracks high-level goals and cross-cutting status.

---

## High-Level Goals

> The why behind everything. Task details stay in the tasks file — this is the direction.

1. **Self-hosted Odoo 19 on Windows.** Install and configure Odoo 19 Community locally. Get the instance running, accessible, and ready for module configuration.
2. **Configure the Project module.** Set up task stages, project templates, and the standard workflow that all other projects will use. This is the foundation — every project and task flows through here.
3. **Create the global project structure.** One "Fabrica-Global" project for cross-project management, plus individual projects for each sub-project. Each gets its own backlog, roadmap, tasks, and DNA.
4. **Set up agent API access.** Configure JSON-2 API endpoint, create bot users, generate API keys. Agents must be able to read and write tasks programmatically.
5. **Configure remaining standard modules.** CRM, Sales, Accounting, Inventory, Helpdesk, HR, etc. — install and configure for our use case.
6. **Connect n8n to Odoo.** (Future phase) Wire up n8n automation workflows that read/write Odoo data through the API.

---

## Phases

### Phase A — Odoo Installation & Core Setup

| ID | Task | Status |
|----|------|--------|
| A1 | Install Odoo 19 on Windows (direct install, no Docker) | ⬜ TODO |
| A2 | Install PostgreSQL on Windows | ⬜ TODO |
| A3 | Create database, verify Odoo web UI accessible at localhost:8069 | ⬜ TODO |
| A4 | Configure odoo.conf (admin password, addons path, proxy mode) | ⬜ TODO |
| A5 | Install Project module and verify it loads | ⬜ TODO |

### Phase B — Project Module Configuration

| ID | Task | Status |
|----|------|--------|
| B1 | Configure 8 task stages (Backlog → Roadmap → To Do → In Progress → Review → Blocked → Done → Cancelled) | ⬜ TODO |
| B2 | Enable task dependencies, sub-tasks, milestones in Project settings | ⬜ TODO |
| B3 | Configure Kanban WIP limits per stage | ⬜ TODO |
| B4 | Set up project roles (if needed for task assignment) | ⬜ TODO |
| B5 | Verify task lifecycle: create → move through stages → done | ⬜ TODO |

### Phase C — Global Project Structure

| ID | Task | Status |
|----|------|--------|
| C1 | Create "Fabrica-Global" project (umbrella for all cross-project work) | ⬜ TODO |
| C2 | Add global DNA to Fabrica-Global project description (mission, vision, values) | ⬜ TODO |
| C3 | Create global milestones (Phase A through E gates) | ⬜ TODO |
| C4 | Create individual Odoo projects for each sub-project (empty, no tasks yet) | ⬜ TODO |
| C5 | Verify each project has its own independent task board | ⬜ TODO |

### Phase D — Agent API Access

| ID | Task | Status |
|----|------|--------|
| D1 | Create dedicated bot users for agents (one per agent role) | ⬜ TODO |
| D2 | Generate API keys for each bot user | ⬜ TODO |
| D3 | Test JSON-2 API: search_read on project.task | ⬜ TODO |
| D4 | Test JSON-2 API: create task, write task, move stage | ⬜ TODO |
| D5 | Document API endpoint + credentials pattern in each project's AGENTS.md | ⬜ TODO |
| D6 | Verify agents can read tasks and update stages end-to-end | ⬜ TODO |

### Phase E — Standard Modules (Future)

| ID | Task | Status |
|----|------|--------|
| E1 | Install and configure CRM module | ⬜ TODO |
| E2 | Install and configure Sales module | ⬜ TODO |
| E3 | Install and configure Accounting module | ⬜ TODO |
| E4 | Install and configure Helpdesk module | ⬜ TODO |
| E5 | Install and configure HR module | ⬜ TODO |

---

## Dashboard

> Updated as tasks complete.

| Phase | Total | Done | In Progress | TODO | Completion |
|-------|-------|------|-------------|------|------------|
| A — Odoo Installation | 5 | 0 | 0 | 5 | 0% |
| B — Project Module Config | 5 | 0 | 0 | 5 | 0% |
| C — Global Project Structure | 5 | 0 | 0 | 5 | 0% |
| D — Agent API Access | 6 | 0 | 0 | 6 | 0% |
| E — Standard Modules | 5 | 0 | 0 | 5 | 0% |
| **Total** | **26** | **0** | **0** | **26** | **0%** |

---

## Current Focus

> What runs NOW.

| Phase | What | Why |
|-------|------|-----|
| **A** | Install Odoo 19 + PostgreSQL on Windows | Foundation — nothing works without the instance |

---

## Next Actions

| # | What | Depends on | Status |
|---|------|-----------|--------|
| 1 | Download Odoo 19 Windows installer | — | ⬜ TODO |
| 2 | Download PostgreSQL Windows installer | — | ⬜ TODO |
| 3 | Install both, create database, verify UI | — | ⬜ TODO |
| 4 | Configure task stages in Project module | #3 done | ⬜ TODO |
| 5 | Create Fabrica-Global project | #4 done | ⬜ TODO |
| 6 | Create bot users + API keys | #3 done | ⬜ TODO |
| 7 | Test API end-to-end from agent | #6 done | ⬜ TODO |

---

*Created: 2026-09-16*
*Last updated: 2026-09-16*
*Status: Planning — Odoo 19 on Windows, no Docker*
