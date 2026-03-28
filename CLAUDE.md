# Reverse Pastry

## What This Is

An evolving architectural pattern for AI-native software. The core idea: strict deterministic
schema + data (the hard core) with a natural language AI interface (the soft exterior).

This repo is both the pattern documentation and a living reference. It should grow with new
examples, refined principles, and community input.

## Goals

- Articulate the pattern clearly enough that anyone can apply it
- Provide concrete examples across different domains (invoicing, inventory, project tracking, etc.)
- Establish best practices for designing the "hard core" (schemas, file formats, data layout)
- Show how to wire up the "soft exterior" (AI skills, agents, natural language interfaces)
- Compare and contrast with vibe coded apps and traditional software architecture

## Repo Structure

- `README.md` — the pattern definition, principles, anti-patterns, case study, vision
- `agent-instructions.md` — instructions for an AI agent learning to apply the pattern
- `examples/` — domain-specific applications of the pattern, each with schemas and docs
  - `examples/invoicer/` — the most complete example (invoicing with JSON files + AI agent)
- `docs/` — website served by GitHub Pages at reversepastry.com
  - `docs/README.md` — symlink to root `README.md` (one file, two places)
  - `docs/_coverpage.md` — docsify cover page (splash screen)
  - `docs/index.html` — docsify config (theme: simple-dark via docsify-themeable)
  - `docs/style.css` — custom styles
  - `docs/invoicer-example.md` — invoicer content for the site
  - `docs/CNAME` — custom domain config
- `LICENSE` — MIT

## Website

Domain: reversepastry.com
Hosted via GitHub Pages from the `docs/` folder.
Built with [docsify](https://docsify.js.org/) using the [docsify-themeable](https://github.com/jhildenbiddle/docsify-themeable) simple-dark theme.

Since `docs/README.md` is a symlink to the root `README.md`, any changes to the root README
automatically appear on the website. Keep this in mind when editing — the content serves both
GitHub visitors and website visitors.

## How to Enhance

- Add new examples in `examples/{domain}/` with schemas and a README
- Refine principles in the main README as the pattern matures
- Add a `guides/` directory for deeper dives (e.g., "choosing a file format", "designing skills")
- Keep it practical — every principle should have a concrete example
- The invoicer example should remain generic enough to be useful to anyone doing invoicing

## Tone

Direct. Practical. No fluff. The audience is developers and technically-minded people who are
frustrated with traditional software complexity and curious about AI-native alternatives.
