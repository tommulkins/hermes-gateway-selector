# Hermes Gateway Selector

Shell function + CLI tool for choosing between local and remote [Hermes Agent](https://hermes-agent.nousresearch.com) gateways from your terminal.

```
$ hermes
  1) Local
  2) Work Laptop
  3) Home Server

  Select gateway [1]:
```

- Running `hermes` (no args) → interactive selector
- Running `hermes update`, `hermes setup`, etc. → passes through to the real binary
- Works with **zsh** and **bash**

## Quick Start

```bash
# 1. Download the script
curl -fsSL https://raw.githubusercontent.com/tommulkins/hermes-gateway-selector/main/scripts/hermes-gateway -o ~/.local/bin/hermes-gateway
chmod +x ~/.local/bin/hermes-gateway

# 2. Install the shell wrapper function
hermes-gateway install

# 3. Add your remote gateways
hermes-gateway add "Work Laptop" "ws://192.168.1.100:9119/api/ws?token=YOUR_TOKEN"
hermes-gateway add "Home Server" "ws://10.0.0.5:9119/api/ws?token=YOUR_TOKEN"

# 4. Restart your shell, then just type `hermes`
source ~/.zshrc  # or ~/.bashrc
hermes
```

## Commands

| Command | Description |
|---|---|
| `hermes-gateway` | Interactive selector (called by shell function) |
| `hermes-gateway list` | List configured gateways |
| `hermes-gateway add NAME URL` | Add a gateway |
| `hermes-gateway remove NAME` | Remove a gateway |
| `hermes-gateway install` | Write shell function to your rc file |
| `hermes-gateway uninstall` | Remove shell function from your rc file |

## How It Works

1. `hermes-gateway install` appends a shell function to your `.zshrc` or `.bashrc` (auto-detected)
2. The function wraps the `hermes` binary — zero args triggers the selector, any args pass through untouched
3. Remote gateways launch via `HERMES_TUI_GATEWAY_URL` env var with `--tui`
4. Gateway definitions live in `~/.hermes/gateways.json`

## Dependencies

- [`jq`](https://jqlang.github.io/jq/) (preferred) or `python3` (fallback) for JSON parsing
- `bash` — the script runs in bash, but the shell wrapper works in both zsh and bash

## Uninstall

```bash
hermes-gateway uninstall
rm ~/.local/bin/hermes-gateway
rm ~/.hermes/gateways.json
```

## License

MIT
