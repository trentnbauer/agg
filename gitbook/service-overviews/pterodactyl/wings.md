# Wings

[Link to GitHub or Website](https://github.com/pterodactyl/wings)

Wings is Pterodactyl's server control plane, built for the rapidly changing gaming industry and designed to be highly performant and secure. Wings provides an HTTP API allowing you to interface directly with running server instances, fetch server logs, generate backups, and control all aspects of the server lifecycle.

This app is hosted on Mocha as a docker container

This stack is made up of 2 containers,

* Wings
* MariaDB

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
version: '3.8'
services:
  wings:
    image: ghcr.io/pterodactyl/wings:v1.11.7
    restart: always
    networks:
      - wings
    ports:
      - $PORT_SFTP:2022
      - 443:443
      - 8080:8080
    tty: true
    environment:
      - TZ=$TZ
      - WINGS_UID=988
      - WINGS_GID=988
      - WINGS_USERNAME=pterodactyl
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/lib/docker/containers/:/var/lib/docker/containers/
      - config:/etc/pterodactyl/  #config file from Panel
      - /var/lib/pterodactyl/:/var/lib/pterodactyl/ #game server files
      - logs:/var/log/pterodactyl/
      - /tmp/pterodactyl/:/tmp/pterodactyl/
      - /srv/daemon-data/:/srv/daemon-data/

  db:
    image: mariadb:10.11
    restart: always
    command: --default-authentication-plugin=mysql_native_password
    ports:
      - $PORT_DB:3306
    networks:
      - wings
    volumes:
      - db:/var/lib/mysql
    environment:
      - MYSQL_DATABASE="servers"
      - MYSQL_USER="pterodactyl"
      - MYSQL_PASSWORD=$SQL_PASS
      - MYSQL_ROOT_PASSWORD=$SQL_PASS_ROOT

networks:
  wings:
    name: wings
    driver: bridge
    driver_opts:
      com.docker.network.bridge.name: wings


volumes:
  config: 
  lib:
  logs:
  daemon:
  db:
```
{% endcode %}

### Wings

| Port | Purpose               |
| ---- | --------------------- |
| 443  | Panel Connection Port |
| 2022 | SFTP                  |
| 8080 | Daemon port (unused)  |

| Host Volume                 | Container Volume           | Purpose                             |
| --------------------------- | -------------------------- | ----------------------------------- |
| /var/run/docker.sock        | /var/run/docker.sock       | Manage Docker containers ( servers) |
| /var/lib/docker/containers/ | var/lib/docker/containers/ |                                     |
| etc                         | /etc/pterodactyl/          | Config file lives here              |
| /var/lib/pterodactyl/       | /var/lib/pterodactyl/      |                                     |
| /var/log/pterodactyl/       | /var/log/pterodactyl/      |                                     |
| /tmp/pterodactyl/           | /tmp/pterodactyl/          |                                     |
| /srv/daemon-data/           | /srv/daemon-data/          |                                     |

| Integration | Purpose                      |
| ----------- | ---------------------------- |
| Panel       | Panel manages wings instance |

### **MariaDB**

| Port | Purpose                 |
| ---- | ----------------------- |
| 3306 | Port to access Database |

| Host Volume | Container Volume | Purpose           |
| ----------- | ---------------- | ----------------- |
| db          | /var/lib/mysql   | Database location |

| Integration | Purpose                |
| ----------- | ---------------------- |
| Panel       | Panel manages database |

\
\
