---
name: setup
description: "Onboard a job seeker into job-hunt-agent, or add, edit or switch profiles. Interviews the user, extracts facts from their resume and has them confirm each one, then builds a local workspace with profile.md, rules.md, resume variants and a tracker. Use this whenever someone wants to start a job search with the plugin, says \"set me up\", \"register\", \"create a profile\", \"new profile\", \"switch profile\", \"update my rules\", \"change my salary floor\", or when the job-hunt skill finds no workspace. Use it before any first batch, even if the user just says \"find me jobs\"."
---

# Setup — onboarding and profile management

Setup is the plugin's onboarding layer. There are no required external accounts or servers. The goal is to create a durable candidate workspace containing only user-confirmed facts, search rules, resume variants, Resume Lab reports, applications, runs, and a tracker.

The single most important output is `profile.md`, the list of facts the user has confirmed about themselves. Every later skill treats it as the ceiling on what can be claimed. Build it carefully and keep the conversation warm and quick.

## Where the workspace lives

This skill runs on any agent platform. Use the most durable storage the current environment offers:

- **A persistent working folder** (Claude Code, Cowork, ChatGPT Work/projects, Codex, or any agent with file access): create `job-hunt/` there. Suggest a synced folder (Dropbox, iCloud, OneDrive, Google Drive desktop) so the workspace survives a lost laptop.
- **Connected Google Drive / Google Sheets**: when the user wants cloud persistence, store candidate files in a dedicated Drive folder and use a Google Sheet as the tracker. Never require Drive if the user prefers local files.
- **A chat with no file persistence** (for example claude.ai chat or plain ChatGPT): build the workspace, zip it, and hand it to the user to download. Tell them to upload the zip at the start of the next session.

All paths in this plugin are relative to the workspace root, the `job-hunt/` folder. Run commands from there.

Recommended logical structure:
```
job-hunt/
├── WORKSPACE.md
├── ats-playbooks/      # learned routes through application portals, no personal data
└── profiles/<slug>/
    ├── profile.md
    ├── rules.md
    ├── memory.md      # voice, company history, application history
    ├── resumes/
    ├── resume-lab/
    ├── applications/
    ├── runs/
    └── tracker.xlsx   # or a connected Google Sheet
```

Templates are in `assets/templates/` inside this skill. Replace every `{{...}}` placeholder. Delete sections that do not apply instead of leaving them empty.

Slugs are short and lowercase: `alex`, `alex-academia`, `maria-uk-nursing`. One person can hold several profiles when they are running different searches with different rules.

## The interview

Ask in small groups, three or four questions at a time, and adapt to the person. A nurse and a software engineer need different follow-ups. Skip anything already answered by an uploaded resume.

**Round 1: the resume.** Ask the user to upload their current resume (PDF, DOCX or pasted text). Read it, then show back a compact list of extracted facts: roles with dates, scope, metrics, tools, education, certifications, languages. Ask them to correct anything wrong and confirm the rest. Only confirmed items go into `profile.md`. Watch especially for numbers. A resume that says "grew traffic 300%" goes in only if the user confirms it is accurate.

If they have no resume, build the same list by asking about each role.

**Round 2: direction.**
- What kind of role do you want next? What would be a stretch?
- Anything you are trying to move away from?
- Which titles would you accept, and which titles would you reject even with good pay?

Turn the answers into priority, stretch and low-priority role lists. Suggest common title synonyms for their field and let them prune.

**Round 3: money and place.**
- Country or countries to search (this sets the region packs and currency).
- How do you think about salary: monthly or annual, gross or take-home? What is the lowest you would accept, what is your real target, and what would be a great outcome?
- Locations in order of preference, remote stance, relocation stance.
- Work authorisation: where can you work without sponsorship?

**Round 4: conditions and dealbreakers.**
- Schedule and shift preferences, six-day weeks, travel.
- Absolute dealbreakers (these become hard filters).
- Things that should always come back to you for a decision (extra approval triggers).
- Companies to exclude, including a current employer.

**Round 5: guardrails and standard answers.**
- Anything the agent must never imply about you? Suggest likely ones from what you have heard. For example, someone with writing experience and no ad-buying experience should never be positioned as a paid-ads specialist.
- Notice period, reason for leaving, how you want an employment gap described in your own words, willingness to relocate.

**Round 6: resume variants and scoring.**
- If one resume serves every target, one variant is fine. If their targets split (for example writing roles and strategy roles), propose two or three variant names with a one-line emphasis each, and save the uploaded resume as the first. They can add tailored files later, or the `tailor` skill can draft them.
- Show the default scoring weights in one line and ask if any matter more or less to them. Typical changes: academics weight Growth up and Comp down; parents weight Work-Life up. Weights must sum to 100, so rebalance for them.
- Autonomy: explain the three levels in two sentences each and recommend `draft`. If they choose `assisted-apply`, tell them once that several job boards, LinkedIn among them, prohibit automated applications and can restrict accounts. Then ask about `board_apply`: with `guided` (recommended) the agent fills employer careers sites and ATS forms, and walks them through job-board forms like LinkedIn Easy Apply with a fill card; with `assist` the agent fills job-board forms too. Say plainly that `assist` is the setting that carries the account risk, and that the risk falls on their own account. Record their choice.
- Mode: ask what plan they run on. `full` sends company research and form filling to the plugin's agents in parallel, which is faster and keeps long batches sharp but uses more of the plan's usage limits. `lite` runs everything in one thread with batches of at most five roles, which suits free or low-limit plans. Recommend `full` for paid plans and `lite` for free ones. If the platform cannot run plugin agents at all (standalone skill uploads, ChatGPT), set `lite` without asking.

**Round 7: voice (optional, skip if they are in a hurry).** Ask for one to three things they wrote themselves without AI help: an old cover letter, a LinkedIn post, an email to a recruiter. Read them and note how they write: sentence length, formality, words they favour, words they never use, how they open and close. Show the notes back and ask them to confirm or correct. Confirmed notes go into `memory.md` under Voice. This is what lets drafts sound like the person who will sign them.

## Write the files

1. Fill `profile.md`, `rules.md` and `memory.md` from the templates. Create an empty `ats-playbooks/` folder at the workspace root.
2. Copy uploaded resumes into `resumes/` with clear names. Create `resume-lab/` for durable audits and JD-match reports.
3. Create the tracker with the profile's weights, running from the workspace root:
   ```bash
   python <tracker.py> init profiles/<slug>/tracker.xlsx --weights '{"role":25,"skills":15,"growth":15,"comp":15,"company":10,"reviews":10,"worklife":5,"location":5}'
   ```
   `<tracker.py>` is `<this-skill-dir>/../job-hunt/scripts/tracker.py` when the plugin is installed whole, or `<this-skill-dir>/scripts/tracker.py` when this skill was uploaded on its own. It needs `openpyxl`; install it if the import fails. If the user chose a Google Sheet, build the same columns and dropdowns there instead.
4. Write or update `WORKSPACE.md`, setting the active profile.
5. Show the user a short summary: target roles, comp bands, locations, hard filters, weights, autonomy, mode. Ask them to confirm. Then offer to run a baseline resume audit with `resume-lab` and/or a first batch with `job-hunt`.

## Editing an existing profile

Read the current files first. Change only what the user asked. When weights change, the tracker's score columns no longer match, so create a new tracker (`tracker-v2.xlsx`), set `tracker: tracker-v2.xlsx` in `rules.md`, and keep the old one as history. When the user adds a fact ("I got certified in X last month"), confirm the details and add it with today's date as `last_confirmed`.

Switching profiles only changes `active_profile` in `WORKSPACE.md`.

## Privacy

Everything stays in the workspace. Never write profile content into the plugin's own folder, and remind users who cloned the repo that `job-hunt/` is gitignored for a reason.
