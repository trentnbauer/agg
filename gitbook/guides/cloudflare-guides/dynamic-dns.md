# Dynamic DNS

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>15 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

You can use a Dynamic DNS container to automatically update your Cloudflare managed domain, to keep it up to date with your dynamic public IP address

Your domains DNS will need to be managed by Cloudflare, per [domains.md](domains.md "mention")

## Compose Files

You will be able to use the [portainer-and-gitops](../../service-overviews/portainer-and-gitops/ "mention") guide to put these compose files into production

{% code title="Subdomain" lineNumbers="true" %}
```yaml
version: "3"
services:
  play:
    image: oznu/cloudflare-ddns:latest
    restart: always
    environment:
      - API_KEY=$APIKEY
      - ZONE=$DOMAIN #website.com
      - SUBDOMAIN=$SUBDOMAIN
      - PROXIED=$PROXY #true or false
```
{% endcode %}

{% code title="Root domain" lineNumbers="true" %}
```yaml
version: "3"
services:
  play:
    image: oznu/cloudflare-ddns:latest
    restart: always
    environment:
      - API_KEY=$APIKEY
      - ZONE=$DOMAIN #website.com
      - PROXIED=$PROXY #true or false
```
{% endcode %}

_I've broken one of my own rules with these 2 compose files - they both are using the 'tatest' tag. This is due to the container being depreciated - it will not receive anymore updates. The author hasn't done the best tagging either, and 'latest' is the best for x86 and ARM compatibility_
