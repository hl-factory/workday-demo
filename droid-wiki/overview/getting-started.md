# Getting started

This page covers cloning the repo, running the validator and the audit locally, previewing the gallery, and using an entry in a Workday tenant. None of the tooling needs to be installed globally, and the scripts under `scripts/` have no npm dependencies.

## Prerequisites

| Tool | Needed for | Notes |
| --- | --- | --- |
| git | Everything | `scripts/audit/diff.mjs` shells out to `git diff`, `git ls-tree`, and `git merge-base` |
| Node.js 20+ | Validator, audit, Node scaffolder, gallery | CI pins Node 20 (`actions/setup-node@v7` with `node-version: 20`) |
| bash + curl | `scripts/install-arcane.sh`, `scripts/new-example.sh` | macOS and Linux |
| PowerShell | `scripts/new-example.ps1` | Windows without Node |
| npm | Only for `site/` | The gallery is the one place with dependencies (`site/package.json`) |

To deploy an entry you also need a Workday development tenant and one of the tools the README names: App Builder, the IDE plugins, or the WDCLI for Extend apps, and Orchestration Builder for orchestrations.

## Clone

```bash
git clone https://github.com/hl-factory/workday-demo
cd workday-demo
```

The upstream is `https://github.com/Workday/WorkdayDeveloperProgram`. Some links inside the repo (`hub.config.json`, doc URLs in audit findings, `SOURCE.md` in zips) point at the upstream because `hub.config.json` still names it.

## Validate

```bash
node scripts/validate-examples.mjs           # validate and rewrite the README index tables if stale
node scripts/validate-examples.mjs --check   # validate only; exit 1 if a table is stale (what CI runs)
```

On a clean checkout the check prints `OK: 46 entries validated, README tables in sync.` See [Validator](../systems/validator.md).

## Audit

```bash
./scripts/install-arcane.sh                          # once; installs the Arcane CLI into .arcane-auditor/bin/
node scripts/audit-examples.mjs --changed            # folders changed vs origin/main
node scripts/audit-examples.mjs --dirs examples/stock-notifications
node scripts/audit-examples.mjs --changed --skip-arcane   # hub rules only, no download
```

Exit codes from `scripts/audit-examples.mjs`: `0` clean or advisory, `1` blocking findings, `2` usage error, `3` tooling failure. Without `--skip-arcane`, the script throws if it cannot find `ArcaneAuditorCLI` (it checks `ARCANE_AUDITOR_CMD`, `ARCANE_AUDITOR_BIN`, `.arcane-auditor/bin/`, `~/.arcane-auditor/bin/`, then `PATH`). See [Audit pipeline](../systems/audit/index.md).

`--changed` with no arguments compares against `origin/main`. In a fork or a copy like this one, make sure `origin/main` exists locally (`git fetch origin main`) or pass the base and head explicitly: `--changed main HEAD`.

## Scaffold an example

```bash
node scripts/new-example.mjs my-example --type "Orchestration" --title "My Example"
./scripts/new-example.sh my-example --type "Orchestration"
powershell -ExecutionPolicy Bypass -File scripts\new-example.ps1 my-example -Type "Orchestration"
```

All three copy `examples/_template/` to `examples/my-example/` and set the title and type. See [Scaffolder](../systems/scaffolder.md) and [Contribution flow](../features/contribution-flow.md).

## Run the gallery

```bash
cd site
npm install
npm run dev       # runs build-zips first (predev), then astro dev
npm run build     # runs build-zips first (prebuild), then astro build into site/dist/
npm run preview
```

`site/astro.config.mjs` sets `site` and `base` from the `pagesUrl` in `hub.config.json`, so local URLs include the `/WorkdayDeveloperProgram/` base path. Generated zips go to `site/public/downloads/`, which is gitignored. See [Gallery](../systems/gallery.md).

## Use an entry in a tenant

What "use it" means depends on the type (from `README.md`):

- **Extend app source**: deploy to a WCP development tenant with App Builder, an IDE plugin, or the WDCLI, then install and launch. Most catalog READMEs describe a "Create Copy" flow from the developer Console plus a manual IntelliJ plugin flow.
- **Orchestrations and integration apps**: import into Orchestration Builder, promote if needed, deploy. Integration apps are usually launched with the `Launch / Schedule Integration` task.
- **Agent skills and reference material**: read, copy, adapt.

Before deploying anything, read the entry's "Before you deploy" section (or the catalog README's configuration section) for tenant-specific values: app reference ids, security domains, ISUs, WIDs, and dates. See [Extend app anatomy](../primitives/extend-app-anatomy.md) for what those files are.

## Next steps

- [Development workflow](../how-to-contribute/development-workflow.md)
- [Testing](../how-to-contribute/testing.md)
- [Debugging](../how-to-contribute/debugging.md)
