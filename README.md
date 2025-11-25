# Headscale on Coolify

This repository provides a minimal, Docker Compose–based setup for running a [headscale](https://github.com/juanfont/headscale) coordination server on a self‑hosted [Coolify](https://coolify.io) instance.

The goals are:
- No deploy keys required (public Git clone or optional PAT only).
- Configuration driven entirely by Coolify environment variables (no config files to edit in git).
- Simple "one‑click" style deployment from Coolify.
- Use the Coolify‑hosted headscale instance as the Tailscale control server for your own devices.
- Make a local service (for example, an opencode server on your Arch Linux box) reachable over the Tailscale network.

## How it works

- `docker-compose.yml` defines two services:
  - `headscale-config`: a tiny Alpine container that runs once at startup, reads environment variables, and writes `/etc/headscale/config.yaml`.
  - `headscale`: the official `headscale/headscale` image, which reads that generated config and runs `headscale serve`.
- All configuration (server URL, MagicDNS settings, IP prefixes, DB type/path, log level, etc.) is provided via environment variables, which you set in Coolify.
- Coolify’s Traefik handles HTTPS and domain routing; headscale listens on HTTP inside the container.

## Quick start

1. Push this repo (or your fork) to GitHub as a public repository.
2. In Coolify, create a new **Docker Compose** application from the Git repository and select `docker-compose.yml`.
3. Assign a domain to the `headscale` service (for example `headscale.example.com`) and enable HTTPS.
4. In the app’s environment variables, set at least:
   - `HEADSCALE_DNS_BASE_DOMAIN` (for example `tailnet.example.internal`).
   - Optionally adjust `HEADSCALE_MAGIC_DNS`, `HEADSCALE_IPV4_PREFIX`, `HEADSCALE_IPV6_PREFIX`, etc.
5. Deploy the app; Coolify will run `headscale-config` to generate the config file, then start `headscale`.
6. From the Coolify UI, open a shell in the `headscale` container and use the `headscale` CLI to create a namespace and a preauth key.
7. On your local machine, run Tailscale with `--login-server=https://<your-headscale-domain>` and your preauth key.
8. Run your local opencode server bound to `0.0.0.0:PORT` and access it from other Tailscale nodes via MagicDNS or the Tailscale IP.

See the docs in `docs/` and `coolify/` for detailed, step‑by‑step instructions.
