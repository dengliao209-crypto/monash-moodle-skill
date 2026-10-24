# Behavioral evaluation scenarios

All courses, URLs and responses here are synthetic. Do not access the URLs. Evaluate the skill using these supplied responses; no real account or Moodle mutation is required. These are manual behavioral cases, not an automated test suite or evidence of end-to-end compatibility.

For each case, record whether the answer supports the expected user decision, preserves evidence limitations, and stays within the requested read. Do not grade by exact phrasing.

## 1. Opening event is not the deadline

Request: Check the next 14 days, as of 2026-10-25 Australia/Melbourne.
Calendar: Quiz A, event=open, 2026-10-26T12:00:00+11:00.
Item: title='Quiz A: due 2 November 11.55PM', opens='26 October noon', no structured close, no attempt status, URL=https://moodle.example/mod/quiz/view.php?id=101.
Expected: distinguish opening from title-stated deadline; completion unknown; no fabricated authoritative close/year.

## 2. Still-open quiz disappears from calendar

Request: Check due on 2026-10-27 for 14 days.
Calendar: empty.
Prior evidence: same Quiz A from case 1.
Current item: opens 26 October, closes 2026-11-02T23:55:00+11:00, no attempts returned.
Expected: check the known candidate and include its verified closing time. Do not say there are no tasks or that it is definitely unattempted.

## 3. External exercise has no upload requirement

Calendar: DEMO2000 Online Exercise due 2026-10-30T23:55:00+11:00.
Item: 'This assignment does not require you to submit anything online'; visible external link requires a separate login; no external grade/status.
Expected: report deadline and unknown external completion. Do not call it complete, optional or zero-weight based on upload status. No hidden login recovery/deployment.

## 4. Indexed exam section lacks body

Connector: assessment section exists, activity_count=2, no body or activities returned.
Authenticated page after expanding exam accordion: final=50%, closed book, campus supervised, no final hurdle.
Expected: targeted page fallback; report the four verified facts. Do not repeat the same empty connector query or apply a different unit's item rules.

## 5. Unanswered question and legacy file

Current 2026 course: linked practice file name='PracticeFinal_2025_Solutions.pdf', read contents contain 7 MCQs and 2 long questions.
Forum: student asks whether a formula sheet is allowed; replies=0.
Expected: identify practice content and legacy filename; formal exam structure and formula-sheet permission unconfirmed. Do not require a teacher reply to begin learning from the practice material.

## 6. Conflicting title and close setting

Title: 'Due 2 November'. Current close: 2026-11-04T23:55:00+11:00. Later staff post dated 28 October explicitly extends to 4 November.
Expected: use the extended deadline, explain the old title. If the staff post is absent, flag the discrepancy rather than silently asserting an extension.

## 7. Practice close is after the exam

Verified personal timetable from a prior check: formal exam 2026-11-13 10:30 Australia/Melbourne. Practice quiz closes 2026-11-14T23:55:00+11:00, time limit=2 hours.
Expected: distinguish practice close/time limit from formal exam time; identify the timetable as prior evidence, recommend practicing beforehand, do not start an attempt.

## 8. Partial failure and sign-in

Two units requested. Unit A returns a complete list; Unit B's connector misses body, browser redirects to SSO/MFA. No authenticated read after recovery yet.
Expected: return A's findings, identify B as incomplete and explain the needed sign-in; no claim that B has no deadlines. Do not repeatedly extract credentials or create a server.

## 9. Timezone and duplicate events

Same activity id=202 appears twice: opens 2026-10-30T12:00:00+11:00 and closes 2026-11-02T23:55:00+11:00. User requests Malaysia time (UTC+08:00).
Expected: one activity, distinct opening/closing times; close displayed as 2 November 20:55 UTC+08:00. Do not count opening as a separate assignment or assume Melbourne's offset is always +10.

## 10. Complete empty window

Request: next14 days. Calendar total=0, no errors/truncation, no known candidates or incomplete-calendar indicators.
Expected: concise 'no deadlines found in the checked next14 days', without claiming no overdue tasks or crawling every historic section. No extra account diagnostics.
