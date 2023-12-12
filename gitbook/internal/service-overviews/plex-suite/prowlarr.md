# Prowlarr

[Link to Access App](https://media.xfgn.dev/prowlarr)

[Link to Download App](https://github.com/Prowlarr/Prowlarr)

Prowlarr is an indexer manager/proxy built on the popular \*arr .net/reactjs base stack to integrate with your various PVR apps. Prowlarr supports management of both Torrent Trackers and Usenet Indexers. It integrates seamlessly with Lidarr, Mylar3, Radarr, Readarr, and Sonarr offering complete management of your indexers with no per app Indexer setup required (we do it all).

Prowlarr is hosted on Latte, as a docker container

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
---
version: "2.1"
services:  
  app:
    image: ghcr.io/linuxserver/prowlarr:1.7.4.3769-ls27
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=$TZ
    volumes:
      - config:/config
      - backups:/backups
    ports:
      - $PORT_PROWLARR:9696
    restart: unless-stopped
volumes:
  config:
  backups:
```
{% endcode %}

| **Port** | **Purpose** |
| -------- | ----------- |
| 9696     | WebUI       |

| **Host Volume** | **Container Volume** | **Purpose**         |
| --------------- | -------------------- | ------------------- |
| media\_backups  | /backups             | Backup directory    |
| media\_prowlarr | /config              | Config and database |

| **Integration** | **Purpose**                                           |
| --------------- | ----------------------------------------------------- |
| Flaresolverr    | Bypass Cloudflare capchas                             |
| Radarr & Sonarr | Manages indexes on all instances on Radarr and Sonarr |

\
