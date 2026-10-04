# Fun facts

Small, true, and verifiable things about this repo. Data collected on 2026-10-04.

## One video outweighs everything else

`catalog/ap-einvoice/AP-E-InvoiceReferenceApplicationDemo.mp4` is 18.6 MB. The rest of `catalog/` (904 files, every app in the catalog) adds up to about 7.7 MB. The video is roughly 70% of the catalog by bytes.

## Photos hidden inside a flow

`catalog/workerInboundImageUpload/orchestration/TEST_GenerateMockData_TestPhotos.orchestration` is a 514 KB orchestration, and about 495 KB of it is base64-encoded PNG data. The flow generates mock worker photos so the image upload integration can be tested without real employee pictures. Its sibling, `catalog/workerInboundImageUpload/orchestration/TEST_GenerateMockData_OriginalPhotos.orchestration`, holds another 302 KB.

## Every orchestration is one line long

All 163 `.orchestration` and `.suborchestration` files are single-line JSON, 4.9 MB in total. Change one property in the Orchestration Builder, re-export, and `git diff` shows the entire file as one deleted line and one added line. It also means every line number inside an orchestration is 1. The audit report keeps Arcane's `json_path` for each finding, which is the only thing that identifies the step.

## The rulebook is longer than the code that enforces it

`docs/EXAMPLE_BEST_PRACTICES.md` has 1,769 lines. All of `scripts/` (validator, three scaffolders, the Arcane installer, the audit runner, and six audit modules) has 1,735. Much of the guide quotes Arcane Auditor's rule documentation verbatim, a deliberate choice from commit `6470ee5`.

## A hub in one day

On 2026-07-22, ekwuno made 13 commits: README, template, scaffolder, CI validation, the Astro gallery, the first three examples, gallery auto-deploy, and three separate commits to put the Workday logo in the README, the favicon, and the site header. On 2026-09-10, five pull requests (#8 to #12) were merged in one day to add the audit.

## "Prune" that added 863 files

Commit `86dbd3e` is titled "Prune app catalog to core set of 31 apps". In this repo's history it added 863 files and deleted 3. The pruning happened somewhere else; this repo received the result.

## The oldest artifact predates the repo by almost four years

`catalog/tuitionReimbursement/presentation/presentationLabels/en-US.properties` starts with the Java properties timestamp `#Tue Oct 11 23:17:43 CDT 2022`. The repo's first commit is 2026-07-22.

## The oldest flow format

`catalog/requestCreditCard/orchestration/LogMyOrchestration.orchestration` uses `flowVersion` 2.4.0, the oldest of any orchestration here. The newest files use 3.3.0.

## Stripe is in the README but not the flow

`catalog/requestCreditCard/README.md` says cards are "issued in real-time by Stripe" and step 16 tells you to look for "Logan's new Stripe card". The exported orchestration has no Stripe reference. Its only external URL is a random number API.

## Hong Kong usernames

The `catalog/updateWorkdayAccounts/` integration exists to demonstrate one very specific change: append `_hk` to the Workday account usernames of workers in Hong Kong.

## Almost half the PMDs are not JSON

PMD files look like JSON, but 135 of the 298 in the repo fail a strict JSON parser because Workday's format allows embedded script and other relaxed syntax. The hub's own rules avoid the problem by scanning Workday files with regular expressions; `HubAppReferenceIdRule`, for example, pattern-matches `"applicationId"` in AMD and SMD files instead of parsing them.

## No TODOs

There is not a single `TODO`, `FIXME`, or `HACK` comment in the tracked files.

## A capital "I" that went missing

The folder is `catalog/documentIntelligenceWithTheAIGateway/`, but the app's `applicationId` is `documentIntelligenceWithTheAiGateway`.

## A podium

Among the line, bar, area, and scatter charts in `catalog/chartDictionary/presentation/` is `catalog/chartDictionary/presentation/podiumChart.pmd`.
