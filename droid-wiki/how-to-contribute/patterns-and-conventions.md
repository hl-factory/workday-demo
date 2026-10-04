# Patterns and conventions

The conventions here come from two places: the hub tooling under `scripts/` and `site/`, and the content rules that `docs/EXAMPLE_BEST_PRACTICES.md` and the audit enforce on entries. Follow the first set when changing tooling and the second when adding or editing an entry.

## Tooling conventions

### Zero dependencies, Node built-ins only

Every file under `scripts/` starts with a header comment that says "Zero dependencies" and imports only `node:` modules. `scripts/audit-examples.mjs` says "Zero dependencies, Node 20." Keep it that way: CI runs these scripts with plain `actions/setup-node` and no `npm install`.

### One source of truth, imported not copied

- `hub.config.json` is read by `scripts/validate-examples.mjs` (exported as `config`), the three scaffolders, `site/astro.config.mjs`, `site/src/lib/examples.js`, and `site/scripts/build-zips.mjs`.
- `scripts/validate-examples.mjs` exports `repoRoot`, `config`, `sections`, `validateEntry`, `validateAll`, and `tablesInSync`. The audit modules import from it rather than re-implementing paths or validation. Its comment on `validateEntry` says to keep it free of side effects because `scripts/audit/hub-rules.mjs` calls it.
- The kebab-case regex `^[a-z0-9][a-z0-9-]*$` appears in `scripts/new-example.mjs`, `scripts/new-example.sh`, `scripts/new-example.ps1`, and `scripts/audit/hub-rules.mjs` (with a comment "same rule as scripts/new-example.mjs"). If you change it, change all four.
- `site/src/lib/gitTrace.js` is plain JavaScript "on purpose" so both Astro pages and the Node build script can import it.

### Importable modules with a guarded `main`

`scripts/validate-examples.mjs` only runs `main()` when executed directly:

```js
const isMain = process.argv[1] && fileURLToPath(import.meta.url) === process.argv[1];
if (isMain) main();
```

Use the same guard for any script that other modules import.

### Errors and exit codes

- Validator: collects every problem into an array, prints them all, then exits 1. It never stops at the first error.
- Audit: `fail(code, msg)` with documented codes (0 clean, 1 blocking, 2 usage, 3 tooling). Tool failures become findings (`ArcaneAuditorError`, `ArcaneAuditorWarning`) rather than crashes, so a contributor still gets a report (`scripts/audit/arcane.mjs`).
- PR posting: a failed inline review batch logs a warning and continues; only a failure to post the summary comment fails the job (`scripts/audit/post-review.mjs`).

### Deterministic output

- The validator compares README tables by parsed cell content, not by raw text, "so tools like Prettier can reflow them without the check calling them stale" (`scripts/validate-examples.mjs`).
- `site/scripts/build-zips.mjs` sets a fixed `MTIME` of `2000-01-01` and uses the commit date instead of wall clock, so the same commit builds byte-identical zips.
- Findings are sorted by effective severity, file, line, rule, then message (`scripts/audit/report.mjs`).

### Comments explain why

Comments in the tooling describe constraints, not mechanics. Examples: the reason `.github/workflows/audit-comment.yml` is a separate workflow, why the bash scaffolder writes through a temp file ("BSD and GNU sed disagree about -i"), and why inline comments post in batches of 20 (GitHub can return 502 after creating a large review).

## Content conventions for entries

These are what the audit checks. Each has a section in `docs/EXAMPLE_BEST_PRACTICES.md` named after its rule id.

### Packaging

- Folder names in `examples/` are kebab-case (`HubFolderKebabCaseRule`).
- Each entry has `example.json` and `README.md` (`HubExampleJsonRule`). See [Example entry](../primitives/example-entry.md).
- README has `## What it is`, `## What's inside`, `## How to use it`, and `## Before you deploy` (`HubReadmeSectionsRule`; the first three are ACTION, the fourth ADVICE).
- No leftover template text and no `.gitkeep` in non-empty folders (`HubTemplateBoilerplateRule`, `HubGitkeepRule`).

### Portability

- Never hardcode a `*.workday.com` host. Use `apiGatewayEndpoint` or a data provider via `baseUrlType` (`HardcodedWorkdayAPIRule`, ACTION).
- Never hardcode the app reference id in scripts. Use `site.applicationId` (`HardcodedApplicationIdRule`, raised to ACTION by this hub).
- If a reference id like `myApp_abc123` is baked into `.amd` or `.smd` file names, name it in "Before you deploy" (`HubAppReferenceIdRule`).
- Do not hardcode period or date literals like `2026-Q1` in `.pmd` or `.pod` values without documenting them (`HubHardcodedPeriodLiteralRule`).

### Robustness and clean code

- Every endpoint lists `failOnStatusCodes` with at least 400 and 403 (`EndpointFailOnStatusCodesRule`).
- Every widget has an `id`, and every page has a security domain (`WidgetIdRequiredRule`, `PMDSecurityDomainRule`).
- No `console.*` calls (`ScriptConsoleLogRule`).
- Prefer `let`/`const` over `var` and template strings over concatenation (`ScriptVarUsageRule`, `ScriptStringConcatRule`, both ADVICE).

### Safety

- No credentials, tenant names, or real personal data. Sample data must be clearly fictional (`CONTRIBUTING.md`).
- Credentials in orchestrations are referenced by name (for example a Basic Auth credential or `_DEFAULT_WORKDAY_CREDENTIAL`), never inlined. `examples/stock-notifications/README.md` tells readers to replace placeholder values and "never commit real ones."

### Layout of Extend sources

The `workday-skill` example codifies the layout the catalog uses (`examples/workday-skill/SKILL.md`): `presentation/` for `.amd`, `.smd`, `.pmd`, `graphQueries/`, `wqlQueries/`, `scripts/`, `presentationLabels/`; `model/` for business objects, security domains, and business processes; `orchestration/` for flows; `cards/` and `attributes/` at the app root next to `appManifest.json`. See [Extend app anatomy](../primitives/extend-app-anatomy.md).

## Writing conventions

The docs avoid tool lock-in. Commit `6086f60` (Jul 2026) made them "tool agnostic": READMEs say "App Builder, the IDE plugins, or the WDCLI" rather than naming one. The repo's own docs use plain, second-person instructions, sentence-case headings, and explain the reason behind each rule. `docs/EXAMPLE_BEST_PRACTICES.md` quotes Arcane's rule text verbatim and marks only the severity line and hub policy notes as the hub's own.

## Related pages

- [Tooling](tooling.md)
- [Audit pipeline](../systems/audit/index.md)
- [Design decisions](../background/design-decisions.md)
