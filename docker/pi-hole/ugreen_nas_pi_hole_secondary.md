Remove exposed ports and attach the container to your custom macvlan network.
For DHCP, you must manually assign a static IP inside your subnet — DHCP doesn’t work directly in Docker macvlan mode.

```
version: "3.7"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    networks:
      pihole_net_secondary:
        ipv4_address: 192.168.4.5  # Pick an unused IP in your LAN
    environment:
      TZ: 'Europe/Kyiv'
      FTLCONF_webserver_api_password: '5K!27dwVt%w*'
      FTLCONF_dns_listeningMode: 'all'
    volumes:
      - '/volume2/docker/pihole-secondary/etc-pihole:/etc/pihole'
    restart: unless-stopped
networks:
  pihole_net_secondary:
    external: true
```
