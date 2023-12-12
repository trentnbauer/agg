# Plex Meta Manager

### SOFTWARE HAS BEEN DELETED

\=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

[Link to Download App](https://metamanager.wiki/en/latest/)

Plex Meta Manager is an open source Python 3 project that has been designed to ease the creation and maintenance of metadata, collections, and playlists within a Plex Media Server. The script is designed to be run continuously and be able to update information based on sources outside your plex environment. Plex Meta Manager supports Movie/TV/Music libraries and Playlists.

PMM runs on Late as a docker container

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
---
version: "2.1"
services:
  app:
    image: ghcr.io/linuxserver/plex-meta-manager:1.19.0
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=$TZ
      - PMM_CONFIG=/config/config.yml #optional
      - PMM_TIME=$TIME #optional
      - PMM_RUN=False #optional
      - PMM_TEST=False #optional
      - PMM_NO_MISSING=False #optional
    volumes:
      - app:/config
    restart: unless-stopped
    
volumes:
  app:
```
{% endcode %}

| **Integration** | **Purpose**                |
| --------------- | -------------------------- |
| Plex            | Create and manage metadata |
