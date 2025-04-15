# Guide how to configure a Raspberry Pi as a Pi-hole server

## Introduction

### [Install Docker](https://pimylifeup.com/raspberry-pi-docker/)

```
sudo apt update
sudo apt upgrade
```

    curl -sSL https://get.docker.com | sh

For another user to be able to interact with Docker, it needs to be added to the docker group.

    sudo usermod -aG docker $USER

Since we made some changes to our user, we will now need to log out and log back in for it to take effect.

    logout

Once you have logged back in, you can verify that the docker group has been successfully added to your user by running the following command.

    groups

To test if Docker is working, we are going to go ahead and run the following command on our Pi.

    docker run hello-world

### [Install Portainer](https://pimylifeup.com/raspberry-pi-portainer/)

```
sudo apt update
sudo apt upgrade
```

    docker pull portainer/portainer-ce:latest

Create a new folder /opt/portainer

    sudo mkdir /opt/portainer

```
docker run -d -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /opt/portainer:/data portainer/portainer-ce:latest
```

### [Install Pi-hole](https://docs.pi-hole.net/docker/)

Create a new folder /opt/pihole/pihole/etc-pihole

    sudo mkdir /opt/pihole/etc-pihole

```
services:
    pihole:
        container_name: pihole
        image: pihole/pihole:latest
        ports:
            - "53:53/tcp"
            - "53:53/udp"
            - "8080:80/tcp"   # Use 8080:80/tcp if you have another service running on port 80
            - "8443:443/tcp"
        environment:
            TZ: 'Europe/Kyiv'     # https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
            FTLCONF_webserver_api_password: '5K!27dwVt%w*'
            # If using Docker's default `bridge` network setting the dns listening mode should be set to 'all'
            FTLCONF_dns_listeningMode: 'all'
        volumes:
            - '/opt/pihole/etc-pihole:/etc/pihole'
        restart: unless-stopped
```
