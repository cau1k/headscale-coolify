# Coolify setup

This document walks through importing and deploying this headscale stack into Coolify using only environment variables for configuration.

## 1. Add the repo to Coolify

1. Log in to your Coolify dashboard.
2. Click **New Application**.
3. Choose **Docker Compose**.
4. Select **From Git repository**.
5. Enter the public HTTPS URL of this repo (or your fork).
6. Select the branch you want to deploy.
7. Use the default compose file path (`docker-compose.yml`).

Coolify will clone the public repo over HTTPS without SSH deploy keys and detect the `headscale-config` and `headscale` services.

## 2. Configure the `headscale` service

1. Open the app and go to the **Services** tab.
2. Select the `headscale` service.
3. Assign a domain, for example `headscale.example.com`.
4. Enable HTTPS so Coolify’s Traefik handles certificates and TLS.

Coolify will generate magic environment variables like `SERVICE_FQDN_HEADSCALE` for this service. The stack uses that value to set the `server_url` in headscale’s configuration.

## 3. Configure environment variables

In the app’s environment configuration, set at least:

- `HEADSCALE_DNS_BASE_DOMAIN` – base domain for MagicDNS, for example:
  - `HEADSCALE_DNS_BASE_DOMAIN=tailnet.example.internal`

Optional overrides (all have sensible defaults in `docker-compose.yml`):

- `HEADSCALE_MAGIC_DNS` (`true` or `false`).
- `HEADSCALE_IPV4_PREFIX` (default `100.64.0.0/10`).
- `HEADSCALE_IPV6_PREFIX` (default `fd7a:115c:a1e0::/48`).
- `HEADSCALE_DB_TYPE` (default `sqlite`).
- `HEADSCALE_LOG_LEVEL` (default `info`).

You do not need to create or edit any `config.yaml` file – the `headscale-config` service will generate it at deploy time in `/etc/headscale/config.yaml` based on these env vars.

## 4. Deploy

Click **Deploy** for the app and wait until:

- `headscale-config` completes successfully.
- `headscale` is in **Running** status.

You should now have a headscale server reachable at `https://headscale.example.com` (or your chosen domain), with TLS terminated by Coolify.

Continue with `setup-headscale.md` to initialize namespaces and preauth keys.
