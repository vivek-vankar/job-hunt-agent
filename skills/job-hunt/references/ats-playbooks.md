# ATS playbooks

A playbook maps one applicant tracking system so the next application on it goes faster and breaks less. Playbooks live in the workspace at `ats-playbooks/<ats>.md` (`workday.md`, `greenhouse.md`, `darwinbox.md`), one per platform, with company-specific quirks under Tenants. They never hold personal data, so users may contribute them back to the public repository.

## Format

```
# [ATS name]
Recognise it by: [URL pattern, page footer, logo text]
Last walked: [date]   Runs recorded: [n]
Confidence: high | medium | low

## Account
- Account needed: yes / no / optional ("apply as guest")
- Sign-in options shown: [email, Google, LinkedIn…]  # all are user gates

## Route
1. [page name]: [what's on it] → [action] → [next page]
   GATE: [type]: user does: [plain words]
2. ...

## Field map
| Portal field | Field sheet key | Format and quirks |

## Resume parsing
- [what it pre-fills, what it usually gets wrong]

## Free-text questions seen
- "[question]" | limit [n] | required y/n   # the question only, never an answer

## Errors and fixes
- "[exact message]" → [cause] → [fix]

## Watch for
- [session timeout, multi-tab steps, upload size and type limits, save-and-return behaviour]

## Tenants
### [Company]
- [quirks specific to this company's setup: extra pages, custom questions, assessments]
```

## Rules
- The lead writes and updates playbooks from the operator's reports. The operator only reads them.
- Update after every run, including failed ones. Add the date and what changed.
- When a page differs from the playbook, trust the page, explore, then rewrite that step.
- Mark steps nobody has walked yet as UNWALKED.
- Strip anything personal before saving: names, emails, IDs in URLs, answers, uploaded file names.
