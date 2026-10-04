# How to contribute

There are two kinds of contribution to this repo, and they follow different paths.

| You want to | Where you work | Main tools | Read |
| --- | --- | --- | --- |
| Add or improve an example | `examples/<your-folder>/` | Scaffolder, validator, audit | [Contribution flow](../features/contribution-flow.md), [Development workflow](development-workflow.md) |
| Change a Workday-built app | `catalog/<app>/` | Same tools, plus DevRel review | [Development workflow](development-workflow.md), [Pitfalls](../background/pitfalls.md) |
| Change the hub tooling | `scripts/`, `site/`, `.github/workflows/` | Node 20, npm for `site/` only | [Tooling](tooling.md), [Patterns and conventions](patterns-and-conventions.md) |

`CONTRIBUTING.md` is the contributor-facing guide. It says outside pull requests go to `examples/`, and catalog changes start with an issue because `.github/CODEOWNERS` assigns `/catalog/` to `@Workday/devrel`.

## Pages in this section

- [Development workflow](development-workflow.md): the loop from branch to merged PR, for entries and for tooling
- [Testing](testing.md): what "tested" means here, since there is no unit test suite
- [Debugging](debugging.md): reading validator errors, audit findings, CI failures, and gallery build problems
- [Tooling](tooling.md): every script, flag, and environment variable
- [Patterns and conventions](patterns-and-conventions.md): rules for tooling code and for entry content

## Review expectations

Workday DevRel reviews every pull request. `CONTRIBUTING.md` lists three things reviewers check: it works (following the README produces the result), it teaches (the README explains why), and it is safe (no secrets, real data, or tenant-specific values). Before that, two checks run automatically: Validate examples and Audit examples. Both must pass, unless a maintainer applies the `audit-override` label (see [Severity policy](../systems/audit/severity-policy.md)).

Workday-authored entries are held to a stricter standard than community ones because people copy them as reference ([Source badges and support](../features/source-badges-and-support.md)).
