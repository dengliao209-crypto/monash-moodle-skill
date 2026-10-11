# Course queries

Use the selected working CLI, MCP or browser path. CLI examples assume the Monash site has already been selected. These workflows also apply to equivalent discovered MCP tools.

## Deadlines

For an unspecified “检查 due”, use the next 14 days and state that window:

```bash
MOODLE_BASE_URL=https://learning.monash.edu moodle due --days 14 --json
```

Use the requested unit or horizon when supplied. Inspect completeness and errors before summarizing. If a list limit is reached, use a larger supported `--limit` or query each relevant unit. Do not invent an `--offset` flag for `due`.

Report unit, assessment name, absolute deadline with timezone, available submission status and the activity link. Fetch item details only when needed to distinguish an opening date, due date, cut-off date or submission state. Do not infer “not submitted” from inclusion in a due list; do not describe an item as “submitted” from a completion checkbox alone.

If no items are returned, say “在本次查询的未来 14 天内未查到截止项目”, not “没有作业”. For a full assessment schedule or a named task absent from due, inspect course assessment sections and announcements as well. A future-window query does not establish that there are no overdue tasks.

## Grades and mid-semester results

```bash
moodle grades UNIT --json
```

Match the requested assessment by name and context. If absent, inspect ungraded items with the installed command's supported `--include-ungraded` or `--mode all`. Grade detail results can be paginated; use `--limit` and `--offset` where supported. Read feedback only if needed or requested using `--include-feedback`.

Keep score, maximum score, percentage, weighting and course total distinct. Calculate a percentage only when both score and maximum are numeric and meaningful. A hidden/missing grade is “not visible/released in the checked source”, not a fail or a zero. Link to the actual grade or assessment page when available; do not invent an activity URL from an unrelated numeric ID.

## Exam arrangements and requirements

Resolve the unit and teaching period first. Search incrementally with terms such as `exam`, `final`, `assessment`, `revision` and `hurdle`, selecting terms appropriate to visible course headings.

```bash
moodle find "exam" UNIT --json
moodle UNIT "RESOLVED ITEM" --json
moodle news UNIT --json
moodle forums search "exam" --unit UNIT --json
```

Search results identify sources; read the matched page, staff post or document before claiming a requirement. Prefer current, unit-specific assessment instructions and staff updates. Record the publication/update date where available. An older practice paper or past-year guide is not evidence of this year's rules.

Extract the requested information, usually: format, duration, weighting, hurdle/pass conditions, covered topics, permitted materials/calculator, delivery platform/location and any stated arrival or check-in instructions. Separate assessment duration from the time window in which it can be taken.

If Moodle points to a separate personal exam timetable, follow that authenticated source only through available authorized access. Do not infer the personal exam date, venue, special arrangements or permitted materials from a generic unit guide. Mark missing fields as unverified. For conflicting staff sources, show the conflict and dates; use an explicit later correction when one exists.

## Attendance

Check the relevant current course outline/assessment section and staff announcements. Search selectively for `attendance`, `participation`, `hurdle`, `tutorial`, `workshop`, `lab`, `absence` or visible local equivalents.

Separate these findings:

- Required attendance or a stated minimum attendance percentage.
- Participation marks tied to in-class activities.
- Compulsory practical/lab sessions or hurdle requirements.
- Optional/recommended attendance.

An Attendance activity does not establish a compulsory policy; its absence does not prove attendance is optional. A participation weighting does not itself establish an attendance hurdle. Quote a short relevant policy excerpt and link its source when the distinction matters.

For several units, show one row per unit with the requirement, consequence/exceptions if stated, and source. When evidence is missing, say “所查课程资料未明确说明” and identify the checked sources. Do not apply one unit's policy to others or import a university-wide rule without verifying its applicability.
