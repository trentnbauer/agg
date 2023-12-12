# Wizarr

[Link to Access App](https://invite.xfgn.dev/)

[Link to Download App](https://github.com/Wizarrrr/wizarr)

Wizarr is a automatic user invitation system for Plex, Jellyfin and Emby. Create a unique link and share it to a user and they will automatically be invited to your Media Server! They will even be guided to download the clients and instructions on how to use your requests software!

Wizarr is hosted on Cocoa, as a docker container

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
---
version: "3.8"
services:
  app:
    image: ghcr.io/wizarrrr/wizarr:2.2.0
    #user: 1000:1000 #Optional but recommended, sets the user uid that Wizarr will run with
    ports:
      - $PORT:5690
    restart: unless-stopped
    volumes:
      - app:/data/database
    environment:
      - APP_URL=$URL #URL at which you will access and share 
      - DISABLE_BUILTIN_AUTH=$DISABLE_AUTH #Set to true ONLY if you are using another auth provider (Authelia, Authentik, etc)
      - TZ=$TZ #Set your timezone here

volumes:
  app:
```
{% endcode %}

| **Port** | **Purpose** |
| -------- | ----------- |
| 5690     | WebUI       |

| **Host Volume** | **Container Volume** | **Purpose**          |
| --------------- | -------------------- | -------------------- |
| wizarr\_app     | /data/database       | Application database |

| **Integration** | **Purpose**                             |
| --------------- | --------------------------------------- |
| Plex            | Grants Plex users access to Plex server |

\
