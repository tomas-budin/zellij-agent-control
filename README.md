# zellij-agent-control

> **Disclaimer:** this whole repo is vibecoded. An AI wrote it, a human skimmed it.

Everything that turns zellij into the control center for coding agents: zellij config and layouts, the `zj` command, shell aliases, Ghostty keys, and the hook wiring for Claude Code and Codex.

## Layout

| Path | What |
|---|---|
| `config.kdl` | zellij config and keybinds |
| `layouts/` | session layouts. `worktree-*.kdl` are written by `zj wt` and not tracked |
| `session-names` | labels shown by the session switcher |
| `bin/zj` | dispatcher. `zj <cmd>` runs `bin/zj-<cmd>` with `ZJ_HOME` set to this directory |
| `bin/zj-*` | the commands, see below |
| `hooks/claude.json` | Claude Code hook entries. `zj install` merges them into `~/.claude/settings.json` |
| `hooks/codex.json` | Codex hooks file. `zj install` symlinks `~/.codex/hooks.json` to it |
| `zellij.zsh` | shell aliases, sourced from `.zshrc` |
| `ghostty-keys` | Ghostty keybinds that send zellij's Alt chords, included from the Ghostty config |

## Commands

| Command | Called from | Does |
|---|---|---|
| `zj sessions` | Ctrl Space | fzf session switcher with agent badges |
| `zj repo-tab` | cmd+o, Alt r | open a repo under `$ZJ_WORKTREE` as a tab running claude |
| `zj idea-focus [dir]` | cmd+i, Alt g, `i` | focus the JetBrains window for the project |
| `zj wt <action> <name>` | `wt` | create, extend, delete, or branch a worktree set and its session |
| `zj create-branch [--main] <parts...>` | `nb`, `nbm`, `zj wt branch` | create or switch to a branch |
| `zj ignite` | `ignite` | start a session per layout, attach base-repos |
| `zj notes` | `notes` | start and attach the notes session |
| `zj agent-state hook <claude\|codex>` | agent hooks | record running and waiting agents per zellij session |
| `zj agent-state summary` | `zj sessions` | one line per session: running, waiting |
| `zj agent-state list` | you | one line per live agent |
| `zj tab-title` | Claude UserPromptSubmit | append the git branch to the tab name |
| `zj install [--check]` | you | write or verify the hook wiring |

## Agent state flow

Hook event, then `zj agent-state hook`, then a state file under `~/.local/state/agent-state/`, then `zj agent-state summary`, then badges in `zj sessions`.

## Bootstrap

```sh
git clone git@github.com:tomas-budin/zellij-agent-control.git ~/.config/zellij
ln -s ~/.config/zellij/bin/zj ~/.local/bin/zj
echo 'source ~/.config/zellij/zellij.zsh' >> ~/.zshrc
echo 'config-file = ../zellij/ghostty-keys' >> ~/.config/ghostty/config
zj install
```

Codex asks to trust each hook once after `zj install` changes a command.
