# Exposing opencode over Tailscale

Once your Arch machine is enrolled in your headscale‑managed tailnet, you can make a local opencode server reachable to other Tailscale nodes.

## 1. Start opencode on your Arch machine

Start your opencode server and ensure it listens on all interfaces (0.0.0.0) on a known port, for example 5757:

```bash
# Example only – adjust to your actual command
opencode --listen 0.0.0.0:5757
```

Verify locally:

```bash
curl http://127.0.0.1:5757
```

## 2. Find the Tailscale IP / hostname

On the Arch machine:

```bash
tailscale ip -4
```

Note the `100.x.y.z` address.

If MagicDNS is enabled in your headscale config (`dns_config.magic_dns: true` and a `base_domain` set), you can also use the MagicDNS hostname (for example `arch-opencode.<tailnet-domain>`).

## 3. Access opencode from another node

On any other device enrolled in the same tailnet:

- Using Tailscale IP:

  ```bash
  curl http://100.x.y.z:5757
  ```

- Using MagicDNS (if configured):

  ```bash
  curl http://arch-opencode.<tailnet-domain>:5757
  ```

Replace `<tailnet-domain>` with the `dns_config.base_domain` you configured (for example `tailnet.example.internal`).

All traffic stays inside the encrypted Tailscale network; you do not need to expose your opencode server directly to the public Internet.
