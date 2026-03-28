# Example: Invoicer

A real-world application of the Reverse Pastry pattern, replacing a full Invoice Ninja installation.

## There is no app

This is not software you run. There is no server, no UI, no CLI. You clone this repo, open your AI agent (Claude Code, Cursor, whatever you use), and talk to it. The agent reads `CLAUDE.md`, understands the schemas and data, and operates the invoicing system through conversation.

"Create an invoice for Globex, 3 days of strategy consulting." The agent looks up the client, finds the product, generates the JSON, validates it against the schema, and writes it. Done.

See [CLAUDE.md](CLAUDE.md) for the full agent operating instructions.

## Before (The Old Way)

- Invoice Ninja (Laravel + PHP)
- Docker Compose: 4 services (app, nginx, mysql, redis)
- 75+ database tables
- Web UI requiring browser login
- Headless Chrome for PDF generation
- Subscription for white-labeling

## After (Reverse Pastry)

- A git repo
- JSON files for data
- JSON Schema for validation
- An AI agent as the interface

Total infrastructure: one directory on disk.

## Structure

```
schema/              # JSON Schema definitions
  company.json
  client.json
  invoice.json
  product.json
data/                # Actual data, git-tracked
  company.json
  clients.json
  products.json
  invoices/
    ACME-101.json
    ACME-102.json
    ACME-103.json
CLAUDE.md            # Agent operating instructions
```

## Core Entities

| Entity | Description |
|--------|-------------|
| Company | Your business info (name, address, logo, invoice prefix) |
| Client | Client organizations you invoice |
| Contact | Individual POCs at a client (multiple per client) |
| Product | Reusable line item templates (e.g., daily consulting rates) |
| Invoice | An invoice with embedded line items |

## What the agent can do

| Operation | What happens |
|-----------|-------------|
| Create invoice | Agent asks for client + line items, generates valid JSON, writes to `data/invoices/` |
| List invoices | Agent reads invoice files, filters by status/client/date/amount |
| Update status | Agent updates status and payment fields when invoices are sent or paid |
| Generate PDF | Agent renders invoice data to PDF using company branding |
| Report | Agent queries invoice data for summaries, aging, revenue by client |
