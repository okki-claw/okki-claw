# OKKI Claw

OKKI Claw is a local sidecar that connects OpenClaw and local agent (Claude Code, Opencode, Codex, Oh My PI...) workflows to OKKI Claw agent relay, so you can continue working with your local agents from another device without setting up manual tunneling.

Language: English | [简体中文](README.zh-CN.md)

<p align="left">
  <a href="https://agent-relay.okki.ai">Website</a>
  ·
  <a href="https://agent-relay.okki.com">Chinese Website</a>
  ·
  <a href="https://github.com/okki-claw/okki-claw/releases/latest">Download latest release</a>
  ·
  <a href="https://github.com/okki-claw/okki-claw/issues">Report an issue</a>
</p>

## What It Does

OKKI Claw runs on the same machine as your local OpenClaw gateway. It creates an authenticated outbound connection to OKKI Claw agent relay and forwards approved agent traffic through that relay.

- Access local OpenClaw sessions from another laptop, tablet, or mobile browser.
- Avoid manual ngrok, Tailscale, reverse proxy, or router port forwarding setup.
- Keep OpenClaw gateway credentials and device identity on your local machine.
- Use a release package without exposing OKKI Claw source code in this public repository.

## Official Websites

- Global: <https://agent-relay.okki.ai>
- Chinese: <https://agent-relay.okki.com>

## Repository Scope

This public repository is a distribution repository. It intentionally contains only public-facing materials:

- Installation and usage documentation.
- Release notes and download links.
- GitHub Release artifacts such as `okki-claw-*.tgz`.
- Security reporting guidance.

The source code, build pipeline, private configuration, and signing/release credentials are kept in a separate private repository.

## How It Works

```text
Remote browser / OKKI Claw App
        |
        | HTTPS / WebSocket
        v
OKKI Claw cloud relay
        |
        | authenticated outbound tunnel
        v
OKKI Claw sidecar on your local machine
        |
        | local gateway connection
        v
OpenClaw gateway and local agent workflows
```

The relay is not intended to receive your local OpenClaw credentials. OKKI Claw discovers or reads local gateway configuration on your device and uses it locally.

## Requirements

- Node.js 22 or later.
- A local OpenClaw gateway or compatible local agent gateway.
- An OKKI Claw account and a pairing token from the web app.
- Network access from your local machine to the OKKI Claw relay endpoint.

## Install

Download the latest `okki-claw-*.tgz` package from [GitHub Releases](https://github.com/okki-claw/okki-claw/releases/latest), then install it globally:

```bash
npm install -g ./okki-claw-<version>.tgz
okki-claw --help
```

If you do not want a global install, you can also run the package with your preferred Node.js package tooling after downloading the release artifact.

## Quick Start

### 1. Start OpenClaw locally

Start OpenClaw and make sure its local gateway is running on the same machine as OKKI Claw.

### 2. Pair this device

Get a pairing token from OKKI Claw, then pair this local machine:

```bash
okki-claw pair --pairing-token <token> --device-name "my-openclaw"
```

Pairing stores sidecar credentials in your local OKKI Claw configuration. Do not share this config file.

### 3. Run the sidecar

```bash
okki-claw daemon
```

After the sidecar is connected, open the OKKI Claw web app from another device and choose the paired device.

## Configuration

Common environment variables:

| Variable | Purpose |
| --- | --- |
| `AGENT_RELAY_CONFIG_PATH` | Custom path for OKKI Claw local config. |
| `RELAY_URL` | OKKI Claw tunnel endpoint. |
| `OPENCLAW_GATEWAY_URL` | Local OpenClaw gateway URL when auto-discovery is unavailable. |
| `OPENCLAW_GATEWAY_TOKEN` | Local OpenClaw gateway token, used only on the local machine. |
| `SIDECAR_AUTO_UPDATE` | Enables or disables automatic update checks. |

Use environment variables only on the local machine running OKKI Claw. Do not paste these values into public issues.

## Updates

Check and apply updates:

```bash
okki-claw check-update
okki-claw update
```

When daemon auto-update is enabled, OKKI Claw can periodically check for a newer package and restart into the updated version after a successful install.

## Security Model

OKKI Claw is designed around a simple boundary: local credentials stay local.

- OpenClaw gateway credentials should not be uploaded to the relay.
- Device pairing credentials are stored on your local machine.
- The relay forwards approved traffic; it should not become a general-purpose TCP tunnel.
- Release artifacts are built outside this public repository and published as downloadable packages.
- Public issues should never include `.env` files, tokens, private keys, or local identity files.

See [SECURITY.md](SECURITY.md) for reporting guidance.

## Troubleshooting

### `okki-claw` command not found

Make sure the global npm bin directory is in your `PATH`, then reinstall the downloaded package.

### Pairing fails

Check that the pairing token is still valid, your local clock is correct, and the relay endpoint is reachable from your machine.

### Sidecar cannot connect to OpenClaw

Confirm that OpenClaw is running locally. If auto-discovery does not work, set `OPENCLAW_GATEWAY_URL` and, when required, `OPENCLAW_GATEWAY_TOKEN` on the local machine.

### Remote device cannot see this machine

Confirm that `okki-claw daemon` is still running and that your local machine can reach the configured relay endpoint.

## Releases

Each GitHub Release should include:

- The `okki-claw-*.tgz` package.
- Release notes for user-facing changes.
- Integrity information such as SHA256 checksums when available.

Install only artifacts published by this repository's official releases.

## Support

- Website: <https://agent-relay.okki.ai>
- Chinese website: <https://agent-relay.okki.com>
- Issues: <https://github.com/okki-claw/okki-claw/issues>
