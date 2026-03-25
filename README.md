# linear-browse

Interactive [Linear](https://linear.app) issue browser for the terminal. Built on [fzf](https://github.com/junegunn/fzf) and [linear-cli](https://github.com/schpet/linear-cli).

Browse issues, filter by team/project/status, preview details with rendered markdown — all without leaving the terminal.

## Features

- **Fuzzy search** across all issues in your workspace
- **Live preview** with rendered markdown (via [glow](https://github.com/charmbracelet/glow))
- **Filter by state** — started, unstarted, or all
- **Filter by project** — interactive project picker
- **Switch teams** — interactive team picker
- **Open in browser** — jump to any issue in Linear
- **tmux integration** — works great as a popup

## Requirements

| Dependency | Required | Install |
|------------|----------|---------|
| [linear-cli](https://github.com/schpet/linear-cli) | Yes | `brew install schpet/tap/linear` |
| [fzf](https://github.com/junegunn/fzf) | Yes (≥0.54) | `brew install fzf` |
| [jq](https://github.com/jqlang/jq) | Yes | `brew install jq` |
| [perl](https://www.perl.org/) | Yes | Pre-installed on macOS/Linux |
| [glow](https://github.com/charmbracelet/glow) | No | `brew install glow` |

Without glow, the preview shows raw markdown.

## Install

```sh
# Download the script
curl -fsSL https://raw.githubusercontent.com/nichochar/linear-browse/main/linear-browse \
  -o /usr/local/bin/linear-browse
chmod +x /usr/local/bin/linear-browse

# Or clone the repo
git clone https://github.com/nichochar/linear-browse.git
cd linear-browse
cp linear-browse /usr/local/bin/
```

## Setup

1. **Authenticate** with Linear (if you haven't already):

```sh
linear auth login
```

2. **(Optional)** Set your default team in `.linear.toml`:

```sh
linear config   # interactive setup
```

Or create one manually in your project directory or `$HOME`:

```toml
team_id = "ENG"
```

3. **Run it:**

```sh
linear-browse
```

## Keybindings

| Key | Action |
|-----|--------|
| `Enter` | Open issue in browser |
| `Ctrl-V` | View full issue details |
| `Ctrl-P` | Filter by project |
| `Ctrl-T` | Switch team |
| `Ctrl-S` | Show started issues |
| `Ctrl-U` | Show unstarted issues |
| `Ctrl-A` | Show all issues |
| `Ctrl-R` | Reset all filters |
| `Esc` | Exit |

## tmux integration

Add a keybinding to your `tmux.conf` to open linear-browse in a popup:

```sh
bind-key l display-popup -E -w 90% -h 80% "linear-browse"
```

Then press `prefix + l` to launch.

## Configuration

linear-browse reads your default team from `.linear.toml` (checked in the current directory first, then `$HOME`). This is the same config file used by [linear-cli](https://github.com/schpet/linear-cli).

If no config is found and your workspace has multiple teams, you'll be prompted to pick one on first launch.

Team and project selections are persisted for the session in `$TMPDIR` and reset with `Ctrl-R`.

## License

MIT
