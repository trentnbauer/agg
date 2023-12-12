# ❔ Mealie

[Link to App](https://recipes.xfgn.dev/login)

[Link to GitHub or Website](https://mealie.io/)

Mealie is a self hosted recipe manager and meal planner with a RestAPI backend and a reactive frontend application built in Vue for a pleasant user experience for the whole family. Easily add recipes into your database by providing the url and Mealie will automatically import the relevant data or add a family recipe with the UI editor. Mealie also provides an API for interactions from 3rd party applications.

This app is hosted on Cocoa as a docker container

This stack is made up of 3 containers,

* Instance 1
* Instance 2
* Instance 3

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
version: '3'
services:
  api:
    container_name: mealie_api
    image: hkotel/mealie:api-nightly@sha256:2a8561ba6b5aa127f8de54911c8e1702ce33e5974cb93f05f40b230b5cd9a83a
    restart: unless-stopped
    environment:
      - ALLOW_SIGNUP=true
      - PUID=1000
      - PGID=1000
      - TZ=$TZ
      - MAX_WORKERS=2
      - WEB_CONCURRENCY=2
      - BASE_URL=$URL
      - SMTP_HOST=$SMTP_HOST
      - SMTP_PORT=$SMTP_PORT
      - SMTP_FROM_NAME="aGG | Mealie"
      - SMTP_AUTH_STRATEGY=TLS
      - SMTP_FROM_EMAIL=mealie@xfg.network
      - SMTP_USER=$SMTP_USER
      - SMTP_PASSWORD=$SMTP_PASS
      - MEALIE_HOME=/app
      - APP_PORT=9000
      - RECIPE_PUBLIC=true
      - RECIPE_DISABLE_AMOUNT=false
      - RECIPE_DISABLEAMOUNT=false
      - AUTO_BACKUP_ENABLED=true
    volumes:
      - api:/app/data
  app:
    image: hkotel/mealie:frontend-nightly@sha256:cd50ffc11a00302356ca2e51eae609787aab5c6273d1fe7537cd1a1bf41af59f
    restart: unless-stopped
    deploy:
      resources:
        limits:
          memory: 500M
    ports:
      - $PORT:3001
    environment:
      - API_URL=http://mealie_api:9000
      - HOST=0.0.0.0
      - TZ=$TZ
      - RECIPE_DISABLE_AMOUNT=false
      - RECIPE_DISABLEAMOUNT=false
      # =====================================
      # Light Mode Config
      - THEME_LIGHT_PRIMARY=#E58325
      - THEME_LIGHT_ACCENT=#007A99
      - THEME_LIGHT_SECONDARY=#973542
      - THEME_LIGHT_SUCCESS=#43A047
      - THEME_LIGHT_INFO=#1976D2
      - THEME_LIGHT_WARNING=#FF6D00
      - THEME_LIGHT_ERROR=#EF5350
      # =====================================
      # Dark Mode Config
      - THEME_DARK_PRIMARY=#E58325
      - THEME_DARK_ACCENT=#007A99
      - THEME_DARK_SECONDARY=#973542
      - THEME_DARK_SUCCESS=#43A047
      - THEME_DARK_INFO=#1976D2
      - THEME_DARK_WARNING=#FF6D00
      - THEME_DARK_ERROR=#EF5350
    volumes:
      - app:/app/data

volumes:
  api:
  app:
```
{% endcode %}

### Instance 1

| Port | Purpose |
| ---- | ------- |
|      |         |

| Host Volume | Container Volume | Purpose |
| ----------- | ---------------- | ------- |
|             |                  |         |

| Integration | Purpose |
| ----------- | ------- |
|             |         |

\
