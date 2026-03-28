# Example: Invoicer

A real-world application of the Reverse Pastry pattern, replacing a full Invoice Ninja installation.

## Before (Traditional Software)

- Invoice Ninja (Laravel + PHP)
- Docker Compose: 4 services (app, nginx, mysql, redis)
- 75+ database tables
- Web UI requiring browser login
- Headless Chrome for PDF generation
- Subscription for white-labeling

## After (Reverse Pastry)

- A git repo
- JSON files for data
- Schema documentation
- Claude Code skills for operations
- Lightweight PDF generation

## Schema

See [schema/](schema/) for the data model.

## Core Entities

| Entity | Description |
|--------|-------------|
| Company | Your business info (name, address, logo, invoice prefix) |
| Client | Client organizations you invoice |
| Contact | Individual POCs at a client (multiple per client) |
| Product | Reusable line item templates (e.g., daily training rates) |
| Invoice | An invoice with embedded line items |

## Skills

| Skill | What it does |
|-------|-------------|
| `/new-invoice` | Create a new invoice interactively |
| `/list-invoices` | Show invoices, filterable by status/client/date |
| `/generate-pdf` | Render an invoice to PDF |
| `/email-invoice` | Send an invoice to client contacts |
| `/invoice-summary` | Generate a report for accountants |
