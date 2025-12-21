PiWatch
============
Docker compose repo for watchdog & pihole backup based on a raspberry Pi.

Included services:
- [Pi-hole](https://pi-hole.net): A black hole for Internet advertisements

## Prerequisites

### 1. Install Raspbian Lite
Install Raspbian Lite on the SD-card/SSD-drive through [Raspberry Pi Imager](https://www.raspberrypi.com/software/)

Edit `/boot/config.txt` and add this line:

	gpu_mem=16

### 2. Boot up your device and update
After booting your device, run the basic updates/upgrades:

	sudo apt-get update
	sudo apt-get upgrade

### 3. Install Docker
Start the Docker installer

	curl -sSL https://get.docker.com | sh

Set Docker to auto-start

	sudo systemctl enable docker

### 4. Enable Docker client
The Docker client can only be used by root or members of the docker group. Add pi or your equivalent user to the docker group:

	sudo usermod -aG docker pihome

### 5. Install Portainer agent
Install portainer agent

	docker run -d -p 9001:9001 --name portainer_agent --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/volumes:/var/lib/docker/volumes portainer/agent:latest

*If installing a portainer agent doesn't work, you can enable the Docker API to manage this raspberry through another portainer instance.*

## Install containers through portainer

1. Go to your portainer instance
2. Go to stacks
3. Click "Add stack"
   - Add a name "piwatch"
   - Choose repository as build method
     - Repository URL: https://github.com/glamorous/piwatch
     - Repository reference: refs/heads/master
     - GitOps updates: true
     - Fetch interval: 5m
   - Environment variables
       - Upload the secrets.env and adjust where needed (only first time, otherwise manual adding)
4. Deploy the stack
