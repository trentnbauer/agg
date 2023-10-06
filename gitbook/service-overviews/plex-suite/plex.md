# Plex

Plex can be access via [https://plex.tv](https://plex.tv/)&#x20;

A one-stop destination to stream movies, TV shows, and music, Plex is the most comprehensive entertainment platform available today. Available on almost any device, Plex is the first-and-only streaming platform to offer free ad-supported movies, shows, and live TV together with the ability to easily search—and add to your Watchlist—any title ever made, no matter which streaming service it lives on. Using the platform as their entertainment concierge, 17 million (and growing!) monthly active users count on Plex for new discoveries and recommendations from all their favorite streaming apps, personal media libraries, and beyond.

Plex is installed on Latte as a docker container. The GTX 1650 in the server has been passed through to this VM to allow for hardware encoding.

The video and audio content that Plex streams is stored on Fettuccine

{% code title="docker-compose" overflow="wrap" %}
```yaml
version: "2.1"
services:
  app:
    image: ghcr.io/linuxserver/plex:1.32.5
    network_mode: host
    ports:
      - 32400:32400
      - 1900:1900/udp
      - 3005:3005
      - 5353:5353/udp
      - 8324:8324
      - 32410:32410/udp
      - 32412:32412/udp
      - 32413:32413/udp
      - 32414:32414/udp
      - 32469:32469
    #devices:
     # - /dev/dri:/dev/dri
    environment:
      #- PUID=1000
      #- PGID=1000
      - VERSION=docker
      - PLEX_CLAIM=$PLEX_CLAIM
      - NVIDIA_VISIBLE_DEVICES=ALL
    runtime: nvidia
    volumes:
      - app:/config
      #- $MEDIA:$MEDIA
      - media:/nfs/Media
      - /dev/shm:/dev/shm
    restart: unless-stopped
    healthcheck:
      test: curl --connect-timeout 15 --silent --show-error --fail http://localhost:32400/identity
      interval: 1m
      timeout: 30s
      retries: 3
      start_period: 1m

volumes:
  app:
  media:
    driver_opts:
      type: nfs 
      o: $STORAGE,nfsvers=4
      device: :$MEDIA
```
{% endcode %}

| **Port**             | **Purpose**                   |
| -------------------- | ----------------------------- |
| 32400 (Port Forward) | Display webUI, stream content |

| **Host Volume** | **Container Volume** | **Purpose**                                       |
| --------------- | -------------------- | ------------------------------------------------- |
| plex\_media     | /nfs/Media           | nfs mount of the Media share on Fettuccine        |
| /dev/shm        | /dev/shm             | RAM. Used for storing live-transcoded data        |
| plex\_app       | /config              | Stores the Plex database, config, thumbnails etc. |

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg.local/blob/main/docker-compose/plex.yml" %}
