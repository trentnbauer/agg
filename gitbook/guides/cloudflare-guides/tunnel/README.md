# Tunnel

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>40 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

I personally find the Cloudflare documentation to be written very generically, which is great when you know what you're doing but not when you're setting something up for the first time.

## The Scenario

Our goal here is to make our instance of [Overseerr](https://github.com/sct/overseerr) publicly available, so our friends and family with access to our Plex media can request content without bothering us to do it for them.

In addition to that, we also want to make [Sonarr](https://github.com/Sonarr/Sonarr) and [Radarr](https://github.com/Radarr/Radarr) instances accessible externally but behind Cloudflares authentication. This means that only authorized users can access these services.

Using a Cloudflare tunnel for this does not expose your external IP address and any malicious attacks against your network will hit Cloudflare, not you. This is because the traffic is proxied through Cloudflare's CDN, which hosts the majority of the internet. Another advantage of this set up is it allows you to cache your services, which is very useful for static websites.

_Do NOT route your Plex traffic through Cloudflare, this will result in your account being banned_

Some apps, such as game servers, require a direct connection to your server. Please refer to the [Dynamic DNS](../dynamic-dns.md) guide for setting these up

### Prerequisites

* 1 machine to install the Tunnel on with
  * Docker installed
  * [Portainer](https://github.com/portainer/portainer) (or Portainer Edge Agent) installed
* A private GitHub repo to store Docker Compose files ('[GitOps](../../portainer-and-gitops/)')
* A Domain that's managed by Cloudflare
* An internal service you want to be publicly exposed
* An internal service you want to be publicly exposed but with Authentication

### Recommended

* A second machine (if virtual, preferably on a different host) to install the Tunnel on for failover and load balancing
