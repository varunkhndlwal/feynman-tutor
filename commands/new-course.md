Start a new course: pick a subject, generate a curriculum, scaffold the course files.

Used during first-time onboarding (by `/start-lesson`) and any time the learner wants to
begin an additional subject later. Requires `learner/profile.md` to already exist — if it
doesn't, run `/build-profile` first.

## 1. Get the subject and shape it

Ask what they want to learn. Then probe just enough to aim the curriculum — don't
interrogate, 2–4 questions:

- **Why / the anchor:** is there a concrete project, decision, or goal driving this? A course
  aimed at a real anchor beats a generic survey.
- **Starting point:** what do they already know that's adjacent? (Cross-reference
  `learner/profile.md`'s background — don't re-ask what it already says.)
- **Depth & end state:** working fluency, deep mastery, or just enough to ship one thing?
- **Constraints:** rough time per session, target number of lessons if they have one.

## 2. Generate the lesson plan

Write a curriculum to `courses/<slug>/lesson_plan.md` (slug = kebab-case of the subject).
Generate it from first principles for *this* learner — not a generic syllabus. Design rules:

- **Order by dependency, not by tradition.** Each lesson should unlock the next. Lead with the
  mental model that makes everything after it cheaper to learn.
- **Anchor to their goal.** If they named a project, bend examples and the capstone toward it.
- **Pace per lesson.** Mark each Fast / Medium / Slow based on how much is likely already known
  (use the profile's background) vs genuinely new.
- **Typically 6–12 lessons.** End with an applied capstone that uses the real anchor.
- **Each lesson entry:** goal, pace, a 3–6 step outline, the gotchas/traps that bite people,
  and whether it carries a self-check exercise.

Match this structure (it's what `/replan` and `/end-lesson` expect):

```
# Lesson Plan — <Subject>

## Overview
| # | Lesson | Pace | Why it matters |
...one row per lesson...

## Lesson 1 — <title>
**Goal:** ...
**Pace:** ...
**Outline:** 1. ... 2. ...
**Gotchas:** ...
**Exercise:** generic self-check / none
...repeat per lesson...

## Cadence
One lesson per session. Note which lessons can be short and which need full time.
```

Show the learner the overview table and get a quick thumbs-up before committing the file.
This is the one structural anchor; worth 30 seconds of confirmation. After approval, only
`/replan` changes it.

## 3. Scaffold the course

Create under `courses/<slug>/`:
- `lesson_plan.md` (from step 2)
- `course_profile.md` — the subject-specific synthesis, seeded empty:

  ```
  # Course Profile — <Subject>

  > Subject-specific. Rewritten by `/end-lesson`. The durable, cross-subject view of the
  > learner lives in `learner/profile.md`.

  ## Working analogies
  _None yet — populated as lessons run._
  ## Failed analogies
  _None yet._
  ## Shaky concepts
  _None yet._
  ## Mastered concepts
  _None yet._
  ```

- `progress.json`:

  ```json
  {
    "schema_version": 1,
    "subject": "<Subject>",
    "current_lesson": 1,
    "lessons_completed": [],
    "session_count": 0,
    "exercises": {},
    "shaky_concepts": [],
    "mastered_concepts": [],
    "last_updated": "<ISO timestamp>",
    "notes": "Course created. Run /start-lesson to begin lesson 1."
  }
  ```

- `next_lesson_brief.md` — a brief for lesson 1 derived from the plan and `learner/profile.md`
  (framing, delivery shape, analogies to lead with, gotchas, what to skip/compress). Keep it
  short for lesson 1.
- empty `lesson_log/`, `reading/`, `exercises/` directories (a `.gitkeep` is fine).

## 4. Set the active course

Write `.teaching/state.json` → `{ "schema_version": 1, "active_course": "<slug>" }`. If switching
between existing courses later, only `active_course` changes.

Confirm: "Course '<Subject>' created and set active. Run `/start-lesson` to begin lesson 1."
