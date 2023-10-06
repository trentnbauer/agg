# DDNS for joining servers

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>10 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

## Deploy the CloudflareDDNS container

Create your Portainer stack using the below compose and env file.

This container will update your DNS records when your public IP changes. This isn't needed if you have a static IPv4 or probably for a cloud hosted server, but I'd still recommend it as things sometimes do change and this removes the need to manage and plan for that.

If you have a static IP, you can manually set an A record pointing at your servers public IP, without a proxy enabled, and that will achieve the same functionality as this.

_This container is required per public IP you have. I would recommend installing this container on your Wings host, as that is the server players will be connecting too. If each Wings host has a different IP, you will need a different subdomain per IP._

#### Compose File

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg/blob/main/docker-compose/ddns.yml" %}

#### .ENV FIle

```
API_KEY=
ZONE=
SUBDOMAIN=
PROXIED=false
```

* _The API\_KEY variable is your Cloudflare API, please follow_ [generate-an-api-key.md](../cloudflare/generate-an-api-key.md "mention")
* _The Zone is your domain / website_
* _The Subdomain is the address players will connect on, eg **play**.example.com or **join**.example.com_
* _The Proxied variable MUST be 'false', otherwise your players will not be able to join_

If you choose join.domain.com and host a Minecraft server on 25565, players will put in join.domain.com:25565 when connecting to your server.

You don't need to use a subdomain but if you are or plan on hosting a website on your root domain ( example.com ) it is best to configure the join address with a subdomain.

#### Example ENV file

```
API_KEY=abc123
ZONE=domain.com
SUBDOMAIN=join
PROXIED=false
```

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
