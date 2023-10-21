---
coverY: 0
---

# Pterodactyl

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>1 Hour</td></tr><tr><td>Difficulty</td><td>Easy</td></tr></tbody></table>

_Come across an issue with this documentation? You can make an edit request yourself, using the 'Submit Change' button on the left, or report it on our_ [_Discord_](https://discord.agamersgrind.com)

## The Scenario

Our goal is to create a new Pterodactyl Panel and Wings node for hosting game servers.

This documentation is NOT intended for a professional / reseller environment. Please do not follow this guide if you intend on selling or publicizing your server resources, as

* Proxy / Tunneling wings and panel is NOT supported by Pterodactyl developers
* This guide is provided with best effort support

### Panel

The Pterodactyl Panel is the front end gui for managing your servers. The panel can be connected to multiple Wings nodes (or hosts), which this documentation is written for.

### Wings

Wings hosts the game server compute (CPU) and storage. As this machines job is to process data, a lot of high performing single thread cores are required as well as a lot of RAM.

### Prerequisites

* Panel machine
  * [Docker and compose installed](https://docs.docker.com/engine/install/ubuntu/)
* 1 machine to install the Wings node on (can be the same as the panel machine)\
  _You can create multiple node machines_
  * [Docker and Compose installed](https://docs.docker.com/engine/install/ubuntu/)
  * A high performance, well spec'd machine;
    * Lots of RAM
    * High single thread passmark CPUs
    * SSD storage
* [A Domain that's managed by Cloudflare](../cloudflare/domains.md)
* A [Cloudflare Tunnel](../cloudflare/tunnel/)&#x20;
* A [Cloudflare DDNS container with proxy turned off](../cloudflare/dynamic-dns.md) (this address will be your game server IP. Eg play.yourdomain.com)

### Recommended

* [Portainer & GitOps](../portainer-and-gitops/)
* A seperate server for the Panel and each Wings node
