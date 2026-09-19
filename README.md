# bin

Personal `~/bin` scripts for spinning up new project workspaces in [cmux](https://cmux.app) with Claude Code ready to go.

## Requirements

- [cmux](https://cmux.app) (provides the `cmux` CLI)
- `jq`
- [Claude Code](https://claude.com/claude-code) (`claude` on `$PATH`)

## Scripts

### `cnew <name>`

Creates a new cmux workspace named `<name>` rooted at `~/Developer/2026`, focused. No project folder or panes are created — just a bare workspace at that directory.

```sh
cnew scratch
```

### `cnew-project <name>`

Scaffolds a new project at `~/Developer/2026/projects/<name>`:

1. Creates the project directory.
2. Marks it as trusted in `~/.claude.json` so Claude Code won't prompt with its trust dialog.
3. Opens a new cmux workspace at that path, split into two panes.
4. Runs `claude` in the left pane and `git init` in the right pane.
5. Sends a desktop notification when it's done.

```sh
cnew-project my-app
```

### `cnew-learn <name>`

Same as `cnew-project`, but scaffolds under `~/Developer/2026/learn/projects/<name>` instead.

```sh
cnew-learn algorithms-practice
```

### `cnew-common`

Shared implementation behind `cnew-project` and `cnew-learn`. Not intended to be run directly:

```sh
cnew-common <base_dir> <project_name>
```
