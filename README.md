# Feynman Tutor

An adaptive, first-principles tutor that runs inside Claude Code. Pick a subject — it builds you a
course, learns *how you* learn, and keeps tuning its teaching so you learn faster.

It doesn't lecture. It builds understanding from the ground up, makes you predict before it
reveals, names the patterns so they stick, and opens each session by checking what actually
survived from the last one — then adjusts on the spot.

---

## Install

```
/plugin marketplace add varunkhndlwal/feynman-tutor
/plugin install feynman-tutor@tutor-marketplace
```

(Replace `varunkhndlwal/feynman-tutor` with wherever you host this repo.)

Then make a folder to hold your learning — your courses and profile live there, not in the plugin:

```
mkdir my-learning && cd my-learning
claude
```

Run `/start-lesson`. The first time, it onboards you.

---

## First run

`/start-lesson` detects you're new and walks you through onboarding:

1. **It learns how you learn.** Either a short interview, or — if you already use a chat assistant
   that knows you well — it hands you a prompt to paste there, and that model writes your profile
   from everything it remembers about you. (See [the bootstrap prompt](templates/profile_bootstrap_prompt.md).)
2. **You pick a subject.** Anything — databases, Rust, music theory, special relativity.
3. **It generates a curriculum** aimed at *your* goal and starting point, and shows you the lesson
   plan for a thumbs-up.
4. **Lesson 1 begins.**

Returning? `/start-lesson` just resumes where you left off.

---

## How it works

Two kinds of memory, deliberately separated:

- **`learner/profile.md`** — how *you* learn. Pace, what makes ideas click, what to avoid. This is
  durable and carries to every future course.
- **`courses/<subject>/`** — everything about one subject: the lesson plan, what's landed, what's
  shaky, your progress.

After each lesson, the tutor rewrites both from what it observed. Before each lesson, it runs a
no-notes recall check on the shaky bits and steers the session live off the result. That loop —
teach, observe, retain-check, adjust — is the whole point: the tutor gets better at teaching *you*
the longer you go.

Everything it writes lives in your working directory as plain files you own and can read.

---

## Your workspace is yours

The plugin is just the engine. Your learning — profile, courses, progress, every session note —
lives in *your* working directory as plain files. So:

- **Version and publish it.** `git init` your learning folder and push it anywhere. It's your
  data; the plugin never holds it. (On first run the tutor offers to set this up.)
- **Upgrades don't reset you.** When a new engine version ships, run `/plugin update` — the better
  engine picks up right where you were, on your existing profile and history. The more you've
  done, the better it teaches you, and upgrades only add to that.
- **Forward-compatible by design.** State files carry a `schema_version`; a newer engine migrates
  older workspaces forward in place and never drops your data.

The engine improves over time; your accumulated learning carries across every version of it.

---

## Commands

| Command | What it does |
|---|---|
| `/start-lesson` | Onboard (first run) or resume; runs the lesson, including the cold-open recall check. |
| `/end-lesson` | Synthesize the session, update both profiles, generate the next brief + reading. |
| `/new-course` | Pick a subject; generate and scaffold a curriculum for it. |
| `/build-profile` | Seed your learner profile (interview, or a prompt for your own long-memory chat). |
| `/submit-exercise` | Grade your self-check for the current lesson (cold-blooded; separate persona). |
| `/review-profile` | Show what the tutor currently believes about you and where you are. |
| `/replan` | Propose curriculum changes when the plan has drifted (needs your approval). |
| `/pause-session` | Save a clean handoff mid-lesson before you stop. |

---

## What lives where

| Path | Role | Written by |
|---|---|---|
| `CLAUDE.md` | The teaching engine (subject-agnostic persona + rules) | the plugin |
| `commands/`, `agents/` | Slash commands and the grader subagent | the plugin |
| `learner/profile.md` | How you learn — durable, cross-course | `/build-profile`, `/end-lesson` |
| `courses/<subject>/lesson_plan.md` | The curriculum | `/new-course`, `/replan` |
| `courses/<subject>/course_profile.md` | Subject-specific analogies + concept mastery | `/end-lesson` |
| `courses/<subject>/progress.json` | Where you are, scores | `/end-lesson`, `/submit-exercise`, cold-open |
| `courses/<subject>/lesson_log/` | Raw session notes + cold-open retention records | `/start-lesson`, `/end-lesson` |
| `courses/<subject>/reading/` | Pre-read primers | `/end-lesson` |

The `learner/`, `courses/`, and `.teaching/` directories are created in *your* working directory
when you run the commands — they're never part of the plugin.

---

## License

MIT. See [LICENSE](LICENSE).
