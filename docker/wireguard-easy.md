```
version: "3.8"
services:
  wg-easy:
    environment:
      - WG_HOST=185.130.53.99
      - PASSWORD_HASH=$$2a$$12$$jnXRwK4fMQp5WhiZ/nGRWekMCBPjzf4wYRwWC1Vfck33GFCmiiUfC
      - WG_DEFAULT_DNS=10.8.1.3
      - WG_DEFAULT_ADDRESS=10.8.0.x
    image: ghcr.io/wg-easy/wg-easy
    container_name: wg-easy
    volumes:
      - /volume2/docker/wireguard-easy/etc-wireguard:/etc/wireguard
    ports:
      - "51820:51820/udp"
      - "51821:51821/tcp"
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.ip_forward=1
      - net.ipv4.conf.all.src_valid_mark=1
    networks:
      wg-easy:
        ipv4_address: 10.8.1.2

  pihole:
      image: pihole/pihole
      container_name: wg-pihole
      environment:
        TZ: "Europe/Kyiv"
        FTLCONF_webserver_api_password: "5K!27dwVt%w*"
        FTLCONF_dns_listeningMode: "all"
      volumes:
        - /volume2/docker/wireguard-easy/pihole/etc-pihole:/etc/pihole
        - /volume2/docker/wireguard-easy/pihole/etc-dnsmasq.d:/etc/dnsmasq.d
      ports:
        - "5353:80/tcp"
        - "8453:443/tcp"
      restart: unless-stopped
      networks:
        wg-easy:
          ipv4_address: 10.8.1.3

networks:
  wg-easy:
    ipam:
      config:
        - subnet: 10.8.1.0/24
```
