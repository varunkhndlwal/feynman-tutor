Begin a teaching session. Handles first-time onboarding, then runs the lesson.

## 0. First-time user? Onboard.

Check for `learner/profile.md` in the working directory.

**If it does NOT exist, this is a first-time user. Onboard before any teaching:**

1. Welcome them in one or two lines — no wall of text. Explain what's about to happen: you'll
   learn how they learn, then build them a course on whatever they want to study.
2. **Build the learner profile** — run the `/build-profile` flow. This produces
   `learner/profile.md` (how this person learns — durable across every future course).
3. **Build the first course** — run the `/new-course` flow. Ask what they want to learn,
   generate a `lesson_plan.md` for it, and scaffold `courses/<slug>/`.
4. `/new-course` sets `.teaching/state.json` → `active_course`. Confirm it's set.
5. Then fall through to step 1 below and start lesson 1.

**If `learner/profile.md` exists, skip onboarding** and go straight to step 1.

## 1. Load state

Read `.teaching/state.json` for the active course slug, then load from `courses/<slug>/`:
- `lesson_plan.md`, `next_lesson_brief.md`, `course_profile.md`, `progress.json`
- and `learner/profile.md`.

Confirm `progress.json.current_lesson` matches what `next_lesson_brief.md` is about. If
they're out of sync, surface that before starting.

## 2. Resume a paused session?

Check `courses/<slug>/lesson_log/lesson_NN_inprogress.md` (NN = `current_lesson`, zero-padded).

- **If it exists**: load it, briefly orient the learner ("picking up from where we paused —
  we'd covered X and Y, still need Z"), then open with its "Resume hook." The handoff
  supersedes the brief's opener. Skip the cold open (step 3) — they're already mid-lesson.
- **If it doesn't**: open with the hook in `next_lesson_brief.md`. Don't recap the whole
  curriculum. Don't list what you're about to teach. Just start.

## 3. Cold open: retrieval + calibration

Run this *before* new material — unless it's lesson 1 (nothing to recall) or you just loaded
a paused handoff.

a. Gather ~4 targets max (keep it under ~3 min): `shaky_concepts` from `progress.json`, 1–2
   recently mastered concepts to spot-check for decay, and the prior lesson log's candidate
   callbacks.

b. For each: ask for a fast gut confidence rating (1–5), *then* pose one no-notes retrieval
   prompt — predict-before-reveal applies. Say it's diagnostic, not a test.

c. Score each **clean / partial / lost**, and note calibration gaps (high confidence + lost,
   or low confidence + clean, are both signal).

d. **Steer live.** Lost → fold 2–3 min of remediation in *now*, before new material, and say
   you're doing it. All clean → compress the planned reinforcement and move faster. State the
   adjustment in one line.

e. **Write the feedback artifact** `courses/<slug>/lesson_log/lesson_NN_coldopen.md`: targets,
   confidence ratings, clean/partial/lost, calibration notes, the live adjustment. Then update
   `progress.json` by retention (promote a recalled shaky concept; demote a lost "mastered"
   one). Do NOT rewrite the profiles — `/end-lesson` folds this log into them.

## 4. Teach

Follow the brief's delivery shape. Use the analogies it recommends; avoid the ones it flags.
Apply `learner/profile.md` over the engine defaults. At natural breaks, check tracking with
*specific* prompts ("describe an index in one sentence"), never generic ("any questions?").

## 5. Close

When the lesson reaches a natural end, tell the learner and suggest `/end-lesson`. Don't
auto-run it. If there's an exercise for this lesson, point them at it and at `/submit-exercise`.
