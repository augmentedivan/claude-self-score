# claude-self-score

<div align="center">

**"LLMs often 'satisfice' (accept good-enough) rather than optimize."**
*— Claude*

</div>

A Claude Code plugin that makes Claude evaluate its own work quality before declaring any task complete. Instead of accepting the first working solution, Claude scores its output against a 7-dimension rubric and improves anything that falls short.

**This skill was itself self-scored and improved after it was created** — the initial implementation scored below the 9.5 threshold, triggered the improvement loop, and was refined until it passed. The tool practices what it preaches.

## Why This Exists

Without a quality gate, Claude tends to stop at "it works" rather than "it's good." Self-score adds a structured reflection step that catches:

- Code that works but is messy or over-engineered
- Solutions that meet the literal request but miss the user's actual intent
- Missing edge cases, security gaps, or performance issues
- Incomplete implementations that skip implicit requirements

## The Rubric

Every task is scored across 7 dimensions (1.0–10.0 each):

| Dimension | What It Measures | 10 (Excellent) | 5 (Mediocre) |
|-----------|-----------------|----------------|--------------|
| **Correctness** | Works as intended, edge cases handled | Bulletproof, no logical errors | Partially works, known issues |
| **Code Quality** | Clean, readable, idiomatic | Senior engineer approves without comments | Functional but messy |
| **Completeness** | All requirements addressed | Every requirement met, including implicit ones | Significant gaps |
| **Elegance** | Right abstraction, not over-engineered | Nothing to add, nothing to remove | Over- or under-abstracted |
| **Performance** | Efficient, no wasted resources | Optimal for the context | Noticeable issues |
| **Security** | No vulnerabilities, inputs validated | No attack surface added | Potential vulnerabilities |
| **User Intent Alignment** | Solves what was *meant*, not just literal | Spirit and letter nailed | Technically correct, misses the point |

## How It Works

1. **Score** — After completing work, Claude reviews each dimension and assigns a score
2. **Evaluate** — If the overall average is >= 9.5, the task is complete
3. **Improve** — If below 9.5, Claude identifies specific targeted fixes for each weak dimension
4. **Iterate** — With user approval, improvements are implemented and re-scored

The plugin also detects **red flags** like score inflation (every dimension 9.5+), N/A abuse, and lazy identical scores across dimensions.

## Installation

Clone this repo into your Claude Code plugins directory:

```bash
git clone https://github.com/augmentedivan/claude-self-score.git ~/.claude/plugins/self-score
```

That's it. The skill activates automatically when Claude is about to declare a task complete.

### Recommended: Add a CLAUDE.md enforcement rule

For stronger enforcement, add this line to your `~/.claude/CLAUDE.md` (or project-level `CLAUDE.md`):

```markdown
- **Self-score on completion** — invoke the `self-score` skill before declaring any task complete. Skip for non-task interactions (questions, explanations, lookups).
```

This makes the self-score step an explicit rule rather than relying solely on the skill's auto-trigger.

## Example Output

After completing a task, Claude produces a scorecard like this:

```
## Self-Score

| Dimension              | Score |
|------------------------|-------|
| Correctness            | 9.5   |
| Code Quality           | 8.5   |
| Completeness           | 9.5   |
| Elegance               | 9.0   |
| Performance            | N/A   |
| Security               | N/A   |
| User Intent Alignment  | 9.5   |
| **Overall**            | **9.2** |

### Improvements to reach 9.5+

| Dimension | Current | Target | What to change |
|-----------|---------|--------|----------------|
| Code Quality | 8.5 | 9.5 | Extract repeated validation logic into a shared helper |
| Elegance | 9.0 | 9.5 | Replace nested conditionals with early returns |

Want me to implement these improvements?
```

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI

## License

MIT

---

Created by [Ivan](https://github.com/augmentedivan)
