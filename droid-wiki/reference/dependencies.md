# Dependencies

The hub tooling has almost no dependencies by design. Everything external falls into three groups: the gallery's npm packages, GitHub Actions, and the Arcane Auditor CLI. Workday services that entries depend on are listed at the end.

## Hub scripts

`scripts/*.mjs` and `scripts/audit/*.mjs` import only Node built-ins (`node:fs`, `node:path`, `node:url`, `node:child_process`, `node:os`). Runtime: Node 20, as pinned in every workflow. The shell scaffolders need only bash or PowerShell; `scripts/install-arcane.sh` needs bash and `curl`.

## Gallery (`site/`)

Declared in `site/package.json`, resolved in `site/package-lock.json` (lockfile version 3, 440 packages):

| Package | Declared | Locked | Role |
| --- | --- | --- | --- |
| `astro` | `^5.0.0` | 5.18.2 | Static site framework; renders the README markdown that pages import through `import.meta.glob` |
| `tailwindcss` | `^4.1.0` | 4.3.3 | Styling |
| `@tailwindcss/vite` | `^4.1.0` | 4.3.3 | Tailwind as a Vite plugin (`site/astro.config.mjs`) |
| `@tailwindcss/typography` | `^0.5.20` | 0.5.20 | `prose` styles for rendered READMEs |
| `fflate` (dev) | `^0.8.3` | 0.8.3 | Zip creation in `site/scripts/build-zips.mjs` |

Notable transitive packages: `shiki` 3.23.0 (code highlighting in Astro markdown), `esbuild` 0.27.7, and `rollup` 4.62.2.

There are two copies of Vite in the lockfile. Astro 5.18.2 depends on `vite ^6.4.1` and gets its own nested `node_modules/astro/node_modules/vite` at 6.4.3. `@tailwindcss/vite` accepts Vite 5 through 8 as a peer, so npm hoisted `vite` 8.1.5 to the top level. The build works, but a Vite plugin that expects to share Astro's Vite instance may behave differently from what its docs describe.

Astro 5.18.2's `engines` field accepts Node `18.20.8 || ^20.3.0 || >=22.0.0`, which is consistent with CI's Node 20.

There are no runtime dependencies in the deployed site. GitHub Pages serves static HTML, CSS, and the prebuilt zips.

## GitHub Actions

| Action | Version | Used in |
| --- | --- | --- |
| `actions/checkout` | `@v7` | All four workflows |
| `actions/setup-node` | `@v7` | All four workflows |
| `actions/upload-artifact` | `@v4` | `.github/workflows/audit-examples.yml` |
| `actions/download-artifact` | `@v4` | `.github/workflows/audit-comment.yml` |
| `actions/github-script` | `@v7` | `.github/workflows/audit-comment.yml` |
| `actions/configure-pages` | `@v6` | `.github/workflows/deploy-gallery.yml` |
| `actions/upload-pages-artifact` | `@v5` | `.github/workflows/deploy-gallery.yml` |
| `actions/deploy-pages` | `@v5` | `.github/workflows/deploy-gallery.yml` |
| `Ekwuno/ArcaneAuditor` | commit `c31316d1147cb0e2d3f47688e668f7f3b2f9e887` | `.github/workflows/audit-examples.yml` |

Commit `c789865` ("Update GitHub Actions workflows to use latest action versions") bumped the actions in the validate and deploy workflows on Aug 5 2026. The two audit workflows arrived on Sep 10 already using these versions.

## Arcane Auditor

| Item | Value |
| --- | --- |
| Upstream project | `Developers-and-Dragons/ArcaneAuditor` (linked from `CONTRIBUTING.md` and `.arcane-auditor/README.md`) |
| Action and installer source | The `Ekwuno/ArcaneAuditor` fork, pinned by SHA in `.arcane-auditor/action-ref` |
| CLI version | v2.0.0 (per the workflow comment and `.arcane-auditor/README.md`) |
| Binary name | `ArcaneAuditorCLI` |
| Local install path | `.arcane-auditor/bin/` (gitignored) |
| Commands used | `review-app <dir> --agent [--config] [--rules] [--exclude-rules]`, `list-rules --format json`, `generate-config` (manually, when bumping) |

The upgrade procedure is in `.arcane-auditor/README.md` and summarized in [Arcane](../systems/audit/arcane.md).

## Services the entries depend on

These are not hub dependencies, but readers need them to use entries:

| Service | Needed by |
| --- | --- |
| Workday development tenant with Extend | Every Extend App and most Reference entries |
| Workday Orchestrate | All orchestrations and integration apps |
| Workday Prism Analytics | `catalog/prismAndExtendDesignPatterns/` |
| Workday Adaptive Planning | `catalog/capitalProjectPlanning/` (optional link) |
| Workday AI Gateway | `catalog/documentIntelligenceWithTheAIGateway/`, `catalog/generateWQL/`, `catalog/charitableDonationsWithSentimentAnalysis/` |
| AWS (Lambda, Translate, Textract, Comprehend, EventBridge), SAM CLI | `catalog/AWSStarterKit/`, `catalog/AWSBadgeGenerator/` |
| External HTTP APIs | `examples/stock-notifications/` (`stockinfo-a6k3.onrender.com`), `catalog/requestCreditCard/` (random number API) |
| An AI agent that loads `SKILL.md` files | The ten Agent Skill entries |

## Related pages

- [Tooling](../how-to-contribute/tooling.md)
- [Security](../security.md)
- [Configuration](configuration.md)
