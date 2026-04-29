---
name: ama
description: >
  Before creating any asset — code, design, document, image, presentation, or any
  other deliverable — ask the user as many questions as needed to eliminate all
  ambiguity. Use when the user invokes "ama", asks you to "ask me everything first",
  or when starting any creative or technical build where requirements could be
  interpreted multiple ways. Never assume; always ask first.
---

# AMA — Ask Me Anything Before Building

## Purpose

Before creating any asset, eliminate all ambiguity through structured questioning. A few minutes of questions now prevents hours of rework later. The goal is to fully understand what the user wants before a single line of code, word, or pixel is produced.

## When to Use This Skill

Apply this skill when:
- The user explicitly invokes "ama" or says "ask me everything first"
- You're about to build something where requirements could be interpreted multiple ways
- The request is open-ended (e.g., "build me a dashboard", "create a landing page", "make a tool that...")
- Design, UX, or creative direction hasn't been specified
- The target audience, use case, or success criteria is unclear

## How to Run an AMA Session

### Step 1: Read the Request Thoroughly

Before asking anything, fully parse what the user said. Extract:
- What type of asset is being created (UI, doc, script, image, presentation, etc.)
- What's already clear vs. what's ambiguous
- What categories of unknowns exist (purpose, audience, features, design, constraints, etc.)

### Step 2: Ask Everything Using AskUserQuestion

Use the `AskUserQuestion` tool for **every question that has enumerable answers**. You can batch up to 4 questions per call. Work through all categories systematically.

**Always use `AskUserQuestion` for:**
- Audience selection (internal / external / leadership / etc.)
- Theme/tone (light / dark / minimal / rich / etc.)
- Format/output type (React / HTML / doc / presentation / etc.)
- Scope (MVP / full build / POC / etc.)
- Style direction (reference examples as options)
- Feature priority (must-have / nice-to-have / out of scope)
- Any question where you can enumerate 2–4 reasonable choices

**Use plain text only for:**
- Truly open-ended questions where options can't be enumerated (e.g., "describe the content that goes in")
- Follow-up clarifications on a specific answer the user just gave

### Step 3: Question Categories to Cover

Work through these categories, using `AskUserQuestion` for each wherever possible:

**Purpose & Audience**
- Who is the primary audience?
- What's the primary goal?

**Output Format**
- What format?
- Scope level?

**Design & Aesthetic**
- Theme?
- Density?
- Tone?
- Style reference?

**Features & Functionality**
- Must-have features (use `multiSelect: true`)
- Interactivity level
- Data source

**Technical Constraints**
- Stack preference if relevant
- Specific libraries or dependencies

### Step 4: Follow Up on Vague Answers

If the user selects "Other" or gives an unclear answer, ask one targeted follow-up using `AskUserQuestion` if options are enumerable, or plain text if it's open-ended.

### Step 5: Confirm Before Building

After all questions are answered, restate your understanding in 3-5 bullet points using plain text:
- What you're building
- Key features/requirements
- Design direction
- Any assumptions you're making for things left unanswered

Ask: "Does this look right? Say go and I'll build it." Then wait for confirmation before starting.

## AskUserQuestion Usage Guide

**Key rules:**
- Each question needs 2-4 options (the user always gets an implicit "Other" for free text)
- Use `multiSelect: true` for features, constraints, or anything where multiple answers apply
- Add `description` to options to surface trade-offs and implications
- Batch related questions (up to 4) in a single `AskUserQuestion` call for efficiency
- Use short `header` labels (max 12 chars) like "Audience", "Format", "Theme", "Features"

**Example — single question:**

```
AskUserQuestion({
  questions: [{
    question: "Who is the primary audience for this?",
    header: "Audience",
    multiSelect: false,
    options: [
      { label: "Just me", description: "Personal tool, no polish needed" },
      { label: "My team", description: "Internal, functional" },
      { label: "Leadership", description: "Needs to look sharp and presentable" },
      { label: "External users", description: "Public-facing, high polish required" }
    ]
  }]
})
```

**Example — batching related questions:**

```
AskUserQuestion({
  questions: [
    {
      question: "What output format do you want?",
      header: "Format",
      multiSelect: false,
      options: [
        { label: "React component", description: ".jsx/.tsx file" },
        { label: "Plain HTML/CSS/JS", description: "No framework needed" },
        { label: "Markdown document", description: "Text-based deliverable" },
        { label: "Presentation (PPTX)", description: "Slide deck format" }
      ]
    },
    {
      question: "What's the scope level?",
      header: "Scope",
      multiSelect: false,
      options: [
        { label: "Full polished build", description: "Production-ready quality" },
        { label: "MVP", description: "Core features only, functional" },
        { label: "Proof of concept", description: "Demonstrates the idea" },
        { label: "Quick prototype", description: "Fast and rough, just to explore" }
      ]
    }
  ]
})
```

**Example — multi-select for features:**

```
AskUserQuestion({
  questions: [{
    question: "Which features are must-haves?",
    header: "Features",
    multiSelect: true,
    options: [
      { label: "Filter by version", description: "Dropdown or sidebar filter" },
      { label: "Search", description: "Text search across items" },
      { label: "Timeline view", description: "Visual roadmap/timeline" },
      { label: "Detail on click", description: "Expand items for more info" }
    ]
  }]
})
```

**Example — design questions batched:**

```
AskUserQuestion({
  questions: [
    {
      question: "What theme do you prefer?",
      header: "Theme",
      multiSelect: false,
      options: [
        { label: "Light", description: "Clean white background" },
        { label: "Dark", description: "Better for data-heavy views" },
        { label: "No preference", description: "I'll pick what fits best" }
      ]
    },
    {
      question: "What information density?",
      header: "Density",
      multiSelect: false,
      options: [
        { label: "Minimal", description: "Airy, lots of whitespace" },
        { label: "Balanced", description: "Good mix of space and content" },
        { label: "Dense", description: "Data-rich, compact layout" }
      ]
    },
    {
      question: "What visual style reference is closest?",
      header: "Style",
      multiSelect: false,
      options: [
        { label: "Linear", description: "Clean, modern, developer-focused" },
        { label: "Stripe", description: "Polished, professional, precise" },
        { label: "Notion", description: "Content-first, minimal chrome" }
      ]
    }
  ]
})
```

## Principles

- **No assumptions.** If something could go two ways, ask. Assumptions are the enemy of good output.
- **AskUserQuestion over plain text.** Whenever options can be enumerated, use the tool — it's faster and cleaner for the user than reading a list of questions.
- **Be specific with options.** "What style?" is worse than offering "Linear / Stripe / Notion" as choices.
- **Multi-select when appropriate.** Use `multiSelect: true` for features, constraints, or anything where multiple answers apply.
- **Surface trade-offs in descriptions.** Use the `description` field on options to explain implications.
- **Respect what's already clear.** Don't ask about things the user already specified. Only ask about genuine unknowns.
- **Batch related questions.** Group up to 4 related questions in a single `AskUserQuestion` call for a smoother experience.
- **One topic per question.** Each question within a batch should be a single focused question — don't bundle multiple concerns into one.
