---
name: ama
description: >
  Use when the user says "ama" or "ask me everything first", or before building
  any asset whose requirements could be read more than one way — code, design,
  document, image, presentation, or anything else. Interrogate the request until
  the ambiguity is gone, restate it as a short spec, and wait for "go".
  Never assume silently: ask, or state the assumption.
---

# AMA — Ask Me Anything Before Building

## Purpose

Eliminate ambiguity before producing an asset. A few minutes of questions now
prevents hours of rework later. The goal is to understand the request fully
before a line of code, a word, or a pixel exists.

## When to Use

- The user invokes "ama" or says "ask me everything first"
- You're about to build something the request could be read more than one way
- The request is open-ended ("build me a dashboard", "create a landing page")
- Audience, design direction, or success criteria are unspecified

## Ask Only What Changes the Build

The core rule. For every unknown, ask yourself:

> Would a different answer change what I build, or how I build it?

- **Yes** — ask it.
- **No, or it's cheap to reverse** — take the sensible default, state it as an
  assumption, and move on.
- **Only if it's expensive or irreversible to undo** — stop and wait.

Two consequences matter more than the rule itself:

- **Look it up before asking.** Workspace conventions, the repo, attached files,
  and earlier conversation usually answer half the list. A cheap lookup beats a
  question — asking what you could have read wastes the user's patience.
- **Never ask what the user already told you.** Re-asking a settled point reads
  as not listening.

## When Not to Ask

If the user says "just build it", "don't ask, just do it", or repeats a request
you have already questioned, respect it: state your assumptions in one short
list, then build. This skill exists to prevent rework, not to gate the work.

## Budget

- First batch: at most 5 questions, the highest-leverage ones.
- At most one follow-up round, and only for answers that opened a new fork.
- Stop as soon as further answers would no longer change the build.

## Asking the Question

Ask through your host's interactive question tool — the one that renders
selectable options the user picks from. Use it whenever the answer is
enumerable: it's faster to answer than prose, and it records the choice.

**If your host has no such tool**, render the same question as a short numbered
list in plain text and stop for the answer. Never bury options inside a
paragraph, and never silently assume instead of asking.

Host tools, as examples only — use whatever yours provides:

- Claude Code → `AskUserQuestion`
- Hermes → `clarify`

Map the shape, not the name:

- Give 2–4 options per question; the user can always answer something else.
- Put your recommended option first when the tool marks it.
- No per-option description field? Fold the trade-off into the option text —
  "Light — clean white background" beats a bare "Light".
- Multi-select for anything where several answers apply (features,
  constraints); single-select for choices that exclude each other.
- Batch independent questions in one call; one topic per question.

If a question comes back skipped, unanswered, or times out, don't stall: proceed
on the option you recommended and label it as an assumption in the confirmation
step.

## What to Ask About

First parse the request: what's already specified, what's genuinely unknown, and
which unknowns are material. Then work through these categories, skipping
anything already answered, already looked up, or immaterial:

- **Purpose & audience** — who it's for, what it's for
- **Output format** — which artifact, and what scope (full build / MVP / proof of
  concept)
- **Design & tone** — theme, density, and a *named* style reference; "Linear",
  "Stripe", "Notion" anchor taste far better than "modern and clean"
- **Features** — must-have, nice-to-have, and explicitly out (multi-select)
- **Constraints** — stack, integrations, data source, deadline, budget
- **Definition of done** — how they'll judge that it worked

## Confirm Before Building

Restate the understanding as a short spec — not a chat summary, and not a
paraphrase of their words:

- **Building** — one or two lines
- **In scope** — the must-haves
- **Out of scope** — what you're explicitly not doing. Ask for this if the user
  hasn't said it; exclusions are where rework comes from.
- **Assumptions** — every default you took instead of asking
- **Done when** — the acceptance criteria

Then ask: "Does this look right? Say go and I'll build it." Wait for the answer.

## Examples

**Enumerated question** — Hermes `clarify` shape. The first choice is marked
recommended; the user can still answer freely.

```
clarify(questions=[{
  "question": "Who is the primary audience?",
  "choices": ["My team — internal, functional",
              "Leadership — needs to look sharp",
              "External users — high polish required"]
}])
```

**Multi-select for features** — Claude `AskUserQuestion` shape.

```
AskUserQuestion({questions: [{
  question: "Which features are must-haves?",
  header: "Features",
  multiSelect: true,
  options: [
    { label: "Search",            description: "Text search across items" },
    { label: "Filter by version", description: "Dropdown or sidebar filter" },
    { label: "Timeline view",     description: "Visual roadmap" }
  ]
}]})
```

**Design questions batched** — theme, density, and a named style reference in
one call, 2–4 options each.

## Principles

- **Ask, or state the assumption.** Silence is the only wrong answer. A stated
  assumption the user can correct beats a question they didn't need.
- **Enumerated options over open prose**, whenever the answer set is finite.
- **Specific beats abstract.** Name real references; "clean and modern" means
  nothing.
- **Multi-select when several answers apply**, single-select when they exclude.
- **Surface trade-offs** in the option text, so the cheaper choice is visible.
- **One topic per question.**
- **Respect the answers you already have** — including "just build it".
