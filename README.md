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

# 2. Install the shell wrapper function (writes to .zshrc or .bashrc, auto-detected)
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

1. `hermes-gateway install` appends a shell function to your `.zshrc` or `.bashrc` (auto-detected based on `$SHELL`)
2. The function wraps the `hermes` binary — zero args triggers the selector, any args pass through untouched
3. Remote gateways launch via `HERMES_TUI_GATEWAY_URL` env var with `--tui`
4. Gateway definitions live in `~/.hermes/gateways.json`

The `install` command uses marker comments (`# >>> hermes-gateway-selector >>>` / `# <<< hermes-gateway-selector <<<`) in your rc file so `uninstall` cleanly removes only the wrapper function without touching the rest of your config.

## Dependencies

- [`jq`](https://jqlang.github.io/jq/) (preferred) or `python3` (fallback) for JSON parsing
- The script runs in bash; the shell wrapper works in both zsh and bash

## Notes

- The selector only appears for bare `hermes` (zero arguments). Any arguments at all pass straight to the real `hermes` binary — `hermes update`, `hermes setup`, `hermes --tui` all work as expected.
- Remote gateways always launch with `--tui` since they're headless connections.
- Gateway tokens and hostnames are stored locally in `~/.hermes/gateways.json` — never committed or shared.

## Shell Compatibility

The interactive `read` prompt uses `echo -n` + plain `read` for portability. This avoids two common pitfalls:

- `read -p "prompt" var` — **bash-only**, fails in zsh with "no coprocess"
- `read "var?prompt"` — **zsh-only**, fails in bash with "not a valid identifier"

The `echo -n` + `read` approach works identically in both shells.

## Uninstall

```bash
hermes-gateway uninstall
rm ~/.local/bin/hermes-gateway
rm ~/.hermes/gateways.json
```

## License

MIT
