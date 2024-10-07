---
title: ActualBudget - Self-hosted budgeting
tags:
  - docker
---

```yml
services:
  actualbudget:
    image: actualbudget/actual-server:latest
    container_name: actualbudget
    restart: unless-stopped
    environment:
      - ACTUAL_PORT=5006
      # - ACTUAL_UPLOAD_FILE_SYNC_SIZE_LIMIT_MB=20
      # - ACTUAL_UPLOAD_SYNC_ENCRYPTED_FILE_SYNC_SIZE_LIMIT_MB=50
      # - ACTUAL_UPLOAD_FILE_SIZE_LIMIT_MB=20
    volumes:
      - ./data:/data
    labels:
      - com.centurylinklabs.watchtower.enable=true
      - traefik.enable=true
      - traefik.http.routers.actualbudget.rule=Host(`*redacted*`)
      - traefik.http.routers.actualbudget.entrypoints=https
      - traefik.http.routers.actualbudget.tls=true
      - traefik.http.routers.actualbudget.tls.certresolver=le
      - traefik.http.routers.actualbudget.tls.domains[0].main=iwa.sh
      - traefik.http.routers.actualbudget.tls.domains[0].sans=*.iwa.sh,*.lab.iwa.sh
      - traefik.http.services.actualbudget.loadbalancer.server.port=5006

networks:
  default:
    name: traefik_link
    external: true
```