# Portable Learning-Profile Prompt

This is a self-contained prompt. Paste everything in the fenced block below into a chat assistant
that already knows you well — one you've used heavily, with long-running memory of how you think
and work (your main ChatGPT / Claude.ai / Gemini account). It will produce a `learner/profile.md`
you can drop into your tutoring working directory.

No heavy-memory chat to mine? Use **interview mode** instead: run `/build-profile interview` and
the tutor interviews you fresh.

Before pasting, replace `{{SUBJECT}}` with what you want to learn and `{{NAME}}` with your name.

---

```
You have a lot of accumulated context about me from our past conversations and your memory. I'm
about to start a structured, one-on-one tutoring curriculum on {{SUBJECT}} with a separate AI
tutor. That tutor adapts entirely off a single file — a "learning profile." Write that file for
me using everything you actually know about how I learn and work — not generic advice.

Produce a single Markdown document with EXACTLY these sections, in this order. Keep the whole
thing under ~600 words — a tight synthesis, not a transcript. Be specific and falsifiable:
"responds to novel cross-domain analogies, glazes on clichés" beats "likes analogies."

# Learning Profile — {{NAME}}

> How this person learns. Durable across courses. Rewritten by the tutor after each session.
_Last updated: seeded from chat memory, no sessions yet._

## How they learn best
Concrete mechanisms that have demonstrably worked with me — pacing, formats, what makes a concept
stick, what kind of examples land. Cite patterns you've actually seen, not ideals.

## What to avoid
What reliably loses me, annoys me, or wastes my attention — phrasings, framings, overused
metaphors, pacing mistakes. Be blunt; this section protects my time.

## Known background
Languages, tools, domains, adjacent expertise I already have — so the tutor neither re-teaches
what I know nor assumes what I don't. Flag strong vs. surface-level.

## Current goals
Why I'm learning {{SUBJECT}} — the real practical anchor (a project, a decision, a gap), plus any
secondary goals. Concrete over aspirational.

## Working analogies
_None yet — populated as lessons run._
## Failed analogies
_None yet — populated as lessons run._
## Shaky concepts
_None yet — populated as lessons run._
## Mastered concepts
_None yet — populated as lessons run._

Rules:
- Third person, using my name or pronoun, matching the heading style above.
- The last four sections stay as the placeholder lines — the tutor fills them over time.
- Don't flatter me and don't hedge. If I have a known weakness (impatience, skipping
  fundamentals, novelty-seeking, ADHD, dyslexia, whatever shapes how I should be taught), name it
  — it makes the tutor better.
- Output only the Markdown file, nothing else.
```

---

After the assistant produces the file:

1. Save its output as `learner/profile.md` in your tutoring working directory.
2. Skim it. Correct anything wrong — you know yourself better than its memory does.
3. Run `/new-course` to pick your subject, then `/start-lesson`.
