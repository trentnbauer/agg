# Cloudflare Tunnel

[Link to App](https://www.cloudflare.com/en-au/products/tunnel/)

The Cloudflare tunnel is a port-forwarding-less reverse proxy. The traffic is tunneled through Cloudflare servers reducing the risk of DDOS  and other malicious attacks. Another advantage is that Cloudflare handles authentication, so its harder to brute force one of our internal services.

Refer to [external access to systems](../../policies-and-procedures/external-access-to-systems.md) for documentation on creating a proxy

### Flowchart <a href="#bkmrk-flowchart" id="bkmrk-flowchart"></a>

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

## Authentication

For the public facing services that require authentication (such as Radarr) I have configured 2 policies;

1. Bypass Policy\
   This policy looks at the public IP address of the user accessing the site.&#x20;
   * If it matches the IP's in the policy the user is allowed through
   * Block all traffic outside of Australia
2. Email Auth\
   This policy presents the user with a login page asking for an email address or Google account
   * If a valid address is input, a 6 digit code is emailed to this address which allows the user to authenticate
   * If a valid Google account is selected they are able to authenticate

_The list for both the 6 digit code and Google accounts are the same_



The Tunnels are hosted as Docker containers, on Espresso and Americano. This is to allow for load balancing / fail over when required.

<figure><img src="https://kb.xfgn.dev/uploads/images/gallery/2023-05/scaled-1680-/cloudflare.jpg" alt=""><figcaption></figcaption></figure>
