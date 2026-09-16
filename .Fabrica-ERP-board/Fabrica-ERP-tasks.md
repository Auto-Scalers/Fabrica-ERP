# Fabrica ERP — Tasks

> Single source of truth for Odoo ERP implementation. The Roadmap (`Fabrica-ERP-roadmap.md`) tracks high-level phases — this file owns execution details.
>
> **Principle:** We implement standard Odoo 19 modules. We do NOT develop custom modules from scratch.

---

## What This Project Is

Self-hosted Odoo 19 ERP instance configured for our use case. The foundation for all Fabrica business operations. Humans manage work through Odoo's web UI. Agents manage work through the JSON-2 API.

**Tech stack:** Odoo 19 Community + PostgreSQL, installed directly on Windows.

---

## Odoo Integration (For Agents)

| Field | Value |
|-------|-------|
| API Endpoint | `http://localhost:8069` |
| API Protocol | JSON-2 (`POST /json/2/<model>/<method>`) |
| Database | Set during Odoo setup |
| Auth | Bearer token (API key per bot user) |
| Bot Users | One per agent role, created in Phase D |

### How Agents Use This

1. Read this file's **Checkpoint** table first.
2. Call `POST /json/2/project.task/search_read` to fetch tasks for your project.
3. Pick highest-priority task from Backlog or To Do.
4. Call `POST /json/2/project.task/write` to move stage to "In Progress".
5. Do the work.
6. Call `POST /json/2/project.task/write` to move stage to "Done" + update description.

---

## High-Level Goals

1. **Odoo 19 running locally.** Installed on Windows, database created, web UI accessible.
2. **Project module configured.** 8 task stages matching our workflow. Dependencies, sub-tasks, milestones enabled.
3. **Global project structure.** Fabrica-Global + per-sub-project Odoo projects, each with independent boards.
4. **Agent API access.** Bot users, API keys, verified end-to-end JSON-2 API calls.
5. **Standard modules configured.** CRM, Sales, Accounting, Helpdesk, HR installed and ready.

---

## Rollup

| Metric | Value |
|---|---|
| Total tasks | 26 |
| ✅ DONE | 0 |
| 🔶 IN_PROGRESS | 0 |
| 👀 VERIFY | 0 |
| ⬜ TODO | 26 |
| 🚫 BLOCKED | 0 |
| ❌ CANCELLED | 0 |
| Completion | 0% |

---

## Group A — Odoo Installation & Core Setup

> **WHAT THIS GROUP DOES:** Install Odoo 19 and PostgreSQL on Windows. Get the instance running and accessible.
> **WHAT THIS GROUP DOES NOT DO:** Configure modules, create projects, or set up API access.

| # | Task | Status | Output/Notes |
|---|------|--------|--------------|
| A1 | Download and install Odoo 19 Community on Windows (from odoo.com/download) | ⬜ TODO | Use direct Windows installer. NOT Docker. |
| A2 | Download and install PostgreSQL on Windows (from postgresql.org) | ⬜ TODO | Use stackbuilder or standalone installer. |
| A3 | Create Odoo database, verify web UI loads at `http://localhost:8069` | ⬜ TODO | Create database via Odoo database selector page. Set master password. |
| A4 | Configure `odoo.conf` — set admin password, addons path, proxy mode | ⬜ TODO | Config file location: check Odoo install dir or `~/.config/odoo/odoo.conf` |
| A5 | Verify Project module is installable and loads without errors | ⬜ TODO | Apps → search "Project" → Install |

---

## Group B — Project Module Configuration

> **WHAT THIS GROUP DOES:** Set up the Project module's task stages, features, and workflow.
> **WHAT THIS GROUP DOES NOT DO:** Create actual projects or tasks (that's Group C).

| # | Task | Status | Output/Notes |
|---|------|--------|--------------|
| B1 | Configure 8 task stages: Backlog, Roadmap, To Do, In Progress, In Review, Blocked, Done, Cancelled | ⬜ TODO | Project → Configuration → Task Stages. Set sequence order. |
| B2 | Enable task dependencies, sub-tasks, milestones in Project settings | ⬜ TODO | Project → Configuration → Settings → Tasks Management |
| B3 | Configure Kanban WIP limits per stage (if supported in Community) | ⬜ TODO | May require Enterprise. Check if available. If not, skip. |
| B4 | Set up project roles for task assignment | ⬜ TODO | Odoo 19 feature. Project → Configuration → Roles. |
| B5 | Verify full task lifecycle: create task → move Backlog → Roadmap → To Do → In Progress → Review → Done | ⬜ TODO | Test with a dummy task. Confirm stage transitions work. |

---

## Group C — Global Project Structure

> **WHAT THIS GROUP DOES:** Create the Odoo projects that mirror our folder structure.
> **WHAT THIS GROUP DOES NOT DO:** Fill projects with real tasks yet (structure only).

| # | Task | Status | Output/Notes |
|---|------|--------|--------------|
| C1 | Create "Fabrica-Global" project in Odoo | ⬜ TODO | This is the umbrella project for cross-project management |
| C2 | Add DNA to Fabrica-Global description field (mission, vision, values, anti-goals) | ⬜ TODO | Copy from `.Fabrica-board/Fabrica-DNA.md` |
| C3 | Create global milestones in Fabrica-Global (Phase A through E gates) | ⬜ TODO | One milestone per phase from Fabrica-Roadmap.md |
| C4 | Create individual Odoo projects for each sub-project | ⬜ TODO | Fabrica-ERP, Fabrica-n8n (future), Fabrica-web, Fabrica-app, etc. All empty, no tasks. |
| C5 | Verify each project has independent task board and stage configuration | ⬜ TODO | Open each project → Kanban view → confirm empty board |

---

## Group D — Agent API Access

> **WHAT THIS GROUP DOES:** Set up JSON-2 API access so agents can read and write tasks programmatically.
> **WHAT THIS GROUP DOES NOT DO:** Build integrations or connect n8n.

| # | Task | Status | Output/Notes |
|---|------|--------|--------------|
| D1 | Create dedicated bot users for agents (one per role) | ⬜ TODO | Settings → Users → New. Name: "Agent-PM", "Agent-Worker", etc. |
| D2 | Generate API keys for each bot user | ⬜ TODO | User → Account Security → New API Key. Store securely. |
| D3 | Test JSON-2 API: `search_read` on `project.task` | ⬜ TODO | `POST /json/2/project.task/search_read` with bearer token. Verify returns task list. |
| D4 | Test JSON-2 API: `create` task, `write` task, move stage | ⬜ TODO | Create dummy task → update description → move to "In Progress" → move to "Done" |
| D5 | Document API endpoint + credentials pattern in each project's AGENTS.md | ⬜ TODO | Add "Odoo Integration" section to every AGENTS.md |
| D6 | Verify agents can read tasks and update stages end-to-end | ⬜ TODO | Run a full agent workflow: read → pick task → update → complete |

---

## Group E — Standard Modules (Future)

> **WHAT THIS GROUP DOES:** Install and configure additional Odoo modules for business operations.
> **WHAT THIS GROUP DOES NOT DO:** Custom development. Only standard module configuration.

| # | Task | Status | Output/Notes |
|---|------|--------|--------------|
| E1 | Install and configure CRM module | ⬜ TODO | Apps → Install CRM. Configure pipeline stages. |
| E2 | Install and configure Sales module | ⬜ TODO | Apps → Install Sales. Configure quotation templates. |
| E3 | Install and configure Accounting module | ⬜ TODO | Apps → Install Accounting. Configure chart of accounts. |
| E4 | Install and configure Helpdesk module | ⬜ TODO | Apps → Install Helpdesk. Configure ticket stages. |
| E5 | Install and configure HR module | ⬜ TODO | Apps → Install HR. Configure departments, job positions. |

---

## Checkpoint (Current State)

| Field | Value |
|---|---|
| **Current Group** | Group A — Odoo Installation |
| **Current Task** | A1 — Download Odoo 19 Windows installer |
| **Last Action** | Planning complete. Tasks file created. |
| **Next Action** | Download and install Odoo 19 on Windows |
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
