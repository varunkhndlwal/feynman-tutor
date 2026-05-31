Propose changes to the active course's `lesson_plan.md` based on accumulated data. Do NOT modify
the file without explicit approval.

Work inside the active course (`.teaching/state.json` → `<slug>`).

## 1. Read everything

`courses/<slug>/lesson_plan.md`, `course_profile.md`, `progress.json`, all of `lesson_log/`, and
`learner/profile.md`.

## 2. Identify drift

Look for specific misalignment:
- A future lesson plans to teach something the profiles say is already mastered.
- A completed lesson left a shaky concept the next lesson assumes is solid.
- Pace has been consistently faster or slower than the plan's labels.
- A topic they care about (per `learner/profile.md` goals) is missing or under-weighted.
- A topic the plan emphasizes turned out irrelevant to the real goal.

If no significant drift, say so and stop. Don't suggest cosmetic edits.

## 3. Propose in chat

For each change: **What** (exact lesson/section) · **Why** (cite the signal — quote the profile,
progress, or log line) · **Alternative** (what else could be done). Show, don't tell.

## 4. Wait for explicit approval

"Yes do it" or equivalent. If they want to think or ask, engage but don't modify.

## 5. On approval

- Snapshot: copy `lesson_plan.md` to `courses/<slug>/lesson_plan_history/lesson_plan_<YYYY-MM-DD>.md`.
- Apply the approved changes to `lesson_plan.md`.
- Regenerate `next_lesson_brief.md` if the upcoming lesson changed.
- Confirm what changed.

## Restraint

Replanning is rare. If you'd run this more than once every three or four lessons, the original
plan was probably wrong — say so and propose a structural rewrite, not another patch.
