---
name: hermes-gateway-selector
description: Manage and select from multiple Hermes Agent gateways. Add/remove remote agents, install a shell function that presents a selector when running bare `hermes`, and pass through all other commands unchanged.
category: software-development
triggers:
  - "hermes gateway"
  - "gateway selector"
  - "multiple hermes"
  - "remote gateway"
  - "switch gateway"
---

# Hermes Gateway Selector

Shell function + CLI tool for choosing between local and remote Hermes Agent gateways.

## How it works

- Running `hermes` (no args) → interactive selector menu
- Running `hermes update`, `hermes setup`, etc. → passes through to the real binary
- `hermes-gateway run NAME` → open TUI directly on a named gateway
- No gateways configured? Runs `hermes` directly — no selector
- Gateways are stored in `~/.hermes/gateways.json`
- Works with **zsh** and **bash**

## Install

### 1. Copy the script to your PATH

```bash
cp scripts/hermes-gateway ~/.local/bin/hermes-gateway
chmod +x ~/.local/bin/hermes-gateway
```

### 2. Install the shell function

```bash
hermes-gateway install
```

This writes the wrapper function to `~/.hermes/hermes-gateway.sh` and adds a single `source` line to your shell rc file — `.zshrc` or `.bashrc`, auto-detected based on `$SHELL`. Uses marker comments (`# >>> hermes-gateway-selector >>>` / `# <<< hermes-gateway-selector <<<`) so `uninstall` cleanly removes only the source line.

Restart your shell or `source` the rc file.

### 3. Add gateways

```bash
# Add a remote agent
hermes-gateway add "Agent Name" "ws://HOST:PORT/api/ws?token=YOUR_TOKEN"

# List configured gateways
hermes-gateway list

# Remove a gateway
hermes-gateway remove "Agent Name"
```

## Usage

```bash
# Interactive selector (bare hermes, at least one gateway configured)
hermes
#   1) Local
#   2) Agent Name
#
#   Select gateway [1]:

# Open TUI directly on a named gateway (skip the selector)
hermes-gateway run "Agent Name"

# All other hermes commands pass through untouched
hermes update
hermes setup
hermes --tui
```

## Notes

- The selector only appears for bare `hermes` (zero arguments). Any arguments at all pass straight to the real `hermes` binary.
- `hermes-gateway run NAME [args]` connects directly to a named gateway via `--tui`, no selector needed. Pass hermes args directly (no `--` separator):
  - `hermes-gateway run Hugh` — open TUI
  - `hermes-gateway run Hugh --continue` — continue last session
  - `hermes-gateway run Hugh --resume SESSION_ID` — resume specific session
- Remote gateways always launch with `--tui` since they're headless connections.
- Gateway tokens and hostnames are stored locally in `~/.hermes/gateways.json` — never committed or shared.

## Uninstall

```bash
hermes-gateway uninstall
rm ~/.local/bin/hermes-gateway
rm ~/.hermes/gateways.json
```

## Dependencies

- `jq` (preferred) or `python3` (fallback) for JSON parsing
- `bash` (the script itself) — the shell wrapper works in both zsh and bash

## Shell Compatibility

The interactive `read` prompt uses `echo -n` + plain `read` for portability. This avoids two common pitfalls:

- `read -p "prompt" var` — **bash-only**, fails in zsh with "no coprocess"
- `read "var?prompt"` — **zsh-only**, fails in bash with "not a valid identifier"
