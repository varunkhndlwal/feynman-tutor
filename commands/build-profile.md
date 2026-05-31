Seed `learner/profile.md` — the durable record of how this person learns.

> Runs under the same teaching engine as `/start-lesson` (the operative persona lives in the
> command files, not the plugin's CLAUDE.md): adapt to `learner/profile.md` as it forms, build
> from first principles, keep it tight — no padding.

The profile is the one document about *the learner*, not the subject. It carries across every
course they ever take. Never invent it from thin assumptions — derive it from the learner's own
words or their own chat history. Two modes; ask which if not given.

## Mode: `portable` (mine an existing long-memory chat)

Best when the learner already uses a chat assistant heavily that has real memory of how they
think. Let the model that already knows them write the profile.

1. Read `templates/profile_bootstrap_prompt.md` from this plugin.
2. Print the fenced portable prompt to chat, with `{{SUBJECT}}` and `{{NAME}}` filled in (ask
   for these).
3. Tell them: paste it into the chat assistant that knows them best, then bring back the result
   (paste it, or save it as `learner/profile.md`).
4. When they return a profile, verify it has all the sections in the right order and the last
   four (analogies/concepts) are placeholders. Fix structure silently; flag anything that reads
   like flattery or a guess rather than evidence.

## Mode: `interview` (no chat history to mine, or starting fresh)

Run a tight interview as the tutor — a profile, not a therapy session. ~8–12 questions,
adapting to answers. Cover:

- **Background:** languages, tools, domains, depth in each — where strong vs. surface.
- **How they learn:** what's made a hard concept finally click before? Examples-first,
  theory-first, or build-it-themselves? Pace preference.
- **What loses them:** overused metaphors, padding, being talked down to, pacing misses.
- **Constraints:** time per session, attention patterns (e.g. ADHD), reading on mobile/desktop.

Probe vague answers once — "give me a concrete time that worked" beats accepting "I like
hands-on stuff."

## Both modes — write the file

Write `learner/profile.md` with this structure. The first four sections are real synthesis; the
last four start as placeholders and fill in as lessons run.

```
# Learning Profile — <Name>

> How this person learns. Durable across courses. Rewritten by `/end-lesson`; don't append.
_Last updated: seeded, no sessions yet._

## How they learn best
Concrete mechanisms that demonstrably work — pacing, formats, what makes concepts stick.
## What to avoid
What reliably loses, annoys, or wastes their attention. Be blunt; this protects their time.
## Known background
What they already know, so the tutor neither re-teaches nor over-assumes.
## Current goals
Why they're learning — the real anchor.
## Working analogies
_None yet — populated as lessons run._
## Failed analogies
_None yet._
## Shaky concepts
_None yet._
## Mastered concepts
_None yet._
```

Keep it under ~600 words — a synthesis, not a transcript. Don't flatter and don't hedge; if
there's a known weakness (impatience, skipping fundamentals, novelty-seeking), name it — it
makes the tutor better.

After writing: show it, ask them to correct anything wrong (they know themselves better than any
model's memory does), apply edits. Then: "Profile seeded. Next: `/new-course` to pick a subject."

## Restraint

This is a seeding tool, run once at the start. Ongoing evolution is `/end-lesson`'s job — it
rewrites the synthesis from observed behavior. Don't use `/build-profile` to patch a profile
mid-course; that throws away accumulated evidence.
