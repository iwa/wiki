---
title: Adguard - DNS Adblock
tags:
  - docker
---

```yml
services:
  adguard:
    image: adguard/adguardhome:latest
    container_name: adguard
    restart: unless-stopped
    volumes:
      - ./data/work:/opt/adguardhome/work
      - ./data/conf:/opt/adguardhome/conf
    ports:
      #- 80:80
      - 53:53/tcp
      - 53:53/udp
    labels:
      - traefik.enable=true
      - traefik.http.routers.adguard.rule=Host(`*redacted*`)
      - traefik.http.routers.adguard.entrypoints=https
      - traefik.http.routers.adguard.tls=true
      - traefik.http.routers.adguard.tls.certresolver=le
      - traefik.http.routers.adguard.tls.domains[0].main=iwa.sh
      - traefik.http.routers.adguard.tls.domains[0].sans=*.iwa.sh,*.lab.iwa.sh
      - traefik.http.services.adguard.loadbalancer.server.port=80
    networks:
      default: null
      dns:
        ipv4_address: 172.23.0.3
networks:
  default:
    name: traefik_link
    external: true
  dns:
    external: true
    ipam:
      config:
        - subnet: 172.23.0.0/16
```

> [!ask] Why assigning a static IP to the container?
> After some tests, it seems that assigning a static IP to the container makes the networking more consistent and allows for less overhead, better performance and an actual working IP logging.
> Without a static IP, all the requests will be seen by the container as the base route network address. (Here, as 172.23.0.1 instead of client's IPs)