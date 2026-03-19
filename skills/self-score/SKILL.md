---
name: self-score
description: This skill should be used when about to declare a task complete, "finished implementing", "done with the task", "task complete", or any moment where work is about to be presented as finished. Provides structured self-evaluation against a 7-dimension quality rubric before completion. Skip for non-task interactions (answering questions, explanations, simple lookups).
---

# Self-Score

## Overview

Perform a structured self-evaluation of work quality before declaring any task complete. This skill complements `superpowers:verification-before-completion` (which checks *evidence* — did tests pass, did it build?) with a *quality reflection* layer — is the work actually good?

**Iron Law:** No task completion without a self-score. If the overall score is below 9.5, identify what to improve and ask before implementing.

## The Rubric

Score each dimension from 1.0 to 10.0 (one decimal place). Mark dimensions **N/A** when they genuinely do not apply to the task (e.g., Security for a documentation-only change).

| Dimension | What It Measures | 10 | 7 | 5 |
|-----------|-----------------|-----|-----|-----|
| **Correctness** | Works as intended, edge cases handled | Bulletproof, no logical errors | Happy path works, minor edges missed | Partially works, known issues |
| **Code Quality** | Clean, readable, idiomatic | Senior engineer approves without comments | Minor style or naming issues | Functional but messy |
| **Completeness** | All requirements addressed | Every requirement met, including implicit ones | Core met, minor aspects overlooked | Significant gaps |
| **Elegance** | Right abstraction, not over-engineered | Nothing to add, nothing to remove | Some unnecessary complexity | Over- or under-abstracted |
| **Performance** | Efficient, no wasted resources | Optimal for the context | Minor inefficiencies | Noticeable issues |
| **Security** | No vulnerabilities, inputs validated | No attack surface added | Minor hardening opportunities | Potential vulnerabilities |
| **User Intent Alignment** | Solves what was *meant*, not just literal | Spirit and letter nailed | Literal request met, nuance missed | Technically correct, misses the point |

## The Process

### Step 1: Score

After completing the work but before declaring it done:

1. Review the work against each dimension
2. Assign a score (1.0-10.0) or N/A for each
3. Calculate the overall score as the simple average of all scored dimensions (exclude N/A)
4. Present the scores in the output format below

### Step 2: Evaluate

- **Overall >= 9.5:** Declare task complete. Present the scorecard.
- **Overall < 9.5:** Proceed to Step 3.

### Step 3: Identify Improvements

For each dimension scoring below 9.5:

1. State what specific change would raise the score
2. Estimate what the new score would be after the change
3. Keep improvements proportional — small targeted fixes, not rewrites

Present improvements to the user with this format:

```
### Improvements to reach 9.5+

| Dimension | Current | Target | What to change |
|-----------|---------|--------|----------------|
| Code Quality | 8.0 | 9.5 | Extract repeated validation logic into a shared helper |
| Elegance | 8.5 | 9.5 | Replace nested conditionals with early returns |

Want me to implement these improvements?
```

### Step 4: Implement (if approved)

If the user approves:
1. Implement the proposed improvements
2. Re-score all dimensions
3. If still below 9.5, repeat from Step 3
4. If the user declines, the task is complete as-is

## Output Format

Always present scores in this exact format:

```
## Self-Score

| Dimension              | Score |
|------------------------|-------|
| Correctness            | X.X   |
| Code Quality           | X.X   |
| Completeness           | X.X   |
| Elegance               | X.X   |
| Performance            | X.X   |
| Security               | X.X   |
| User Intent Alignment  | X.X   |
| **Overall**            | **X.X** |
```

## Red Flags

These patterns indicate the self-scoring is not being done honestly:

| Red Flag | What Is Happening | Correction |
|----------|-------------------|------------|
| Every dimension is 9.5+ | Score inflation to skip the improvement loop | Re-evaluate with genuine self-criticism. A first pass rarely scores 9.5+ on everything. |
| Scoring N/A on applicable dimensions | Avoiding low scores by marking dimensions irrelevant | Only mark N/A when the dimension truly cannot apply (e.g., Performance on a typo fix) |
| Proposing full rewrites as improvements | Disproportionate response to small gaps | Improvements should be targeted fixes, not architectural overhauls |
| Identical scores across all dimensions | Lazy scoring without genuine reflection | Each dimension measures something different — scores should vary |

## When to Apply

**Apply to:** Code changes, feature implementations, bug fixes, refactoring, configuration changes, script creation — any work that produces artifacts.

**Skip for:** Answering questions, providing explanations, simple lookups, brainstorming, planning (the plan itself is not a deliverable — the implementation is).

## Integration

- **Pairs with:** `superpowers:verification-before-completion` — run verification first (evidence that it works), then self-score (reflection on quality)
- **Sequencing:** Verification -> Self-Score -> Completion declaration
- **Mandated by:** Global CLAUDE.md "Always (both modes)" rules
