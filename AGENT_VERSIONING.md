# Agent Versioning Workflow (ClaudeCode vs Codex)

Use this workflow to track and compare code written by each agent.

## Branch naming

Create agent-specific branches from the same base:

- `codex/<topic>`
- `claude/<topic>`

Example:

```bash
git new-agent-branch codex parser-cleanup main
git new-agent-branch claude parser-cleanup main
```

If you omit `[base]`, the current branch is used as the base.

## Commit labeling

This repo uses `.githooks/commit-msg` (enabled by `core.hooksPath=.githooks`).

On branches matching `codex/*` or `claude/*`, the hook will automatically:

- Prefix subject with `[codex]` or `[claude]`
- Add a trailer line: `Agent: codex` or `Agent: claude`

That gives you searchable provenance per commit.

## Compare agent output

Compare history unique to each branch:

```bash
git compare-agents codex/parser-cleanup claude/parser-cleanup
```

Compare exact code differences between branches:

```bash
git diff-agents codex/parser-cleanup claude/parser-cleanup
```

See all branches/merges visually:

```bash
git agent-graph
```

## Recommended flow per task

1. Pick a base branch (`main`, `local-patches`, etc.).
2. Create one branch per agent for the same topic.
3. Let each agent work only on its own branch.
4. Compare `codex/<topic>` vs `claude/<topic>` using aliases above.
5. Merge/cherry-pick the winner into your integration branch.

## One-time setup check

```bash
git config --get core.hooksPath
# expected: .githooks

git config --get-regexp '^alias\.(new-agent-branch|compare-agents|diff-agents|agent-graph)$'
```
