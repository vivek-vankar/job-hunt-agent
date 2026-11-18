# Changelog

## [1.1.0] — Agents, candidate memory and guided fill

**Nothing changes for existing profiles.** A v1.0.0 profile has no `mode` or `board_apply` line, so it runs exactly as before: single thread, its own batch size, and assisted-apply filling forms wherever it used to. The run mentions the new settings once; they turn on only when the user adds them.

**Agents**
- New `company-researcher` agent (Sonnet, web only). The lead sends one per shortlisted company in parallel and gets back a company card: identity check, evidence level, red flags with counts and dates, scam check, same-function reviews, pay signals and suggested s_company / s_reviews.
- New `application-operator` agent (Sonnet, browser). Fills one approved application on an employer's careers site or ATS from a field sheet. Stops at every gate, leaves self-ID questions blank, sends every free-text question back for an approved answer, checks resume-parser pre-fills, hands job-board-hosted apply flows to the user, and never presses submit.
- The operator observes before it fills: surveys the whole step, reads labels with their help text, re-reads the page when an answer reveals new fields, reads back every value to confirm it landed, and checks for validation errors before moving on. It never imitates human behaviour to get past bot detection.
- `board_apply: guided | assist` in `rules.md`. `guided` (default): the agent fills employer sites and ATS forms and guides the user through job-board forms. `assist`: the user opens the job-board apply flow, the agent fills it, and the user sends, as v1.0.0's assisted-apply allowed. Setup states the account risk once when asking.
- Guided fill (`skills/job-hunt/references/guided-fill.md`): when the agent can't fill a form (LinkedIn Easy Apply, Naukri, a portal that blocks automation, no browser tool), the user fills it with the lead beside them: a fill card with every value ready to paste in page order, help on each field as it appears, screenshot checks, and a line-by-line review check before submit. Works in both modes and under `draft` as well. The operator reports BLOCKED and hands over the fields it already filled.
- ATS playbooks: the lead records each portal's route in the workspace's `ats-playbooks/`, so the next application on Workday, Greenhouse or Darwinbox goes faster. Format in `skills/job-hunt/references/ats-playbooks.md`. Playbooks hold no personal data.
- `skills/job-hunt/references/roles.md`: roles, handoff formats, report handling and escalation.

**Modes**
- `mode: full | lite` in `rules.md`. Full uses the agents. Lite runs in one thread with batches capped at 5, for free or low-limit plans and skills-only hosts. Setup asks; runs fall back to lite automatically when agents can't start. Profiles without a `mode` line keep working.

**Memory and voice**
- New per-profile `memory.md`: voice notes from the user's own confirmed writing, edits the user made to drafts, company history, screening questions seen. Setup gains an optional Round 7 to learn voice. `tailor` and `follow-up` write in that voice; `job-hunt` and `follow-up` keep company history current. Memory never adds a claim; `profile.md` stays the ceiling.
- Form-filling stops for both modes now live in `references/application-controls.md`.

## [1.0.0] — First public release

The first publicly versioned release of Job Hunt Agent. This release packages the existing open-source agent capabilities under a consistent 1.0.0 version across ChatGPT, Codex and Claude plugin manifests. It does not introduce a new web UI or hosted service.

### Included
- **Setup:** guided onboarding, candidate verification, reusable profile and job-search rules.
- **Job Hunt:** discovery guidance, deduplication, hard filters, job/company research, employee-review evidence, transparent scoring and ranked tracking.
- **Resume Lab:** resume health audits, transparent **Estimated ATS Compatibility**, JD matching, truthful resume creation and improvement passes.
- **Tailor:** role-specific resume drafts, cover letters, recruiter messages and application answers grounded in the confirmed profile.
- **Follow-up:** application tracking, recruiter response follow-up, interview preparation and next actions.
- **Regional guidance:** India, UK, US, Germany and a global fallback.
- **Cross-platform packaging:** Agent Plugins 1.0, Codex, Claude Code/Cowork and a script for standalone skills.
- **Privacy and safety:** user workspaces kept separate from public source; assisted-apply controls, human verification and no fabricated claims.

### Known limitations
- There is no built-in hosted UI, job-feed backend or automatic cloud synchronization.
- Job discovery, browser form filling and review-site access depend on the AI host, available integrations and the policies of third-party sites.
- ATS compatibility is a transparent estimate, **not** a score returned by an employer's applicant-tracking system.
- Job submissions are not guaranteed and must be confirmed before the tracker is marked applied.

### Upgrade note
The public release starts at **1.0.0**. Earlier internal prototypes are not published as separate public releases.
