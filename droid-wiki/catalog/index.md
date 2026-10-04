# Catalog

`catalog/` holds 32 Workday-built apps. `catalog/README.md` says the section "is not open to external pull requests; CODEOWNERS requires a DevRel review on any change" (`.github/CODEOWNERS` has `/catalog/ @Workday/devrel`). Every entry carries the Workday badge. 31 of the apps arrived in one commit on Aug 5 2026 (`86dbd3e`, 863 files added), and `catalog/ap-einvoice/` was added in Sep 2026 (PRs around `8fa7b03` and #13).

## By type

| Type | Count | Entries |
| --- | --- | --- |
| Extend App | 17 | AWSBadgeGenerator, AWSStarterKit, capitalProjectPlanning, charitableDonations, charitableDonationsWithSentimentAnalysis, createAWorkEvent, documentIntelligenceWithTheAIGateway, employeeRecognition, employeeRelationsIncidentManagement, generateWQL, helpCaseCreation, multiRater, prismAndExtendDesignPatterns, requestCreditCard, tuitionReimbursement, vehicleRegistration, workFromAlmostAnywhere |
| Integration App | 7 | employeeDemographicOutbound, learningEnrollments, locationChangesOutbound, supplierInvoicesInbound, updateServiceDateBoomerang, updateWorkdayAccounts, workerInboundImageUpload |
| Reference | 7 | ap-einvoice, chartDictionary, orchestrateForIntegrationsSampler, orchestrationToolkit, pmdScripting, pmdWidgetDictionary, wql |
| Orchestration | 1 | createSpotBonus |

## Pages in this section

| Page | Entries |
| --- | --- |
| [HR and workforce apps](hr-and-workforce.md) | Charitable donations, work events, recognition, ER incident management, help cases, multi-rater reviews, tuition reimbursement, vehicle registration, work from anywhere |
| [Finance apps](finance.md) | Capital project planning, request credit card, AP e-invoice, Prism design patterns |
| [AI and AWS apps](ai-and-aws.md) | AI Gateway (document intelligence, WQL generation, sentiment) and AWS (starter kit, badge generator) |
| [Integration apps](integration-apps.md) | The seven Integration Apps and the createSpotBonus orchestration |
| [Reference apps](reference-apps.md) | Widget and chart dictionaries, PMD scripting, orchestration toolkit and sampler, WQL |

## Size

| Largest entries by files | Files |
| --- | --- |
| `catalog/capitalProjectPlanning/` | 115 |
| `catalog/pmdWidgetDictionary/` | 97 |
| `catalog/orchestrationToolkit/` | 67 |
| `catalog/employeeRelationsIncidentManagement/` | 59 |
| `catalog/workFromAlmostAnywhere/` | 51 |

The smallest are the single-orchestration Integration Apps and `catalog/createSpotBonus/` at 4 files each.

## The App Catalog format

The 31 apps from Aug 2026 came from Workday's earlier App Catalog and keep its README format:

- YAML frontmatter with `title` and `description`
- a version line such as `_Version 2025.1_`, plus a link to the App Catalog changelog on the developer forum
- headings like "Overview" or "Introduction", "Deploy Instructions" with "Create Copy (Recommended)" and often "Manual Deploy (Alternative)", "Configuration Instructions", and "Usage Instructions"

Versions range from 2024.1 to 2026.2. The newest are `catalog/employeeDemographicOutbound/` (2026.2), `catalog/employeeRelationsIncidentManagement/` and `catalog/orchestrateForIntegrationsSampler/` (2026.1). `catalog/ap-einvoice/` and `catalog/updateWorkdayAccounts/` have no version line.

"Create Copy" refers to a button on the Workday developer site, not to anything in this repo. Many READMEs also link to a forum page per app.

## Catalog versus the audit

The catalog predates the hub rules, so it fails several of them on every folder:

- 30 folders are camelCase and fail `HubFolderKebabCaseRule` (only `ap-einvoice` and `wql` pass)
- 31 READMEs lack the hub's "What it is / What's inside / How to use it / Before you deploy" sections
- `catalog/chartDictionary/` uses a tenant app reference id (`chart2CatalogApp_ycjtxv`)

Because catalog folders are held to the stricter bar and file-level findings are never downgraded, a PR that touches an existing catalog folder will usually fail the audit. See [Severity policy](../systems/audit/severity-policy.md) and [Pitfalls](../background/pitfalls.md).

The [Workday agent skills](../examples/workday-agent-skills.md) point at specific catalog folders as references, so renaming one means updating those skills too.
