version: '3'
services:
  tailscale:
    image: tailscale/tailscale:latest
    hostname: covenant
    network_mode: host
    environment:
          - TS_AUTHKEY=tskey-auth-kdDB8jwXLo11CNTRL-MA1WZBChTsNEKeFdJKYetNVTfnAwqoQS
          - TS_EXTRA_ARGS=--advertise-routes=192.168.4.0/22 --accept-routes
          - TS_STATE_DIR=/var/lib/tailscale
          - TS_USERSPACE=false
    container_name: tailscale
    restart: unless-stopped
    cap_add:
      - net_admin
    devices:
      - /dev/net/tun:/dev/net/tun
    volumes:
      - /volume2/docker/tailscale:/var/lib/tailscale:rw
