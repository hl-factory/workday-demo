# Examples

`examples/` is the community section and the only place outside pull requests land. It holds 14 entries plus `examples/_template/`. Ten are markdown agent skills, two are small Extend apps, and two are orchestrations. `examples/README.md` describes the section and how to add to it, and every entry follows the [example entry](../primitives/example-entry.md) contract.

## Entries

| Entry | Type | Author | Added | Page |
| --- | --- | --- | --- | --- |
| `examples/workday-skill` | Agent Skill | srivilliamsai | Sep 2026 (PR #17) | [Workday agent skills](workday-agent-skills.md) |
| `examples/workday-extend-skill` | Agent Skill | srivilliamsai | Sep 2026 (PR #17) | [Workday agent skills](workday-agent-skills.md) |
| `examples/workday-pmd-skill` | Agent Skill | srivilliamsai | Sep 2026 (PR #17) | [Workday agent skills](workday-agent-skills.md) |
| `examples/workday-home-cards-skill` | Agent Skill | srivilliamsai | Sep 2026 (PR #17) | [Workday agent skills](workday-agent-skills.md) |
| `examples/workday-orchestrate-skill` | Agent Skill | srivilliamsai | Sep 2026 (PR #17) | [Workday agent skills](workday-agent-skills.md) |
| `examples/workday-apis-skill` | Agent Skill | srivilliamsai | Sep 2026 (PR #17) | [Workday agent skills](workday-agent-skills.md) |
| `examples/workday-ai-gateway-skill` | Agent Skill | srivilliamsai | Sep 2026 (PR #17) | [Workday agent skills](workday-agent-skills.md) |
| `examples/workday-developer-copilot-skill` | Agent Skill | srivilliamsai | Sep 2026 (PR #17) | [Workday agent skills](workday-agent-skills.md) |
| `examples/expense-policy-agent-skill` | Agent Skill | obinnacodes | Jul 2026 | [Policy skills](policy-skills.md) |
| `examples/pto-policy-agent-skill` | Agent Skill | srivilliamsai | Aug 2026 (PR #5) | [Policy skills](policy-skills.md) |
| `examples/stock-notifications` | Extend App | tony-gilfillan | Jul 2026 (PR #3) | [Extend examples](extend-examples.md) |
| `examples/peer-kudos-home-card` | Extend App | srivilliamsai | Sep 2026 (PR #16) | [Extend examples](extend-examples.md) |
| `examples/wql-anniversary-celebrations` | Orchestration | srivilliamsai | Sep 2026 (PR #14) | [Orchestrations](orchestrations.md) |
| `examples/employee-data-orchestration` | Orchestration (placeholder) | obinnacodes | Jul 2026 | [Orchestrations](orchestrations.md) |

The first three examples arrived together in commit `50a7c33` on Jul 22 2026 as format samples: `expense-policy-agent-skill`, `employee-data-orchestration`, and a placeholder `work-from-anywhere-extend-app`. The third moved to `catalog/` in the Jul 29 split (`bd640f8`) and was deleted on Aug 5 in commit `86dbd3e`, the same commit that imported the real `catalog/workFromAlmostAnywhere/` app along with 30 others.

## Health against the audit

All 14 pass the validator and every ACTION-level hub rule. Four lack a "Before you deploy" section, which is ADVICE in `examples/`: `employee-data-orchestration`, `expense-policy-agent-skill`, `pto-policy-agent-skill` (both use a "Customizing" heading, which the rule does not accept), and `stock-notifications`. `stock-notifications` also carries a tenant app reference id, `stocknotifications_svfbfp`, which `HubAppReferenceIdRule` flags. See [Hub rules](../systems/audit/hub-rules.md).

## Sub-pages

- [Workday agent skills](workday-agent-skills.md): the router skill and seven artifact-specific review skills
- [Policy skills](policy-skills.md): expense and PTO question-answering skills
- [Extend examples](extend-examples.md): Stock Notifications and Peer Kudos
- [Orchestrations](orchestrations.md): WQL milestone celebrations and the employee data placeholder
- [Template](template.md): `examples/_template/` and how it is used

For the path from idea to merged example, see [Contribution flow](../features/contribution-flow.md).
