# Template

Active contributors: ekwuno

## Purpose

`examples/_template/` is the starting point for every community example. It has two files, `README.md` and `example.json`, and nothing else. The scaffolders copy it, contributors without tooling copy it by hand (even from the GitHub web UI, per `examples/README.md`), and the leading underscore keeps it out of the validator, audit, gallery, and zips.

## `example.json`

```json
{
  "title": "My Example",
  "description": "One or two sentences about what this example shows.",
  "type": "Extend App",
  "components": [],
  "products": [],
  "authors": [],
  "tutorial": "",
  "source": "community"
}
```

The scaffolders replace `title` and `type`. Everything else is left for the contributor. If `title` stays `"My Example"` or `description` stays the placeholder sentence, `HubTemplateBoilerplateRule` reports an ACTION finding.

## `README.md`

The template README has five parts:

1. `# Example title` (the scaffolders replace line 1 with `# <title>`)
2. `## What it is`: one paragraph on what the example shows and who it is for
3. `## What's inside`: a bulleted list of folder contents, with sample bullets for `app/`, `orchestrations/`, `SKILL.md`, and `diagrams/`
4. `## How to use it`: deploy steps per artifact kind (Extend source, orchestration, agent skill), and a reminder never to commit real credentials
5. `## Before you deploy`: a checklist of what a reader must change in their tenant (app reference id, base URLs, security domains, dates or periods, WIDs), with worked examples like `myApp_abc123` and `2026-Q1` in `managerNomination.pmd`

Below a horizontal rule is a sixth part, "Fill in example.json (delete this section before submitting)", with a table describing every field. That heading text is in `TEMPLATE_STRINGS`, so leaving it in fails the audit.

The four `##` sections match the four checks in `HubReadmeSectionsRule`, and the "Before you deploy" checklist matches what `HubAppReferenceIdRule` and `HubHardcodedPeriodLiteralRule` look for. The `managerNomination.pmd` and `2026-Q1` examples appear to come from upstream PR #7 (`Promotion_Nomination`), the audit's regression case.

## Changing the template

If you edit the template, keep these in sync:

- `TEMPLATE_STRINGS` in `scripts/audit/hub-rules.mjs` (the five placeholder sentences)
- the `sed` patterns in `scripts/new-example.sh`, which match `"title": "My Example"` and `"type": "Extend App"` literally
- the README sections in `REQUIRED_SECTIONS` in `scripts/audit/hub-rules.mjs`

## Key source files

| File | Purpose |
| --- | --- |
| `examples/_template/README.md` | README skeleton |
| `examples/_template/example.json` | Metadata skeleton |
| `scripts/new-example.mjs` | Copies and fills the template |
| `scripts/audit/hub-rules.mjs` | Detects leftover template text |

## Related pages

- [Scaffolder](../systems/scaffolder.md)
- [Example entry](../primitives/example-entry.md)
- [Contribution flow](../features/contribution-flow.md)
