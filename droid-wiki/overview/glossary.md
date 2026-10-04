# Glossary

Terms that appear in this repo's files, READMEs, and tooling. Workday platform terms are defined only as far as the repo uses them; the hub's own terms point at the code that defines them.

## Hub terms

| Term | Meaning | Where it is defined |
| --- | --- | --- |
| Entry | One folder under `catalog/` or `examples/` with `example.json` and `README.md` | `scripts/validate-examples.mjs` (`validateEntry`) |
| Catalog | The Workday-built section, `catalog/`. Displayed as "App catalog" in the gallery. | `README.md`, `catalog/README.md` |
| Examples | The community section, `examples/`. Open to outside PRs. | `examples/README.md` |
| `example.json` | Entry metadata: `title`, `description`, `type` (required) plus `components`, `products`, `authors`, `tutorial`, `source` | `examples/_template/README.md` |
| Type | One of Extend App, Integration App, Orchestration, Agent Skill, Reference | `hub.config.json` `types` |
| Component | A building block such as Presentation, Model, Card, WQL, Graph API | `hub.config.json` `components` |
| Product | A Workday product such as Workday Extend, Workday Orchestrate, Workday HCM | `hub.config.json` `products` |
| Source | `workday` or `community`. Drives the badge in the gallery and the support policy. | `SUPPORT.md`, `site/src/lib/examples.js` (`sourceLabel`) |
| Index tables | The two tables in `README.md` between `<!-- catalog:start -->`/`<!-- catalog:end -->` and `<!-- examples:start -->`/`<!-- examples:end -->` | `scripts/validate-examples.mjs` |
| Before you deploy | The README section that lists every tenant-specific value a reader must change | `docs/EXAMPLE_BEST_PRACTICES.md` |
| Gallery | The optional Astro site in `site/`, hosted on GitHub Pages | `site/README.md` |
| `SOURCE.md` | A generated file inside each download zip recording repo, folder, commit, and how to contribute back | `site/src/lib/gitTrace.js` |

## Audit terms

| Term | Meaning | Where it is defined |
| --- | --- | --- |
| Arcane Auditor | A community code review CLI for Extend and Orchestrate source (48 rules, v2.0.0 pinned) | `.arcane-auditor/README.md` |
| Hub rules | The seven `Hub*` rules the hub adds on top of Arcane | `scripts/audit/hub-rules.mjs` |
| ACTION | Blocking severity. Fails the Audit examples check in enforcing mode. | `scripts/audit/report.mjs` |
| ADVICE | Non-blocking severity. Shown as suggestions. | `scripts/audit/report.mjs` |
| Effective severity | What CI acts on after policy: catalog promotion, pre-existing-line downgrade, advisory mode | `scripts/audit/report.mjs` (`applyPolicy`) |
| `hub-audit/1` | The JSON schema version of the unified audit report | `scripts/audit/report.mjs` (`SCHEMA`) |
| Enforcing / advisory | Audit modes. Advisory turns every ACTION into ADVICE. | `scripts/audit-examples.mjs` `--mode` |
| `audit-override` | PR label that makes the audit advisory for that PR | `.github/workflows/audit-examples.yml` |
| `AUDIT_MODE` | Repository variable; `advisory` makes the whole check non-blocking | `.github/workflows/audit-examples.yml` |
| Sticky comment | The single summary comment the audit updates on each push, marked `<!-- hub-audit -->` | `scripts/audit/post-review.mjs` |
| One-click suggestion | A GitHub `suggestion` block generated when a fix is a single-line substitution | `scripts/audit/report.mjs` (`suggestionFor`) |

## Workday platform terms

| Term | Meaning in this repo |
| --- | --- |
| Workday Extend | Platform for custom apps that run inside Workday. Most catalog entries are Extend apps. |
| WCP | Workday Cloud Platform. READMEs say "WCP development tenant". |
| AMD | Application metadata file (`presentation/*.amd`). Lists tasks with `routingPattern` and `page.id`, `applicationId`, and data providers. |
| SMD | Site metadata file (`presentation/*.smd`). Holds `siteAuth` (SSO) and `languages`. |
| PMD | Presentation page file (`*.pmd`). JSON with `id`, `securityDomains`, `endPoints`, and `presentation`. 298 in the repo. |
| POD | Reusable presentation fragment (`*.pod`) with `podId` and a `seed` template, such as a footer |
| `.script` | PMD Script functions included by pages, in `presentation/scripts/` |
| Business object | Custom data model (`model/*.businessobject`) with typed fields and `defaultSecurityDomains` |
| Security domain | Access control unit (`model/*.securitydomain`) mapped to security groups in App Manager |
| Business process | Workday approval flow defined for a business object (`model/*.businessprocess`) |
| Task | A launchable entry point in the tenant (`model/*.task`) with a `routePath` |
| Card / card definition | In-page cards (`presentation/cards/*.card`) and Home page cards (`cards/*.carddefinition` plus `*.cardtenantsetting`) |
| App reference id | The app id Workday generates per tenant, such as `stocknotifications_svfbfp`. The `_xxxxxx` suffix is org-specific. |
| `site.applicationId` | The portable way to reference the app id in scripts |
| `apiGatewayEndpoint` | Application variable for the Workday API host; the portable alternative to a hardcoded `*.workday.com` URL |
| `baseUrlType` | Endpoint field that refers to a data provider key, such as `workday-graph` or `workday-apps` |
| Workday Orchestrate | Low-code integration and flow engine. Flows are `.orchestration` and `.suborchestration` files. |
| Orchestration Builder | The Orchestrate editor where flows are imported and deployed |
| Maya | The internal flow format name visible in orchestration JSON (`.maya.FlowSync`, `.maya.FlowSubflow`) |
| Orchestrate for Integrations | Orchestrate used to build integration systems. Several READMEs note an Innovation Service Agreement opt-in. |
| Integration System / ISU / ISSG | A tenant integration definition, its Integration System User, and that user's security group |
| WQL | Workday Query Language. Stored as `.wqlquery` JSON with `id`, `parameters`, `query`. |
| Graph API | Workday Graph queries, stored as `.graphquery` JSON with `id` and `query` |
| RaaS | Report as a Service. Custom reports some integration apps read. |
| AI Gateway | Workday's gateway to AI services such as Document Intelligence, Generate WQL, and sentiment analysis |
| App Manager | Tenant task where readers configure security domains and app attributes after deploying |
| App Hub / Console | Developer site areas where catalog apps are copied, edited, and promoted |
| GMS tenant | The developer tenant data set many READMEs reference (for example users `lmcneil` and `bliu`) |
| Prism / DCT | Workday Prism Analytics and its Data Change Tasks, used by `catalog/prismAndExtendDesignPatterns/` |

## Related pages

- [Extend app anatomy](../primitives/extend-app-anatomy.md)
- [Orchestration files](../primitives/orchestration-files.md)
- [Example entry](../primitives/example-entry.md)
