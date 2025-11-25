# Deploying headscale to Coolify

This guide shows how to import this repository into a Coolify instance and deploy headscale without using deploy keys or editing any config files in git.

## Prerequisites

- A running Coolify instance with a working domain and HTTPS (Traefik + Let’s Encrypt configured).
- A public GitHub repository containing this code (either this repo or your fork).

## 1. Add the repo as a Docker Compose app

1. In Coolify, click **New Application**.
2. Choose **Docker Compose**.
3. Select **From Git repository**.
4. Enter the public HTTPS URL of your GitHub repo (no deploy keys needed).
5. Choose the branch you want to deploy.
6. Leave `docker-compose.yml` path as the default (`docker-compose.yml` at repo root).

Coolify will fetch the repo and detect the `headscale-config` and `headscale` services.

## 2. Configure the `headscale` service and domain

1. Open the app and go to the **Services** tab.
2. Select the `headscale` service (this is the main headscale server).
3. Assign a domain, for example `headscale.example.com`.
4. Enable HTTPS so Coolify’s Traefik terminates TLS for you.

Coolify will also generate magic environment variables like `SERVICE_FQDN_HEADSCALE`, which the stack uses to construct the correct `server_url` inside headscale’s config.

## 3. Configure environment variables

In the app’s **Environment Variables** section, you can control headscale’s behavior. At minimum, set:

- `HEADSCALE_DNS_BASE_DOMAIN` – the base domain for MagicDNS, for example:
  - `HEADSCALE_DNS_BASE_DOMAIN=tailnet.example.internal`

Optionally adjust:

- `HEADSCALE_MAGIC_DNS` (default `true`) to enable/disable MagicDNS.
- `HEADSCALE_IPV4_PREFIX` (default `100.64.0.0/10`).
- `HEADSCALE_IPV6_PREFIX` (default `fd7a:115c:a1e0::/48`).
- `HEADSCALE_LOG_LEVEL` (default `info`).

You do not need to set `SERVICE_FQDN_HEADSCALE` manually – Coolify will populate it based on the domain you assign to the `headscale` service.

## 4. Deploy

1. Click **Deploy** for the app.
2. Wait for the `headscale-config` service to run and complete, and for the `headscale` service to become **Running/Healthy**.

At deploy time, `headscale-config` generates `/etc/headscale/config.yaml` from the environment variables, and then the `headscale` container starts using that config.

## 5. Next steps

After deployment:

- Use the headscale CLI inside the `headscale` container to create namespaces and preauth keys.
- Enroll your devices with the Tailscale client using `--login-server=https://<your-headscale-domain>`.

See `docs/setup-headscale.md` and `docs/setup-tailscale-client-arch.md` for detailed instructions.
