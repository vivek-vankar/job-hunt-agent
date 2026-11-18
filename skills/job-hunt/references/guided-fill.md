# Guided fill

When the agent can't fill a form itself, the user fills it and the lead stays beside them. This covers job-board-hosted apply flows (LinkedIn Easy Apply, Indeed Apply, Naukri and similar), portals that block automation, any form the user would rather fill by hand, and every application on a platform with no browser tool.

The user does the typing and clicking. The lead does the thinking: what each field wants, which value goes there, in which format, and what to check before submit. Nothing here touches the portal on the user's behalf.

## 1. Before they open the form: the fill card

Build it from the approved drafts, `profile.md`, the ATS playbook if one exists (`ats-playbooks/`), and the Screening questions seen in `memory.md`. Give it to the user in page order when the route is known, or grouped by kind when it isn't.

```
FILL CARD: [company], [role], via [portal]
Have open:      [resume file], [cover letter file], this card
Takes about:    [n] minutes, [n] pages if the playbook knows

Ready to paste (copy exactly):
  Full name:        [value]
  Email:            [value]
  Phone:            [value]   format: [with +91 / without, if known]
  Current title:    [value]
  Notice period:    [value]
  ...

Approved answers:
  "[question as the portal words it, or as expected]"
  → [approved text]   ([n] characters; limit [n] if known)

Yours to decide on the spot (I won't fill these):
  - Salary expectation: your profile says [wording], or leave blank if allowed
  - Self-ID questions: answer or decline, your choice
  - Any consent, declaration or "I certify" box: read it first

Watch for:
  - [playbook quirks: resume parser swaps dates, a city autocomplete picks the wrong one, session times out after 20 minutes, a hidden second page]
```

## 2. While they fill: answer, check, keep the place

- **Stuck on a field?** The user pastes the question or a screenshot. Explain what the field wants in plain words, give the value from the card, and flag the format.
- **A new question appears.** If the profile or an approved answer covers it, give the value. If not, draft it with `tailor` in the user's voice, show it, and let the user edit. Once confirmed, it goes into `profile.md` under Standard answers and the question goes into `memory.md`.
- **Pre-filled values.** Remind them to check what the resume parser filled: dates, titles and employers come out garbled often.
- **Screenshots.** When the user shares a screenshot of a page, read every visible field against the card and point to anything wrong, missing or out of format, by field name.
- **Keep the place.** If they say "next page", move to the next block of the card.

## 3. Before they press submit: the review check

Ask for a screenshot of the review screen, or the text of it, and check it line by line against the card: every value, both files attached and correctly named, nothing required left empty, no answer cut off at a limit. Say plainly "looks right, you can submit" or name exactly what to fix.

## 4. After

When the user says it went through, set status `Applied`, add the Company history line to `memory.md`, and update or start the playbook with anything learned about the route (no personal data).

## Never
- Ask for the user's password, OTP or login so the lead can sign in.
- Suggest tricks for getting past a portal's automation block.
- Write an answer into the card that the user hasn't approved. Unapproved drafts are labelled DRAFT.
