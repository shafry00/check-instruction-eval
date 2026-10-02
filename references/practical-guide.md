# Running CHECK in Practice

## Compare step: 3 methods

### Method A: Self-comparison (agent is the evaluator)

1. Write the task prompt WITHOUT the instruction under evaluation
2. Run it as-is, save output
3. Write the task prompt WITH the instruction inserted
4. Run it, save output
5. Compare side by side

Use the same model and temperature for both runs. Vary the inputs, not the config.

### Method B: API comparison (scripted)

Run each test input through an LLM API twice:
- Baseline: system prompt + user input (no instruction)
- Candidate: system prompt + instruction + user input

Log: input, output, latency, token count.

### Method C: Quick spot-check (for simple constraints)

If the instruction is a formatting rule or output constraint:
1. Run one input without it
2. Run one input with it
3. Check if the constraint is met in both cases
4. If met in both = instruction unnecessary. If only met with instruction = it works.

## Test set building

- **Typical (60%):** most common inputs the AI will see
- **Edge cases (25%):** ambiguous inputs, missing info, conflicting constraints
- **Adversarial (15%):** inputs designed to break the instruction

Each test case needs:
- ID (stable, for tracking)
- Input text
- Expected behavior (what good looks like)
- Pass criteria (binary check if possible)

## Rubric anchors

### Accuracy
- 1: Output contains factual errors, hallucinations, or directly contradicts the input
- 3: Output is mostly correct; minor issues that don't change the meaning
- 5: Output is correct, complete, and verifiable against the input

### Relevancy
- 1: Output addresses a different question than what was asked
- 3: Output answers the ask but includes unnecessary information
- 5: Output directly answers the ask; nothing wasted

### Actionability
- 1: Output requires significant rework before it can be used
- 3: Output is usable but requires interpretation or minor edits
- 5: Output can be used as-is; clear next steps or ready-to-use format

## Blind scoring

If possible, score outputs without knowing which version produced which output. Remove identifying markers, shuffle the order, then score. This reduces bias toward the new version.

## Common pitfalls

- Running Compare before Evidence and stopping early is fine, but do not skip Compare. If it fails, Evidence is a waste of time.
- Consequence is the step most people skip. It is also the most important. A cosmetic instruction that passes rubric scores but changes no decisions = KILL.
- Keep a log of verdicts. If the same instruction type keeps failing, the root problem is upstream (unclear task definition, not the instruction itself).
- Do not inflate PASS on Evidence by cherry-picking easy inputs. The test set must include at least one input where the instruction is likely to struggle.
