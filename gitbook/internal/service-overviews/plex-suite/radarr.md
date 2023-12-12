# Radarr

[Link to Github](https://github.com/Radarr/Radarr)

Radarr is a movie collection manager for Usenet and BitTorrent users. It can monitor multiple RSS feeds for new movies and will interface with clients and indexers to grab, sort, and rename them. It can also be configured to automatically upgrade the quality of existing files in the library when a better quality format becomes available. Note that only one type of a given movie is supported. If you want both an 4k version and 1080p version of a given movie you will need multiple instances.

My instances of Radarr are hosted on Latte

<pre class="language-yaml" data-title="docker-compose.yml" data-overflow="wrap"><code class="lang-yaml"><strong>---
</strong>version: "2.1"
services:
  app:
    image: ghcr.io/linuxserver/radarr:4.6.4.7568-ls183
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=$TZ
    volumes:
      - config:/config
      - media:/media
      - downloads:/downloads
      - backups:/backups
    ports:
      - $PORT_RADARR1080:7878
    restart: unless-stopped
volumes:
  config:
  backups:
  media:
    driver_opts:
      type: nfs
      o: addr=$STORAGE,nfsvers=4
      device: :$MEDIA
  downloads:
    driver_opts:
      type: nfs 
      o: addr=$STORAGE,nfsvers=4
      device: :$DOWNLOAD
</code></pre>

## [**1080p**](https://media.xfgn.dev/radarr)

The general instance of Radarr, used to capture 720 and 1080p content

| Port | Purpose |
| ---- | ------- |
| 7878 | Webui   |

| Host Volume       | Container Volume      | Purpose                                                                                                                                       |
| ----------------- | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| media\_downloads  | <p>/downloads<br></p> | Where qBittorrent downloads too. Required to access completed downloads to shift to the Media library. Stored of Fettuccine, accessed via NFS |
| media\_media      | <p><br>/movies</p>    | Media library that Plex views. Required to manage metadata and add to library. Stored of Fettuccine, accessed via NFS                         |
| media\_backups    | /backups              | Dedicated shared backup volume                                                                                                                |
| media\_radarr1080 | /config               | Config files                                                                                                                                  |

| Integration           | Purpose                                                                                                                                                  |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| qBittorrent           | To add and monitor downloading contented                                                                                                                 |
| Sync from 4k instance | Syncs all movies from the 4k instance back to this instance. This is to ensure that any movie in the 4k instance will also be available in 1080p quality |

## [**4k**](https://media.xfgn.dev/r4k)

The 4k instance of Radarr. This content is (generally) not available outside of home

| Port | Purpose |
| ---- | ------- |
| 7879 | Webui   |

| Host Volume      | Container Volume      | Purpose                                                                                                                                       |
| ---------------- | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| media\_downloads | <p>/downloads<br></p> | Where qBittorrent downloads too. Required to access completed downloads to shift to the Media library. Stored of Fettuccine, accessed via NFS |
| media\_media     | <p><br>/movies</p>    | Media library that Plex views. Required to manage metadata and add to library. Stored of Fettuccine, accessed via NFS                         |
| media\_backups   | /backups              | Dedicated shared backup volume                                                                                                                |
| media\_radarr4k  | /config               | Config files                                                                                                                                  |

| Integration | Purpose                                  |
| ----------- | ---------------------------------------- |
| qBittorrent | To add and monitor downloading contented |

## [**Popular**](http://media.xfgn.dev/popular)

The popular instance of Radarr. This container imports movies from lists and automatically deletes them

| Port | Purpose |
| ---- | ------- |
| 7880 | webui   |

| Host Volume          | Container Volume      | Purpose                                                                                                                                       |
| -------------------- | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| media\_downloads     | <p>/downloads<br></p> | Where qBittorrent downloads too. Required to access completed downloads to shift to the Media library. Stored of Fettuccine, accessed via NFS |
| media\_media         | <p><br>/movies</p>    | Media library that Plex views. Required to manage metadata and add to library. Stored of Fettuccine, accessed via NFS                         |
| media\_backups       | /backups              | Dedicated shared backup volume                                                                                                                |
| media\_radarrpopular | /config               | Config files                                                                                                                                  |

| Integration                   | Purpose                                                                                                                                                                                              |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| qBittorrent                   | To add and monitor downloading contented                                                                                                                                                             |
| Various Trakt & MovieDB lists | Syncs popular and upcoming movies                                                                                                                                                                    |
| 1080p Sync                    | Sync's all movies from the 1080p instance and marks them as unmonitored so they do not get downloaded. Used to ensure that duplicate content isn't downloaded between the Popular and 1080p instance |
