# 🧪 Research Reviewer

> An iterative, multi-agent **academic peer-review pipeline** for [Claude Code](https://claude.com/claude-code). It respawns a brutally honest "fresh pair of eyes" Reviewer Agent every round, scores your paper out of 100, refactors the weak spots, and loops until the manuscript hits a flawless **100/100**.

<p align="center">
  <img alt="Claude Code Skill" src="https://img.shields.io/badge/Claude%20Code-Skill-8A2BE2">
  <img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-green.svg">
  <img alt="Domain" src="https://img.shields.io/badge/for-Researchers%20%26%20PhD%20students-orange">
</p>

---

## Why?

Self-review is biased. After staring at your own thesis for weeks you stop seeing the
missing error bars, the undefined symbol in Eq. 7, the overlapping legend in Figure 4, and
the claim of "universal superiority" that three datasets don't support.

**Research Reviewer** fixes that by treating every review as a *blind, first-time* review.
Each iteration spawns a **brand-new** Reviewer Agent with **zero memory** of previous drafts
or scores — so it can't get complacent, can't overfit to the last version, and keeps hunting
for reasons to reject. You apply the fixes, it re-reviews from scratch, and the score climbs.

## How it works

```
            ┌──────────────────────────────────────────────┐
            │   Manuscript (.tex / .md / .pdf + figures)    │
            └──────────────────────┬───────────────────────┘
                                   │
                                   ▼
              ╭───────── fresh, blind subagent ─────────╮
              │   🧐  Reviewer Agent  (no prior memory)  │
              │   • scores X / 100                       │
              │   • itemized deductions → 4 pillars      │
              │   • refactored, polished text            │
              │   • "Required Experiments" checklist     │
              ╰─────────────────────┬───────────────────╯
                                    │ apply fixes
                                    ▼
                         score == 100/100 ?  ──No──┐
                                    │ Yes          │  (respawn fresh eyes)
                                    ▼              └──────────┘
                              ✅  Done
```

A new agent every loop = no cognitive bias, no "I already approved this," no overfitting.

## The four pillars it grades on

| Pillar | What it hunts for |
|---|---|
| **1. Methodological Soundness & Mathematical Precision** | Unstated assumptions, undefined variables, inconsistent notation/dimensions, arbitrary/unjustified design choices. |
| **2. Empirical Rigor & Validity of Claims** | Missing error bars / CIs / p-values, unfair or under-tuned baselines, ablations that change >1 variable at once. |
| **3. Scope of Validity & Narrative Honesty** | Overclaiming beyond the data, no honest limitations / failure modes / scalability discussion. |
| **4. Visual Layout & Editorial Integrity** | Fails the 30-second graphic test, overlapping labels/legends, orphan headings, hallucinated or placeholder citations. |

## Install

### Option A — Plugin marketplace (recommended, one line)

This repo is a Claude Code **plugin marketplace**. Inside Claude Code:

```text
/plugin marketplace add roeimichael/Research-Reviewer
/plugin install research-reviewer@research-reviewer
```

That's it — the `research-reviewer` skill is now available in every session. Update later
with `/plugin marketplace update research-reviewer`, remove with
`/plugin uninstall research-reviewer@research-reviewer`.

### Option B — Manual copy

Drop the skill straight into a `skills/` folder Claude Code already reads:

```bash
git clone https://github.com/roeimichael/Research-Reviewer.git
cp -r Research-Reviewer/skills/research-reviewer ~/.claude/skills/   # global, all projects
# or:  cp -r Research-Reviewer/skills/research-reviewer .claude/skills/   # this project only
```

On Windows (PowerShell):

```powershell
git clone https://github.com/roeimichael/Research-Reviewer.git
Copy-Item -Recurse Research-Reviewer\skills\research-reviewer ~\.claude\skills\
```

## Usage

In Claude Code, just ask — the skill auto-triggers on review intent:

```
> review my thesis in ./paper/main.tex and get it to 100/100
```

or invoke it explicitly:

```
> /research-reviewer
```

Claude will:
1. Read your manuscript (and figures/tables/datasets if you point to them).
2. Spawn a **fresh** Reviewer Agent that scores it `X/100` with itemized deductions.
3. Apply the refactored text and notation fixes back into your paper.
4. Hand you a **Required Experiments** checklist for the things only *you* can run.
5. Respawn a new blind reviewer and repeat until **100/100**.

## Customize

* **`reviewer-agent.md`** — the reviewer's persona, scoring rubric, and output format. Edit
  the *"Paper-specific benchmarks"* block to name your actual method and the baselines it
  must beat (it ships with a `TRALO` vs `Hounie / Fioretto / Heuristic / Danits LP` example).
* **`SKILL.md`** — the orchestration loop and when the skill triggers.

## What it will *not* fake

The reviewer flags missing experiments as a **Required Experiments** checklist instead of
inventing results. Statistical backing you don't have, baselines you didn't run, and ablations
you didn't do stay on the checklist for *you* to execute — the score won't hit 100 by
hallucinating data.

## License

[MIT](LICENSE) — use it, fork it, ship it.

---

<sub>Built as a Claude Code Skill. Not affiliated with NeurIPS, ICML, or IEEE — it just reviews like it wants to be.</sub>
