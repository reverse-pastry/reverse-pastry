# Invoicer Example

A full invoicing system with no application. No server, no UI, no CLI. Just JSON files, schemas, and an AI agent.

This example replaced [Invoice Ninja](https://invoiceninja.com), which required Docker, MySQL, Redis, Nginx, PHP, and a web browser just to create an invoice.

## What's in the core

```
schema/              # JSON Schema definitions
  company.json       # Business entity
  client.json        # Client orgs + contacts
  invoice.json       # Invoice with line items
  product.json       # Reusable line item templates

data/                # Actual data, git-tracked
  company.json
  clients.json
  products.json
  invoices/
    ACME-101.json    # Paid
    ACME-102.json    # Sent, outstanding
    ACME-103.json    # Draft
```

## How it works

You open your AI agent in the repo and talk to it.

- "Create an invoice for Globex, 3 days of strategy consulting."
- "What invoices are outstanding?"
- "Mark ACME-101 as paid."
- "Generate a PDF for ACME-102."

The agent reads `CLAUDE.md`, understands the schemas and data, and does the work. The schema constrains the output so the agent can only produce valid invoices.

## Try it

Clone the repo and point your agent at the invoicer example:

```
git clone https://github.com/reverse-pastry/reverse-pastry.git
cd reverse-pastry/examples/invoicer
```

Then tell your agent: *"Read CLAUDE.md and help me manage invoices."*

## Browse the source

- [Schemas](https://github.com/reverse-pastry/reverse-pastry/tree/main/examples/invoicer/schema)
- [Sample data](https://github.com/reverse-pastry/reverse-pastry/tree/main/examples/invoicer/data)
- [Agent instructions (CLAUDE.md)](https://github.com/reverse-pastry/reverse-pastry/blob/main/examples/invoicer/CLAUDE.md)
