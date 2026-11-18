---
profile: {{SLUG}}
regions: [{{REGION}}]
currency: {{CURRENCY}}
salary_period: {{monthly|annual}}
salary_basis: {{gross|in-hand}}
comp_floor: {{NUMBER}}
comp_target: {{NUMBER}}
comp_stretch: {{NUMBER}}
tracker: tracker.xlsx
autonomy: draft            # research-only | draft | assisted-apply
board_apply: guided        # guided (user fills job-board forms with a fill card) | assist (agent fills them too; account risk)
mode: full                 # full (agents, parallel research) | lite (single thread, for free or low-limit plans)
batch_size: 15             # lite mode caps this at 5
max_posting_age_days: 30
weights: {role: 25, skills: 15, growth: 15, comp: 15, company: 10, reviews: 10, worklife: 5, location: 5}
thresholds: {priority: 80, apply: 70, review: 60}
---

# Search rules

## Direction
- Primary direction:
- Stretch direction:

## Target roles
### Priority
-
### Stretch
-
### Low priority (surface only when pay, company or upside is unusually strong)
-

## Locations, in order of preference
1.
2.
- Remote: {{yes / India-only / worldwide / no}}
- Relocation: ask every time

## Work conditions
- Preferred schedule:
- Accepts night or rotational shifts: {{yes/no}}
- Six-day weeks: {{flag / hard filter}}

## Experience range
- Normal target: {{X–Y}} years asked
- Stretch when core duties match: up to {{Z}} years asked
- Skip above: {{Z}} years unless duties are clearly junior

## Hard filters (any one fails → SKIP)
- Pay below comp_floor
-

## Extra approval triggers
-

## Positioning guardrails
<!-- Things the agent must never imply about this person, even indirectly. -->
-

## Custom criteria
-

## Excluded companies
-
