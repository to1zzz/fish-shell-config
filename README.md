# Fish + Reef DevOps Setup

A minimal, fast Fish shell setup for DevOps work on Gentoo Linux.  
Includes Bash compatibility via Reef, fuzzy finding, smart directory jumping, and contextual shell history.

## Prerequisites

- Gentoo Linux (adjust package manager commands for other distros)
- Fish shell installed
- Rust/Cargo installed (`cargo` available)

## 1. Add Cargo binaries to PATH

```fish
fish_add_path ~/.cargo/bin
```

## 2. Install system packages

```fish
sudo emerge --sync
sudo emerge --ask sys-apps/ripgrep sys-apps/fd sys-apps/bat sys-apps/eza sys-block/dust sys-process/procs app-shells/zoxide sys-apps/sd app-shells/fzf
```

> `fzf` is the binary required by `fzf.fish`.  
> `ripgrep`, `fd`, and `bat` are used by `reef-tools` wrappers.

## 3. Install Reef (Bash compatibility)

```fish
cargo install reef-shell

git clone https://github.com/ZStud/reef.git /tmp/reef
fish /tmp/reef/fish/install.fish --tools
rm -rf /tmp/reef

exec fish

reef persist state
reef confirm on
```

Verify:

```fish
reef status
```

Expected output similar to:

```
reef: enabled (display: bash, history: bash, persist: state, confirm: on)
```

## 4. Install Fisher plugins

```fish
fisher install PatrickF1/fzf.fish
```

## 5. Install Atuin (smart history)

```fish
cargo install --locked atuin
```

> `zoxide` is already installed via `emerge`. Do not install it again with Cargo unless you specifically want the latest version.

## 6. Configure `~/.config/fish/config.fish`

Add the following lines:

```fish
# Smart directory jumping
zoxide init fish | source

# Context-aware shell history
atuin init fish | source
```

## 7. Activate

```fish
exec fish
```

## 8. Verify everything

```fish
type -a reef
reef status
which fzf
z --help
atuin --version
```

## Daily usage cheat sheet

| Tool | Action |
| :--- | :--- |
| `Ctrl+R` | Fuzzy search shell history (fzf.fish / Atuin) |
| `Ctrl+Alt+F` | Fuzzy find files |
| `Ctrl+Alt+L` | Fuzzy search Git log |
| `Ctrl+Alt+S` | Fuzzy search Git status |
| `Ctrl+Alt+P` | Fuzzy search processes |
| `z <partial-path>` | Jump to frequently used directory |
| `zi <partial-path>` | Interactive directory picker |
| `reef confirm off` | Disable confirmation once familiar with Reef |

## Optional: better `man` pages with bat

Add to `config.fish`:

```fish
set -gx MANPAGER "sh -c 'col -bx | bat -l man -p'"
```

## Notes

- Reef translates Bash syntax on the fly. `reef persist state` keeps exported variables across commands with minimal overhead.
- `reef confirm on` shows how Reef will translate or execute a command before running it. Turn it off later with `reef confirm off`.
- The shell prompt is managed by your custom `fish_prompt` function defined in `~/.config/fish/config.fish`.
- `reef-tools` installs wrappers for `grep`, `find`, `cat`, etc. They fall back to the original utilities if the modern replacements are missing.
