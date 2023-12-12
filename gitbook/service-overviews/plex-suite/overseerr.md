# Overseerr

[Link to Access App](https://request.xfgn.dev)

[Link to Download App](https://github.com/sct/overseerr)

Overseerr is a free and open source software application for managing requests for your media library. It integrates with your existing services, such as Sonarr, Radarr, and Plex!

Overseerr is hosted on Cocoa, as a docker container

{% code title="docker-compose.yml" lineNumbers="true" %}
```yaml
version: '3'
services:
  app:
    image: ghcr.io/linuxserver/overseerr:version-v1.33.2
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=$TZ
    volumes:
      - app:/config
      - www:/app/overseerr/public #rebrand overseerr https://github.com/sct/overseerr/issues/404#issuecomment-1659795118
    ports:
      - $PORT:5055
    restart: unless-stopped

volumes:
  app:
  www:
```
{% endcode %}

| **Port** | **Purpose** |
| -------- | ----------- |
| 5055     | WebUI       |

| **Host Volume** | **Container Volume** | **Purpose**         |
| --------------- | -------------------- | ------------------- |
| overseerr\_app  | /config              | Config and datavase |

| **Integration** | **Purpose**                                                               |
| --------------- | ------------------------------------------------------------------------- |
| Plex            | Syncs media content and users from Plex                                   |
| Radarr & Sonarr | Syncs and adds content to the 1080p and 4k instances of Radarr and Sonarr |
| Tautulli        | Pulls watch history from Tautulli                                         |

{% embed url="https://raw.githubusercontent.com/trentnbauer/agg.local/main/docker-compose/overseerr.yml?token=GHSAT0AAAAAACCCAC7PUTZXI3HJEGGGBV7UZEBDDUA" %}

\
