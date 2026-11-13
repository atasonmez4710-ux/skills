---
name: verification-before-completion
description: Run the command that proves a claim before making it. Use when about to say work is complete, fixed, or passing, before committing, pushing, or opening a PR, before moving to the next task, and when a subagent reports success.
---

# Verification Before Completion

**Evidence before claims, always.** If you haven't run the verification command since your last change, you can't claim it passes. A paraphrase of a success claim is still a success claim.

## The gate

Before stating any status or expressing satisfaction:

1. **Identify** the command that proves the claim.
2. **Run** the full command, fresh and complete.
3. **Read** the whole output: exit code, failure count, warnings.
4. **Compare.** Does the output confirm the claim?
   - No: state the actual status, with the evidence.
   - Yes: state the claim, with the evidence.
5. Only then make the claim.

Skipping a step isn't verifying, it's guessing out loud.

## What counts as evidence

| Claim | Requires | Not sufficient |
|-------|----------|----------------|
| Tests pass | Test command output showing 0 failures | An earlier run, "should pass" |
| Linter clean | Linter output showing 0 errors | A partial check, extrapolation |
| Build succeeds | Build command exiting 0 | The linter passing, logs that look fine |
| Bug fixed | The original symptom, reproduced and now passing | The code changed, so it's assumed fixed |
| Regression test works | A red-green cycle you watched | The test passing once |
| Subagent finished | The VCS diff shows the changes | The subagent saying "done" |
| Requirements met | A line-by-line check against the spec | Tests passing |

A regression test only proves something if you saw it fail: write it, run it (passes), revert the fix, run it (must fail), restore the fix, run it (passes).

For requirements, re-read the spec or plan, turn it into a checklist, verify each line, then report either the gaps or completion.

## Stop signals

Stop and run the gate when you notice any of these:

- Hedged wording: "should", "probably", "seems to".
- Satisfaction before evidence: "Great!", "Done!", "Perfect!".
- About to commit, push, or open a PR.
- About to trust a subagent's report without checking its diff.
- Relying on a partial check because the full one is slow.
- "Just this once."

| Excuse | Reality |
|--------|---------|
| "Should work now" | Run it. |
| "I'm confident" | Confidence isn't evidence. |
| "The linter passed" | The linter doesn't compile or run anything. |
| "The subagent said it succeeded" | Check the diff and run the tests yourself. |
| "A partial check is enough" | A partial check proves only the part it checked. |

---

Adapted from [obra/superpowers](https://github.com/obra/superpowers) (MIT, Copyright (c) 2025 Jesse Vincent).
