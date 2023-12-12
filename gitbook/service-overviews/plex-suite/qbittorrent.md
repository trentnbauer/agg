# qBittorrent

[Link to Access App](https://torrent.xfgn.dev/)

[Link to Download App](https://www.qbittorrent.org/)

qBittorrent is a cross-platform free and open-source BitTorrent client written in native C++. It relies on Boost, Qt 6 toolkit and the libtorrent-rasterbar library (for the torrent back-end), with an optional search engine written in Python.

qBittorrent is hosted on Fettuccine, as a docker containe

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
version: "3"
services:
  app:
    image: emmercm/qbittorrent:4.5-alpine
    restart: unless-stopped
    dns:
      - 1.1.1.1
      - 8.8.8.8
    environment:
      - TZ=$TZ
    ports:
      - $WEBPORT:8080
      - 6881:6881/tcp #Torrent Traffic
      - 6881:6881/udp #Torrent Traffic
    volumes:
      - config:/config
      - data:/data
      - $DOWNLOADS:$DOWNLOADS
volumes:
  config:
  data:
```
{% endcode %}

| **Port** | **Purpose** |
| -------- | ----------- |
| 8080     | WebUI       |

| **Host Volume**     | **Container Volume** | **Purpose**          |
| ------------------- | -------------------- | -------------------- |
| /volume1/downloads  | /volume1/downloads   | Downloading directly |
| qbittorrent\_config | /config              | config files         |
| qbittorent\_data    | /data                | logs, backups        |

\
