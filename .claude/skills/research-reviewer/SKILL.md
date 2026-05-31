---
name: research-reviewer
description: >
  Iterative, multi-agent academic peer-review pipeline. Spawns a harsh "fresh eyes"
  Reviewer Agent every iteration that blind-reviews a thesis/paper, deducts points for
  logical, empirical, mathematical, or layout flaws, and scores it out of 100. Refactors
  the manuscript and re-reviews until it reaches a flawless 100/100. Use when the user
  asks to review, critique, harden, or "get to 100/100" a research paper, thesis, or
  manuscript, or invokes /research-reviewer.
---

# Research Reviewer — Iterative Blind Peer Review

Run an iterative, multi-agent review loop that drives a manuscript to a flawless
**100/100**. Each iteration spawns a **fresh Reviewer Agent** with no memory of prior
drafts so the review stays unbiased and never "overfits" to earlier versions.

## How to run

1. **Locate the manuscript.** Ask the user for the paper (a `.tex`/`.md`/`.pdf` path,
   a folder, or pasted text) plus its figures/tables/datasets if available. Do not
   proceed without the actual content to review.

2. **Spawn a fresh Reviewer Agent (blind).** For each iteration, launch a *new* subagent
   via the `Agent` tool (`general-purpose`). Pass it:
   - the full system instructions from `reviewer-agent.md` (read it, inline it),
   - the **current** manuscript content only — never the previous review, score, or
     change log. This is what keeps the "fresh pair of eyes."

3. **Collect the review.** The agent returns: a score `X/100`, itemized deductions mapped
   to the 4 pillars, refactored/polished text for the flawed sections, and a
   **Required Experiments** checklist.

4. **Apply fixes.** Edit the manuscript with the agent's refactored sections and notation
   fixes. Surface the Required Experiments checklist to the user — those need real runs
   and cannot be auto-resolved.

5. **Loop.** Re-spawn a brand-new Reviewer Agent on the updated manuscript. Repeat until
   the score is **100/100 with zero remaining deductions**, or the only blockers left are
   Required Experiments the user must run. Report the score trajectory each round.

## The four pillars the reviewer enforces

1. **Methodological Soundness & Mathematical Precision** — assumptions stated, notation
   consistent, every design choice justified.
2. **Empirical Rigor & Validity of Claims** — error bars / CIs / p-values, fair baselines,
   clean single-variable ablations.
3. **Scope of Validity & Narrative Honesty** — no overclaiming, explicit limitations.
4. **Visual Layout & Editorial Integrity** — 30-second graphic test, no collisions, valid
   citations.

## Notes

- The full reviewer persona, scoring protocol, and output format live in
  **`reviewer-agent.md`** (same folder). Read it and pass it verbatim to each subagent.
- Keep iterations genuinely blind: a new `Agent` call starts fresh — do **not** reuse a
  prior agent via SendMessage, and do **not** leak prior scores into the prompt.
- If the paper has named benchmarks (e.g. method must beat specific baselines), tell the
  reviewer to enforce them — see the "Paper-specific benchmarks" section in
  `reviewer-agent.md`.
