# Reverse Pastry: Agent Instructions

You are reading the Reverse Pastry pattern repo. This is a meta repo. It documents an architectural pattern for AI-native software and provides reference examples. It is not an application itself.

## What this repo teaches you

How to build software using the Reverse Pastry pattern: a hard, deterministic core with a soft AI exterior.

- The **core** is schemas, data, and business rules. Strict, version-controlled, inspectable. Plain files in a git repo.
- The **exterior** is you, the AI agent. You are the interface. There is no UI, no CLI, no web app. The user talks to you, and you operate on the core.

## What an app repo looks like

A repo built with this pattern has:

```
schema/          # JSON Schema definitions for all entities
data/            # Plain files (JSON, CSV, SQLite) containing actual data
capabilities/    # What operations are possible (optional, can live in CLAUDE.md)
CLAUDE.md        # Instructions for the AI agent operating this core
```

There is no `src/`, no `app/`, no `components/`, no `pages/`. There is no code to run. The agent reads the schemas, understands the data, and performs operations through conversation.

## How to build a new reverse pastry app

If someone asks you to help them build a new application using this pattern:

1. **Start with the domain.** What entities exist? What are their relationships? What are the constraints? Don't write any code. Design the data model.

2. **Write schemas first.** Use JSON Schema (draft-07 or later). Every field should have a type, description, and constraints. Required fields must be explicit. Enums for any field with a fixed set of values. Patterns for formatted strings.

3. **Create sample data.** Populate the `data/` directory with realistic examples that conform to the schemas. This makes the core tangible and testable immediately.

4. **Define capabilities.** What can be done with this data? Creating records, querying, generating reports, sending notifications. Document these as skills or capabilities the agent can perform.

5. **Write the CLAUDE.md.** This is the agent's operating manual for this specific core. It should cover:
   - What this app does
   - Where schemas and data live
   - What operations are available
   - Rules and constraints the agent must follow
   - How to validate data before writing

6. **Do not build an interface.** No web UI, no CLI, no API server. The user talks to their agent. The agent reads the CLAUDE.md, understands the core, and operates on it. If the user wants a PDF, the agent generates it. If the user wants a report, the agent queries the data. The interface is the conversation.

## Key principles

- **Schema is the contract.** If the schema says a field is required, it's required. If the schema says a value must match a pattern, it must match. Don't rely on prompting to enforce rules. Structure enforces rules.
- **Files over services.** JSON files in a git repo. SQLite if you need relational queries. No hosted databases, no API servers, no Docker.
- **The agent is replaceable.** If someone swaps you for a different AI, the core still works. The schemas are still valid. The data is still there. Don't build anything that depends on a specific agent.
- **Version everything.** The core lives in git. Every change is tracked. Data and schema evolve together.
- **Skills over features.** Don't build features into an application. Define skills the agent can perform. Skills are composable and don't require deployment.

## Examples in this repo

Look at `examples/` for reference implementations. Each example has its own README, schemas, sample data, and a CLAUDE.md showing how an agent would operate that specific core.

The invoicer example (`examples/invoicer/`) is the most complete. It shows how a full invoicing workflow (create, send, track, report) works with nothing but JSON files and an AI agent.
