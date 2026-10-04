# Pitfalls

Mistakes that are easy to make here, why they happen, and how to avoid them.

## Touching a catalog folder raises the bar for the whole folder

Editing one line in `catalog/someApp/` puts that folder in the audit. Three things then work against you:

- The catalog bar promotes every ADVICE finding to ACTION.
- Folder-level findings are reported on line 0. `HubFolderKebabCaseRule` (every catalog folder is camelCase) and `HubReadmeSectionsRule` (no catalog README except `catalog/ap-einvoice/README.md` has the hub sections) cannot be matched to a diff line, so the "pre-existing line" downgrade never applies.
- Running `node scripts/audit-examples.mjs --all --hub-only` at `8b3e7c7` shows 95 ACTION findings after policy, almost all from catalog folders.

**Avoid it:** expect a maintainer to apply `audit-override` for catalog fixes, or add the four README sections while you are there. Do not rename the folder to fix the kebab-case finding (see below).

## A freshly scaffolded folder fails the audit

The scaffolder copies `examples/_template/` and replaces the title and type, but leaves the rest of the README, including the "Fill in example.json (delete this section before submitting)" section. `HubTemplateBoilerplateRule` flags that text as ACTION (and would flag the template title "My Example" in a hand-copied folder). That is intended, but it surprises people who run the audit immediately after scaffolding to "see if it works".

## `--changed` ignores uncommitted work

`--changed` with no arguments diffs `origin/main...HEAD`, which only includes commits. A folder you have edited but not committed is not audited, and the command may report nothing to do. Commit first, or use `--dirs examples/my-folder`.

In this fork, `origin` is `hl-factory/workday-demo`, not upstream. If you are preparing a PR for `Workday/WorkdayDeveloperProgram`, run `--changed upstream/main HEAD`.

## Forgetting to commit the regenerated README

`node scripts/validate-examples.mjs` rewrites the index tables in the root `README.md`. If you commit your folder but not the README, CI's `--check` fails with "A README table is out of date". Changing an entry's title, description, or type has the same effect.

## Renaming catalog folders breaks the agent skills

The eight skills under `examples/workday-*-skill/` cite catalog folders by path (for example `catalog/documentIntelligenceWithTheAIGateway/`). No check verifies those paths. Renaming a catalog folder, even to fix the kebab-case finding, silently breaks the skills. It also breaks every forum post and tutorial link that points at the folder on GitHub.

## Single-line orchestrations

Every `.orchestration` and `.suborchestration` file is one line of JSON. Consequences:

- `git diff` and PR review show the whole file as changed for any edit.
- Any line-based finding in the file is on line 1. Only the finding's `json_path` identifies the step. The "pre-existing line" downgrade also treats the whole file as touched once you edit it, so old findings in that file start blocking.
- Merge conflicts in these files cannot be resolved by hand in practice. Re-export from Orchestration Builder instead.

Do not pretty-print them before committing. Keeping the exported format means the next re-export produces a comparable file instead of a full reformat.

## PMD files are not strict JSON

135 of the 298 PMD files fail `JSON.parse`. Tools that assume JSON (including a quick `jq` or a Node script) will fail on them. Treat Workday files as text unless you know the specific file parses.

## PowerShell 5.1 and BOMs

`scripts/new-example.ps1` writes files with `Set-Content -Encoding UTF8`. On Windows PowerShell 5.1 that encoding writes a byte order mark. The validator reads `example.json` with `JSON.parse`, which rejects a leading BOM as invalid JSON. PowerShell 7 and later write UTF-8 without a BOM by default. If the validator reports `example.json is not valid JSON` right after scaffolding on Windows, re-save the file without a BOM or use PowerShell 7.

## Links still point at the upstream repo

`hub.config.json` holds `repoUrl` and `pagesUrl` for `Workday/WorkdayDeveloperProgram`. In this fork, those values feed:

- The gallery's base path and site URL (`site/astro.config.mjs`)
- The repository and commit links in every zip's `SOURCE.md`
- The `doc_url` on each audit finding (`scripts/audit/report.mjs` builds `<repoUrl>/blob/main/docs/EXAMPLE_BEST_PRACTICES.md#<rule>`)

`.github/ISSUE_TEMPLATE/config.yml` and `.github/PULL_REQUEST_TEMPLATE.md` also hardcode upstream URLs. Update all of these before deploying the fork's gallery.

## The AP e-invoice source is invisible to the audit

`catalog/ap-einvoice/` ships its app as `catalog/ap-einvoice/apeinvoice.zip`. The audit does not unzip it, so changes inside the zip are never checked in CI. Run Arcane against the unzipped source yourself, as the author did before commit `c96d5d4`. The 18.6 MB demo video in the same folder also ends up in that entry's gallery zip.

## README claims that do not match the artifact

Catalog READMEs were written for the original tenant demos. At least one no longer matches its export: `catalog/requestCreditCard/README.md` describes Stripe card issuing, but the orchestration has no Stripe call. Follow the README, but read the artifact before promising a customer what it does.

## Hardcoded tenant details in older apps

Many catalog AMD and SMD file names include a tenant-generated suffix (for example `catalog/chartDictionary/presentation/chart2CatalogApp_ycjtxv.amd`). `HubAppReferenceIdRule` only reports this when the README lacks a "Before you deploy" section, and most catalog READMEs lack one. When you deploy, expect to change the reference id, and check `site.applicationId` usage in scripts.

## Related pages

- [Debugging](../how-to-contribute/debugging.md)
- [Design decisions](design-decisions.md)
- [Severity policy](../systems/audit/severity-policy.md)
