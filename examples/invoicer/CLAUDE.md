# Invoicer

This is a Reverse Pastry invoicing core. There is no application, no UI, no CLI. You are the interface. The user talks to you, and you operate on the data.

## Structure

```
schema/              # JSON Schema definitions
data/
  company.json       # Business entity info
  clients.json       # Client organizations and contacts
  products.json      # Reusable line item templates
  invoices/          # One JSON file per invoice
    ACME-101.json
    ...
```

## Rules

- Every invoice file must validate against `schema/invoice.json`
- Invoice numbers follow the pattern `{company.invoice_prefix}-{number}`, e.g. ACME-104
- When creating a new invoice, increment `company.invoice_counter` and use that number
- Line item `amount` must equal `quantity * unit_price`
- Invoice `subtotal` must equal the sum of all line item amounts
- Invoice `total` must equal `subtotal + tax`
- Invoice `amount_due` must equal `total - amount_paid`
- Status transitions: draft -> sent -> partial -> paid. Cancelled is valid from any state.
- `due_date` is calculated from the invoice `date` plus the client's `payment_terms` (or company `default_payment_terms` if the client doesn't override)

## Operations

**Creating invoices:** Ask the user which client, what line items, and any notes. Look up the client in `clients.json` and products in `products.json`. Generate the invoice JSON, validate it against the schema, and write it to `data/invoices/`.

**Listing invoices:** Read all files in `data/invoices/`. Filter by status, client, date range, or amount as requested.

**Updating status:** When an invoice is sent or paid, update the status and `amount_paid` fields. Update `updated_at`.

**Reporting:** Summarize outstanding invoices, total revenue, amounts by client, aging reports. Query the JSON files directly.

**PDF generation:** If the user asks for a PDF, generate one from the invoice data. Use the company info for branding.

## Validation

Before writing any file, validate it against the corresponding schema. If validation fails, tell the user what's wrong. Do not write invalid data.
