# Security Policy

OKKI Claw keeps local OpenClaw credentials on your device. The relay layer should not receive local OpenClaw tokens, private keys, or `.env` files.

## Reporting

Please do not disclose vulnerabilities publicly before they are triaged. Open a private security advisory on GitHub, or contact the maintainers through the support channel listed in the latest release notes.

## Sensitive Data

When reporting issues, remove:

- `OPENCLAW_GATEWAY_TOKEN`
- `DEVICE_TOKEN`
- `AGENT_RELAY_CONFIG_PATH` contents
- `~/.openclaw/identity/*`
- `.env` files
- Local filesystem paths that reveal private project names
