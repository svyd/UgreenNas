# Guide how to configure a Raspberry Pi with Docker and Portainer

## Introduction
In case of ssh remote host identification has changed error during ssh to host
```bash
ssh-keygen -R 192.168.4.3
```
### [Install Docker](https://pimylifeup.com/raspberry-pi-docker/)

```bash
sudo apt update
```
```bash
sudo apt upgrade
```
```bash
curl -sSL https://get.docker.com | sh
```
For another user to be able to interact with Docker, it needs to be added to the docker group.
```bash
sudo usermod -aG docker $USER
```
Since we made some changes to our user, we will now need to log out and log back in for it to take effect.
```bash
logout
```
Once you have logged back in, you can verify that the docker group has been successfully added to your user by running the following command.
```bash
groups
```
To test if Docker is working, we are going to go ahead and run the following command on our Pi.
```bash
docker run hello-world
```
### [Install Portainer](https://pimylifeup.com/raspberry-pi-portainer/)

```bash
sudo apt update
sudo apt upgrade
```
```bash
docker pull portainer/portainer-ce:latest
```
Create a new folder /opt/portainer
```bash
sudo mkdir /opt/portainer
```
```bash
docker run -d -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /opt/portainer:/data portainer/portainer-ce:latest
```
