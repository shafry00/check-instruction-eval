---
name: "check-instruction-eval"
description: "CHECK framework: evaluate whether an AI instruction genuinely improves output. Verdict: keep, rewrite, or kill."
status: proposal
version: "v1"
date: "2026-10-02"
---

# CHECK: Instruction Evaluation

Decide whether an instruction given to an AI genuinely improves output or is merely cosmetic. Verdict: KEEP, REWRITE, or KILL.

## Gather first

- The instruction under evaluation (exact text)
- The task it's meant to improve (what the AI is asked to do)
- What "good" looks like for that task

Methods, test-set building, rubric anchors, blind scoring, pitfalls: see `references/practical-guide.md`.

## Run 5 steps in order

### 1. Compare (with vs without)

Run the same task twice: with the instruction, and without (baseline). Use 3+ inputs covering typical and edge cases.

- **PASS:** outputs differ measurably AND the instruction version is better
- **FAIL:** outputs are identical, or the difference doesn't matter

If you can't tell which output came from which version, the instruction does nothing.

### 2. Human Rubric

Score both versions 1-5 on each dimension:

- **Accuracy** — 1: wrong/hallucinated. 3: mostly correct. 5: correct and complete
- **Relevancy** — 1: off-topic. 3: addresses ask, some noise. 5: direct, nothing wasted
- **Actionability** — 1: can't act without rework. 3: usable with interpretation. 5: ready to use

- **PASS:** at least 2 of 3 dimensions improve by 1+ point with the instruction
- **FAIL:** scores flat or regressed. Verbose but same score = cosmetic

### 3. Evidence (test set)

Test against 20-50 inputs (minimum 10). Mix: typical 60%, edge 25%, tricky 15%. Define expected behavior BEFORE running.

- **PASS:** 70%+ of inputs show measurable improvement
- **FAIL:** improvement sporadic (<50%) or only on cherry-picked examples

### 4. Consequence (decision quality)

Does the instruction change what someone DOES with the output?

- **PASS:** changes decisions, reduces errors, or prevents specific failure modes
- **FAIL:** output looks "better" but same decisions would be made. Longer is not better

### 5. Keep or Kill (verdict)

| Steps passed | Verdict |
|-------------|---------|
| 4-5 | KEEP — earning its place |
| 3 | KEEP WITH CONDITIONS — monitor weak spots |
| 2 | REWRITE — right direction, weak execution |
| 0-1 | KILL — cosmetic or harmful |

## Output report

```
## CHECK Verdict: [KEEP / KEEP WITH CONDITIONS / REWRITE / KILL]

**Instruction:** [text]
**Task:** [what it improves]

| Step | Result | Evidence |
|------|--------|----------|
| Compare | PASS/FAIL | [summary] |
| Human Rubric | PASS/FAIL | [A/R/Act scores, with vs without] |
| Evidence | PASS/FAIL | [X/Y inputs improved] |
| Consequence | PASS/FAIL | [decision impact or cosmetic] |

**Verdict:** [verdict]
**Reason:** [1-2 sentences]
**Recommendation:** [if not KEEP: what to change]
```

## Adapted from

CHECK framework (FounderPlus, 2026) + prompt evaluation best practices (gold test sets, anchored rubrics, A/B methodology).
