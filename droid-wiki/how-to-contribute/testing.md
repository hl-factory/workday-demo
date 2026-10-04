# Testing

There is no unit test suite in this repo: no `test/` folder, no `*.test.*` files, and no `test` script in `site/package.json`. Quality is checked in three other ways: the hub's own checks, a gallery build, and testing each entry in a Workday tenant.

## Hub checks

These are the checks CI runs, and they double as the test suite for the hub tooling.

| Check | Command | What it proves |
| --- | --- | --- |
| Metadata and index | `node scripts/validate-examples.mjs --check` | Every `example.json` is valid against `hub.config.json`, every entry has a README, and the root `README.md` index tables match the entries |
| Hub rules | `node scripts/audit-examples.mjs --dirs <folder> --skip-arcane` | Folder naming, README sections, template leftovers, `.gitkeep`, period literals, and app reference ids |
| Arcane Auditor | `node scripts/audit-examples.mjs --dirs <folder>` (after `./scripts/install-arcane.sh`) | 48 code rules over PMD, pod, script, AMD, SMD, WQL, and orchestration files |
| Gallery build | `cd site && npm run build` | Every entry renders, README markdown converts, and zips build |

### Regression checks for tooling changes

When changing a hub rule or the severity policy, run the whole repo before and after:

```bash
node scripts/audit-examples.mjs --all --hub-only --format json --mode advisory > /tmp/before.json
# make the change
node scripts/audit-examples.mjs --all --hub-only --format json --mode advisory > /tmp/after.json
```

At commit `8b3e7c7` the baseline is 95 ACTION and 6 ADVICE findings after policy. A change that moves these numbers should be intentional.

`.arcane-auditor/README.md` also documents a formal regression check. Upstream PR #7 (`examples/Promotion_Nomination`, never merged) contains most of the mistakes the audit exists to catch. The README gives the commands to fetch it into a worktree under `/tmp`, copy the current `scripts/` and config in, and run `--changed main HEAD`. With Arcane v2.0.0 the expected result is 62 findings, 32 blocking, and the README lists every expected finding by rule. Re-run it after bumping Arcane or changing hub rules. In this fork, `origin` is `hl-factory/workday-demo`, so fetch PR #7 from the `upstream` remote instead.

For the scaffolders, run all three with the same name and diff the output. `CONTRIBUTING.md` promises "they produce identical folders", and nothing in CI checks that.

## Testing an entry in a tenant

The hub cannot run Workday artifacts. Contributors test them in a Workday development tenant before submitting, and reviewers follow the README to confirm "it works". `CONTRIBUTING.md` lists this as the first review criterion.

Some catalog apps include their own test aids:

| Kind | Where | How it works |
| --- | --- | --- |
| Presentation mocks | `presentation/testing/*.mock_response` and `*.mock_config` in `catalog/vehicleRegistration/`, `catalog/AWSBadgeGenerator/`, `catalog/capitalProjectPlanning/` | A `mock_config` maps endpoint names (for example `me`, `getVehicles`) to mock ids; each `mock_response` holds an `id`, `responseCode`, and `responseBody`, so pages can be previewed without live data |
| Test orchestrations | `catalog/workerInboundImageUpload/orchestration/TEST_GenerateMockData_*.orchestration`, `catalog/orchestrateForIntegrationsSampler/orchestration/TextFilePrePostProcessor_Test.orchestration`, `catalog/capitalProjectPlanning/orchestration/QuickTester.orchestration` | Flows that generate fixtures or drive another flow |
| Test pages | `catalog/AWSStarterKit/presentation/testAnyAWSAPI.pmd`, `catalog/orchestrationToolkit/presentation/testPMDBulkErrors.pmd`, `catalog/orchestrationToolkit/presentation/testOrchestrateBulkErrors.pmd` | Pages that exercise an orchestration interactively |

There are 8 `.mock_response` files in the repo. Mock data in these files uses made-up Workday IDs, which is what `CONTRIBUTING.md` requires ("Sample data must be clearly fictional").

## Agent skills

Agent Skill entries are markdown. The audit runs only hub rules on them (Arcane has no files to read). The useful test is to load the skill in an agent and ask it to review one of the catalog folders the skill references, then check that every referenced path still exists. Renaming a catalog folder breaks those references without any check failing ([Pitfalls](../background/pitfalls.md)).

## Related pages

- [Debugging](debugging.md)
- [Validator](../systems/validator.md)
- [Audit pipeline](../systems/audit/index.md)
