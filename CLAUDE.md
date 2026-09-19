# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is Dr. Mike Hopper's personal `~/bin` directory: a small collection of standalone shell scripts (no build system, no tests, no dependencies to install). Scripts here are meant to be run directly from the shell (they're on `$PATH`), not imported as a library.

## Scripts

- `cnew <name>` — zsh script. Creates a new cmux workspace named `<name>` rooted at `~/Developer/2026`, focused. Simplest of the three; no project-folder scaffolding.
- `cnew-project <name>` — creates a new project workspace under `~/Developer/2026/projects/<name>` via `cnew-common`.
- `cnew-learn <name>` — same as above but under `~/Developer/2026/learn/projects/<name>`.
- `cnew-common <base_dir> <project_name>` — shared implementation used by `cnew-project` / `cnew-learn`. It:
  1. Creates `<base_dir>/<project_name>`.
  2. Marks that path as trusted in `~/.claude.json` (via `jq`, sets `hasTrustDialogAccepted: true`) so Claude Code skips its trust dialog there.
  3. Creates a new `cmux` workspace at that path, split into two panes (left/right).
  4. Runs `claude` in the left pane and `git init` in the right pane.
  5. Fires a `cmux notify` desktop notification when setup finishes.

All of `cnew`, `cnew-project`, and `cnew-learn` are thin wrappers; the actual logic to understand or modify lives in `cnew-common`.

## Key dependency: cmux

These scripts drive **cmux**, a terminal/workspace multiplexer app, via its CLI (`cmux workspace create`, `cmux send`, `cmux list-pane-surfaces`, `cmux new-split`, `cmux notify`). Workspace/pane targets are addressed by refs like `workspace:41` or `surface:86` (see `run_cmd` in `cnew-common` for the calling convention). Run `cmux --help` or `cmux guide` to inspect the current CLI surface before changing how these scripts invoke it — command flags are cmux's, not this repo's, and can change independently.

## Editing conventions

- Keep scripts POSIX/bash-compatible except `cnew`, which is intentionally zsh (uses zsh's `$1`/quoting behavior with cmux).
- `cnew-project` and `cnew-learn` should stay minimal — one line delegating to `cnew-common` with a different `base_dir`. Put shared behavior changes in `cnew-common`, not in the per-variant wrappers.
- No test suite exists; verify changes by running the script against a throwaway project name and confirming the cmux workspace/panes and `~/.claude.json` trust entry look right.
