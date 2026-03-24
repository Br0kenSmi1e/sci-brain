---
name: overview
description: Use when giving a field overview after a survey — teaches newcomers through a failure-driven four-act narrative that explains the field's central problem, failed attempts, solutions, and open frontier
---

## Overview

An interactive field overview that teaches newcomers through a failure-driven, four-act narrative built on survey registry data. The overview *teaches* — it does not brainstorm (that's `/ideas`) or produce a research report (that's `/writer`).

### Four Pedagogical Principles

These principles drive every explanation throughout the session:

1. **Motivate through irreplaceability** — show why the problem *must* be solved and can't be avoided.
2. **Explain through history, not textbooks** — go back to the origin scenario, not abstract definitions.
3. **Teach through failure** — success is explained by why alternatives fail. Never say "this method succeeds because it can do X." Say "this method succeeds because others cannot do X."
4. **Ground in concrete scenarios** — every claim gets a specific example or thought experiment. No claim without illustration.

---

### Prologue

**Entry:** `/overview <topic>` — topic is a required argument.

**Step 1 — Load survey registry.** Check for the survey registry at `~/.claude/survey/<topic>/` and `.claude/survey/<topic>/`. If no registry is found at either location, tell the user to run `/survey <topic>` first and stop.

Read `summary.md` and `references.bib` from the registry.

**Step 2 — User background.** Check for `docs/discussion/user-profile.md`. If found, read it and summarize what you know:

> "I have your profile from before — [brief summary]. Anything changed, or shall we dive in?"

If not found, ask the user's background: field, experience level, and what they already know about this topic. Save the profile to `docs/discussion/user-profile.md`.

**Step 3 — Identify the central problem.** Analyze the survey registry to identify the field's central problem — the one thing that makes this field necessary.

If the registry reveals multiple independent problems (i.e., the topic spans several sub-fields), present them via `AskUserQuestion` and let the user pick which one to focus on.

---

### Additional Research

Before starting the narrative, search beyond the survey registry for:
- **Origin story** — who first faced this problem, when, what was the context
- **Historical failed approaches** — what was tried and why it didn't work
- **Common misconceptions** newcomers have about this field

Store additional references in `docs/discussion/YYYY-MM-DD-HHMMSS-overview-references.bib`. Quality is relaxed compared to the survey — blogs, tutorials, lecture notes, Wikipedia articles are all fine. Only requirement: a working URL.

**Never modify the survey registry.**

---

### Conversation Log

Maintain a running log at `docs/discussion/YYYY-MM-DD-HHMMSS-overview-log.md` (timestamp from session start). Create the `docs/discussion/` directory if it doesn't exist.

**Append-only logging.** Save progress by appending to the log at checkpoints. Each append captures the **full conversation content** since the last save — all options presented (with descriptions), reasoning shared, user responses, search results, and key explanations. Not a summary — a readable record of what was actually said.

**When to append (checkpoints):**
- Every 3-5 exchanges, at a natural pause — when a topic wraps up or the conversation shifts
- At act transitions (entering Act 1, Act 2, Act 3, Act 4)
- At session wrap-up (Epilogue)

Don't log after every message. Wait for a natural checkpoint — the end of a thread, a comprehension check, a topic shift.

**Order: log first, then reply.** At a checkpoint, append to the log file before writing your response to the user. This ensures progress is saved even if the session is interrupted mid-reply.

**File header** — write once when creating the log:

```markdown
# Overview Session — YYYY-MM-DD HH:MM
## Topic: <topic>
```

---

### Act 1 — The Problem

Show why this field exists and why the problem *cannot* be left unsolved.

- **What breaks without this field?** Give a concrete scenario showing what goes wrong.
- **Why can't you ignore the problem?** Show that avoidance leads to something worse.
- **Why can't existing tools from other fields solve it?** Demonstrate with a specific example where an obvious approach from outside the field falls apart.

**Goal:** The user feels this problem *cannot* be left unsolved — it demands its own field.

**Pause for questions.** After presenting Act 1, check comprehension:

> "Does it make sense why [the problem] can't just be handled by [obvious alternative]? Any questions before we look at what people tried?"

---

### Act 2 — The Failed Attempts

Walk through naive, classical, or simpler approaches — the things people tried before the field matured.

For each failed approach:
- **What it tried** — the intuition behind it
- **Where it breaks** — a concrete example of the failure mode
- **What it teaches** — what this failure reveals about the shape of the problem

The user builds intuition for *why the problem is hard* by seeing what doesn't work. Each failure narrows the space of possible solutions, making the real solution feel inevitable.

**Pause for questions.** After presenting the failed attempts, check comprehension:

> "Does it make sense why [approach] breaks down when [scenario]? Before we see what actually works — based on these failures, what properties would a solution need to have?"

---

### Act 3 — The Solution Landscape

Now the real solutions, motivated by the failures in Act 2. The user should feel that these solutions are *inevitable* given what they've learned about the problem.

**If multiple solutions exist:** Compare by what the *others can't do*, not what each one can do. For each solution, explain which failure from Act 2 it specifically overcomes — and which failures it doesn't.

**If one solution dominates:** Explain why alternatives fail, which reveals why the winner works. The dominant solution is understood through the inadequacy of its competitors.

Always connect back to Act 2: "Remember how [naive approach] broke because of [X]? [Solution] handles this by [Y]."

**Pause for questions.** After presenting the solution landscape, check comprehension:

> "Does it make sense how [solution] avoids the problems we saw with [failed approach]? Any questions before we look at what's still open?"

---

### Act 4 — The Open Frontier

What's still unsolved. For each open problem, explain three things using concrete scenarios:

1. **Why it matters** — what breaks or stays impossible if unsolved, what becomes possible if solved.
2. **What the barrier is** — a concrete example of where current approaches fail. The barrier can take many forms: engineering/resource limitations, missing mathematical proofs, knowledge gaps between communities, lack of experimental data, or the need for a fundamentally new idea. Explain the specific barrier concretely rather than categorizing it.
3. **What people believe could work** — promising directions and what's still missing.

**Pause for questions.** After presenting the open frontier, check comprehension:

> "Any of these open problems particularly interesting to you? Questions about the barriers?"

---

### Epilogue

**Step 1 — Recap.** Give a brief recap of the four-act arc: the problem that demanded this field, the approaches that failed and what they taught, the solutions that emerged, and the frontier that remains.

**Step 2 — Written summary.** Ask if the user wants a written summary via `AskUserQuestion`:

> "Would you like a written summary of this overview?"
> - **(a)** Yes — I'll generate a document
> - **(b)** No — we're done, the conversation log is saved

**If yes:** Ask format via `AskUserQuestion`:

> "What format?"
> - **(a)** Typst (`.typ`) — recommended, native BibTeX support
> - **(b)** LaTeX (`.tex`) — traditional academic format
> - **(c)** Markdown (`.md`) — note: citations will be inline text

**Generate the summary** at `articles/YYYY-MM-DD-<topic>-overview.{typ,tex,md}` with a merged `references.bib` (combining the survey registry bib and the overview references bib).

The summary follows the four-act structure:

```
Title: Overview of <topic>

1. The Problem
   - What the field solves and why it can't be avoided
   - Concrete scenario showing what breaks without it

2. The Failed Attempts
   - Historical/naive approaches that didn't work
   - For each: what it tried, concrete example of where it breaks

3. The Solution Landscape
   - How the field actually solves the problem
   - If multiple solutions: comparison by what others can't do
   - If one dominant: why alternatives fail
   - Key references from survey registry

4. The Open Frontier
   - For each open problem:
     - Why it matters (what stays impossible)
     - What the barrier is (concrete example)
     - What people believe could work, what's still missing

References (merged from survey bib + overview bib)
```

For Typst output, use CeTZ diagrams where helpful (e.g., solution landscape comparisons, field development timeline). Refer to `skills/writer/typst-reference.md` for CeTZ patterns and syntax.

**Tone:** Motivated, concrete, failure-driven — not a dry survey paper. The written summary should read like the conversation felt.

---

### Interaction Style

- **Section-by-section pauses** — pause for questions after each act. Don't rush through; let the user absorb.
- **Comprehension checkpoints** on hard conceptual leaps: "Does it make sense why X fails here?"
- **Re-explain on confusion** — if the user is confused, re-explain using a different analogy or concrete example. Never repeat the same explanation.
- **Cite key format** — use `[AuthorYear]` when referencing papers (e.g., `[Shor1994]`).
