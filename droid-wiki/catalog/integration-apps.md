# Integration apps

Active contributors: cngan-wd, ekwuno

## Purpose

Integration apps are orchestrations with no user interface. Each one runs as an Integration System inside a Workday tenant, usually launched from a custom report or another integration, and moves data in or out. Seven catalog entries have type `Integration App`, and `catalog/createSpotBonus/` (type `Orchestration`) follows the same shape, so it is covered here too.

These are the smallest folders in the catalog. Four of them are just `README.md`, `example.json`, `appManifest.json`, and one `.orchestration` file.

## The apps

| Entry | Direction | Products | Orchestrations | Pattern |
| --- | --- | --- | --- | --- |
| `catalog/employeeDemographicOutbound/` | Outbound file | Orchestrate, HCM | `EmployeeDemographicFull` | Build a delimited demographic file |
| `catalog/locationChangesOutbound/` | Outbound file | Orchestrate, HCM | `OutputTransform`, `CCW_InputFields` (sub) | Post-process Core Connector: Locations output |
| `catalog/learningEnrollments/` | Boomerang | Orchestrate, RaaS, HCM | `LearningEnrollmentBoomerang` | Report finds workers, orchestration enrolls them in a course |
| `catalog/updateServiceDateBoomerang/` | Boomerang | Orchestrate, RaaS, HCM | `UpdateServiceDatesBoomerang` | Report finds workers, orchestration updates service dates |
| `catalog/updateWorkdayAccounts/` | Boomerang | Orchestrate, RaaS, HCM | `BoomerangWDAccnt` | Adds a `_hk` suffix to Hong Kong workers' usernames |
| `catalog/supplierInvoicesInbound/` | Inbound | Orchestrate, Financials, SOAP API | `INT_Supplier_Invoices_Orch_Inbound`, `INT_Check_Import_Result_Orch` | Import supplier invoices and check the result |
| `catalog/workerInboundImageUpload/` | Inbound | Orchestrate, SOAP API, HCM | `WorkerPhotoInbound` plus 9 subflows and 2 test flows | Load worker photos |
| `catalog/createSpotBonus/` | API | Orchestrate, HCM, Graph API | `OneTimePaymentWithFeedback` | One request creates a one-time payment with anytime feedback |

"Boomerang" in Workday's naming means the integration reads from the tenant (through a RaaS custom report) and writes back to the same tenant.

## How the READMEs are organized

The integration READMEs share one outline: Deploy Instructions (Create Copy from the Developer Site), Configuration Instructions (create a custom report, create an Integration System, configure security), and Usage Instructions. `catalog/supplierInvoicesInbound/README.md` is the longest because it needs two Integration Systems and an integration business process. `catalog/updateWorkdayAccounts/README.md` has no `_Version_` line, unlike the other seven. None of these READMEs use the headings the hub expects for new entries, so the audit's `HubReadmeSectionsRule` flags them when run with `--all` (see [Hub rules](../systems/audit/hub-rules.md)).

## workerInboundImageUpload

This entry is the most structured integration in the repo. `catalog/workerInboundImageUpload/orchestration/WorkerPhotoInbound.orchestration` delegates to subflows named by role: `LOGIC_Disposition*` for routing, `WorkdayApi_POST` for the SOAP call, `EventLog_Message` and four `Debug_*` subflows for logging. Two `TEST_GenerateMockData_*` orchestrations create test data. `catalog/workerInboundImageUpload/orchestration/TEST_GenerateMockData_TestPhotos.orchestration` is about 514 KB because it embeds roughly 495 KB of base64 PNG images, which makes it the second-largest orchestration in the repo after `catalog/prismAndExtendDesignPatterns/orchestration/install.orchestration`.

## Key source files

| File | Purpose |
| --- | --- |
| `catalog/supplierInvoicesInbound/README.md` | Most complete integration setup guide |
| `catalog/workerInboundImageUpload/orchestration/WorkerPhotoInbound.orchestration` | Main flow with subflow decomposition |
| `catalog/learningEnrollments/orchestration/LearningEnrollmentBoomerang.orchestration` | Typical boomerang flow |
| `catalog/createSpotBonus/orchestration/OneTimePaymentWithFeedback.orchestration` | Graph API single-request example |

## Related pages

- [Orchestration files](../primitives/orchestration-files.md)
- [Reference apps](reference-apps.md) for the Orchestrate Sampler, which collects reusable integration patterns
