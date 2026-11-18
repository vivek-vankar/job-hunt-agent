---
name: follow-up
description: "Review a job-hunt-agent pipeline and say what needs doing today: follow-ups due, recruiter replies, assessments, interview prep, deadlines and stale applications. Optionally checks connected email and calendar. Use this whenever the user asks \"what should I do next\", \"where am I with my applications\", \"any replies\", \"review my pipeline\", \"prep me for the interview at X\", \"mark X as rejected\", or wants a status update on their job search."
---

# Follow-up — pipeline review and next actions

## 1. Read the pipeline
Use the active profile unless the user names another. Read the live tracker from the workspace (paths are relative to the `job-hunt/` folder) or from the connected Google Sheet. For a local XLSX, use `tracker.py list` and `tracker.py update`: the script is `<this-skill-dir>/../job-hunt/scripts/tracker.py` when the plugin is installed whole, or `<this-skill-dir>/scripts/tracker.py` when this skill was uploaded on its own. Focus first on statuses such as Awaiting Approval, Drafted, Applied, Assessment, Interviewing, and Offer.

## 2. Check connected sources (when available)
If an email connector is connected, search for each company in the active pipeline from the last 30 days: replies, assessment invites, interview scheduling, rejections. If a calendar connector is connected, look for scheduled interviews. Show the user what you found and ask before changing any tracker status based on it. Never send email without the user's explicit go-ahead for that message.

## 3. Decide what is due
- **Awaiting Approval**: list each with its trigger. These need a decision before anything else moves.
- **Drafted** for more than three days: nudge to review and submit, since postings close.
- **Applied**, no reply after 7 to 10 working days: suggest a short follow-up where a contact exists. After about 30 days with no reply, suggest marking Closed.
- **Assessment**: show the deadline. Offer to help prepare. Never complete a graded assessment for the user.
- **Interviewing**: offer prep: company brief from the tracker's research, likely questions from the JD, STAR-style stories built only from confirmed profile facts, and questions for the interviewer drawn from the review red flags worth probing.
- **Offer**: compare against the profile's comp bands and red flags. Give facts and trade-offs. The decision is the user's.

## 4. Report
Open with the two or three actions that matter most today, then the rest grouped by status. Keep it short.

## 5. Update the tracker
Apply only the changes the user confirmed. Update the same live tracker source that was read at the start of the follow-up review. Preserve existing formulas, validation, formatting and application history. When a status changes (reply, assessment, interview, rejection, offer), add one dated line to the company's entry under Company history in `memory.md` if the file exists, so the next application to that employer knows what happened.

## Follow-up message drafts
Write them in the user's voice from the Voice notes in `memory.md` when they exist. Short, specific, polite: the role, the date applied, one line of genuine interest or a relevant update, and an easy ask. Draft it; the user sends it.
