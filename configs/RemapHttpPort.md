# Guide how to use your own service with docker compose on port 80
[Reddit url](https://www.reddit.com/r/UgreenNASync/comments/1decrnn/guide_how_to_use_your_own_service_with_docker/)

**TL;DR**: To use port 80, you need to change the default config in`/etc/nginx/ugreen_redirect.conf`. As it is overwritten by the UGOS software, you need to setup a service that constantly watches for changes.

**1. Go to the app center and download Docker**
**2. Make sure that SSH is enabled and login to your NAS**
**3. Connect to your NAS**  (where USERNAME is your account name and IP is the ip of your NAS).

`$ ssh USERNAME@IP`

When asked for a password, use your account password.

**4. Add your docker compose service.**

Create and navigate to the directory  `/volume1/docker_compose/`  and create a directory for your service (I would not recommend to use the already created  `docker`  directory as the permissions inside that are overwritten by the system). Place your  `docker-compose.yml`  file inside that directory. You can also use VS Code for example to start a remote SSH session which makes file handling a lot easier.

    $ mkdir /volume1/docker_compose/
    $ cd /volume1/docker_compose/
    $ mkdir your_service
    $ cd your_service
    $ nano docker-compose.yml

And after you put in your config, start the service.

`$ sudo docker compose up`

**5. Setup a service to change the nginx port.**

Depending on your service, you need a specific port to connect to it. I wanted to use traefik as a reverse proxy, so I ran into the issue that port 80 was already in use by nginx that ships with UGOS.

    User@DXP4800PLUS:~$ sudo netstat -ltnp | grep -w ':80'
    tcp6       0      0 :::80      :::*     LISTEN      34106/nginx: master

To change that, we need to modify the nginx config inside  `/etc/nginx/ugreen_redirect.conf`. However, on each settings change or system reboot, that file is overwritten. To fix that, we need to setup a system service (crontab is no option here as it gets overwritten as well by the system).

Create the file  `/usr/local/bin/update_nginx_listen.sh`

    $ sudo nano /usr/local/bin/update_nginx_listen.sh

and paste the following content inside:

```
#!/bin/bash

CONFIG_FILE="/etc/nginx/ugreen_redirect.conf"

SEARCH_LISTEN="listen 80;"
REPLACE_LISTEN="listen 8081;"

SEARCH_LISTEN_IPV6="listen \[::\]:80;"
REPLACE_LISTEN_IPV6="listen \[::\]:8081;"

# Directory and command for restarting Docker Compose
DOCKER_COMPOSE_DIR="/volume1/docker_compose/traefik"
DOCKER_COMPOSE_CMD="sudo docker compose restart"

# Function to update listen directives
update_listen_directives() {
    local changed=false

    if grep -q "$SEARCH_LISTEN" "$CONFIG_FILE"; then
    echo "Updating IPv4 config..."
        sed -i "s/$SEARCH_LISTEN/$REPLACE_LISTEN/" "$CONFIG_FILE"
        changed=true
    fi

    if grep -q "$SEARCH_LISTEN_IPV6" "$CONFIG_FILE"; then
    echo "Updating IPv6 config..."
        sed -i "s/$SEARCH_LISTEN_IPV6/$REPLACE_LISTEN_IPV6/" "$CONFIG_FILE"
        changed=true
    fi

    if [ "$changed" = true ]; then
        echo "Changes detected."

        echo "Reloading nginx..."
        sudo systemctl reload nginx

        echo "Waiting for nginx to reload..."
        sleep 3

        echo "Restarting traefik..."
        (cd "$DOCKER_COMPOSE_DIR" && $DOCKER_COMPOSE_CMD)
    fi
}

# Initial update
update_listen_directives

while inotifywait -e close_write "$CONFIG_FILE"; do
    echo "Detected changes in $CONFIG_FILE, updating listen directives..."
    update_listen_directives
done
```
Next, create a file for the service definition like /etc/systemd/system/nginx-listen-monitor.service and paste the following content inside:

```
[Unit]
Description=Monitor /etc/nginx/ugreen_redirect.conf and update listen directives
After=network.target

[Service]
ExecStart=/usr/local/bin/update_nginx_listen.sh
Restart=always
User=root
ExecStartPre=/bin/bash -c 'while ! systemctl is-active docker; do echo "Waiting for docker..."; sleep 5; done'

[Install]
WantedBy=multi-user.target
```

Reload the systemctl deamon:

    $ sudo systemctl daemon-reload

Enable and start the service:

    $ sudo systemctl enable nginx-listen-monitor.service
    $ sudo systemctl start nginx-listen-monitor.service

You can always get the current status (or find information to debug errors) with:

    $ sudo systemctl status nginx-listen-monitor.service

A quick check shows that port 80 is no longer used.

    User@DXP4800PLUS:~$ sudo netstat -ltnp | grep -w ':80'

6. Profit.

Now you can use your NAS just like any other server.
