# VimMux

A Vim-like tmux configuration, ported from [Omarchy](https://learn.omacom.io/2/the-omarchy-manual/53/hotkeys#tmux) to Ubuntu/Debian.

- Vim muscle memory: `Prefix + v` splits beside, `Prefix + h` splits below, `Prefix + x` kills.
- Prefix-free chords: `Alt`, `Ctrl + Alt` and `Ctrl + Alt + Shift` for panes, windows and sessions.
- Vi copy mode, mouse support, system clipboard, status bar on top.

# Getting Started

## Requirements


| Component | Version |
| --- | --- |
| tmux | 3.2+ (Ubuntu 22.04+, Debian 12+) — needs `terminal-features` and `extended-keys` |
| Terminal | 256 colors, `tmux-256color` terminfo (bundled with tmux 3.2+) |

## Install (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install -y tmux

git clone https://github.com/CeleronD256ram/VimMux.git ~/.config/tmux
tmux new -s main
```

Keep the repository elsewhere (dotfiles, work tree) and link it in:

```bash
mkdir -p ~/.config/tmux
ln -s ~/code/VimMux/tmux.conf ~/.config/tmux/tmux.conf
```

Reload a running tmux with `Prefix + q`, or from the shell:

```bash
tmux source-file ~/.config/tmux/tmux.conf
```

### Start on login (optional)

```ini
# ~/.config/systemd/user/tmux.service
[Unit]
Description=tmux main session
After=graphical-session.target

[Service]
Type=forking
ExecStart=/usr/bin/tmux new-session -A -s main
ExecStop=/usr/bin/tmux kill-session -t main

[Install]
WantedBy=default.target
```

```bash
systemctl --user daemon-reload
systemctl --user enable --now tmux.service
```

## Cheatsheet

`Prefix` is `Ctrl + Space`. `Ctrl + B` works as a secondary prefix for every prefixed key.

### Prefix

| Key | Action |
| --- | --- |
| `Ctrl + Space` | Prefix (primary) |
| `Ctrl + B` | Secondary prefix |
| `Prefix`, `Prefix` | Send a literal prefix to the foreground program |

### Panes

| Key | Action |
| --- | --- |
| `Prefix + v` | Split pane beside you (vertical divider, like `:vsp`) |
| `Prefix + h` | Split pane below (horizontal divider, like `:sp`) |
| `Prefix + x` | Kill pane |
| `Prefix + z` | Toggle pane zoom |
| `Prefix + o` | Next pane |
| `Prefix + Up/Down/Left/Right` | Focus pane in that direction |
| `Prefix + {` / `Prefix + }` | Swap current pane up / down |
| `Prefix + C-Arrow` | Resize pane by 1 |
| `Prefix + M-Arrow` | Resize pane by 5 |
| `Alt + Enter` | Split pane beside (no prefix) |
| `Alt + Shift + Enter` | Split pane below (no prefix) |
| `Alt + Escape` | Kill pane (no prefix) |
| `Ctrl + Alt + Arrows` | Focus pane (no prefix) |
| `Ctrl + Alt + Shift + Arrows` | Resize pane by 5 (no prefix) |

### Windows

| Key | Action |
| --- | --- |
| `Prefix + c` | New window (opens in the current directory) |
| `Prefix + k` | Kill window |
| `Prefix + r` | Rename window (prompt prefilled with the current name) |
| `Prefix + 1` … `Prefix + 9` | Go to window N (numbering starts at 1) |
| `Prefix + n` / `Prefix + p` | Next / previous window |
| `Prefix + f` | Find window by name |
| `Prefix + &` | Kill window with confirmation |
| `Prefix + w` | Window list |
| `Prefix + Space` | Cycle through layouts |
| `Prefix + M-1` … `Prefix + M-5` | even-horizontal, even-vertical, main-horizontal, main-vertical, tiled |
| `Alt + 1` … `Alt + 9` | Go to window N (no prefix) |
| `Alt + Left` / `Alt + Right` | Previous / next window (no prefix) |
| `Alt + Shift + Left` / `Alt + Shift + Right` | Move window left / right (no prefix) |

### Sessions

| Key | Action |
| --- | --- |
| `Prefix + C` | New session (in the current directory) |
| `Prefix + K` | Kill session |
| `Prefix + R` | Rename session |
| `Prefix + N` / `Prefix + P` | Next / previous session |
| `Prefix + L` | Last session |
| `Prefix + s` | Session list |
| `Prefix + d` | Detach from the session |
| `Alt + Up` / `Alt + Down` | Previous / next session (no prefix) |

### Copy mode (vi)

| Key | Action |
| --- | --- |
| `Prefix + [` | Enter copy mode |
| `v` | Begin selection (in copy mode) |
| `y` | Copy selection and exit copy mode |
| `q` | Cancel selection (in copy mode) |
| `Prefix + ]` | Paste buffer |
| `Prefix + #` | Paste buffer list |
| Mouse drag | Select text (mouse is on) |
| Mouse wheel | Scroll through history |

### Config and meta

| Key | Action |
| --- | --- |
| `Prefix + q` | Reload this config file |
| `Prefix + ?` | List every key binding |
| `Prefix + /` | Search key bindings |
| `Prefix + :` | Command prompt |
| `Prefix + i` | Session info |
| `Prefix + C-z` | Suspend client (shell job control) |

### Shell commands

| Command | Action |
| --- | --- |
| `tmux new -s main` | Create session `main` and attach |
| `tmux ls` | List sessions |
| `tmux attach -t main` | Attach to a session |
| `tmux kill-session -t main` | Kill a session |
| `tmux rename-session work` | Rename the current session |
| `tmux source-file ~/.config/tmux/tmux.conf` | Reload the config |
| `tmux list-keys` | Dump all key bindings |

### Options in this config

| Option | Effect |
| --- | --- |
| `mouse on` | Click, drag-select and scroll anywhere |
| `set-clipboard on` | Copy/paste through the system clipboard |
| `mode-keys vi` | Copy mode driven by vi keys |
| `base-index 1` / `pane-base-index 1` | Windows and panes are numbered from 1 |
| `renumber-windows on` | Window numbers never leave gaps |
| `status-position top` | Status bar on top (session, windows, host) |
| `default-terminal tmux-256color` | 256-color programs |
| `history-limit 50000` | 50k lines of scrollback per pane |
| `focus-events on` | Programs know when a pane gains focus |
| `extended-keys on` | Correct reporting of Shift/Alt/Super combinations |

## Porting notes

- The Omarchy `Prefix + ?` popup was dropped: it shelled out to `omarchy-menu-tmux-keybindings`, which does not exist outside Omarchy. `Prefix + ?` now falls back to tmux's own binding list.
- No dependency on Hyprland, kitty or any Omarchy script — the theme uses plain ANSI colors, so it works in any terminal.
- `Prefix + q` sources `~/.config/tmux/tmux.conf`, so keep the file at that path.
- `extended-keys` is only advertised to terminals that support it (for example kitty). Elsewhere `Ctrl + Alt + Shift + Arrows` may not reach tmux; every other binding is unaffected.

## Credits

Key bindings follow the [Omarchy manual](https://learn.omacom.io/2/the-omarchy-manual/53/hotkeys#tmux) (DHH).

## License

[GPL-3.0](LICENSE).
