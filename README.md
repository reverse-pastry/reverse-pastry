# Reverse Pastry

An architectural pattern for building AI-native software.

## The Idea

Traditional software is a **hard shell with a soft interior** — rigid UI, messy internals. Users conform to the interface. The data model is an afterthought buried under layers of framework.

**Reverse Pastry inverts this.** Hard, deterministic core. Soft, adaptive exterior.

```
Traditional Software          Reverse Pastry
┌─────────────────────┐       ┌─────────────────────┐
│ ░░░ Hard UI ░░░░░░░ │       │ ~~~ Soft AI ~~~~~~~~ │
│ ░░░░░░░░░░░░░░░░░░░ │       │ ~ natural language ~ │
│                     │       │ ~ adapts to you ~~~~ │
│   soft messy guts   │       │                     │
│   scattered logic   │       │   ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  │
│   implicit schema   │       │   ▓ hard schema ▓▓  │
│                     │       │   ▓ strict data ▓▓  │
│ ░░░░░░░░░░░░░░░░░░░ │       │   ▓ clear rules ▓▓  │
└─────────────────────┘       └─────────────────────┘
```

The **core** is your schema, your data, your business rules — deterministic, version-controlled, inspectable. The **exterior** is an AI agent that speaks human and translates intent into operations on that core. The AI is replaceable. The core is not.

## Why It Works

1. **The AI doesn't need to be perfect.** When the schema is strict and well-defined, even a probabilistic AI produces deterministic outcomes. It can only create valid invoices, valid records, valid queries — because the core constrains it.

2. **No UI to maintain.** The interface is natural language. It adapts to the user, not the other way around. No buttons to arrange, no forms to build, no CSS to debug.

3. **The data is yours.** Plain files in a git repo. JSON, CSV, SQL — not locked in a database behind a web app you have to host and maintain. You can read it, diff it, back it up, move it anywhere.

4. **It's already how experts work.** A senior engineer doesn't need a GUI to query a database. They need the schema and a terminal. Reverse Pastry makes that accessible to everyone through AI.

## Principles

### 1. Schema is your contract, not your UI
The data model is the most important artifact. Design it first. Make it strict. Everything else flows from it.

### 2. The AI is replaceable, the core is not
If you swap Claude for GPT or Gemini or whatever comes next, the schema, data, and rules still work. The soft exterior is interchangeable. The hard core persists.

### 3. Version your core, let the interface be ephemeral
Your schema and data belong in git. Your AI conversations don't. The core has history. The interface is disposable.

### 4. Constrain the AI with structure, not instructions
Don't tell the AI "make sure the invoice number is formatted correctly." Define a schema where it *can't* be formatted incorrectly. Structure beats prompting.

### 5. Files over services
A JSON file beats a REST API. A SQLite database beats a hosted MySQL server. A CSV beats a dashboard. Reduce infrastructure to near zero.

### 6. Skills over features
Instead of building features into an application, define skills that an AI agent can invoke. Skills are composable, discoverable, and don't require a deployment pipeline.

## Anti-Patterns

| Pattern | Core | Exterior | Result |
|---------|------|----------|--------|
| **Reverse Pastry** | Hard | Soft | Reliable + flexible |
| Traditional Software | Soft | Hard | Rigid + fragile |
| Vibe Coding | Soft | Soft | Chaos — nothing is reliable |
| Enterprise Software | Hard | Hard | Reliable + miserable to use |

The worst failure mode is **soft core + soft exterior** — asking AI to build on top of unstructured data with no schema. It feels magical until it hallucinates a field that doesn't exist or silently corrupts your data.

## How To Apply It

### 1. Define your core

Start with the data model. What entities exist? What are their relationships? What fields are required? What are the constraints?

```
schema/
  company.json       # company profile schema
  clients.json       # client + contact schema
  products.json      # reusable line item templates
  invoices.json      # invoice schema with line items
```

### 2. Store data as plain files

```
data/
  company.json
  clients.json
  products.json
  invoices/
    INV-001.json
    INV-002.json
```

Git-tracked. Human-readable. Diffable. No server required.

### 3. Query when needed, don't maintain a database

Use DuckDB, jq, or even grep to query your files on demand:

```sql
-- DuckDB can query JSON files directly with SQL
SELECT number, amount, status
FROM read_json('data/invoices/*.json')
WHERE status = 'sent' AND amount_due > 0
```

### 4. Define skills for your AI agent

Skills are the "soft exterior." They translate natural language into operations on your core:

- `/new-invoice` — creates a new invoice file from a conversation
- `/list-unpaid` — queries outstanding invoices
- `/generate-pdf` — renders an invoice to PDF
- `/email-invoice` — sends an invoice to the client

### 5. Let the AI handle the rest

Reporting, formatting, edge cases, explanations — the AI handles the soft parts. The schema handles the hard parts. Together, they're a full application.

## Case Study: JZSW Invoicer

This pattern emerged from replacing Invoice Ninja — a full-featured invoicing platform that required Docker, MySQL, Redis, Nginx, PHP, and a web browser just to create an invoice.

The replacement:
- A git repo with JSON files and a documented schema
- Claude Code skills for creating invoices, generating PDFs, emailing clients
- No server. No Docker. No login screen. No subscription.

Total infrastructure: one directory on disk.

## Getting Started

1. Pick a domain (invoicing, inventory, project tracking — anything with structured data)
2. Design your schema first
3. Store your data as plain files in a git repo
4. Write skills for your AI agent to operate on the data
5. Stop building UIs

## Contributing

This is an evolving pattern. If you're applying it, open an issue or PR with your experience. What worked, what didn't, what's missing.

## License

MIT
