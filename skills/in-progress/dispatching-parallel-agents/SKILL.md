---
name: dispatching-parallel-agents
description: Split two or more independent problems across subagents that run at the same time, one per problem. Use when facing several failures or tasks in different files or subsystems that share no state and don't depend on each other.
---

# Dispatching Parallel Agents

When problems are unrelated (different test files, different subsystems, different bugs), working through them one by one wastes time. Dispatch one subagent per independent problem and let them run concurrently. Start each subagent with a fresh context window: it must not inherit this conversation. You write exactly the context it needs, which also keeps your own context free for coordinating.

## When it fits

Use it when:

- Several failures have different root causes.
- Each problem can be understood without the others.
- The agents won't touch the same files or resources.

Don't use it when:

- The failures might be related, so fixing one could fix the rest. Investigate them together first.
- You don't yet know what's broken. Explore before splitting.
- Understanding the problem needs a view of the whole system.
- The agents would edit the same files or share state. Run them one after another instead.

## The pattern

1. **Group by domain.** Sort the problems by what's broken, e.g. abort handling in one file, batch completion in another, approval races in a third. Check that fixing one can't affect another.
2. **Write one focused prompt per domain.** Each prompt gets:
   - **Scope:** one test file or subsystem.
   - **Context:** the failing test names and error messages, pasted in. The agent has none of yours.
   - **Goal:** e.g. make these tests pass.
   - **Constraints:** e.g. don't change production code, don't just raise timeouts.
   - **Output:** what to report back, e.g. root cause and what changed.
3. **Dispatch them all in the same response.** Several dispatch calls in one response run in parallel. One per response runs them one at a time.
4. **Review and integrate.** When they return:
   - Read each summary.
   - Check whether any two touched the same code.
   - Run the full test suite yourself.
   - Spot-check the diffs. Agents can make the same mistake systematically, and a report of success is not evidence of it.

## Example prompt

```markdown
Fix the 3 failing tests in src/agents/agent-tool-abort.test.ts:

1. "should abort tool with partial output capture": expects 'interrupted at' in message
2. "should handle mixed completed and aborted tools": fast tool aborted instead of completed
3. "should properly track pendingToolCount": expects 3 results but gets 0

These look like timing issues. Read the test file, work out whether each failure
is a timing problem or a real bug in the abort implementation, and fix the cause:
replace arbitrary timeouts with event-based waiting, or fix the bug. Don't just
increase timeouts. Don't edit files outside src/agents/.

Return: the root cause of each failure and what you changed.
```

## Common mistakes

| Mistake | Fix |
|---------|-----|
| "Fix all the tests": the agent gets lost | One file or subsystem per agent |
| "Fix the race condition": the agent doesn't know where | Paste the error messages and test names |
| No constraints: the agent refactors everything | Say what it must not touch |
| "Fix it": you can't tell what changed | Ask for the root cause and a list of changes |

---

Adapted from [obra/superpowers](https://github.com/obra/superpowers) (MIT, Copyright (c) 2025 Jesse Vincent).
