# Debugging

Most problems here show up as a failing check on a pull request. This page maps each symptom to its cause and fix.

## Validator failures

`scripts/validate-examples.mjs` prints every problem, then "N problem(s). Fix them and re-run." and exits 1.

| Message | Cause | Fix |
| --- | --- | --- |
| `examples/x: missing README.md` or `missing example.json` | One of the two contract files is absent | Add it, or copy from `examples/_template/` |
| `example.json is not valid JSON: ...` | Syntax error, often a trailing comma | Fix the JSON |
| `"Foo" is not an approved type. Pick from: ...` | `type`, `components`, or `products` value not in `hub.config.json` | Use a listed value, or open an issue to extend the list |
| `"tutorial" should be an https link, or left out` | An `http://` or relative tutorial link | Use `https://` or drop the field |
| `catalog/x: catalog apps are Workday-maintained, so "source" cannot be "community"` | Community entry placed under `catalog/` | Move it to `examples/` |
| `A README table is out of date. Run: node scripts/validate-examples.mjs` | Only with `--check` (CI); the root README index does not match the entries | Run the validator without `--check` and commit the README |
| `README.md is missing the <!-- ... --> markers` | Someone edited the root README and removed the table markers | Restore `<!-- examples:start -->`/`<!-- examples:end -->` (and the catalog pair) |

The table comparison ignores column alignment, so Prettier or an editor reflowing the table will not cause a failure. A changed title, description, or type will.

## Audit failures

### "Arcane Auditor not found"

`scripts/audit/arcane.mjs` looks for the CLI in this order: the `ARCANE_AUDITOR_CMD` environment variable (a full command line), `ARCANE_AUDITOR_BIN`, `.arcane-auditor/bin/ArcaneAuditorCLI`, `~/.arcane-auditor/bin/ArcaneAuditorCLI`, then `PATH`. Run `./scripts/install-arcane.sh`, set one of the variables, or pass `--skip-arcane`. The audit exits with code 3 in this case.

### "--changed" finds nothing

`--changed` with no arguments diffs `origin/main...HEAD`. It ignores uncommitted and unstaged work, and files sitting directly under `examples/` (such as `examples/README.md`) do not count as an entry. Commit your work, run `git fetch origin main` if `origin/main` is stale, or use `--dirs examples/my-example` to audit a folder directly. In a fork where `origin` is not the upstream repo, pass the base explicitly: `--changed upstream/main HEAD`.

### `git diff failed`

The base ref does not exist locally. The message suggests `git fetch origin main` or `--dirs`. In CI this would mean the checkout lost `fetch-depth: 0`.

### A finding blocks on code you did not write

The severity policy downgrades ACTION findings on lines your PR did not touch, but only in folders that already existed. It cannot downgrade:

- File-level findings reported on line 0, such as README section and folder name findings
- Anything in a new folder (every line is new)
- ADVICE findings in `catalog/` on lines you did touch, because the catalog bar promotes ADVICE to ACTION before the downgrade step runs

See [Severity policy](../systems/audit/severity-policy.md). If the finding is wrong for your entry, explain it in the PR; a maintainer can apply the `audit-override` label.

### `ArcaneAuditorError` or `ArcaneAuditorWarning` in the report

These are synthetic findings. An error means the CLI crashed, exited with code 2 or higher, or printed output that was not JSON; the report includes the last 20 lines of stderr. A warning means Arcane's parser skipped a block, so its script rules did not run on that block. `.arcane-auditor/README.md` documents one known case: a `var x =call.invoke(` line in a PMD that the grammar cannot parse.

### No PR comment appeared

`.github/workflows/audit-comment.yml` runs after "Audit examples" finishes, from the default branch. Check that:

1. The audit run uploaded the `audit-report` artifact (it skips upload when no example folders changed).
2. The run was not cancelled by a newer push (the comment job skips cancelled runs).
3. `scripts/audit/post-review.mjs` accepted the report. It rejects anything without `schema_version: "hub-audit/1"`, with too many findings, or with paths containing `..`.

Inline comments post in batches of 20. A failed batch logs a warning and the summary comment still posts ([PR comments](../systems/audit/pr-comments.md)).

## Gallery build failures

| Symptom | Cause | Fix |
| --- | --- | --- |
| `npm run dev` or `build` fails before Astro starts | `site/scripts/build-zips.mjs` runs first through `predev`/`prebuild`, so a broken entry fails there | Run `node scripts/validate-examples.mjs` to find the broken `example.json` |
| `SOURCE.md` in a zip says "unknown (local build)" | `site/scripts/build-zips.mjs` found no `GITHUB_SHA` and `git rev-parse HEAD` failed | Expected outside a git checkout; CI always has a commit |
| Links point to `Workday/WorkdayDeveloperProgram` | `hub.config.json` still holds the upstream URLs | Edit `repoUrl` and `pagesUrl` for a fork ([Configuration](../reference/configuration.md)) |
| Assets 404 on GitHub Pages | Astro `base` comes from the path in `pagesUrl` | Make `pagesUrl` match the real Pages URL |
| Deploy step times out | Pages processing is slow | The workflow already allows 20 minutes; re-run the job |

## Workday artifacts

The hub cannot debug a Workday app for you, but some entries help. `catalog/pmdScripting/presentation/logging.pmd` shows how to write `console.*` messages and where to find them: Analytics > Logs on the Developer Site, filtered by `wd_category is console`. Remember to remove live `console` calls before submitting, since `ScriptConsoleLogRule` is an ACTION rule. `catalog/orchestrationToolkit/` has orchestrations for error handler patterns, and `catalog/workerInboundImageUpload/` shows a set of `Debug_*` subflows for logging inside integrations.

## Related pages

- [Testing](testing.md)
- [Tooling](tooling.md)
- [Pitfalls](../background/pitfalls.md)
