# my-skills

Nigel Reilly's Claude skills and the project context they support.

## Layout

```
skills/<skill-name>/SKILL.md      One folder per skill (Claude Agent Skills format: YAML front matter + instructions)
projects/<project>/               Portable project context and handover files that skills work against
```

## Skills

| Skill | Purpose | Status |
|---|---|---|
| [proposal-evidence-and-commitment-control](skills/proposal-evidence-and-commitment-control/SKILL.md) | Stops business proposals and AI handovers turning brainstorms or competitor claims into commitments | Proposed, 28 Sep 2026 |
| [jbb-virtual-cfo](skills/jbb-virtual-cfo/SKILL.md) | Virtual CFO for Jewel Bespoke Build: evidence-graded business intelligence report, project margins tender to final account, and 30/60/90-day advice | Draft, 28 Sep 2026; first run completed |
| [ybt-weekly-board-review](skills/ybt-weekly-board-review/SKILL.md) | Weekly YourBoardroomToday flash report for Jewel Bespoke Build, read-only via the JewelBB Portal and Xero | Draft, 28 Sep 2026; not yet run on live data |

## Projects

| Project | File | Version / date | Status |
|---|---|---|---|
| YourBoardroomToday (YBT) | [master-proposal.md](projects/yourboardroomtoday/master-proposal.md) | 2.1, 28 Sep 2026 | Proposal for director scoping review; Jewel pilot confirmed; not approved to build or launch |

### YourBoardroomToday pilot

Jewel Bespoke Build Ltd (https://www.jewelbb.co.uk/) is the first test business (confirmed 28 Sep 2026). Weekly reviews read the JewelBB Portal (`https://mcp.jewelbb.co.uk/api/mcp`) and Xero connectors using [ybt-weekly-board-review](skills/ybt-weekly-board-review/SKILL.md). The schedule is not yet set.

Skills are stored here only, not in the JewelBB Portal's skill store. Jewel's business data is never committed to this repository.

## Adding a skill

1. Create `skills/<kebab-case-name>/SKILL.md` with `name` and `description` front matter.
2. Add a row to the Skills table above.
3. When revising, keep dated entries in the skill's failure log or revision record rather than deleting history.
