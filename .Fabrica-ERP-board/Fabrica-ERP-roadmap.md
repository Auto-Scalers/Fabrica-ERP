# Fabrica ERP — Roadmap

> A multi-entity, multi-project ERP for both humans and agents. The goal is a single system where CLI agents and humans work on the same tasks, same projects, same entities — real-time, no sync issues.

---

## Vision

Build an ERP where agents and humans are first-class citizens. Agents read and write directly to Odoo via JSON-2 API. Humans use the Odoo web UI. Both see the same data, work on the same tasks, and follow the same workflows.

**Core principle:** Odoo is the source of truth. Markdown files are reference material for agents. The database is what matters.

---

## Data Model

```
Entity (Company / Business Unit / Brand)
 ├── Projects
 │    ├── Backlog      (random ideas, things to consider)
 │    ├── Roadmap      (high-level goals, direction)
 │    ├── Tasks        (work items: todo, in progress, done)
 │    └── Guidelines   (redlines, greenlines, rules)
 └── Agents
      └── Bot users with API keys
```

**Entity:** Top-level org unit. Each entity owns its projects and has full data isolation.

**Project:** Belongs to one entity. Has its own Backlog, Roadmap, Tasks, and Guidelines.

**Backlog:** Ideas and items waiting to be promoted to Roadmap or Tasks. Low-commitment holding area.

**Roadmap:** High-level goals and strategic direction. What are we building and why?

**Tasks:** Actual work items. Assigned to humans or agents. Has stages, priorities, deadlines.

**Guidelines:** Rules, redlines, greenlines. What agents must do, must not do, and should do.

---

## Fabrica Integration

Fabrica-ERP syncs with the Fabrica desktop app. Entities and projects come from Fabrica's data model — no duplicate entry.

**Data source:** `%APPDATA%\Fabrica\profiles\local-default\fabrica-data.json`

**Mapping:**
| Fabrica | ERP | Direction |
|---------|-----|-----------|
| Project Group | Entity | Fabrica → ERP |
| FolderWorkspace | Project | Fabrica → ERP |
| `folderPath` | Project folder path | Fabrica → ERP |

**Sync behavior:**
- **Manual trigger** — User clicks "Sync" button in ERP to pull from Fabrica
- **One-way** — Fabrica is source of truth for structure (entities, projects)
- **Archive on delete** — When entity/project is deleted in Fabrica, it gets archived in ERP (not deleted)
- **Restore on re-add** — When entity/project is re-added in Fabrica, it restores from ERP archive

**Folder structure:**
```
<folderPath>/
├── AGENTS.md    # Auto-created if missing (entity or project version)
└── README.md    # Auto-created if missing (task added to generate it)
```

**AGENTS.md auto-creation:**
- Check if exists in entity/project folder
- If not: create from preset template with entity/project names filled in
- Two versions: entity-level and project-level
- Last line: "Read README.md for project description"

**README.md auto-creation:**
- Check if exists in entity/project folder
- If yes: do nothing
- If no: add first task to project's task list to generate it

---

## Disk Layout

Each entity gets a folder. Inside it, each project gets a subfolder. These files are **reference material for agents** — not the source of truth (Odoo DB is).

```
<workspacePath>/
├── Fabrica/                      # Entity folder (from Fabrica Project Group)
│   ├── AGENTS.md                 # Entity-level agent instructions (auto-created)
│   ├── README.md                 # Entity description
│   └── Fabrica-app/              # Project folder (from Fabrica FolderWorkspace)
│       ├── AGENTS.md             # Project agent instructions (auto-created)
│       └── README.md             # Project description
│   └── Fabrica-web/
│       ├── AGENTS.md
│       └── README.md
```

**Why in workspace folders:** Agents already work in these folders. AGENTS.md and README.md are where agents look for context. No separate ERP directory needed.

---

## Agent ↔ ERP Integration

### How Agents Work

Agents use **JSON-2 API directly** — no MCP overhead. Simple HTTP calls to Odoo.

| Action | Method | Endpoint |
|--------|--------|----------|
| List tasks | `search_read` | `POST /json/2/project.task/search_read` |
| Create task | `create` | `POST /json/2/project.task/create` |
| Update task | `write` | `POST /json/2/project.task/write` |
| Read guidelines | `search_read` | `POST /json/2.note.note/search_read` |
| Read roadmap | `search_read` | `POST /json/2.project.project/search_read` |

**Auth:** Each agent is a bot user with its own API key. Bearer token in header.

### How Orchestrator Works

Orchestrator uses **MCP server** for configuration and setup tasks:
- Install/configure modules
- Create projects and entities
- Inspect model fields and relationships
- Diagnose issues

**MCP Server:** `uvx mcp-server-odoo` (YOLO mode, username/password auth)

**Config:** `opencode.json` in project root

**Available MCP tools:** `search_records`, `get_record`, `get_fields`, `get_current_context`, `list_models`, `create_record`, `update_record`, `delete_record`, `post_message`, `aggregate_records`

**Rule:** Use MCP tools for implementation/development only. Do not use as end-user.

### Concurrency Handling

| Scenario | How It Works |
|----------|--------------|
| **Agent vs Agent** | Agent claims task by moving stage to "In Progress". Second agent sees it's taken, picks another. Odoo's PostgreSQL handles atomic writes. |
| **Human vs Human** | Odoo's built-in record locking. Kanban drag-and-drop is atomic. Multiple humans can work on different tasks simultaneously. |
| **Agent vs Human** | Same API, same UI. If human moves a task while agent is reading, agent re-reads on next API call. No stale data — agents always fetch fresh state before acting. |

**Claim protocol:** Agent reads task → checks stage is not "In Progress" or "Done" → moves to "In Progress" → does work → moves to "Done". If stage changed during work, agent re-evaluates.

---

## Odoo Apps

### Installed
| Module | Purpose |
|--------|---------|
| **Project** | Core task management. Stages, dependencies, milestones. |
| **Discuss** | Messaging between agents and humans. Notifications, updates, threads. |

### Future (as needed)
| Module | Purpose |
|--------|---------|
| **Notes** | Guidelines, rules, redlines. Each entity/project can have note records as guidelines. |
| **Contacts** | Entity and contact management. Companies, people, relationships. |
| **Calendar** | Scheduling, deadlines, milestones. Visual timeline view. |
| **Documents** | File attachments, specs, design docs. Linked to projects/tasks. |
| **Helpdesk** | Support tickets, issues, bugs. Separate workflow from project tasks. |
| **CRM** | Leads, opportunities, pipeline. For entities that do sales. |
| **Sales** | Quotations, orders. For entities that sell products/services. |
| **Accounting** | Invoicing, financials. For entities that need billing. |
| **HR** | People, departments, roles. For entities with employees. |
| **Website** | External-facing portal. For entities that need a public site. |

---

*Created: 2026-09-16*
*Last updated: 2026-09-17*
