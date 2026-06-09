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

This writes a wrapper function to your shell rc file (`.zshrc` or `.bashrc` automatically detected). Restart your shell or `source` the rc file.

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
# Interactive selector (bare hermes)
hermes
#   1) Local
#   2) Agent Name
#
#   Select gateway [1]:

# All other commands pass through untouched
hermes update
hermes setup
hermes --tui
```

## Uninstall

```bash
hermes-gateway uninstall
rm ~/.local/bin/hermes-gateway
rm ~/.hermes/gateways.json
```

## Dependencies

- `jq` (preferred) or `python3` (fallback) for JSON parsing
- `bash` (the script itself) — works inside both zsh and bash shells

## Notes

- The selector only appears for bare `hermes` (zero arguments). Any arguments at all pass straight to the real `hermes` binary.
- Remote gateways always launch with `--tui` since they're headless connections.
- Gateway tokens and hostnames are stored locally in `~/.hermes/gateways.json` — never committed or shared.
