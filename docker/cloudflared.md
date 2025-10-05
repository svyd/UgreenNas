```
version: '3.8'
services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    restart: unless-stopped
    network_mode: host
    environment:
      - TZ='Europe/Kyiv'
    command: tunnel --no-autoupdate run --token eyJhIjoiZmM1ZDQxMmE1NDcxNjBjM2YzM2U5ODg0YjA0NWRlYzgiLCJ0IjoiYTYwZjE1OGEtNDJhMC00ZjNjLWI4NDYtODE1MjgzNTY1YjFkIiwicyI6IlpqSTBOV0l3TXpNdFl6aGxaaTAwT1RKakxUZzROV1F0WWpjME5HSmxOR1ZtTTJOaCJ9
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "3"
```
