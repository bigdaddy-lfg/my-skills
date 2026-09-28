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

## Projects

| Project | File | Version / date | Status |
|---|---|---|---|
| YourBoardroomToday (YBT) | [master-proposal-v2.0.md](projects/yourboardroomtoday/master-proposal-v2.0.md) | 2.0, 28 Sep 2026 | Proposal for director scoping review; not approved to build or launch |

### YourBoardroomToday pilot

Jewel (https://www.jewelbb.co.uk/) is intended as the first test business, with weekly reviews to be scheduled via connectors. No review skill or schedule exists yet.

Note: the handover instructions in the v2.0 proposal say to "keep this venture separate from Jewel". Using Jewel as the pilot client is a change of direction, so record it as confirmed direction (with its data-handling authority) in the next proposal revision.

## Adding a skill

1. Create `skills/<kebab-case-name>/SKILL.md` with `name` and `description` front matter.
2. Add a row to the Skills table above.
3. When revising, keep dated entries in the skill's failure log or revision record rather than deleting history.
