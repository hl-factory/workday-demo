# Maintainers

Who built and reviews each part of the repo, based on git history through 2026-10-01 and `.github/CODEOWNERS`. The two import commits on 2026-10-04 by `hl-factory` are not counted as authorship.

## Ownership rules

`.github/CODEOWNERS` has one line: `/catalog/ @Workday/devrel`. Any change under `catalog/` needs approval from the Workday DevRel team. Everything else has no code owner. `CONTRIBUTING.md` says "Workday DevRel reviews every pull request before merge", and `SUPPORT.md` commits DevRel to maintaining everything in `catalog/` plus any example marked `"source": "workday"`. Community examples are maintained by the people in their `authors` field.

## People

| Person (GitHub) | Areas | Role in history | Last active |
| --- | --- | --- | --- |
| ekwuno (Obinna Ekwuno, also commits as `Obinna Ekwuno`) | Everything: `scripts/`, `site/`, `.github/`, `docs/`, `.arcane-auditor/`, `hub.config.json`, early `examples/`, README | Created the repo, wrote all hub tooling, merged most pull requests | 2026-10-01 |
| cngan-wd | `catalog/ap-einvoice/` | Contributed the AP e-invoice reference app (PR #13) | 2026-09-15 |
| srivilliamsai (SRI VILLIAM SAI) | `examples/pto-policy-agent-skill/`, `examples/wql-anniversary-celebrations/`, `examples/peer-kudos-home-card/`, `examples/workday-*-skill/` | Most active community contributor (PRs #5, #14, #16, #17) | 2026-09-22 |
| tony-gilfillan (also commits as `gilfila`) | `examples/stock-notifications/`, the 31-app catalog import | Brought in the catalog (PR #4) and the first community example (PR #3); merged PR #5 | 2026-08-24 |
| chumphrey-wd | Reviews | Merged PRs #1, #2, and #16 | 2026-09-21 |

## By area

| Area | Official owners (CODEOWNERS) | Recent contributors (git history) | Last activity |
| --- | --- | --- | --- |
| Validator and scaffolders (`scripts/`) | none | ekwuno | 2026-09-10 |
| Audit pipeline (`scripts/audit/`, `.arcane-auditor/`) | none | ekwuno | 2026-09-10 |
| Gallery (`site/`) | none | ekwuno | 2026-09-15 (zip downloads, naming) |
| CI (`.github/workflows/`) | none | ekwuno | 2026-09-10 |
| Best-practices guide (`docs/EXAMPLE_BEST_PRACTICES.md`) | none | ekwuno | 2026-09-10 |
| Hub config (`hub.config.json`) | none | ekwuno | 2026-08-06 (repo rename) |
| Catalog (`catalog/`) | Workday/devrel | cngan-wd (ap-einvoice), tony-gilfillan (import), ekwuno (structure) | 2026-09-15 |
| Community examples (`examples/`) | none | srivilliamsai, tony-gilfillan, ekwuno | 2026-09-22 |
| `README.md` | none | srivilliamsai, ekwuno | 2026-09-22 |
| `CONTRIBUTING.md`, `SUPPORT.md` | none | ekwuno | 2026-09-15 and 2026-08-06 |

## Who to ask

| Question | Ask |
| --- | --- |
| Audit rule, severity policy, PR comments, validator, gallery | ekwuno |
| Changing anything in `catalog/` | `@Workday/devrel` (open an issue first, per `CONTRIBUTING.md`) |
| AP e-invoice app | cngan-wd (Chris Ngan) or William Tam, the two authors named in `catalog/ap-einvoice/example.json` |
| Agent skills suite, WQL example, Peer Kudos card | srivilliamsai |
| Stock notifications example, original catalog apps | tony-gilfillan |
| Arcane Auditor itself | The Arcane project (`Developers-and-Dragons/ArcaneAuditor`); the CI action comes from ekwuno's fork |

## Bus factor

All hub tooling (validator, scaffolders, audit, gallery, workflows) has a single author. The design is well commented and the [Background](background/index.md) pages record the reasoning, but anyone taking over the tooling should start with `.arcane-auditor/README.md` and the header comments in `scripts/audit-examples.mjs` and `scripts/audit/report.mjs`.

The people above maintain the upstream repo. Changes made in this copy (`hl-factory/workday-demo`) diverge from upstream unless they are sent back as pull requests to `Workday/WorkdayDeveloperProgram`.
