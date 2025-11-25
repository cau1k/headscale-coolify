# Headscale on Coolify

This repository provides a minimal, Docker Compose–based setup for running a [headscale](https://github.com/juanfont/headscale) coordination server on a self‑hosted [Coolify](https://coolify.io) instance.

The goals are:
- No deploy keys required (public Git clone or optional PAT only).
- Simple "one‑click" style deployment from Coolify.
- Use the Coolify‑hosted headscale instance as the Tailscale control server for your own devices.
- Make a local service (for example, an opencode server on your Arch Linux box) reachable over the Tailscale network.

## Quick overview

- `docker-compose.yml` defines a single `headscale` service using the official headscale image and SQLite for storage.
- `headscale/config.example.yaml` is a template headscale configuration; you create your own `config.yaml` from it.
- Coolify handles HTTPS and domain routing via its built‑in Traefik proxy.
- Your local machine runs the official Tailscale client, pointing at your Coolify‑hosted headscale instance as `--login-server`.

## Getting started

1. Copy `headscale/config.example.yaml` to `headscale/config.yaml` and adjust values (especially `server_url` and `dns_config.base_domain`).
2. Push this repo (or your fork) to GitHub as a public repository.
3. Import the repo into Coolify as a Docker Compose app and assign a domain (for example `headscale.example.com`).
4. Deploy the app from Coolify.
5. Use the `headscale` CLI in the container to create a namespace and a preauth key.
6. On your local machine, run Tailscale with `--login-server=https://headscale.example.com` and your preauth key.
7. Run your local opencode server bound to `0.0.0.0:PORT` and access it from other Tailscale nodes via MagicDNS or the Tailscale IP.

See the docs in `docs/` and `coolify/` for detailed, step‑by‑step instructions.
