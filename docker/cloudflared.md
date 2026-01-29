Cloudflare Tunnel (cloudflared) lets you securely expose your homelab services to the internet without opening any inbound firewall ports — perfect for homelabs behind CGNAT, restrictive ISPs, or if you just want to hide your home IP.

```bash
Internet → Cloudflare edge (e.g., yourdomain.com)
                     ↓ (encrypted tunnel)
           Your homelab → cloudflared container
                     ↓
           Internal service (e.g., Home Assistant on :8123)
```

💡 Pro tip: Combine this with Cloudflare Zero Trust for free access policies (e.g., "only allow my devices" or "require 2FA") — no extra cost for homelab use.

In short: it's your homelab's secure, no-port-forwarding gateway to the internet. 🌐🔐

```bash
version: '3.8'
services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    restart: unless-stopped
    network_mode: host
    environment:
      - TZ='Europe/Kyiv'
    command: tunnel --no-autoupdate run --token <TOKEN_HERE>
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "3"
```
