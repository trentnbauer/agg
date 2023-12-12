# Tautulli

[Link to Access App](https://media.xfgn.dev/tautulli)

[Link to Download App](https://github.com/Tautulli/Tautulli)

A python based web application for monitoring, analytics and notifications for [Plex Media Server](https://plex.tv/).

This project is based on code from [Headphones](https://github.com/rembo10/headphones) and [PlexWatchWeb](https://github.com/ecleese/plexWatchWeb).

Tautulli runs on Cocoa as a docker container

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
version: '3'
services:
  app:
    image: ghcr.io/linuxserver/tautulli:v2.12.5-ls88
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=$TZ
    volumes:
      - app:/config
    ports:
      - $PORT:8181
    restart: unless-stopped

volumes:
  app:
```
{% endcode %}

| **Port** | **Purpose** |
| -------- | ----------- |
| 8181     | WebUI       |

| **Host Volume** | **Container Volume** | **Purpose**                |
| --------------- | -------------------- | -------------------------- |
| tautulli\_app   | /config              | configuration and database |

| **Integration** | **Purpose**                                    |
| --------------- | ---------------------------------------------- |
| Plex            | Syncs media and user data (eg user watch time) |

\
