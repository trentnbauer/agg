# FlareSolverr

[Link to Download App](https://github.com/FlareSolverr/FlareSolverr)

FlareSolverr starts a proxy server, and it waits for user requests in an idle state using few resources. When some request arrives, it uses [Selenium](https://www.selenium.dev/) with the [undetected-chromedriver](https://github.com/ultrafunkamsterdam/undetected-chromedriver) to create a web browser (Chrome). It opens the URL with user parameters and waits until the Cloudflare challenge is solved (or timeout). The HTML code and the cookies are sent back to the user, and those cookies can be used to bypass Cloudflare using other HTTP clients.

**NOTE**: Web browsers consume a lot of memory. If you are running FlareSolverr on a machine with few RAM, do not make many requests at once. With each request a new browser is launched.

It is also possible to use a permanent session. However, if you use sessions, you should make sure to close them as soon as you are done using them.

Flaresolverr is hosted on Latte, as a docker container

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
---
version: "2.1"
services:
  app:
    image: ghcr.io/flaresolverr/flaresolverr:v3.3.2
    environment:
      - LOG_LEVEL=${LOG_LEVEL:-info}
      - LOG_HTML=${LOG_HTML:-false}
      - CAPTCHA_SOLVER=${CAPTCHA_SOLVER:-none}
      - TZ=$TZ
    ports:
      - $PORT:8191
    restart: unless-stopped
    mem_limit: 2G
```
{% endcode %}

| **Port** | **Purpose** |
| -------- | ----------- |
| 8191     | Unknown     |

\
