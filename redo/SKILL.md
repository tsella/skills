---
name: redo
description: >
  Reimplement the solution you just built — better, simpler, and bug-free.
  Use this skill after completing a feature or fix. It treats the first implementation
  as an exploration phase: you reflect on what you learned, delete the code, and
  rebuild with the benefit of hindsight. Invoke this whenever the user says /redo,
  or when they ask you to "redo", "rewrite from scratch", "try again cleanly",
  "delete and redo", or "implement it properly now". Also consider suggesting this
  skill yourself after a messy or heavily-iterated implementation where a clean
  rewrite would clearly produce better code.
---

# Redo: Delete and Reimplement with Hindsight

You just finished building something. It works — but you made discoveries along the way: edge cases you didn't anticipate, abstractions that turned out wrong, code paths that got patched instead of designed. The first implementation was exploration. Now you reimplement with everything you learned.

This is the "write one to throw away" pattern. The second pass is always better because you're no longer guessing — you know the shape of the problem.

## Process

### 1. Reflect

Before touching any code, think through what you learned during the first implementation. What surprised you? What was harder than expected? What would you do differently if starting fresh?

**For small changes** (1-3 files, straightforward logic): hold the reflection in your head. No need to write anything down.

**For medium-to-large changes** (4+ files, architectural decisions, tricky edge cases): write a brief plan capturing:
- Edge cases and gotchas discovered during the first pass
- Architectural decisions you'd make differently
- Bugs you hit and their root causes
- The simpler design you now see is possible
- Any lessons specific to the codebase (APIs that behave unexpectedly, patterns that didn't work)

Keep it concise — bullet points, not paragraphs. This is a scratchpad for the rebuild, not documentation.

### 2. Delete

Reset all changes from the first implementation:

```
git checkout -- <changed files>
git clean -fd <new files/dirs>
```

Full reset is the default. You're not salvaging — you're rebuilding. The whole point is that the second implementation is structurally different, not a patch on top of the first.

**Exception**: keep purely mechanical artifacts that don't influence design — things like CSS class names you already synced across files, config entries, or asset files. Use judgment, but err on the side of deleting. If you're unsure whether something should stay, delete it.

### 3. Reimplement

Build it again from scratch. You will naturally write better code because:

- You know the edge cases upfront, so you handle them in the design rather than bolting them on
- You know which abstractions actually help and which were premature
- You know the minimal set of changes needed — no leftover scaffolding from abandoned approaches
- You've seen the bugs, so you avoid the patterns that caused them

Don't rush. The goal isn't speed — it's quality. Take the cleaner path even if it means a slightly different approach than the first time.

### 4. Verify

Run the same verification you'd normally do — build, test, check the UI if applicable. The bar is the same as any implementation; the difference is you should hit it more cleanly this time.

## When NOT to redo

- The first implementation was already clean and simple — no need to redo for ceremony
- The change is trivial (typo fix, one-line change) — redo adds nothing
- You're on iteration 3+ and not finding meaningful improvements — diminishing returns

## Mindset

The first implementation isn't a failure. It's how you learned the problem. Treat it that way — extract the lessons, discard the code, and build something you're proud of.
