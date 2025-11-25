# Coolify setup

This document walks through importing and deploying this headscale stack into Coolify.

## 1. Prepare configuration

On your workstation, in this repo:

```bash
cp headscale/config.example.yaml headscale/config.yaml
$EDITOR headscale/config.yaml
```

Set at least:

- `server_url` to the final public URL you will assign, for example:
  - `server_url: "https://headscale.example.com"`
- `dns_config.base_domain` to your desired tailnet domain, for example:
  - `dns_config.base_domain: "tailnet.example.internal"`

Commit and push your changes to GitHub.

## 2. Add the repo to Coolify

1. Log in to your Coolify dashboard.
2. Click **New Application**.
3. Choose **Docker Compose**.
4. Select **From Git repository**.
5. Enter the public HTTPS URL of this repo (or your fork).
6. Select the branch you want to deploy.
7. Use the default compose file path (`docker-compose.yml`).

Coolify will clone the public repo over HTTPS without SSH deploy keys.

## 3. Configure the `headscale` service

1. Open the app and go to the **Services** tab.
2. Select the `headscale` service.
3. Set the internal port to `8080`.
4. Attach a domain, for example `headscale.example.com`.
5. Enable HTTPS so Coolifys Traefik handles certificates and TLS.

## 4. Deploy

Click **Deploy** for the app and wait until the status is **Running**.

You should now have a headscale server reachable at `https://headscale.example.com` (or your chosen domain), with TLS terminated by Coolify.

Continue with `setup-headscale.md` to initialize namespaces and preauth keys.
