# Reference apps

Active contributors: cngan-wd, ekwuno

## Purpose

Reference apps are not business solutions. They are galleries of techniques: every page or orchestration demonstrates one feature, and developers copy the parts they need. Seven catalog entries have type `Reference`. Six are covered here; `catalog/ap-einvoice/` is a reference implementation for a finance scenario and is described in [Finance apps](finance.md).

## The apps

| Entry | Files | Focus | Products |
| --- | --- | --- | --- |
| `catalog/pmdWidgetDictionary/` | 97 | Presentation widgets, one page per widget family (62 PMD files) | Extend |
| `catalog/chartDictionary/` | 38 | Chart 2.0 widgets and chart features | Extend |
| `catalog/pmdScripting/` | 23 | Common PMD scripting uses | Extend |
| `catalog/wql/` | 7 | Compare a standard REST endpoint with a WQL query | Extend, REST API |
| `catalog/orchestrationToolkit/` | 67 | Error handling, paging, validation, files, OAuth, in Extend apps | Extend, Orchestrate |
| `catalog/orchestrateForIntegrationsSampler/` | 25 | Loops, batching, Excel and PDF output, import processing, in integrations | Orchestrate |

## Presentation references

`catalog/chartDictionary/presentation/` has one page per chart type (`catalog/chartDictionary/presentation/bubbleChart2.pmd`, `catalog/chartDictionary/presentation/waterfallChart2.pmd`, `catalog/chartDictionary/presentation/podiumChart.pmd`, and others) and one page per chart feature (`catalog/chartDictionary/presentation/featureTooltip.pmd`, `catalog/chartDictionary/presentation/featureAccessibilityMode.pmd`, `catalog/chartDictionary/presentation/featureNumberFormatting.pmd`, and others).

`catalog/pmdScripting/presentation/` is organized by topic: `catalog/pmdScripting/presentation/dataManipulation.pmd`, `catalog/pmdScripting/presentation/dateCalculations.pmd`, `catalog/pmdScripting/presentation/gridEvents.pmd`, `catalog/pmdScripting/presentation/invokingEndpoints.pmd`, `catalog/pmdScripting/presentation/logging.pmd`, `catalog/pmdScripting/presentation/pageEvents.pmd`, and `catalog/pmdScripting/presentation/updateWidgetState.pmd`. The `catalog/pmdScripting/presentation/logging.pmd` page calls `catalog/pmdScripting/presentation/scripts/logging.script`, whose four `console.debug`, `console.error`, `console.info`, and `console.warn` calls are intentional demonstrations rather than leftover debugging. The page tells the user to look under Analytics > Logs on the Developer Site with the filter `wd_category is console`.

`catalog/pmdWidgetDictionary/` is the largest reference app, with 89 files under `presentation/`, two cards, and two orchestrations. It is the most direct way to look up how a widget tag is written.

## Orchestration references

The two orchestration references overlap in places (both have `CreateExcel` and `CreateSingleSheet` subflows) but target different audiences.

`catalog/orchestrationToolkit/` is for Extend developers. Its file names describe the pattern exactly, for example `HTTPwithValidResponseCodesConfig`, `LocalErrorHandlerPropagateNoGlobalHandler`, `ValidateWithValidationIterator`, `RESTPagedGet`, `OAuthCCExample`, and `LunchAccountUpdateWithRollback`. It also contains `catalog/orchestrationToolkit/orchestration/AsynchGlobalErrorHandlerLogging.orchestration`, the only orchestration in the repo with the `Mixed.MixedFlow` flow type. The app includes 16 model files under `model/` so the samples have business objects to work against.

`catalog/orchestrateForIntegrationsSampler/` is for integration developers: `BatchLoop_SizeStrategy`, `BatchLoop_GroupStrategy`, `BatchLoop_CustomStrategy`, `JoinLoop`, `AnyLoop_CombineIterators`, `ImportProcessor`, `TextFilePrePostProcessor`, `Simple_PDF`, `Token_PDF`, `Translate_PDF`, `UnzipOneFile`, and `WQLQuery`. Its README has only two usage steps and points to a forum post that explains each orchestration.

## Key source files

| File | Purpose |
| --- | --- |
| `catalog/pmdWidgetDictionary/README.md` | Deploy steps for the widget gallery |
| `catalog/chartDictionary/presentation/home.pmd` | Chart gallery landing page |
| `catalog/pmdScripting/presentation/invokingEndpoints.pmd` | Calling endpoints from PMD script |
| `catalog/orchestrationToolkit/orchestration/AsynchGlobalErrorHandlerLogging.orchestration` | Global error handler pattern |
| `catalog/orchestrateForIntegrationsSampler/orchestration/BatchLoop_SizeStrategy.orchestration` | Batching pattern |
| `catalog/wql/README.md` | WQL comparison app setup |

## Related pages

- [Extend app anatomy](../primitives/extend-app-anatomy.md)
- [Orchestration files](../primitives/orchestration-files.md)
- [Integration apps](integration-apps.md)
