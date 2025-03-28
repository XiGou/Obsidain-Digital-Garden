---
title: "Calibra: Ebook Management System"
dg-publish: true
date: 2024-01-06 01:14:31 +0800
---

> [!info] Github Repo  
> [GitHub - janeczku/calibre-web: :books: Web app for browsing, reading and downloading eBooks stored in a Calibre database](https://github.com/janeczku/calibre-web)

Webui can be found at `http://your-ip:8083`

```yaml
---
services:
  calibre-web:
    image: lscr.io/linuxserver/calibre-web:latest
    container_name: calibre-web
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - DOCKER_MODS=linuxserver/mods:universal-calibre #optional
      - OAUTHLIB_RELAX_TOKEN_SCOPE=1 #optional
    volumes:
      - /path/to/data:/config
      - /path/to/calibre/library:/books
    ports:
      - 8083:8083
    restart: unless-stopped
```

```bash
docker run -d \
  --name=calibre-web \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Etc/UTC \
  -e DOCKER_MODS=linuxserver/mods:universal-calibre `#optional` \
  -e OAUTHLIB_RELAX_TOKEN_SCOPE=1 `#optional` \
  -p 8083:8083 \
  -v /path/to/data:/config \
  -v /path/to/calibre/library:/books \
  --restart unless-stopped \
  lscr.io/linuxserver/calibre-web:latest
```

It is heavy and old, finally I decide not to use it.