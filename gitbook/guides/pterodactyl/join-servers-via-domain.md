# Join servers via domain

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>10 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

There are 2 ways to do this;

* Using a Dynamic DNS container or DDNS app
* Manually setting a DNS record on your domain

The DDNS route isn't needed if you have a static IPv4 or probably for a cloud hosted server, but I'd still recommend it as things sometimes do change and this removes the need to manage and plan for that. If you have a static IP, you can manually set an A record pointing at your servers public IP and that will achieve the same functionality as this.

I would recommend picking a subdomain for hosting your game servers, such as join.example.com or play.example.com. This means you can still host a website on the root of your domain

Some things to note with using a domain;

* If your game server reports back to a game browser, such as the Steam browser, it will always show your IP, not your domain.
* If you choose join.example.com and host a Minecraft server on port 25565, players will put in join.example.com:25565 when connecting to your server.
* You don't need to use a subdomain but if you are or plan on hosting a website on your root domain ( example.com ) it is best to configure the join address with a subdomain.
* It can take some time for DNS to propogate. If you're using Cloudflare as your DNS / domain manager, set your computer or routers DNS to 1.1.1.1 and 1.0.0.1 to view the updates quicker.

## Static IP

If you have a static IP you can manually set an A record in your DNS, pointing towards your IP

## Dynamic IP

Follow the instructions listed in [dynamic-dns.md](../cloudflare/dynamic-dns.md "mention"), but please note:

* The Proxied variable MUST be 'false', otherwise your players will not be able to join.
* This container is required per public IP you have. I would recommend installing this container on your Wings host, as that is the server players will be connecting too. If each Wings host has a different IP, you will need a different subdomain per IP.

