# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A dotfiles repository originally forked from [mijndert/dotfiles](https://github.com/mijndert/dotfiles). It manages shell configuration, terminal tooling, and application settings for a macOS development machine.

## Installation

```bash
./install.sh
```

The script: installs Homebrew if missing, runs `brew bundle` from `Brewfile`, registers zsh as a login shell, clones zsh plugins, sets up vim-plug, symlinks all dotfiles into `$HOME`, and installs themes/configs for k9s, ghostty, and bat.

To update Homebrew packages (alias defined in `.alias`):
```bash
brewup
```

## File Map

| File | Purpose |
|------|---------|
| `.zshrc` | Zsh entry point — sets env vars, completions, loads plugins, sources `.alias` and `.functions` |
| `.alias` | Shell aliases |
| `.functions` | Shell functions (Docker helpers, kubectl lazy-load, `batdiff`, `eks-auth`, AWS SSO helpers) |
| `.gitconfig` | Git config — includes conditional identity includes for `~/dev/personal/` vs `~/dev/work/` |
| `.gitconfig-personal` | Personal git identity |
| `.gitconfig-work` | Work git identity |
| `.tmux.conf` | Tmux config — Catppuccin Mocha status bar, TPM plugins (resurrect + continuum), key bindings |
| `ghostty_config` | Ghostty terminal config (theme, font, keymap) |
| `Brewfile` | All Homebrew packages and casks |
| `k9s_config.yml` | k9s Kubernetes TUI config |
| `vscode_settings.json` | VS Code user settings |
| `config` | SSH client config |
| `.vimrc` | Vim config with vim-plug |

## Key Design Decisions

**Git identity switching**: `.gitconfig` uses `includeIf "gitdir:..."` to automatically switch between personal and work identities based on repo path. Personal repos live under `~/dev/personal/`, work repos under `~/dev/work/`.

**Tool replacements**: Several standard tools are replaced with modern alternatives. Don't revert these:
- `cat` → `bat` (with `-p` flag in alias to suppress line numbers)
- `ls` → plain `ls` with color flags (note: the active system may have `lsd` from the yadm dotfiles, but this repo uses standard `ls`)

**Zsh plugins**: Loaded via manual clone into `~/.zsh/` — no plugin manager (no oh-my-zsh, no zinit). The three plugins are: `fzf-tab`, `zsh-autosuggestions`, `zsh-syntax-highlighting`.

**kubectl completion**: Lazy-loaded inside a wrapper function in `.functions` to avoid slow startup.

**AWS SSO**: `aws-sso-profile` / `aws-sso-clear` functions wrap `aws-sso-cli` for credential injection; completions are wired via `compdef`.

**Tmux**: Uses TPM for plugin management. Key bindings: `\` = vertical split, `-` = horizontal split, `Shift+Left/Right` = window navigation, `r` = reload config. Sessions auto-save every 5 minutes via `tmux-continuum`.

## Adding New Packages

Add to `Brewfile`, then run `brew bundle` (or `brewup` to also update and clean).

## Adding New Symlinks

Add the filename to the `for f in ...` loop in `install.sh` and create the file in the repo root.
