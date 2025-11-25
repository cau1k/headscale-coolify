# Tailscale client setup on Arch Linux

This document shows how to install the Tailscale client on Arch Linux and connect it to your self‑hosted headscale server.

## 1. Install Tailscale

On your Arch machine:

```bash
sudo pacman -S tailscale
sudo systemctl enable --now tailscaled.service
```

## 2. Enroll the machine with headscale

Assuming your headscale instance is reachable at `https://headscale.example.com` and you have a preauth key from `setup-headscale.md`, run:

```bash
sudo tailscale up \
  --login-server=https://headscale.example.com \
  --authkey=<your-preauth-key> \
  --hostname=arch-opencode \
  --accept-dns=true
```

Replace `<your-preauth-key>` with the value printed by `headscale preauthkeys create`.

If successful, this machine will appear in `headscale nodes list` inside the container.

## 3. Verify connectivity

Check status on the Arch machine:

```bash
tailscale status
tailscale ip -4
```

You should see this node online with a `100.x.y.z` address.

From another Tailscale node (once enrolled), you will be able to reach services on this machine using its MagicDNS name or Tailscale IP.

Continue with `opencode-over-tailscale.md` to expose your local opencode server.
