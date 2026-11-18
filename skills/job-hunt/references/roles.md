# Roles, modes and handoffs

## Modes (set as `mode` in rules.md)

- **full**: the lead hands company research and form filling to the plugin's agents. Research runs in parallel, so batches finish faster and the lead's context stays clean for scoring.
- **lite**: the lead does everything in one thread, as in v1.0.0, with `batch_size` capped at 5. For free or low-limit plans, standalone skill uploads, and platforms that cannot run plugin agents.

A profile from before v1.1.0 has no `mode` line. Run it as `lite`, which is exactly how v1.0.0 worked, and keep its `batch_size` as written rather than capping it at 5. Mention once that full mode now exists and what it adds, and switch only if the user says so.

**Fallback.** If the first agent call in a run fails because the platform cannot start the agent (the agent type is unknown, or agents are not supported here), switch the rest of the run to lite, say so in one line, and carry on. One failed research result is different: rerun that company once in the lead, then move on.

## Roles

| Role | Model | Does | Never does |
|---|---|---|---|
| **Lead** (the `job-hunt` skill) | Whatever runs the session; the strongest available is best | Loads the profile, discovers and filters, scores, picks variants, writes the tracker, runs tailor and resume-lab, writes ATS playbooks and memory, talks to the user | Bulk browser clicking in full mode |
| **company-researcher** agent | Sonnet | Researches one employer, returns a company card with suggested s_company and s_reviews | Scores anything else, writes files, contacts anyone |
| **application-operator** agent | Sonnet | Fills one approved application on the employer's site or ATS (or, under `board_apply: assist`, a job-board apply flow the user opened), explores unknown portals, reports gates, questions and the review screen | Gates, self-ID questions, writing answers, submit, job-board-hosted apply flows unless `board_apply: assist` |
| **tracker.py** | Code | Adds and updates tracker rows, totals, duplicate checks | Choosing scores |

The lead is the only writer to the tracker, `profile.md`, `memory.md` and the ATS playbooks. Agents return text; the lead decides what gets saved. That keeps parallel work from colliding and keeps every saved fact checked.

Model choice lives in each agent file's `model:` line. Users on large plans can raise it; the shipped defaults suit most paid plans.

## Handoff formats

**Lead → company-researcher** (one per company, all sent at once)
```
COMPANY: [name, website or careers URL]
ROLE: [title, function, location]
REGION PACK: [review sites and forums from references/regions/<region>.md]
METHOD: [paste of references/review-research.md]
WEIGHTS: company [n], reviews [n]
RUBRIC: [Company quality and Employee-review signals sections of references/scoring.md]
```

**Lead → application-operator**
```
ROLE: [tracker ID, company, title, apply URL]
APPROVED: yes, by the user in this conversation, on [date]
BOARD_APPLY: [guided | assist, from rules.md]
FIELD SHEET: [field: value (source)]
FILES: [approved resume and cover letter paths]
PLAYBOOK: [paste of ats-playbooks/<ats>.md, or "none: explore mode"]
TAB: [tab id, or "create"]
```

The field sheet holds only confirmed facts from `profile.md`, approved draft text, and answers the user gave in this conversation. It never holds passwords, OTPs, ID numbers, bank details, or self-ID answers.

## Handling the operator's reports

- **GATE**: tell the user in plain words what to do, wait, then resume the operator.
- **GATE: BOARD-HOSTED** (only under `board_apply: guided`) or **BLOCKED**: switch to guided fill (`references/guided-fill.md`). Build the fill card, give it to the user with the link, and stay with them through the form and the review check. For BLOCKED, tell them which fields the operator already filled so they can check those first.
- **SELF-ID**: tell the user these questions are waiting and are theirs to answer or decline.
- **QUESTION** on free text: draft with `tailor`, following the Voice notes in `memory.md`, show the user, and send back only the approved wording. Add a confirmed new answer to `profile.md` under Standard answers and note the question in `memory.md`.
- **ASSESSMENT**: hand to the user; note the deadline in the tracker.
- **ERROR** after three tries: look at the page yourself and give a new plan, or ask the user with a plain question.
- **READY FOR REVIEW**: show the user every field the operator listed, side by side with the approved drafts. The user checks and presses submit. Only after the user says it went through: set status `Applied`, update `memory.md`, update the playbook.

## Escalation ladder
1. The operator retries a failed step twice with a different approach.
2. Third failure: the lead looks at the page and gives a new plan.
3. Still stuck: the user gets a screenshot and one plain question.
4. Portal blocking automation: switch to guided fill. Portal down: tell the user and set a next action to retry.
