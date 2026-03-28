# Reverse Pastry

An architectural pattern for building AI-native software.

## The Idea

Vibe coded apps look like progress but repeat the oldest mistake in software: **hard shell, soft interior.** The AI generates a rigid UI fast, but the internals are a mess. Scattered logic, implicit schema, no structure anyone can inspect or trust. Users still conform to the interface. The data model is still an afterthought.

**Reverse Pastry inverts this.** Hard, deterministic core. Soft, adaptive exterior.

```
     Vibe Coded Apps                  Reverse Pastry Apps
┌─────────────────────────┐       ┌─────────────────────────┐
│ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │       │ ░░░░░░░░░░░░░░░░░░░░░░░ │
│ ▓▓▓▓▓▓  Hard UI  ▓▓▓▓▓▓ │       │ ░░░ Chat with Agent  ░░ │
│ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │       | ░░░░░░░░░░░░░░░░░░░░░░░ │
│                         │       |                         |
| ░░░░░░░░░░░░░░░░░░░░░░░ |       | ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │
│ ░░  soft messy guts  ░░ │       │ ▓▓▓▓  hard schema  ▓▓▓▓ │
│ ░░  scattered logic  ░░ │       │ ▓▓▓▓  strict data  ▓▓▓▓ │
│ ░░  implicit schema  ░░ │       │ ▓▓▓▓  clear rules  ▓▓▓▓ │
│ ░░░░░░░░░░░░░░░░░░░░░░░ │       │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │
└─────────────────────────┘       └─────────────────────────┘
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
| Vibe Coded Apps | Soft | Hard | Fast to build, fragile to run — AI generates the same rigid UI with messier guts |
| Enterprise Software | Hard | Hard | Reliable + miserable to use |

The worst failure mode is vibe coding without a core. AI generates a UI, generates a backend, generates a database schema on the fly. It feels magical until it hallucinates a field that doesn't exist or silently corrupts your data. The generated UI looks solid but there's nothing underneath holding it together.

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

## Case Study: Invoicer

This pattern emerged from replacing Invoice Ninja — a full-featured invoicing platform that required Docker, MySQL, Redis, Nginx, PHP, and a web browser just to create an invoice.

The replacement:
- A git repo with JSON files and a documented schema
- Claude Code skills for creating invoices, generating PDFs, emailing clients
- No server. No Docker. No login screen. No subscription.

Total infrastructure: one directory on disk.

## The Bigger Picture: Cores Replace Apps

Today you go to an app store, download an invoicing app, and conform to its interface. Reverse Pastry points to a different future.

### Clone, don't install

Instead of app stores, imagine **core stores** — repositories of structured functionality that you clone. An invoicing core. A time-tracking core. A CRM core. Each one is a schema, a set of rules, sample data, and skill definitions. You clone it, your AI agent reads it, and you're running.

There is no app to install. No account to create. No subscription. You own the core, the data lives on your machine, and the interface is whatever AI agent you already use.

### Capabilities, not commands

A traditional CLI exposes rigid commands: `create-invoice --client=foo --amount=100`. But even CLIs are too rigid for this model. A core exposes **capabilities** — declarations of what operations are possible and what inputs they need:

```json
{
  "capability": "create_invoice",
  "requires": ["client_id", "line_items"],
  "optional": ["due_date", "notes", "po_number"],
  "produces": "invoice"
}
```

The AI agent reads the capability and figures out how to invoke it — through conversation, through voice, through another agent, through whatever interface exists tomorrow. The core doesn't care how it's called. It only cares that the inputs are valid.

### The agent is the integration layer

If you have an invoicing core and a time-tracking core, they don't need to know about each other. Your AI agent connects them: *"invoice the client for last week's tracked hours."* The agent reads both cores, understands their schemas, and bridges them.

This replaces the entire integration industry — Zapier, custom APIs, webhooks, middleware. The cores are just structured data. The agent is the glue.

### Interfaces are fluid

Today the interface is a chat in the terminal. Tomorrow it might be voice. Next year, AI agents might generate visual interfaces on the fly — dashboards, forms, charts — rendered from the core's schema and data for a specific moment, then discarded.

The core doesn't change. The interface is ephemeral. **Invest your effort in the core, not the interface.** The interface will be rebuilt by every generation of AI. The core persists.

### What vibe coding gets backwards

Vibe coding spends all the AI's effort on the interface and almost none on the data model. Reverse Pastry inverts this. The data model is the product. The interface is free, provided by the AI agent the user already has.

This is why the pattern matters beyond any single use case. It's not about invoicing. It's about a fundamental shift in where value lives in software: **the core is the product, the interface is ambient.**

## This Repo

This is a **meta repo**. It documents the pattern and provides reference examples. It is not an application.

If you want to learn the pattern or build a new app using it, point your AI agent at [`agent-instructions.md`](agent-instructions.md). The agent will learn how to apply the pattern and help you build your own core.

If you want to see the pattern applied, look at [`examples/`](examples/). Each example has its own `CLAUDE.md` with operating instructions that an agent can read and use immediately.

**Meta repo (this):** teaches the pattern. How to design cores, structure schemas, define capabilities.

**App repo (what you build):** is the pattern applied. Schemas, data, agent instructions, and nothing else. No code to run. The agent is the interface.

## Open Questions

### Trust and verification
If you clone a core from a public store, how do you know the schema is sound? Traditional apps have reviews and ratings. Cores might need schema validation standards, community audits, or signed schemas. This is unsolved.

### Migration
Cores will evolve. Version 2 of an invoicing core might add `tax_rules` as a new entity. With plain files in git, migration could be a documented transform — a script or a skill that reshapes data from v1 to v2. But the pattern needs a convention for this.

### Composability standards
For agents to bridge cores, cores might need a shared way to describe their capabilities and schemas. Something lighter than OpenAPI, more structured than a README. Possibly just a convention for where to put schema files and capability definitions.

## Try It

Point your AI agent at the repo and say: *"Read agent-instructions.md and help me build a [time tracker / CRM / inventory system / whatever] using the Reverse Pastry pattern."*

The agent learns the pattern and walks you through building your own core. No setup. No install. Just a conversation.

## Getting Started

1. Pick a domain (invoicing, inventory, project tracking — anything with structured data)
2. Design your schema first
3. Store your data as plain files in a git repo
4. Write a CLAUDE.md so any agent can operate your core
5. Stop building UIs

## Explore

- [GitHub repo](https://github.com/reverse-pastry/reverse-pastry) with full pattern docs, examples, and schemas
- [Agent instructions](https://github.com/reverse-pastry/reverse-pastry/blob/main/agent-instructions.md) to point your AI agent at
- [Invoicer example](https://github.com/reverse-pastry/reverse-pastry/tree/main/examples/invoicer) showing the pattern applied to invoicing with working sample data
- MIT licensed. Open to contributions.

## Contributing

This is an evolving pattern. If you're applying it, open an issue or PR with your experience. What worked, what didn't, what's missing.

## License

MIT
