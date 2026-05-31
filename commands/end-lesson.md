End the current lesson. Synthesize, update state, generate the next brief and reading.

Work inside the active course: read `.teaching/state.json` → `<slug>`, operate on
`courses/<slug>/`. Run every step in order.

## 1. Lesson number

Read `courses/<slug>/progress.json`. The lesson just completed is `current_lesson` = N. Next is N+1.

## 2. Fold any handoff and cold-open

- If `lesson_log/lesson_NN_inprogress.md` exists (from `/pause-session`), read it, fold its
  observations into step 3, then delete it after step 3 is saved.
- If `lesson_log/lesson_NN_coldopen.md` exists (this session's retrieval quiz), read it. This is
  the most honest retention signal you have — it's what survived from the *prior* lesson cold, not
  what looked understood in the moment. Fold it into step 3 and weight it heavily in steps 4–5: a
  concept that looked solid last session but came back **lost** never consolidated. Calibration
  gaps (high-confidence-but-lost) are learner-level data. Keep this file — it's the longitudinal
  record.

## 3. Write raw observations

Create `lesson_log/lesson_NN.md`:
- **Date** — **get the real time from the shell first (`date -u +%Y-%m-%dT%H:%M:%SZ`) and use that;
  never write a guessed date.** Same rule for every timestamp this command writes.
- **What was covered** (concept bullets).
- **What landed** — specific framings/examples/analogies they visibly grasped or built on.
- **What stumbled** — specific confusion points, even if resolved.
- **Failed analogies** — any that didn't connect, one sentence why.
- **Unresolved questions** — anything left open.
- **Candidate callbacks** — concrete moments the next lesson should reference back to.

Be specific. "They grasped joins" is useless. "The wedding-guest-list framing for INNER vs LEFT
landed; they predicted a LEFT JOIN + null-filter result correctly before I asked" is useful.

## 4. Rewrite the two profiles in full (don't append)

Two separate syntheses — keep the split clean, because the learner profile follows them to every
future course and the course profile does not.

**`learner/profile.md`** — durable, about the person. Update only with what *generalizes beyond
this subject*: pacing, what makes concepts click for them, calibration patterns, what to avoid.
Promote a session insight here only if it would still be true for a totally different subject
(e.g. "needs the concrete breaking case before accepting an abstraction"). Sections: How they
learn best / What to avoid / Known background / Current goals / (the analogy & concept sections
stay placeholders here — those are course-level).

**`courses/<slug>/course_profile.md`** — subject-specific. Working analogies (the ones that
worked for *this* material), Failed analogies, Shaky concepts, Mastered concepts. This is where
subject content lives.

If unsure which profile a fact belongs in, ask: "is this about *them* or about *the subject*?"
That's the whole rule.

## 5. Update `progress.json`

`current_lesson` → N+1; append N to `lessons_completed`; increment `session_count`; update
`shaky_concepts` / `mastered_concepts` (honor the cold-open retention signal over in-the-moment
impressions); set `last_updated` (real shell time per the timestamp rule in step 3). **Carry
`schema_version` forward** — keep the existing value; never drop it when rewriting the file.

## 6. Generate `next_lesson_brief.md` for lesson N+1

Overwrite it. Pull from `lesson_plan.md` (structure for N+1), both profiles (delivery + what to
lean on/avoid), recent `lesson_log/` (callbacks), and `progress.json` (what to reinforce).
Sections: What this session is about · Delivery shape · Analogies to lead with (with one-line
refreshers for any callback) · Analogies to avoid · Gotchas to surface · Skip/compress · Watch
for (confusion + landing signals) · Wrap (bridge to N+2) · Exercise notes (what it is, where it
lives).

## 7. Generate `reading/lesson-NN-reading.html` for lesson N+1

A self-contained primer the learner skims before the live session — readable on mobile and
desktop. ≤5 minutes / ≤500 words prose plus 1–3 diagrams.

- All CSS inline in one `<style>`; system font stack; `max-width: 720px; margin: auto; padding:
  16px; box-sizing: border-box`; line-height ≥1.5; dark text on light. Code blocks monospace with
  `overflow-x: auto`.
- For diagrams, use Mermaid via pinned CDN at the end of `<body>`:
  ```html
  <script src="https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.min.js"></script>
  <script>mermaid.initialize({ startOnLoad: true, theme: 'neutral' });</script>
  ```
  Diagrams in `<pre class="mermaid">…</pre>`. Stick to well-tested types (`flowchart`,
  `sequenceDiagram`, `erDiagram`, `classDiagram`); avoid `sankey`/`mindmap`/`xychart`. Quote node
  labels with `["…"]` if they contain special characters. Keep diagrams small (≤6 nodes).
- Scope: a primer, not the lesson. Set up vocabulary and one structural visual. Save the *why*,
  the analogies, and the gotchas for the live session. Do NOT include analogies here.

## 8. Drift check

Compare the upcoming `lesson_plan.md` against what the profiles/progress now suggest. If there's a
significant mismatch (mastered something a future lesson plans to teach; hit a wall needing
remediation), say so and recommend `/replan`. Do NOT edit `lesson_plan.md` here.

## 9. Session summary

One paragraph to chat: what was covered, what landed best, what's next. No praise, no padding.
End with: "Brief and reading generated for lesson N+1. Run `/start-lesson` when ready."
