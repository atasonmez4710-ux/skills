---
name: receiving-code-review
description: Evaluate code review feedback against the codebase before acting on it, then fix or push back with reasons. Use when you receive review comments (from the user, a reviewer, a bot, or a PR thread), before implementing any suggestion, and especially when feedback seems unclear or technically questionable.
---

# Receiving Code Review

Review feedback calls for technical evaluation, not social performance. Verify before implementing, ask before assuming, and put technical correctness ahead of comfort.

## The response pattern

1. **Read** all the feedback before reacting.
2. **Restate** each item in your own words, or ask about it.
3. **Verify** it against the code as it actually is.
4. **Evaluate** whether it's sound for this codebase.
5. **Respond** with a technical acknowledgment or reasoned pushback.
6. **Implement** one item at a time, testing each.

## Unclear items block everything

If any item is unclear, stop and ask before implementing anything. Items are often related, and a partial understanding produces a wrong implementation.

> The user says "fix 1 to 6". You understand 1, 2, 3, and 6.
>
> Wrong: implement 1, 2, 3, and 6 now and ask about 4 and 5 later.
>
> Right: "I understand 1, 2, 3, and 6. I need clarification on 4 and 5 before I start."

## Weigh the source

**From the user:** trusted. Implement once you understand it, and still ask if the scope is unclear.

**From an external reviewer or bot:** check before implementing.

- Is it correct for this codebase, stack, and supported platforms?
- Does it break existing behaviour?
- Is there a reason the current code is the way it is?
- Does the reviewer have the full context?

If you can't easily verify it, say so: "I can't verify this without X. Should I investigate, ask, or proceed?" If it conflicts with a decision the user already made, stop and raise it with the user first.

## YAGNI check

When a reviewer asks to "implement this properly" (metrics, export, configurability), search the codebase for actual usage first. If nothing uses it: "Nothing calls this endpoint. Remove it instead?" If something does, implement it properly.

## Order of work

1. Clarify everything unclear.
2. Fix blocking issues (breakage, security).
3. Fix simple things (typos, imports).
4. Fix complex things (refactors, logic).
5. Test each fix on its own, then check for regressions.

## When to push back

Push back when a suggestion breaks existing behaviour, the reviewer lacks context, it adds an unused feature, it's wrong for this stack, legacy or compatibility constraints apply, or it contradicts the user's architectural decisions.

Push back with evidence: specific questions, the tests or code that show the problem, and the user brought in when it's architectural.

## How to respond

When feedback is right, state the fix and move on:

- "Fixed: \<what changed\>."
- "Good catch, \<specific issue\>. Fixed in \<location\>."
- Or just fix it and let the diff speak.

Skip performative agreement and gratitude ("You're absolutely right!", "Great point!", "Thanks for catching that!"). It says nothing the fix doesn't.

When you pushed back and turn out to be wrong, correct it factually: "You were right. I checked X and it does Y. Fixing now." No long apology, no defending the original pushback.

| Reviewer | Response |
|----------|----------|
| "Remove the legacy code" | "Checked: the build targets 10.15+, and this API needs 13+, so the legacy path is still needed. The bundle ID in it is wrong though. Fix that, or drop pre-13 support?" |
| "Add proper metrics tracking with a database, date filters, CSV export" | "Nothing calls this endpoint. Remove it instead, or is there a caller I'm missing?" |

## PR threads

When replying to inline review comments on GitHub, reply in that comment's thread, not as a top-level PR comment.

---

Adapted from [obra/superpowers](https://github.com/obra/superpowers) (MIT, Copyright (c) 2025 Jesse Vincent).
