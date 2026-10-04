# Configuration

Every configuration surface in the repo, with each key and who reads it. There are no environment-specific config files and no `.env` usage; `.gitignore` excludes `.env` and `.env.local` as a precaution.

## `hub.config.json`

The single source of truth for repository identity and the approved metadata vocabularies.

| Key | Value | Read by |
| --- | --- | --- |
| `org` | `Workday` | Not read by any script or page at this commit |
| `repo` | `WorkdayDeveloperProgram` | Sparse-checkout snippet in `site/src/lib/gitTrace.js` (`cd <repo>`) |
| `repoUrl` | `https://github.com/Workday/WorkdayDeveloperProgram` | Gallery header, "view code" and contribute links (`site/src/pages/index.astro`, `site/src/components/EntryPage.astro`), zip `SOURCE.md`, audit `doc_url` in `scripts/audit/report.mjs` |
| `pagesUrl` | `https://workday.github.io/WorkdayDeveloperProgram` | `site/astro.config.mjs` (`site` = origin, `base` = path), `SOURCE.md` hub page links |
| `defaultBranch` | `main` | Gallery links, `SOURCE.md` links, audit `doc_url` |
| `types` | 5 values | Validator (required, must match), scaffolders (`--type`, default is the first) |
| `components` | 10 values | Validator (optional list, each must match) |
| `products` | 10 values | Validator (optional list, each must match) |

Approved values:

| List | Values |
| --- | --- |
| `types` | Extend App, Integration App, Orchestration, Agent Skill, Reference |
| `components` | Presentation, Model, Orchestration, Template, Card, Business Process, Graph API, WQL, AWS Integration, 3rd-Party Technology |
| `products` | Workday REST API, Workday SOAP API, Workday RaaS, Workday Extend, Workday Orchestrate, Workday Prism Analytics, Workday Absence Management, Workday HCM, Workday Financials, Workday Payroll |

`CONTRIBUTING.md` says to open an issue if a real Workday capability is missing from the lists. Adding a value is a one-line change, but removing one fails validation for every entry that uses it.

Section definitions are not in this file. They are hardcoded in `scripts/validate-examples.mjs` as `sections`: `catalog` with default source `workday` and `examples` with default source `community`, each with its own README marker pair.

## `.arcane-auditor/config.json`

Arcane Auditor's configuration, generated with `ArcaneAuditorCLI generate-config` and then edited. Passed to Arcane with `--config` locally and through the action's `config:` input in CI.

| Key | Value |
| --- | --- |
| `rules` | 48 rules, all `enabled: true`. Each has `severity_override`, `fix_strategy_override`, and `custom_settings` |
| Overrides | `HardcodedApplicationIdRule` to ACTION; `OrchestrationGlobalErrorHandlerRule` and `OrchestrationApiStepErrorHandlerRule` to ADVICE; `PMDSectionOrderingRule` fix strategy to `human_review`. No rule has custom settings |
| `file_processing.relevant_extensions` | `.pod`, `.pmd`, `.script`, `.amd`, `.smd`, `.wqlquery`, `.orchestration`, `.suborchestration` (same list as `ARCANE_EXTENSIONS` in `scripts/audit/arcane.mjs`) |
| `file_processing.max_file_size` | 52,428,800 bytes (50 MB) |
| `file_processing.max_zip_size` | 524,288,000 bytes (500 MB) |
| `file_processing.encoding` / `fallback_encodings` | `utf-8`, then `latin-1`, `cp1252`, `iso-8859-1` |
| `fail_on_severe`, `fail_on_warning` | `false`; pass and fail is decided by `scripts/audit/report.mjs` |
| `output` | text format, sorted by severity, rule details included |

## `.arcane-auditor/action-ref`

One line: `Ekwuno/ArcaneAuditor@c31316d1147cb0e2d3f47688e668f7f3b2f9e887`. Read by `scripts/install-arcane.sh`. Must match the `uses:` line in `.github/workflows/audit-examples.yml`.

## Workflow configuration

| Setting | Where | Effect |
| --- | --- | --- |
| Repository variable `AUDIT_MODE` | GitHub repo settings, read in `.github/workflows/audit-examples.yml` | `advisory` makes the audit never fail; default `enforcing` |
| Label `audit-override` | On a PR | Makes that PR's audit advisory |
| Required checks | GitHub branch protection | Validate examples and Audit examples are required on `main` (per the workflow comments; the settings themselves are not in the repo) |
| GitHub Pages source | GitHub repo settings | Must be "GitHub Actions" for `.github/workflows/deploy-gallery.yml` |

## `.github/CODEOWNERS`

One rule: `/catalog/ @Workday/devrel`. Everything else has no code owner, so any maintainer can approve.

## `site/package.json` and `site/astro.config.mjs`

`site/package.json` defines `dev`, `build`, and `preview`, with `predev` and `prebuild` hooks that run `node scripts/build-zips.mjs`. `site/astro.config.mjs` reads `hub.config.json`, sets `site` and `base` from `pagesUrl`, adds the Tailwind Vite plugin, and allows Vite to serve files from the parent directory (`server.fs.allow: [".."]`) so pages can read entries outside `site/`.

## Constants in code

Values that act as configuration but live in source:

| Constant | File | Value |
| --- | --- | --- |
| `MAX_FINDINGS` | `scripts/audit/post-review.mjs` | 500 findings per report |
| `MAX_INLINE` | `scripts/audit/post-review.mjs` | 40 inline comments per PR |
| `BATCH` | `scripts/audit/post-review.mjs` | 20 comments per review request |
| `KEBAB` | `scripts/audit/hub-rules.mjs` (and three scaffolders) | `^[a-z0-9][a-z0-9-]*$` |
| `REQUIRED_SECTIONS` | `scripts/audit/hub-rules.mjs` | What it is, What's inside, How to use it (ACTION), Before you deploy (ADVICE) |
| `DOC_PATH` | `scripts/audit/report.mjs` | `docs/EXAMPLE_BEST_PRACTICES.md` |
| `MTIME` | `site/scripts/build-zips.mjs` | `2000-01-01T00:00:00Z` |
| `SKIP` | `site/scripts/build-zips.mjs` | `.DS_Store`, `.gitkeep`, `example.json`, `node_modules`, `.git` |

## Entry-level configuration

Workday apps carry their own configuration, which readers edit per tenant: `.amd` files (application id, data providers, base URL types), `.attributes` files (app attributes, 4 in the repo), `presentationLabels/*.properties` (translated labels), and `.cardtenantsetting` files. See [Extend app anatomy](../primitives/extend-app-anatomy.md).
