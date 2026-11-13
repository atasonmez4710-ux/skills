---
name: using-git-worktrees
description: Make sure work happens in an isolated workspace, preferring the harness's own worktree tool and falling back to `git worktree`. Use when starting feature work that should not touch the current checkout, before executing an implementation plan, or when the user asks for a worktree.
---

# Using Git Worktrees

Detect existing isolation first, then use the harness's native tool, then fall back to git. Never fight the harness.

## 1. Check for existing isolation

Before creating anything, check whether you're already in a linked worktree:

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
BRANCH=$(git branch --show-current)
```

`GIT_DIR != GIT_COMMON` is also true inside a git submodule, so rule that out:

```bash
# Prints a path when you're in a submodule: treat that as a normal checkout
git rev-parse --show-superproject-working-tree 2>/dev/null
```

- **Already in a linked worktree** (and not a submodule): don't create another. Report where you are (`Already isolated at <path> on branch <name>`, or note a detached HEAD that will need a branch at finish time) and skip to step 3.
- **In a normal checkout:** if the user's instructions already state a worktree preference, follow it. Otherwise ask: "Want me to set up an isolated worktree? It keeps your current branch untouched." If they decline, work in place and skip to step 3.

## 2. Create the workspace

### Native tool first

If you have a native way to create a worktree (a tool such as `EnterWorktree`, a `/worktree` command, a `--worktree` flag), use it and skip to step 3. The harness then owns placement, branching, and cleanup. Running `git worktree add` behind its back creates state it can't see or clean up.

### Git fallback

Only when no native tool exists. Pick the directory in this order:

1. A worktree directory the user's instructions name.
2. An existing `.worktrees/` at the project root, else an existing `worktrees/`.
3. Otherwise, `.worktrees/` at the project root.

A project-local directory must be git-ignored before you create anything in it, or the whole tree can end up committed:

```bash
git check-ignore -q .worktrees || git check-ignore -q worktrees
```

If it isn't ignored, add it to `.gitignore` and commit that first. Then:

```bash
path="$LOCATION/$BRANCH_NAME"
git worktree add "$path" -b "$BRANCH_NAME"
cd "$path"
```

If `git worktree add` fails with a permission error (a sandbox denial), tell the user the sandbox blocked it and carry on in the current directory.

## 3. Set up the project

Install dependencies the way the project does: check its lockfile, scripts, and contributor docs rather than guessing (`npm install` vs `pnpm install`, `poetry install` vs `pip install -r requirements.txt`, and so on). Skip this when there's nothing to install.

## 4. Verify a clean baseline

Run the project's test command before changing anything. A dirty baseline makes every later failure ambiguous.

- **Tests fail:** report the failures and ask whether to proceed or investigate. Proceeding past them is the user's call.
- **Tests pass:** report ready.

```
Worktree ready at <full-path>
Tests passing (<N> tests, 0 failures)
Ready to implement <feature>
```

---

Adapted from [obra/superpowers](https://github.com/obra/superpowers) (MIT, Copyright (c) 2025 Jesse Vincent).
