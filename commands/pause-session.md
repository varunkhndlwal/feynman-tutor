Pause the current lesson mid-session so it can resume cleanly later.

Use this BEFORE ending a session or running `/clear` when the lesson isn't finished. Without it,
the next `/start-lesson` reopens with the brief's hook and loses the thread.

Work inside the active course (`.teaching/state.json` → `<slug>`).

## 1. Identify the lesson

Read `courses/<slug>/progress.json` → `current_lesson` = N. Use NN zero-padded for the filename.

## 2. Write the handoff

Create (or overwrite) `courses/<slug>/lesson_log/lesson_NN_inprogress.md`:

- **Paused at** — ISO timestamp. **Get the real time from the shell first
  (`date -u +%Y-%m-%dT%H:%M:%SZ`) and use that; never write a guessed date.**
- **Covered so far** — concepts already taught, across this and any prior paused sessions for the
  same lesson.
- **Still pending** — items from `next_lesson_brief.md` not yet covered.
- **Active threads** — any analogy or example mid-flight; what was just said before pausing.
- **Confusion points** — what they stumbled on; anything unresolved.
- **Resume hook** — one sentence: exactly how the next session picks back up.

Be specific. The next session loads this blind — every detail left out is a beat the resume misses.

## 3. Confirm

Print: "Paused. Handoff written to `courses/<slug>/lesson_log/lesson_NN_inprogress.md`. Safe to
`/clear`."

Do NOT run any other end-lesson side effects. `progress.json`, the profiles, and
`next_lesson_brief.md` stay untouched.
