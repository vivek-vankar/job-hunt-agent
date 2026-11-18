# Application controls

These rules protect the user. Profiles can make them stricter. Nothing in a profile can loosen the never-autonomous list or the truthfulness rule.

## Autonomy levels (set in rules.md)
- **research-only**: discover, research, score, log. Nothing more.
- **draft** (default): also prepare tailored materials via the `tailor` skill. The user submits every application.
- **assisted-apply**: also fill application forms in a browser, only for roles the user approved in the current conversation, pausing before each final submit.

Several job boards prohibit automated applications in their terms, LinkedIn among them. Accounts get restricted for it. That is why the default stops at drafts, and why filling job-board forms needs its own setting, `board_apply: assist`, on top of assisted-apply. Setup explains this once when the user chooses.

## Approval triggers (always queue as Awaiting Approval)
- score in the STRETCH / REVIEW band
- relocation, even when the role is otherwise ideal
- full night shift, or a six-day week
- an employment bond, exit penalty, or non-compete
- references requested at application stage
- a substantial take-home assignment or writing test
- salary expectation required while the listing is undisclosed
- any application question the profile does not answer
- any unusual declaration or commitment
- anything the profile lists under extra approval triggers

## Never autonomous, under any setting
- OTPs, passwords, security answers, 2FA codes
- government ID numbers, tax IDs, passport details, bank or payment details
- payments of any kind
- salary history beyond what the user wrote in the profile
- legal declarations, signatures, or anything legally binding
- consent to background checks
- voluntary demographic or equal-opportunity self-identification (gender, race or ethnicity, disability, veteran status, sexual orientation, caste, religion, age), even when the profile contains the answer. These are always the user's own choice to answer or decline.

When a CAPTCHA, login, OTP or anti-bot screen appears, stop, tell the user what it is, and wait for them to clear it.

## Form-filling stops (assisted-apply, either mode)
Whoever fills the form, the `application-operator` agent in full mode or the lead in lite mode, works only on the employer's own careers site or its applicant tracking system. Observe each step before filling it, the way a careful person would: survey the whole page, read each label with its help text, re-read the page after any answer that could reveal new fields, and read back every value to confirm it landed. Never imitate human behaviour to get past bot detection; when a portal blocks automation, switch to guided fill. Stop at:
- **Gates**: account creation, login, OTP, CAPTCHA, "apply with LinkedIn/Google/Indeed" sign-in, consent, terms, privacy or data-retention boxes, background-check consent, declarations and "I certify" boxes, e-signatures, payment, and the final submit.
- **Job-board-hosted apply flows** (LinkedIn Easy Apply, Indeed Apply, Naukri and similar): under `board_apply: guided` (the default) the user fills them with the lead guiding through `references/guided-fill.md`. Under `board_apply: assist` the user opens the flow, the agent fills it under these same stops, and the user sends. A profile without a `board_apply` line keeps the behaviour it was set up with: `assist` when its autonomy is `assisted-apply` (as in v1.0.0), `guided` otherwise. Mention the new setting once.
- **Self-ID questions**: left blank for the user, always.
- **Free-text questions**: drafted with `tailor`, approved by the user, pasted exactly as approved.
- **Assessments and video interviews**: the user's to do.
- **Resume parsing**: every pre-filled value is checked against the profile and corrected, and each correction is reported.
- **The review screen**: every field shown to the user before they submit.

## Guided fill
Any time the agent can't or shouldn't fill a form, the user fills it and the lead guides: a fill card with every value ready to paste, help with each field as it comes up, and a line-by-line check of the review screen before submit. See `references/guided-fill.md`. Guided fill works under every autonomy level above research-only and on every platform, including ones with no browser tool.

## Truthfulness
Application material may select, order, rephrase and emphasise confirmed facts from `profile.md`. It may never add a qualification, metric, tool, client, employer, title, budget, team size, result or date the profile does not contain. If a form asks for something the profile lacks, ask the user. Their answer goes into the profile only after they confirm it.
