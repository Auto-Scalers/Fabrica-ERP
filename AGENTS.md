# Fabrica ERP — Worker Instructions

## What This Folder Is

**Fabrica ERP** is a self-hosted Odoo 19 ERP instance configured for our use case. Real human users manage business operations through standard Odoo UI. Agents manage tasks through the JSON-2 API.

## Tech Stack

- Self-hosted Odoo 19 Community (installed directly on Windows)
- PostgreSQL database
- Standard Odoo modules: Project, CRM, Sales, Accounting, Inventory, Helpdesk, Manufacturing, Purchase, HR, Email, Website
- JSON-2 API for agent integration

## Odoo Integration

| Field | Value |
|-------|-------|
| Odoo Project Name | `Fabrica-ERP` |
| API Endpoint | `http://localhost:8069` |
| API Protocol | JSON-2 (`POST /json/2/<model>/<method>`) |
| Database | Set during Odoo setup |
| Auth | Bearer token (API key from bot user) |
| API Key | Set via env var `FABRICA_ODOO_API_KEY` |

### How to Read Tasks

```
POST /json/2/project.task/search_read
Headers: Authorization: Bearer <API_KEY>, Content-Type: application/json
Body: {
  "domain": [["project_id.name", "=", "Fabrica-ERP"], ["stage_id.name", "!=", "Done"]],
  "fields": ["name", "stage_id", "priority", "user_ids", "date_deadline", "description"],
  "order": "priority desc, sequence asc"
}
```

### How to Update a Task Stage

```
POST /json/2/project.task/write
Body: {
  "ids": [<task_id>],
  "values": {"stage_id": <new_stage_id>}
}
```

### How to Create a Task

```
POST /json/2/project.task/create
Body: {
  "name": "Task title",
  "project_id": <project_id>,
  "stage_id": <backlog_stage_id>,
  "description": "Detailed description"
}
```

## Conventions

1. We **implement** existing Odoo modules — we do NOT develop custom modules from scratch.
2. Configure standard modules for our use case.
3. Agents use the JSON-2 API to read/write tasks — Odoo is the source of truth.
4. Humans use the Odoo web UI — Kanban, Gantt, list views.

## Key Directories

```
.Fabrica-ERP-board/   — Roadmap, tasks, planning docs
config/               — Odoo configuration (future)
```

## What You Do NOT Do

- Do NOT develop custom Odoo modules from scratch
- Do NOT create human-readable markdown mirrors — Odoo IS the source
- Do NOT commit or push

---

*Last updated: 2026-09-16*
