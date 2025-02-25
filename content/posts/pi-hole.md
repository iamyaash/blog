+++
date = '2025-03-03T19:51:16+05:30'
draft = false
title = 'Self-Host: Pi Hole'
+++

# What is `pi-hole`?

It's a Linux **network-level advertisement and internet tracker blocking application**, which acts as a DNS sinkhole and an _optional DHCP server intended for use on a private network_.

To get an overview, please make sure to learn:
- DNS Sinkhole
- DHCP Server

## Install `pi-hole` in a [`docker`](https://iamyaash.github.io/stashed/posts/docker-sudo-fix/):

> This is a beginner setup with no prior knowledge, so make sure to learn essential things before working on this!

1. Head over to github page of [`docker-pi-hole`](https://github.com/pi-hole/docker-pi-hole/):
2. Copy this `docker compose` scripts into `docker-compose.yml`:
```yml
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      # DNS Ports
      - "53:53/tcp"
      - "53:53/udp"
      # Default HTTP Port
      - "80:80/tcp"
      # Default HTTPs Port. FTL will generate a self-signed certificate
      - "443:443/tcp"
      # Uncomment the below if using Pi-hole as your DHCP Server
      - "67:67/udp"
    environment:
      TZ: 'Asia/Kolkata'
      FTLCONF_webserver_api_password: 'password-here'
      FTLCONF_dns_listeningMode: 'all'
    volumes:
      - './etc-pihole:/etc/pihole'
    cap_add:
      - NET_ADMIN
      - SYS_TIME
      - SYS_NICE
    restart: unless-stopped
```
> This script has already been copied and modified as per the use-cases, so make sure to [look at this script](https://docs.pi-hole.net/docker/) and proceed with setting things up.

**Verify the below lists are checked!**
- Make sure your timezone matches your current location - `TZ: 'Asia/Kolkata'`
- Change the password - `FTLCONF_webserver_api_password: 'password-here'`
- Mention the right directory location - `'./etc-pihole:/etc/pihole'`
  - `./etc-pihole` - will simply use the current location of `docker-compose.yml` to create the `etc-pihole` directory and store things there.
  - `/etc/pihole/` - the direcory name in the docker container.

3. Create directory for storing the `pi-hole` configuration & data:
```sh
mkdir pi-hole-config
cd pi-hole-config
```
4. Create a file named `docker-compose.yml` and store the [2] into this file:
```sh
touch docker-compose.yml
# paste the script into this file & then use docker compose
docker compose up -d
```
> Make sure to execute the docker compose in the **root directory of `docker-compose.yml`**. 