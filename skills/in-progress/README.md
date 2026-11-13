# In Progress

Beta. These skills are public on purpose: try them and tell me what breaks. They're excluded from the plugin and the top-level README until they graduate to a stable bucket, they get no docs pages, and they can change or disappear without warning.

The plugin won't give you these. Install one directly:

```bash
npx skills@latest add mattpocock/skills --skill=<name>
```

- **[loop-me](./loop-me/SKILL.md)**: Grill yourself into implementable workflow specs over multiple sessions, using the current directory as a stateful workspace. User-invoked.
- **[writing-beats](./writing-beats/SKILL.md)**: Shape an article as a journey of beats, choose-your-own-adventure style. Pick a starting beat, write only that beat, then pivot to the next, until the article reaches a natural end.
- **[writing-fragments](./writing-fragments/SKILL.md)**: Grilling session that mines you for fragments (heterogeneous nuggets of writing) and appends them to a single document as raw material for a future article.
- **[writing-shape](./writing-shape/SKILL.md)**: Take a markdown file of raw material and shape it into an article paragraph by paragraph, arguing format choices at each step.
- **[claude-handoff](./claude-handoff/SKILL.md)**: Hand the current conversation off to a fresh background agent that picks up the work immediately, seeded with a handoff summary via `claude --bg`. User-invoked.
- **[setup-ts-deep-modules](./setup-ts-deep-modules/SKILL.md)**: Wire dependency-cruiser into a TypeScript repo so each package is a deep module: implementation hidden in subfolders, reachable only through its entry-point files, tests exercising it through those. User-invoked.
- **[software-factory](./software-factory/SKILL.md)**: Shared language for designing a software factory: the tasks it runs and the services it plugs into. Call it alongside a grilling session. User-invoked.
- **[verification-before-completion](./verification-before-completion/SKILL.md)**: Run the command that proves a claim before saying work is done, fixed, or passing. Adapted from obra/superpowers.
- **[using-git-worktrees](./using-git-worktrees/SKILL.md)**: Make sure work happens in an isolated workspace, preferring the harness's own worktree tool over `git worktree`. Adapted from obra/superpowers.
- **[dispatching-parallel-agents](./dispatching-parallel-agents/SKILL.md)**: Split independent problems across subagents that run at the same time, one per problem. Adapted from obra/superpowers.
- **[receiving-code-review](./receiving-code-review/SKILL.md)**: Check review feedback against the codebase before acting on it, then fix or push back with reasons. Adapted from obra/superpowers.
