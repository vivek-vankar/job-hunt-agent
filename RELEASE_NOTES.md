# Job Hunt Agent — Release notes

# v1.1.0 — Agents, candidate memory and guided fill

**Release date:** 2026-11-17
**Git tag:** `v1.1.0`
**License:** MIT
**Status:** Feature release. Profiles made under v1.0.0 keep working unchanged.

## Highlights

1. **Two agents (Claude Code and Cowork).** `company-researcher` researches one employer per agent, in parallel, and returns a company card with an evidence level and suggested company and review scores. `application-operator` fills one approved application on an employer's careers site or ATS, stops at every gate, and never presses submit.
2. **Full and lite modes.** `mode: full` uses the agents. `mode: lite` runs in one thread with batches of at most five, for free or low-limit plans and skills-only hosts.
3. **Guided fill.** When the agent can't or shouldn't fill a form (LinkedIn Easy Apply, Naukri, a portal that blocks automation, a host with no browser tool), the user fills it with a fill card and the lead checks each field and the review screen.
4. **`board_apply: guided | assist`.** Job-board-hosted apply flows are guided by default. `assist` lets the agent fill them once the user opens them, at the user's own account risk.
5. **Candidate memory.** A per-profile `memory.md` holds voice notes, edits the user made to drafts, company history and screening questions seen. Drafts follow the user's voice. Memory never adds a claim; `profile.md` is still the limit on what drafts can say.
6. **ATS playbooks.** Each portal's route is saved in `ats-playbooks/`, with no personal data, so the next application on the same ATS goes faster.

## Upgrading from v1.0.0

Nothing changes until you opt in. A profile without a `mode` line runs as lite with its own `batch_size`. A profile without a `board_apply` line keeps its v1.0.0 behaviour: `assist` if its autonomy is `assisted-apply`, `guided` otherwise. The first run mentions the new settings once and offers to create `memory.md`. To opt in, add `mode` and `board_apply` lines to `rules.md`, or run `setup` and ask to update your rules.

## Current limitations

- **The agents run only on Claude Code and Cowork.** ChatGPT, Codex, claude.ai and standalone skill uploads run in lite mode.
- **The application operator needs the Claude in Chrome browser tools.** Without them, applications fall back to guided fill.
- **Guided fill can't see your screen.** The review check before submit works only if you share a screenshot or paste the page text. Without one, the lead can't confirm what the portal will send.
- **Review sites often block automated reading.** Many company cards will show evidence as `Thin` or `None` and score reviews at the neutral midpoint. Low evidence is not a red flag.
- **Playbooks start empty.** The first application on each ATS runs in explore mode and is slower. Playbooks improve with use, and tenant-specific quirks still surprise the agent.
- **Voice depends on samples.** Voice notes come only from writing you confirm in setup Round 7, which is optional. Without samples, drafts are plain and direct rather than in your voice.
- **Full mode uses more of your plan's usage.** Parallel research is faster but costs more. Lite mode is the cheaper choice.
- **`board_apply: assist` carries account risk.** Several job boards, LinkedIn among them, prohibit automated applications. The risk falls on your account.
- **Agents are set to Sonnet.** Model choice is the `model:` line in each agent file; change it there if your plan allows.
- All v1.0.0 limitations below still apply.

## Publication checklist

- [ ] Tag the commit as `v1.1.0`
- [ ] Create a GitHub Release named **Job Hunt Agent v1.1.0 — Agents, candidate memory and guided fill**
- [ ] Confirm the `memory.md` template and any contributed playbooks hold no personal data
- [ ] Rebuild standalone skill zips with `python tools/package_skills.py`

---

# v1.0.0 — First public release

**Release date:** 2026-11-16
**Git tag:** `v1.0.0`
**License:** MIT
**Status:** Initial repository release

## What is Job Hunt Agent?

An open-source, skills-based career agent for candidates in different industries and regions. It helps a user set up a verified profile, find and research jobs, assess fit, audit and tailor resumes truthfully, prepare applications, and track follow-ups.

## Highlights

1. **Candidate Setup** — personal constraints, targets and confirmed experience become reusable workspace files.
2. **Job Discovery and Research** — role filtering, transparent scores, employer-review research and ranked queues.
3. **Resume Lab** — resume audits, JD comparisons, resume drafting and Estimated ATS Compatibility with clear limitations.
4. **Application Assistance** — tailored truthful materials, explicit autonomy rules and human approval at sensitive steps.
5. **Pipeline Tracking** — spreadsheet tracker, follow-up workflow, recruiter replies and interview preparation.
6. **Platform Support** — manifests for ChatGPT, Codex, Claude Code/Cowork, plus a standalone-skill packaging script.

## Getting started

Read [README.md](README.md) for platform-specific installation. Start with `Set me up.` and confirm facts extracted from your resume before searching or tailoring.

For a downloadable standalone skill package, run:

```bash
python tools/package_skills.py
```

The tracker helper requires Python 3 and `openpyxl` (`pip install openpyxl`).

## Current limitations

- **No UI yet:** this release consists of plugin/skill instructions, reference files and a tracker script; a web dashboard is planned.
- **Not an autonomous job-board scraper:** search, live review-site access and form filling depend on the host environment and applicable site rules.
- **No universal ATS score:** the score is an explainable estimate based on the resume and job description.
- **No guaranteed applications or interviews:** verify submission status from the target site before marking applications as sent.
- **No bundled personal data:** user profiles, resumes, credentials and trackers must remain outside the repository.

## Planned next steps

Structured storage/API contracts, optional integrations, product UI, and outcome analytics are planned future work, not part of v1.0.0.

## Publication checklist

- [ ] Upload the repository contents to GitHub
- [ ] Tag the commit as `v1.0.0`
- [ ] Create a GitHub Release named **Job Hunt Agent v1.0.0 — GitHub Release**
- [ ] Confirm no user data, secrets or private workspace files are in the committed history
- [ ] Configure GitHub repository social preview using `assets/social-preview.png` (optional)
