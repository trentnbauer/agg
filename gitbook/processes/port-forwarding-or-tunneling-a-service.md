# ❔ Port Forwarding or Tunneling a Service

This guide will step you through Port Forwarding or Tunneling a service

## Required Knowledge

[external-access-to-systems.md](../policies/external-access-to-systems.md "mention")

[monitoring-and-alerting.md](../policies/monitoring-and-alerting.md "mention")

## Process

### Cloudflare Tunnel



### Port Forward

#### Enable Firewall

As port forwarding opens the server to direct access via the internet, we require the firewall to be enabled.

Ubuntu:

1.  SSH into server and input the below commands

    ```
    ufw allow ssh
    ufw enable
    ```
2.  Run the below command to get a list of active ports on the server and take note of anything that may need to be allowed through. Take note of anything with 'docker', as these will be containers running on the host (you can compare this data against Portainer)

    ```
    lsof -i -P -n | grep LISTEN
    ```
3.  Use the below command to allow ports through the firewall

    ```
    ufw allow PORT
    ```
4. Ensure that the [Crowdsec Firewall bouncer ](crowdsec-modules.md)is enabled and configured

_Please note: allowing a port through the firewall does NOT allow public access to the port. Even if a service is secured behind a Cloudflare tunnel, the port will need to be opened for the CF Tunnel app to connect to the server._

{% code title="Generic "allow list", applicable to all our servers" %}
```
ufw allow 8080 #crowdsec
```
{% endcode %}

#### Port Forward in UniFi

1. Navigate to the [UniFi site manager ](https://unifi.ubnt.com/)and select Rigatoni
2. Click on the settings cog in the bottom left
3. Select Security > Port Forwarding
4. Click on Create Entry

Please ensure the port forward rule is named using the below scheme

| Host (forwarding too) | Purpose                                                    | Firewall Rule Name    |
| --------------------- | ---------------------------------------------------------- | --------------------- |
| Lungo                 | API to allow remote devices to connect to Wazuh            | Lungo - Wazuh API     |
| Latte                 | Ports to make Plex available externally                    | Latte - Plex Ports    |
| Espresso              | Portainer Edge Agent connection to Portainer main instance | Espresso - Edge Agent |

Where possible, limit the 'From' IP to the relevant client (eg VPS)

