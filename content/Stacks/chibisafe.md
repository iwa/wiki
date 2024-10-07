---
title: Chibisafe - Share files
tags:
  - docker
---

```yml
services:
  share:
    image: chibisafe/chibisafe:v5.6.10
    container_name: share
    restart: unless-stopped
    volumes:
      - ./data/database:/home/node/chibisafe/database
      - ./data/uploads:/home/node/chibisafe/uploads
      - ./data/logs:/home/node/chibisafe/logs
    ports:
      - 8000:8000
    labels:
      - com.centurylinklabs.watchtower.enable=true
```