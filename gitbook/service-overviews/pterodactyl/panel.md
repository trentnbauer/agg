# Panel

The Panel is hosted on Cocoa, as a docker container

The Panel stack is made up of 3 containers,

* Redis
* MariaDB
* Panel Application

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
version: '3.8'
services:
  database:
    image: mariadb:10.5
    restart: always
    command: --default-authentication-plugin=mysql_native_password
    volumes:
      - db:/var/lib/mysql
    networks:
      - panel
    environment:
      MYSQL_PASSWORD: $MYSQL_PASS
      MYSQL_ROOT_PASSWORD: $MYSQL_PASS_ROOT
      MYSQL_DATABASE: "panel"
      MYSQL_USER: "pterodactyl"
      
  cache:
    image: redis:alpine3.18
    networks:
      - panel
    restart: always
    volumes:
      - cache:/data
      
  panel:
    image: ghcr.io/pterodactyl/panel:v1.11.3
    restart: always
    networks:
      - panel
    ports:
      - $PORT_HTTP:80
    #dns:
    #  - 1.1.1.1
    links:
      - database
      - cache
    volumes:
      - env:/app/var
      - logs:/app/storage/logs
    environment:
      MAIL_FROM: $MAIL_FROM
      MAIL_DRIVER: "smtp"
      MAIL_HOST: $MAIL_SERVER
      MAIL_PORT: $MAIL_PORT
      MAIL_USERNAME: $MAIL_USERNAME
      MAIL_PASSWORD: $MAIL_PASS
      MAIL_ENCRYPTION: "true"
      APP_URL: $PTERO_PANEL_URL
      APP_TIMEZONE: $TZ
      APP_SERVICE_AUTHOR: $MAIL_FROM
      TRUSTED_PROXIES: "*" 
      DB_PASSWORD: $MYSQL_PASS
      APP_ENV: "production"
      APP_ENVIRONMENT_ONLY: "false"
      CACHE_DRIVER: "redis"
      SESSION_DRIVER: "redis"
      QUEUE_DRIVER: "redis"
      REDIS_HOST: "cache"
      DB_HOST: "database"
      DB_PORT: "3306"
networks:
  panel:

volumes:
  cache:
  db:
  env:
  logs:
```
{% endcode %}

### Redis

| Host Volume        | Container Volume | Purpose                              |
| ------------------ | ---------------- | ------------------------------------ |
| Randomly Generated | /data            | Stores the cached files... I assume? |

### MariaDB

| Host Volume               | Container Volume | Purpose           |
| ------------------------- | ---------------- | ----------------- |
| /srv/pterodactyl/database | /var/lib/mysql   | Database location |

### Panel

| Port | Purpose                |
| ---- | ---------------------- |
| 809  | WebUI HTTP             |
| 4439 | WebUI HTTPS (not used) |

| Host Volume       | Container Volume   | Purpose                          |
| ----------------- | ------------------ | -------------------------------- |
| pteropanel\_var   | /app/var           | Contains configuration .env file |
| pteropanel\_nginx | /etc/ngix/http.d   |                                  |
| pteropanel\_certs | /etc/letsencrypt   |                                  |
| pteropanel\_logs  | /apps/storage/logs |                                  |

| Integration | Purpose                                          |
| ----------- | ------------------------------------------------ |
| Mocha       | Connect to the Wings container to manage servers |

\