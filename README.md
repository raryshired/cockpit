# focus-mode

A single command to launch a full development workspace in tmux — editor, shell, and Claude side by side.

## What it does

`repo-work` reads a YAML config, clones or navigates to a repo, and opens a tmux session with three panes:

1. **Neovim** — opens the project root
2. **Shell** — a zsh pane for running commands
3. **Claude** — an AI assistant pane

If the session already exists, it reattaches instead of creating a duplicate.

## Quick start

```bash
# 1. Copy the example config
mkdir -p ~/config
cp config/workspace.yaml ~/config/workspace.yaml

# 2. Edit it with your repos
vim ~/config/workspace.yaml

# 3. Run it
bin/repo-work myapp
```

## Configuration

The workspace config lives at `~/config/workspace.yaml` by default.

```yaml
version: 1

paths:
  repos: /Users/yourname/repos

defaults:
  branch: develop

repos:
  myapp:
    type: git
    url: git@github.com:yourname/myapp.git
    path: /Users/yourname/repos/myapp

  notes:
    type: path
    path: /Users/yourname/repos/notes
```

### Repo types

| Type   | Behavior |
|--------|----------|
| `git`  | Clones if missing, fetches, checks out the default branch, and pulls |
| `path` | Creates the directory if missing, then opens it directly |

## Usage

```
repo-work <key> [--layout auto|horizontal|vertical]
```

- `<key>` — a repo key from your `workspace.yaml`
- `--layout` — pane arrangement (default: `auto`)
  - `auto` — picks horizontal or vertical based on terminal dimensions
  - `horizontal` — editor left, shell + claude stacked right
  - `vertical` — editor top, shell middle, claude bottom

## Environment variables

| Variable         | Default                                  | Description |
|------------------|------------------------------------------|-------------|
| `WORKSPACE_YAML` | `~/config/workspace.yaml`                | Path to the config file |
| `LAYOUT`         | `auto`                                   | Default layout |
| `SHELL_CMD`      | `zsh`                                    | Shell to launch in the shell pane |
| `YQ_BIN`         | `yq`                                     | Path to the `yq` binary |

## Dependencies

The following are auto-installed via Homebrew if missing:

- `git`
- `tmux`
- `zsh`
- `neovim`
- `yq`

**Claude Code** must be installed separately before use. See the [Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code) for installation instructions.

## License

MIT
