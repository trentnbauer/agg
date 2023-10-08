# Add Health Check to Container

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>5 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

The easiest way to add health checks to your docker containers is to update the Compose file, below you will find some examples. The Health Check needs to be added at the same indentation (how many 'TAB' keys) as your 'image', 'volume' and/or 'environment' lines, per below

```yaml
version: "2.1"
services:
  app:
    image: ghcr.io/linuxserver/plex:1.32.6
    network_mode: host
    ports:
      - 32400:32400
    volumes:
      - app:/config
      - media:/media
    restart: unless-stopped
    healthcheck:
      test: curl --connect-timeout 15 --silent --show-error --fail http://localhost:32400/identity
      interval: 1m
      timeout: 30s
      retries: 3
      start_period: 1m
```

One thing to note is that not all containers will need health checks. Health checks do require additional compute to do and it may be better using a cloud service (such as [UptimeRobot](https://uptimerobot.com/)) or an internally hosted service like UptimeKuma

## Health Checks

Please read the code you are adding and ensure your variables match, otherwise you will have issues!

### Web Page check

{% code overflow="wrap" %}
```yaml
#This will check if $HEALTHCHECK (eg http://myplexserver:32400 ) is accessible and if not, restart the container.
healthcheck:
  test: curl --connect-timeout 15 --silent --show-error --fail $HEALTHCHECK
  interval: 1m
  timeout: 30s
  retries: 3
  start_period: 1m
```
{% endcode %}





