# Policy skills

Active contributors: obinnacodes, srivilliamsai

## Purpose

Two agent skills answer employee questions from a company policy table instead of reviewing code: `examples/expense-policy-agent-skill` (expenses) and `examples/pto-policy-agent-skill` (time off and leave). Both are templates. Their policy values are placeholders that an adopter replaces with the real handbook, and both put escalation rules first so the agent says "I can't confirm" rather than guessing about money or leave.

## Directory layout

```text
examples/expense-policy-agent-skill/
├── SKILL.md       # 43 lines, name: expense-policy-helper
├── README.md
└── example.json   # author obinnacodes
examples/pto-policy-agent-skill/
├── SKILL.md       # 46 lines, name: pto-policy-helper
├── README.md
└── example.json   # author srivilliamsai
```

## The two skills

| | Expense policy helper | PTO and leave policy helper |
| --- | --- | --- |
| Added | Jul 22 2026, one of the first three examples (`50a7c33`) | Aug 20 2026, PR #5 (`a3d7ea4`) |
| Triggers on | What can be expensed, limits, how to file a report | Time off, vacation days, sick leave, rollover, holidays |
| Policy table | Meals 75 USD/day, hotel 250 USD/night, ground transport at cost, home office 500 USD/year | PTO by tenure (15, 20, 25 days), 5-day rollover by Mar 31, 10 sick days, 12 weeks parental, 3 to 5 days bereavement |
| How to answer | Find the category, quote the limit, explain filing in Workday under Expenses within 30 days | Quote the rule; for requests and balances point to Workday > Absence > Request Absence or Absence Balance |
| Escalate when | Category not in the table, employee claims an exception, amount above the limit | Negative balance, medical leave over 3 days (send clearance to a placeholder HR address, never collect diagnoses in chat), parental or FMLA questions, disputed balance or tenure |
| Test questions | Three, at the end of `SKILL.md` | Four, at the end of `SKILL.md` |

`examples/expense-policy-agent-skill/README.md` says it "shows the shape of a skill submission in this hub: the skill itself is a single markdown file, and this README explains how to adapt it." It was written as the reference format for Agent Skill entries, and the PTO skill copies its structure.

## Audit status

Both READMEs end with a "Customizing" section instead of "Before you deploy". The `HubReadmeSectionsRule` alternatives accept "customize for your tenant" but not "Customizing", so each gets an ADVICE finding. Neither folder contains files Arcane Auditor reads, so Arcane skips them.

## Key source files

| File | Purpose |
| --- | --- |
| `examples/expense-policy-agent-skill/SKILL.md` | Expense skill |
| `examples/expense-policy-agent-skill/README.md` | How to adapt it |
| `examples/pto-policy-agent-skill/SKILL.md` | PTO skill |
| `examples/pto-policy-agent-skill/README.md` | How to adapt it |

## Related pages

- [Workday agent skills](workday-agent-skills.md)
- [Hub rules](../systems/audit/hub-rules.md)
