# Deploying headscale to Coolify

This guide shows how to import this repository into a Coolify instance and deploy headscale without using deploy keys.

## Prerequisites

- A running Coolify instance with a working domain and HTTPS (Traefik + Lets Encrypt configured).
- A public GitHub repository containing this code (either this repo or your fork).

## 1. Prepare `config.yaml`

Before deploying, copy the example configuration and adjust it for your environment:

```bash
cp headscale/config.example.yaml headscale/config.yaml
```

Edit `headscale/config.yaml` and at minimum update:

- `server_url`: set to the final public URL you will assign in Coolify, for example:
  - `server_url: "https://headscale.example.com"`
- `dns_config.base_domain`: set to the internal tailnet domain you want for MagicDNS, for example:
  - `dns_config.base_domain: "tailnet.example.internal"`

Commit and push your changes to GitHub.

## 2. Create a Docker Compose app in Coolify

1. In Coolify, click **New Application**.
2. Choose **Docker Compose**.
3. Select **From Git repository**.
4. Enter the public HTTPS URL of your GitHub repo (no deploy keys needed).
5. Choose the branch you want to deploy.
6. Leave `docker-compose.yml` path as the default (`docker-compose.yml` at repo root).

Coolify will fetch the repo and detect the `headscale` service.

## 3. Configure the service and domain

1. In the apps **Services** view, select the `headscale` service.
2. Set the internal port to `8080` (the containers listen port).
3. Add a domain, for example `headscale.example.com`.
4. Enable HTTPS so Coolifys Traefik terminates TLS for you.

There is no need to expose additional ports directly from the Compose file; Coolify handles routing from the public domain to `headscale:8080`.

## 4. Deploy

1. Click **Deploy** for the app.
2. Wait for the `headscale` service to become **Running/Healthy**.
3. Verify that the domain (e.g. `https://headscale.example.com`) responds; you should see the headscale status endpoint or a 404 (headscale does not serve a full UI by default).

## 5. Next steps

After deployment:

- Use the headscale CLI inside the container to create namespaces and preauth keys.
- Enroll your devices with the Tailscale client using `--login-server=https://headscale.example.com`.

See `docs/setup-headscale.md` and `docs/setup-tailscale-client-arch.md` for detailed instructions.
