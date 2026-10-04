# Finance apps

Active contributors: cngan-wd, tony-gilfillan

## Purpose

Four catalog entries target Workday Financials or analytics: capital project planning with Adaptive Planning, corporate card requests, inbound electronic invoices, and Prism Analytics design patterns. They are among the most complex entries in the repo.

## The apps

| Entry | Type | What it does | Files |
| --- | --- | --- | --- |
| `catalog/capitalProjectPlanning/` | Extend App | Request capital projects and manage planned capital funds; built to complement Workday Adaptive Planning | 115 |
| `catalog/requestCreditCard/` | Extend App | Employees request a corporate credit card that Stripe issues in real time | 17 |
| `catalog/ap-einvoice/` | Reference | An Extend app with an inbound endpoint and orchestration that turns XML e-invoices into Supplier Invoices | 10 (source in a zip) |
| `catalog/prismAndExtendDesignPatterns/` | Extend App | Scalable pages over large data sets, and a single-threaded pattern for triggering Prism Data Change Tasks | 29 |

## Capital project planning

The largest entry in the repo by file count, with 115 files (version 2025.1). It has 21 pages, 12 orchestrations, and 45 suborchestrations, plus model files, app attributes, test mocks, and scripts. The README describes two components, the Extend app and an Adaptive Planning instance, and supports running standalone first and linking Adaptive Planning later. `catalog/capitalProjectPlanning/presentation/ApplicationConfigEdit.pmd` (1,440 lines) and `catalog/capitalProjectPlanning/presentation/CapitalPlanningRequestEdit.pmd` (1,290 lines) are the two longest PMDs in the repo. `catalog/capitalProjectPlanning/orchestration/SetRequestToApproved.orchestration` is one of only three business-process-triggered flows.

## Request credit card

A small Extend app (version 2025.1) with a `RequestCreditCard` business process, two orchestrations, and four pages. `catalog/requestCreditCard/orchestration/RequestCreditCardOrchestration.orchestration` is business-process triggered. `catalog/requestCreditCard/orchestration/LogMyOrchestration.orchestration` is the oldest flow format in the repo (`flowVersion` 2.4.0). The README walks through creating a `Default_ISU` (the name must match exactly if you deploy without editing the orchestration), issuing a card to a sample worker, and seeing it in Stripe. The exported orchestration itself contains no Stripe reference; its only external URL is `https://www.randomnumberapi.com/api/v1.0/random`, so the card issuing step appears to be simulated or configured outside the flow in this copy.

## AP e-invoice

Added by cngan-wd in Sep 2026 (PR #13), the only catalog entry not from the Aug import. It is packaged differently from every other entry:

```text
catalog/ap-einvoice/
├── README.md, example.json
├── apeinvoice.zip                                         # app source (92 files)
├── scripts/updateAppid.sh, scripts/updateAppid.ps1        # rewrite the app reference id in the source
├── sampleInvoices/                                        # ubl_2_1.xml and batch.json
├── AP-E-InvoiceReferenceApplicationDemo.mp4              # 18.6 MB, the largest file in the repo
├── AP-E-InvoiceReferenceApplicationOverview.pdf
└── ConfiguringOptionalCustomObjectForE-InvoiceApplication.pdf
```

The delivered source uses the app reference id `apeinvoice_pgbbrx`. The README explains that the suffix is org-specific and the scripts rewrite it before deployment. Because the source is zipped, neither Arcane nor the hub rules can see the PMDs or orchestrations inside it; commit `c96d5d4` ("Fix Arcane ACTION findings in the AP e-invoice app zip") shows the author ran Arcane on the unzipped source separately. It is the only catalog README with the hub's "What it is", "What's inside", and "Before you deploy" sections, and `example.json` lists two authors by name.

## Prism and Extend design patterns

Requires a Prism-enabled development tenant. `catalog/prismAndExtendDesignPatterns/orchestration/install.orchestration` (560 KB, single line) is the largest file of its kind in the repo.

## Key source files

| File | Purpose |
| --- | --- |
| `catalog/capitalProjectPlanning/README.md` | Deployment scenarios with Adaptive Planning |
| `catalog/capitalProjectPlanning/presentation/ApplicationConfigEdit.pmd` | Longest PMD in the repo |
| `catalog/requestCreditCard/orchestration/RequestCreditCardOrchestration.orchestration` | Business-process-triggered card request flow |
| `catalog/ap-einvoice/README.md` | Packaging and app id rewrite instructions |
| `catalog/ap-einvoice/scripts/updateAppid.sh` | App id rewrite script |

## Related pages

- [Orchestration files](../primitives/orchestration-files.md)
- [Catalog](index.md)
