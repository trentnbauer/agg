# External Access to Systems

Currently we are using a Cloudflare Tunnel to reverse proxy services to be available externally, either publicly or behind Cloudflare's authentication. Some services also require port forwarding

| **External type**                                                                         | **Why?**                                                                                              |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| [Port Forward](../guides/unifi/port-forwarding.md)                                        | High bandwidth traffic, not HTTP/S traffic (eg game servers, video files), latency sensitive services |
| [Cloudflare Tunnel](../guides/cloudflare-guides/tunnel/create-a-proxy-public-hostname.md) | Standard HTTP/S traffic                                                                               |

## Does my app need to be open to the public?

It depends on the use-case, and if you want to be! If you or someone else will need to access the service outside of the LAN, the app will need to be externally available.

* Is my app secure enough to be open to the public?
  * How is authentication handled?
  * How are updates handled?
* Does my app need to be open to the public?
  * Why would a random internet user access this app?
  * What potential issues could arrise? (eg file sharing service used to host malicious files)
* Is the app designed to be open to the public (eg Overseerr)
* What is the potential risk if the app is breached?
  * What damage can be done? (eg deleting all VM's in Proxmox

If the app needs to be externally available but has some risk associated with it, it could be made available with [Cloudflare Authentication](../guides/cloudflare-guides/tunnel/enable-authentication.md) in front of it



