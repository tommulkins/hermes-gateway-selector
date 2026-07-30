# Contributing

## Repo structure

```
hermes-gateway-selector/
├── skills/
│   └── hermes-gateway-selector/
│       ├── SKILL.md              # skill instructions
│       └── scripts/
│           └── hermes-gateway    # the bash script
├── README.md
├── CONTRIBUTING.md
└── LICENSE
```

Taps discover skills by listing subdirectories under `skills/` and probing each for `SKILL.md`.

## Distribution channels

This skill is distributed three ways:

1. **skills.sh** — cross-agent directory (Hermes, Claude Code, Cursor, Codex, 15+ agents)
2. **Hermes tap** — `hermes skills tap add tommulkins/hermes-gateway-selector`
3. **Manual** — curl the script directly from GitHub

## Publishing updates

Push changes to `main`. Users get updates via `hermes skills update`.

To refresh the skills.sh listing after pushing:

```bash
cd /tmp && npx skills add tommulkins/hermes-gateway-selector --yes
```

## Scanner compliance

The Hermes skill scanner checks for security-sensitive patterns before allowing install. To pass cleanly:

- **No real credentials in examples** — use placeholders like `YOUR_PASSWORD`, `your-username`, `192.168.1.100`. The scanner flags environment variable names that look like real service credentials as CRITICAL exfiltration.
- **Keep images under 1MB** — large assets are flagged as MEDIUM structural bloat.
- **Shell rc modifications are expected** — the scanner flags `.zshrc`/`.bashrc` edits as MEDIUM persistence. This is inherent to the tool and won't block install.
- **Network references in docs are expected** — WebSocket URLs, tunnel tools (Tailscale, ngrok) are flagged at MEDIUM/HIGH. Informational only.

## Testing changes locally

```bash
# Run the script directly
./skills/hermes-gateway-selector/scripts/hermes-gateway list

# Test the install flow in a clean shell
bash -c 'source ~/.hermes/hermes-gateway.sh && hermes'
```
