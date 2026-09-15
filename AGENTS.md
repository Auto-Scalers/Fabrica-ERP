# Fabrica ERP — Worker Instructions

## What This Folder Is

**Fabrica ERP** is a self-hosted Odoo ERP instance configured for our use case. Real human users manage business operations through standard Odoo modules.

## Tech Stack

- Self-hosted Odoo (standard modules)
- Standard Odoo modules: CRM, Sales, Accounting, Inventory, Project, Helpdesk, Manufacturing, Purchase, HR, Email, Website

## Conventions

1. We **implement** existing Odoo modules — we do NOT develop custom modules from scratch.
2. Configure standard modules for our use case.
3. Connect to n8n through standard APIs and webhooks.

## Key Directories

```
config/           — Odoo configuration
modules/          — References to standard Odoo modules
users/            — Human user accounts
```

## What You Do NOT Do

- Do NOT develop custom Odoo modules from scratch
- Do NOT commit or push
