# Dynamic DNS

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>5 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

You can use a Dynamic DNS container to automatically update your Cloudflare managed domain, to keep it up to date with your dynamic public IP address

## Requirements

* A Cloudflare API key per [generate-an-api-key.md](generate-an-api-key.md "mention")
* Your domains DNS will need to be managed by Cloudflare, per [domains.md](domains.md "mention")

## Compose Files

You will be able to use the [Broken link](broken-reference "mention") guide to put these compose files into production

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg/blob/main/docker-compose/ddns.yml" %}

Example .ENV file

```
CLOUDFLARE_APIKEY=
DOMAIN=
SUBDOMAIN=
PROXIED=true
```

### _Some things to note_

* If you are setting up DDNS for your root domain, leave the subdomain variable blank
* It is recommended to leave proxied as true but if your service fails to load, try it as false.&#x20;
* You may need to configure a Cloudflare tunnel, per [tunnel](tunnel/ "mention")

## Confirm the Container is working

Log into Portainer and view the logs for the container, you should see something similar to below

```bash
[s6-init] making user provided files available at /var/run/s6/etc...exited 0.
[s6-init] ensuring user provided files have correct perms...exited 0.
[fix-attrs.d] applying ownership & permissions fixes...
[fix-attrs.d] done.
[cont-init.d] executing container initialization scripts...
[cont-init.d] 30-cloudflare-setup: executing... 
DNS Zone: <REDACTED> (ebf622fd8125a2b85523ddd64814a66e)
DNS record for '<REDACTED>' was not found in <REDACTED> zone. Creating now...
DNS Record: <REDACTED> (5109ffe562242554b4a4599b06ffa0f0)
[cont-init.d] 30-cloudflare-setup: exited 0.
[cont-init.d] 50-ddns: executing... 
No DNS update required for <REDACTED> (<REDACTED>).
[cont-init.d] 50-ddns: exited 0.
[cont-init.d] done.
[services.d] starting services
Starting crond...
crond: crond (busybox 1.31.1) started, log level 6
[services.d] done.
```
