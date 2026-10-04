# Design decisions

Each decision below is visible in the code, and most are explained in a comment, a README, or a commit message. The source is named for each.

## Two files are the whole contract

**Decision.** An entry is a folder with an artifact, `example.json`, and `README.md`. Nothing else is required.

**Why.** `CONTRIBUTING.md` says "Adding an example is deliberately low effort: a folder, two small files, one validation command", and "The scaffolder and validator are conveniences, not requirements." The root README index, gallery cards, zip downloads, and audit all derive from those two files, so there is no second registry to keep in sync.

**Trade-off.** Metadata is only as good as contributors make it. The validator checks enum values from `hub.config.json` but cannot check that a description is accurate.

## Zero-dependency tooling

**Decision.** Every script in `scripts/` uses only Node built-ins. Bash and PowerShell scaffolders exist alongside the Node one.

**Why.** Contributors include Workday developers who may not have a JavaScript toolchain. Commit `2142079` is titled "Add bash and PowerShell scaffolders for machines without Node". CI runs the scripts with no `npm install`, which keeps the required checks fast and removes a supply chain surface.

**Trade-off.** The kebab-case regex and some template logic are repeated across four files (see [Patterns and conventions](../how-to-contribute/patterns-and-conventions.md)), and nothing checks that the three scaffolders stay identical.

## The gallery is optional

**Decision.** `site/` is a separate npm project. The README calls it an "Optional Astro gallery (not required to use the examples)".

**Why.** The examples must be usable straight from GitHub. Keeping the gallery separate means a broken site never blocks a contribution, and the only dependency tree in the repo stays isolated in `site/`.

## Catalog and examples are separate sections

**Decision.** `catalog/` is Workday-built and owned by `@Workday/devrel` through `.github/CODEOWNERS`. `examples/` is open to everyone.

**Why.** Commit `bd640f8` split them, and `7273c09` added CODEOWNERS right after. Catalog entries are what people copy as reference, so `SUPPORT.md` commits DevRel to fixing them, while community entries are maintained by their authors. The validator enforces that a catalog entry cannot claim `"source": "community"`.

**Trade-off.** The catalog folders kept their original camelCase names, so they permanently fail `HubFolderKebabCaseRule`. Renaming them would break forum links, skill references, and existing clones.

## Workday and Community badges

**Decision.** Every entry shows a source badge, defaulting by section (`workday` for catalog, `community` for examples).

**Why.** Commit `7caccd7` ("Label examples as Workday or Community"). `CONTRIBUTING.md`: "Community examples are held to works, safe, and honest; Workday-authored ones get a stricter pass because people copy them as reference." See [Source badges and support](../features/source-badges-and-support.md).

## Tool-agnostic documentation

**Decision.** Docs list App Builder, the IDE plugins, and the WDCLI as equal ways to produce an artifact.

**Why.** Commit `6086f60` rewrote every mention of App Builder as the single path. The hub "does not care which tooling produced the artifact" (`CONTRIBUTING.md`).

## README tables are generated, compared by content

**Decision.** The validator rewrites the root README index between marker comments, and `--check` compares parsed cells, not raw text.

**Why.** Commit `7982aaa`: "Compare the README table by content so formatters can reflow it". Contributors who run Prettier do not get spurious failures, and those who cannot run Node can leave the table stale for a reviewer to regenerate.

## The audit blocks on what you wrote, not on history

**Decision.** In a folder that already existed, ACTION findings on lines the PR did not touch are downgraded to ADVICE. ADVICE never blocks.

**Why.** `.arcane-auditor/README.md`: "contributors are only blocked on what they wrote." Most catalog apps predate the rules; blocking on old findings would make any fix to them impossible without a full cleanup.

**Trade-off.** File-level findings on line 0 cannot be matched to a diff line, so they are never downgraded. In `catalog/`, ADVICE is promoted to ACTION, which raises the bar further. See [Severity policy](../systems/audit/severity-policy.md).

## Three changes to Arcane's defaults

**Decision.** All 48 Arcane rules stay enabled. `HardcodedApplicationIdRule` goes up to ACTION, the two orchestration error handler rules go down to ADVICE, and `PMDSectionOrderingRule` becomes `human_review`.

**Why.** From `.arcane-auditor/README.md`: "Examples exist to be copied. A hardcoded app id guarantees the copy breaks." Error-handler scaffolding "is not always the lesson an orchestration example teaches." The ordering rule had no automatic fix payload, so it produced empty suggestions. The README also says to "Prefer downgrading a rule over disabling it, so the best-practices doc can still explain it."

## Best-practices doc quotes Arcane verbatim

**Decision.** `docs/EXAMPLE_BEST_PRACTICES.md` has one heading per rule id, and the Arcane sections quote its documentation word for word.

**Why.** Commit `6470ee5` ("Quote Arcane Auditor's rule documentation verbatim"), after `d7a4117` made the guide generic. PR comments link to the heading by rule id, so the doc doubles as the help text for every finding.

## The PR comment runs in a separate workflow

**Decision.** `.github/workflows/audit-examples.yml` produces a report with read-only permissions; `.github/workflows/audit-comment.yml` posts it from the default branch through `workflow_run`.

**Why.** The workflow header: "that workflow has no write token on pull requests from forks." See [Security](../security.md).

## Comments post in batches and the summary always posts

**Decision.** Inline review comments go in batches of 20; a failing batch is logged and skipped; only a failure to post the summary fails the job.

**Why.** Commit `d593882` ("Post inline audit comments in batches and never skip the summary"). Large reviews could return HTTP 502 from GitHub after the review had been created. See [PR comments](../systems/audit/pr-comments.md).

## Reproducible zips

**Decision.** `site/scripts/build-zips.mjs` uses a fixed file time of 2000-01-01 and the commit date, and writes a `SOURCE.md` with the commit SHA into every zip.

**Why.** The script's comments: "Fixed timestamp so building the same commit twice yields identical bytes." `SOURCE.md` ties a downloaded folder back to its commit so readers can find updates and contribute changes ([Example downloads](../features/example-downloads.md)).

## AP e-invoice ships as a zip

**Decision.** `catalog/ap-einvoice/` contains `catalog/ap-einvoice/apeinvoice.zip` plus scripts to rewrite the app reference id, instead of loose source files.

**Why.** The README explains that the app reference id suffix (`apeinvoice_pgbbrx`) is organization-specific, and the scripts rewrite it before deployment. The cost is that neither audit can see the source ([Finance apps](../catalog/finance.md)).
