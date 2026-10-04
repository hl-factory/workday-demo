# Workday Developer Program overview

This repository is the Workday Developer Program examples hub: one open place for Workday Build examples (Extend apps, Orchestrate integrations, agent skills, and reference material) that people can browse, download, copy into their own tenant, and contribute to. It is maintained by the Workday DevRel team and is explicitly not an officially supported Workday product (`SUPPORT.md`).

The copy documented here lives at `hl-factory/workday-demo`. It is an import of `Workday/WorkdayDeveloperProgram` with one extra merge commit on top (see [Lore](../lore.md)).

## What is in the repo

The repo has two kinds of content and a small amount of tooling around them.

| Area | What it holds | Wiki section |
| --- | --- | --- |
| `catalog/` | 32 Workday-built apps (Extend apps, integration apps, orchestrations, reference apps). CODEOWNERS requires DevRel review. | [Catalog](../catalog/index.md) |
| `examples/` | 14 community examples plus `examples/_template/`. This is where outside pull requests land. | [Examples](../examples/index.md) |
| `scripts/` | Zero-dependency Node tools: validator, scaffolder, and the PR audit pipeline. | [Systems](../systems/index.md) |
| `site/` | An optional Astro + Tailwind gallery, deployed to GitHub Pages, with per-example zip downloads. | [Gallery](../systems/gallery.md) |
| `.github/workflows/` | Four workflows: validate, audit, audit comment, and gallery deploy. | [Deployment](../deployment.md) |
| `docs/EXAMPLE_BEST_PRACTICES.md` | The rule-by-rule guide behind the audit. Every heading is a rule id. | [Audit](../systems/audit/index.md) |
| `hub.config.json` | Repo URLs plus the approved lists of types, components, and products. | [Configuration](../reference/configuration.md) |

Every entry, in either section, is a folder with its artifact plus two files: `example.json` (metadata) and `README.md`. That pair is the whole contract. The README index tables, the gallery cards, the download zips, and the audit are all built from it. See [Example entry](../primitives/example-entry.md).

## Who uses it

- **Workday developers and partners** looking for a working starting point. They open a folder, follow its README, and deploy to a development tenant. Or they download a single folder as a zip from the gallery ([Example downloads](../features/example-downloads.md)).
- **Community contributors** adding an example under `examples/`. The flow is scaffold, fill in two files, validate, audit, and open a PR ([Contribution flow](../features/contribution-flow.md)).
- **Workday DevRel maintainers** who review every PR, own `catalog/`, and tune the audit policy ([Maintainers](../maintainers.md)).
- **AI agents**. Eight of the community examples are markdown agent skills that teach an agent how to review Workday artifacts against the catalog ([Workday agent skills](../examples/workday-agent-skills.md)).

## Example types

`hub.config.json` defines five types, and CI rejects anything else:

| Type | Meaning | Count in repo |
| --- | --- | --- |
| Extend App | Full Extend app source, ready to deploy to a development tenant | 19 (17 catalog, 2 examples) |
| Agent Skill | Markdown skills for agents | 10 (all examples) |
| Integration App | Orchestration-driven integration connecting Workday to other systems | 7 (all catalog) |
| Reference | Design patterns, widget dictionaries, sample orchestrations | 7 (all catalog) |
| Orchestration | Focused Orchestration Builder definitions | 3 (1 catalog, 2 examples) |

The counts come from the `type` field of each `example.json` (46 entries total; `examples/_template/` is excluded). By the `source` field, 33 entries carry the Workday badge and 13 carry the Community badge.

## Quick links

- [Architecture](architecture.md): how entries, scripts, CI, and the gallery connect
- [Getting started](getting-started.md): clone, validate, audit, run the gallery
- [Glossary](glossary.md): AMD, SMD, PMD, ISU, WQL, Maya, ACTION and ADVICE, and more
- [Contribution flow](../features/contribution-flow.md): from `node scripts/new-example.mjs` to a merged card
- [Audit pipeline](../systems/audit/index.md): Arcane Auditor plus hub rules on every PR
- [Extend app anatomy](../primitives/extend-app-anatomy.md): the files inside a catalog app
