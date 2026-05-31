Show the learner what the system currently believes about them and where they are.

Print to chat:

1. The full contents of `learner/profile.md` (how they learn — durable).
2. For the active course (`.teaching/state.json` → `<slug>`), the full
   `courses/<slug>/course_profile.md` (subject-specific analogies and concepts).
3. A human-readable summary of `courses/<slug>/progress.json`:
   - Subject, current lesson, lessons completed (with exercise scores if any)
   - Shaky concepts, mastered concepts, session count

Do NOT editorialize or suggest changes. Just show.

If the learner wants to correct something they see, they can say so and you update the relevant
file directly — as a tutor action, not a system action. Keep the learner/course split intact:
durable traits go in `learner/profile.md`, subject content in `course_profile.md`.
