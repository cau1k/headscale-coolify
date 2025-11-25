# Headscale initialization

After deploying the stack to Coolify, you need to initialize headscale: create a namespace and generate preauth keys for your devices.

## 1. Open a shell in the container

From the Coolify UI:

1. Open your headscale app.
2. Go to the `headscale` service.
3. Use the **Console** or **Exec** feature to open a shell inside the container.

## 2. Create a namespace

Run inside the container:

```bash
headscale namespaces create default
```

You can replace `default` with any namespace name you prefer.

## 3. Create a preauth key

Generate a reusable preauth key for your namespace:

```bash
headscale preauthkeys create \
  --namespace default \
  --reusable \
  --expiration 24h
```

Copy the resulting key; you will use it with the Tailscale client.

## 4. List nodes

Once you start enrolling clients, you can list them with:

```bash
headscale nodes list
```

This is useful to verify that your Arch machine and any other devices have successfully joined the tailnet.

Proceed to `setup-tailscale-client-arch.md` to enroll your local machine.
