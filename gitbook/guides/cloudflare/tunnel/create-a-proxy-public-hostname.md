# Create a Proxy (Public Hostname)

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>5 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

The Cloudflare tunnel can be done as a straight proxy, allowing all public access in, or behind an 'Application' (next page), limiting access to authenticated users or users meeting a certain requirement (eg IP range, country). This is part of Cloudflares 'Zero Trust' service, which is accessible via the Cloudflare website

Cloudflare Proxies are referred to as 'Public Hostnames'

## Creating a Proxy (straight public facing - no auth or security)

_Unfortunately the Cloudflare doco for this is rubbish, or non-existent. Hopefully the process stays the same..._

1. Log into Cloudflare and click on Zero Trust
2. Navigate to Access > Tunnels
3. Select the relevant tunnel > configure
4. Click on the Public Hostname tab
5.  Click on 'Add a public hostname' and fill in the relevant details

    <figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption><p>"plex.xfgn.dev" will forward to "latte.agg.local:32400"</p></figcaption></figure>
6. For internal services with self signed certificates,
   1. Click on Additional application settings
   2. Expand TLS
   3. Enable 'No TLS Verify'&#x20;
7. Click on Save Hostname
8. Your service will now be publicly accessible at subdomain.domain.com/path
