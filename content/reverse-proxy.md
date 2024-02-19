---
title: Reverse Proxy
---

Using a reverse proxy is an essential for me.

A reverse proxy basically helps you route every requests you get into the right services.
They have quite a lot of advantages:

- Protect your apps, you only expose the Reverse Proxy
- Automatically support HTTPS with auto SSL cert generation
- Add some security on top of your apps (rate limiter, prevent injections, etc...)
- With Nginx and Traefik, you can also forward TCP/UDP ports from a machine to another *(useful considering my [[index.md#Homelab|two machines setup]])*

It's an essential part of my homelab. It helps me making my services publicly available, as well as organizing and accessing my internal private services with vanity URLs and HTTPS. See [[internal-networking.md|Internal Networking with URLs and HTTPS]].

> [!info]
> Reverse Proxy I'm using as of writing: **Traefik**

## My Traefik Configuration

Traefik is deployed through [[docker.md|Docker]], and has auto-discovery enabled, meaning that all containers with the following labels will be handled by Traefik:

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.whoami.rule=Host(`whoami.example.com`)"
  - "traefik.http.routers.whoami.entrypoints=websecure"
  - "traefik.http.routers.whoami.tls.certresolver=myresolver"
  - "traefik.http.services.whoami.loadbalancer.server.port=80"
```

To communicate, all the containers needs to be in the same Docker network. To do so, manually create a network with `docker network create`, and then apply it in your compose files like so:

```yaml
version: '3.9'

services:
  whoami:
    image: traefik/whoami:latest
    container_name: whoami
    restart: unless-stopped
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.whoami.rule=Host(`whoami.example.com`)"
      - "traefik.http.routers.whoami.entrypoints=websecure"
      - "traefik.http.routers.whoami.tls.certresolver=myresolver"
      - "traefik.http.services.whoami.loadbalancer.server.port=80"

networks:
  default:
    name: traefik_link
    external: true
```

==TBC==