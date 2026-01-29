```bash
version: "3.9"
services:
  homeassistant:
    image: homeassistant/home-assistant
    container_name: Home-Assistant
    mem_limit: 8g
    cpu_shares: 768
    security_opt:
      - no-new-privileges:true
    healthcheck:
     test: curl -f http://localhost:8123/ || exit 1
    restart: on-failure:5
    network_mode: host
    volumes:
      - /volume2/docker/homeassistant:/config:rw
    environment:
      TZ: Europe/Kyiv
```
