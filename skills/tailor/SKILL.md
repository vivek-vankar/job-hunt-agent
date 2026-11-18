---
name: tailor
description: "Draft strictly truthful, role-specific application material for a job seeker with a job-hunt-agent profile: tailored resume versions, cover letters, recruiter messages and answers to application-form questions. Use this whenever the user asks to tailor, adapt or rewrite their resume or CV for a role, write a cover letter, answer \"why do you want to work here\", prepare an application, or after the job-hunt skill shortlists roles under the draft autonomy level."
---

# Tailor — truthful application material

Tailoring means choosing which true things to say first and saying them in the employer's language. It never means adding things that are not true.

## Relationship with Resume Lab

When a JD is available and `resume-lab` is installed, prefer running its JD Match before major resume tailoring. Use its safe-keyword and gap analysis as guidance, but keep this skill's truth rule authoritative. After tailoring, Resume Lab may run a final compatibility/truth audit.

## Inputs
1. `profiles/<slug>/profile.md`: the confirmed facts. This is the ceiling.
2. `profiles/<slug>/rules.md`: positioning guardrails and standard answers.
3. The resume variant named in the tracker row, from `resumes/`.
4. The full job description. If you only have a title, fetch the JD or ask for it.
5. The tracker row's review summary, for anything worth addressing (a company known for valuing a skill the user has).
6. `profiles/<slug>/memory.md` if it exists: the Voice notes, the Edits the user made to past drafts, and Company history for this employer.

## The truth rule

You may select, reorder, merge, compress and rephrase confirmed facts, and mirror the JD's vocabulary when it honestly describes what the user did.

You may never add: a skill, tool, metric, percentage, budget, team size, client, employer, title, date, degree, certification or result that `profile.md` does not contain. A plausible-sounding number is still a fabrication. When the JD asks for something the user lacks, either leave it out or name the nearest true thing ("worked alongside the paid team on campaign copy" when that is confirmed), and list the gap for the user.

Before you return any draft, run this check line by line: point to the profile line that supports each claim. Any line without support gets cut or turned into a question for the user.

## Resume tailoring
- Keep the layout of the variant file. Change content, not design, unless asked.
- Lead with the two or three confirmed achievements closest to the JD's top responsibilities.
- Use the JD's terms for skills the user genuinely has (applicant tracking systems match keywords).
- Cut or shrink bullets that do nothing for this role.
- Keep dates, titles and employers exactly as in the profile.
- Save as `applications/<ID>-<company>-<role>/resume.<ext>` matching the source format. Read the docx or pdf skill first when producing those formats.

## Writing in the user's voice
Drafts go out under the user's name, so they should read as the user wrote them on a good day. Follow the Voice notes in `memory.md`, and where they disagree, follow the Edits the user made to past drafts, since those are newer and come straight from the user. Without voice notes, write plainly and directly and offer once to learn their style (setup Round 7).

Voice shapes wording only. It never adds a claim, and the truth rule below still decides what can be said.

## Cover letter
Under 300 words unless the listing asks for more. Three movements: why this role at this company (specific, drawn from research), the two most relevant proofs from the profile, and a plain close. Match the user's register: no clichés like "I am writing to express my keen interest". Save as `cover-letter.md` (or the format the user prefers).

## Form answers
Answer from the profile's standard answers first. For questions the profile cannot answer, draft an answer clearly marked DRAFT, ask the user, and add the confirmed version to `profile.md` under Standard answers so it is never asked twice.

Never fill in on the user's behalf: salary history beyond what the profile states, legal declarations, ID numbers, references' contact details, consent to checks, or voluntary demographic / equal-opportunity questions (gender, race or ethnicity, disability, veteran status, caste, religion, age). Leave those for the user every time, even when the profile happens to contain the answer.

## Hand back
For each application, tell the user:
- what you emphasised and why, in two lines
- gaps against the JD the user may want to address in interview
- any form questions waiting on them

When the user rewrites part of a draft, note in `memory.md` under Edits the user made to drafts what changed and what it shows ("cut every adjective in the opening", "prefers 'built' to 'spearheaded'"), one line each. Ask before saving if the lesson isn't obvious.

Then update the tracker row, running from the workspace root:
```bash
python <tracker.py> update profiles/<slug>/<tracker> <ID> status=Drafted next_action="Review drafts and submit"
```
`<tracker.py>` is `<this-skill-dir>/../job-hunt/scripts/tracker.py` when the plugin is installed whole, or `<this-skill-dir>/scripts/tracker.py` when this skill was uploaded on its own.
