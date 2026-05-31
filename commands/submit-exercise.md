Grade the learner's self-check exercise for the current lesson.

Subject-agnostic: there's no execution sandbox shipped. The exercise is a problem plus a rubric,
both generated for the lesson; grading is reasoning-based against that rubric. If the subject has
an external runtime (code, SQL, math), the learner runs it themselves and includes the output —
the grader assesses correctness from the output and reasoning, it does not execute anything.

Work inside the active course (`.teaching/state.json` → `<slug>`).

## 1. Find the exercise

The most recent completed lesson is the last item in `progress.json.lessons_completed` (= NN).
The exercise lives in `courses/<slug>/exercises/lesson_NN/`:
- `prompt.md` — the task and the rubric.
- the learner's `submission.*` (their answer — text, code, a schema sketch, an explanation).

If the lesson had no exercise, say so and stop. If there's no submission, tell them to attempt it
first. Stop.

## 2. Generate the exercise on demand if missing

If `exercises/lesson_NN/prompt.md` doesn't exist yet (lessons generate exercises lazily), create
it now from the lesson plan and the lesson log: a single focused task that forces the lesson's
core skill, plus a rubric. Use the rubric type that fits:
- `concept_explanation` — explain the idea from first principles (Feynman test).
- `applied_design` — design/produce something using the concept (a schema, an algorithm, a plan).
- `analysis_review` — find what's wrong in a flawed artifact you provide.
- `produce_and_run` — write something the learner executes externally and pastes the output of.

Then stop and tell them to attempt it before grading.

## 3. Grade via the grader subagent

Invoke the `grader` subagent with: the `prompt.md` (task + rubric + rubric type), the submission,
and any pasted output. The grader returns a structured report. It is cold-blooded by design.

## 4. Record

Append the grader's report verbatim under a `## Exercise grade` heading in
`lesson_log/lesson_NN.md`. Update `progress.json.exercises`:

```json
"lesson_NN": { "score": "X/Y", "verdict": "Pass | Pass with notes | Redo recommended",
               "weak_points": ["..."], "graded_at": "<ISO>" }
```

Add any `weak_points` to `shaky_concepts` (deduplicated).

## 5. Print verbatim

Print the grader's report to chat exactly — do not soften, summarize, or editorialize. If the
learner pushes back, engage the *substance* in tutor persona, but the recorded grade stands; the
conversation around it is separate.
