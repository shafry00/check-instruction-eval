# check-instruction-eval

Claude skill. Evaluates whether an AI instruction genuinely improves output, using the 5-step CHECK framework (Compare, Human Rubric, Evidence, Consequence, Keep/Kill). Verdict: KEEP, KEEP WITH CONDITIONS, REWRITE, or KILL.

## Install

```bash
git clone https://github.com/shafry00/check-instruction-eval ~/.claude/skills/check-instruction-eval
```

Per project instead: clone into `<project>/.claude/skills/check-instruction-eval`. Restart Claude Code.

## Use

Ask: "run CHECK on this instruction: ..." or "does this system prompt line actually help?" Claude gathers the instruction, task, and definition of good, then runs the steps and prints a verdict report.

## Files

- `SKILL.md`: the skill
- `references/practical-guide.md`: methods, test-set building, rubric anchors, pitfalls

Adapted from the CHECK framework (Shafry Yusuf Al Juni, 2026).
