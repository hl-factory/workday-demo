# Tooling

Every tool in the repo, what it needs, and how to call it. All commands run from the repository root unless noted.

## Requirements

| Tool | Needed for | Notes |
| --- | --- | --- |
| Node.js 20 | All `scripts/*.mjs` and the gallery | CI pins 20 in every workflow. The scripts use only `node:` built-ins |
| npm | `site/` only | `site/package-lock.json` pins 440 packages |
| git | `--changed` audits, zip metadata | Audits need the base branch fetched |
| bash or PowerShell | Scaffolders without Node, `scripts/install-arcane.sh` | `scripts/install-arcane.sh` also needs `curl` |
| Arcane Auditor CLI | The Arcane half of the audit | Optional locally; installed into `.arcane-auditor/bin/` (gitignored) |

## Scripts

### `scripts/validate-examples.mjs`

| Invocation | Effect |
| --- | --- |
| `node scripts/validate-examples.mjs` | Validate every entry and rewrite stale index tables in `README.md` |
| `node scripts/validate-examples.mjs --check` | Validate only; exit 1 if a table is stale (CI) |

Also an importable module; see [Validator](../systems/validator.md).

### Scaffolders

| Script | Usage |
| --- | --- |
| `scripts/new-example.mjs` | `node scripts/new-example.mjs <folder> [--type "Extend App"] [--title "My Example"]` |
| `scripts/new-example.sh` | `./scripts/new-example.sh <folder> --type "Extend App"` |
| `scripts/new-example.ps1` | `powershell -ExecutionPolicy Bypass -File scripts\new-example.ps1 <folder> -Type "Extend App"` |

The folder name must be kebab-case, and `--type` defaults to the first entry of `types` in `hub.config.json` (Extend App). See [Scaffolder](../systems/scaffolder.md).

### `scripts/install-arcane.sh`

Reads `.arcane-auditor/action-ref` (currently `Ekwuno/ArcaneAuditor@c31316d1147cb0e2d3f47688e668f7f3b2f9e887`), downloads that commit's `.github/action/install.sh` with `curl`, and runs it with `ARCANE_INSTALL_DIR=.arcane-auditor/bin`. The pin matches the `uses:` line in `.github/workflows/audit-examples.yml`, so local and CI runs use the same Arcane version.

### `scripts/audit-examples.mjs`

Selecting folders (pick one):

| Flag | Folders audited |
| --- | --- |
| `--changed [<base> [<head>]]` | Entry folders changed in `base...head`; defaults `origin/main` and `HEAD` |
| `--dirs <path> [<path> ...]` | The listed folders |
| `--all` | Every folder in `catalog/` and `examples/` except names starting with `_` or `.` |
| `--list-changed <base> <head>` | Print the changed folders and exit; writes `dirs=` to `$GITHUB_OUTPUT` in CI |

Options:

| Flag | Values | Default |
| --- | --- | --- |
| `--mode` | `enforcing`, `advisory` | `enforcing` (exit 1 on blocking findings) |
| `--format` | `console`, `json`, `markdown`, `ci` | `console` |
| `--output <file>` | JSON report path | `audit/report.json` when `--format ci` |
| `--pr <number>` | Record the PR in the report and write `audit/pr.json` | none |
| `--merge <file>` | Use an Arcane report produced by the GitHub Action | none |
| `--skip-arcane` or `--hub-only` | Hub rules only | off |
| `--arcane-only` | Arcane only | off |
| `--rules A,B`, `--exclude-rules A,B` | Forwarded to Arcane | none |

Exit codes: 0 clean or advisory, 1 blocking findings, 2 usage error, 3 tooling failure.

### `site/` npm scripts

Run from `site/`:

| Script | What it runs |
| --- | --- |
| `npm run dev` | `predev` runs `node scripts/build-zips.mjs`, then `astro dev` |
| `npm run build` | `prebuild` runs `site/scripts/build-zips.mjs`, then `astro build` into `site/dist/` |
| `npm run preview` | `astro preview` of the built site |

`site/scripts/build-zips.mjs` writes one zip per entry to `site/public/downloads/<section>/<id>.zip` (gitignored). It skips `.DS_Store`, `.gitkeep`, `example.json`, `node_modules`, and `.git`, and adds a `SOURCE.md`. See [Example downloads](../features/example-downloads.md).

## Environment variables

| Variable | Read by | Purpose |
| --- | --- | --- |
| `ARCANE_AUDITOR_CMD` | `scripts/audit/arcane.mjs` | Full command line for Arcane, split on whitespace; overrides everything else |
| `ARCANE_AUDITOR_BIN` | `scripts/audit/arcane.mjs` | Path to the `ArcaneAuditorCLI` binary |
| `ARCANE_INSTALL_DIR` | Arcane's `install.sh` | Set by `scripts/install-arcane.sh` to `.arcane-auditor/bin` |
| `GITHUB_OUTPUT`, `GITHUB_STEP_SUMMARY` | `scripts/audit-examples.mjs` | Set by Actions; receive `dirs=` and the markdown report |
| `GITHUB_SHA` | `site/scripts/build-zips.mjs` | Commit recorded in each `SOURCE.md` |
| `AUDIT_MODE` | `.github/workflows/audit-examples.yml` | Repository variable; `advisory` makes the audit non-blocking |
| `HEAD_SHA`, `HEAD_BRANCH`, `HEAD_OWNER`, `AUDIT_CONCLUSION` | `.github/workflows/audit-comment.yml` | Passed as arguments to `postReview()` in `scripts/audit/post-review.mjs` to identify the PR and the audit result |

## Editor and formatter

There is no lint or format configuration in the repo (no `.prettierrc`, `.editorconfig`, or ESLint config). The validator tolerates Prettier reformatting the README tables, and commit `a39aed7` mentions letting Prettier format the README, so Prettier with default settings is the de facto formatter for markdown.

## Related pages

- [Development workflow](development-workflow.md)
- [Dependencies](../reference/dependencies.md)
- [Configuration](../reference/configuration.md)
