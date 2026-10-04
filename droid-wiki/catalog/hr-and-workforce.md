# HR and workforce apps

Active contributors: tony-gilfillan, ekwuno

## Purpose

Ten catalog Extend apps cover employee-facing HR scenarios: giving, events, recognition, case management, reviews, reimbursements, and remote work. They share the standard Extend layout (see [Extend app anatomy](../primitives/extend-app-anatomy.md)) and most follow the same page pattern: a `home.pmd` hub, a create wizard, a `microConfirm.pmd` confirmation, and view and edit pages, often with a business process for approval.

## The apps

| Entry | What it does | Notable pieces | Files |
| --- | --- | --- | --- |
| `catalog/charitableDonations/` | One-time and recurring payroll deductions to charities, with hubs and cards | `Charity` business object, `CreateCharity` business process with approve/deny, `deductionCode` app property (`UWAY`), 17 pages | 38 |
| `catalog/charitableDonationsWithSentimentAnalysis/` | Same app plus AI Gateway sentiment rating of charity descriptions | Adds a rating card and pod; see [AI and AWS apps](ai-and-aws.md) | 40 |
| `catalog/createAWorkEvent/` | Propose, approve, and register for work events, with a calendar view | `WorkEvent` and `Registrant` objects, `CreateWorkEvent` business process, 3 Graph queries | 43 |
| `catalog/employeeRecognition/` | Give feedback and gift-card rewards to coworkers | Home cards (`catalog/employeeRecognition/cards/createRecognitionCard.carddefinition` and tenant settings), WQL employee search, runs as a task or a related action on a worker | 21 |
| `catalog/employeeRelationsIncidentManagement/` | HR management of protected incident cases | 8 business objects, 5 orchestrations (export to PDF, delete, cleanup), 13 WQL queries, app attributes, `catalog/employeeRelationsIncidentManagement/tenantConfiguration/config.dat`, links to Workday Help cases | 59 |
| `catalog/helpCaseCreation/` | Custom UI to create Workday Help cases | Two pages and one task; the smallest Extend app in the catalog | 9 |
| `catalog/multiRater/` | Multiple raters give performance-review feedback that the manager can use to update ratings | `MultiRating` business process, 8 orchestrations for reading and updating ratings | 24 |
| `catalog/tuitionReimbursement/` | Tuition reimbursement requests paid as a One Time Payment | `EducationAssistanceApproval` business process, `CreateOneTimePayment` orchestration (business-process triggered), 5 WQL queries, labels file | 37 |
| `catalog/vehicleRegistration/` | Employees register vehicles by location | 8 Graph queries, 5 mock responses, translations in 5 locales (`en-US`, `de-DE`, `es-ES`, `fr-FR`, `ja-JP`), external APIs | 36 |
| `catalog/workFromAlmostAnywhere/` | Request remote work, manager approval, calendar view | `WorkFromAnywhereRequest` business process with approve/deny/send back and a right-to-work action step, Home cards with tenant settings, `Max Remote Days` app attribute | 51 |

## Shared patterns

- **Hub and wizard pages.** `catalog/charitableDonations/presentation/charityHub.pmd`, `catalog/createAWorkEvent/presentation/eventHub.pmd`, `catalog/workFromAlmostAnywhere/presentation/requestHub.pmd`, and `catalog/vehicleRegistration/presentation/vehicleHub.pmd` are landing pages; `catalog/charitableDonations/presentation/createCharityWizard.pmd`, `catalog/createAWorkEvent/presentation/createEventWizard.pmd`, `catalog/workFromAlmostAnywhere/presentation/createRequestWizard.pmd`, and `catalog/vehicleRegistration/presentation/registerVehicleWizard.pmd` are multi-step forms.
- **Business process pages.** Apps with approvals route the approval step and action steps to pages by `pageRoute` (for example `/approve/{eventId}` and `/review/{eventStepId}` in `catalog/workFromAlmostAnywhere/model/WorkFromAnywhereRequest.businessprocess`).
- **Attachment cleanup.** `deleteAttachments.pmd` appears in several apps alongside `.attachment` model files.
- **Shared footer pod.** A `presentation/pods/footer.pod` is reused across pages (for example `catalog/charitableDonations/presentation/pods/footer.pod`; seven catalog apps have one).

## Things to watch

- `catalog/vehicleRegistration/presentation/home.pmd` has two `console.info` calls inside bindings, which Arcane's `ScriptConsoleLogRule` flags.
- `catalog/vehicleRegistration/` Graph queries embed a tenant-specific type prefix (`vehicleRegistration20241_nkzjqw_...`), which has to be regenerated in another tenant.
- `catalog/tuitionReimbursement/` and `catalog/vehicleRegistration/` `.properties` files carry export timestamps from Oct 2022 and Sep 2023.
- `catalog/employeeRelationsIncidentManagement/orchestration/intGeneratePDFandAttachments.orchestration` is 490 KB on a single line.

## Key source files

| File | Purpose |
| --- | --- |
| `catalog/charitableDonations/presentation/charitableDonations.amd` | Compact AMD with flows and data providers |
| `catalog/workFromAlmostAnywhere/model/WorkFromAnywhereRequest.businessprocess` | Business process with action steps |
| `catalog/employeeRecognition/cards/createRecognitionCard.carddefinition` | Home card reference used by the home-cards skill |
| `catalog/vehicleRegistration/presentation/graphQueries/` | Graph query examples used by the APIs skill |
| `catalog/employeeRelationsIncidentManagement/README.md` | The most detailed configuration guide in this group |

## Related pages

- [Catalog](index.md)
- [Workday agent skills](../examples/workday-agent-skills.md)
