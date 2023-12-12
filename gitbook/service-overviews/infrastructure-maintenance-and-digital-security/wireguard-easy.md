# WireGuard Easy

[Link to App](https://wireguard.xfgn.dev/)

[Link to GitHub or Website](https://github.com/wg-easy/wg-easy)

WireGuard® is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache. It intends to be considerably more performant than OpenVPN. WireGuard is designed as a general purpose VPN for running on embedded interfaces and super computers alike, fit for many different circumstances. Initially released for the Linux kernel, it is now cross-platform (Windows, macOS, BSD, iOS, Android) and widely deployable. It is currently under heavy development, but already it might be regarded as the most secure, easiest to use, and simplest VPN solution in the industry.

WireGuard Easy,

* All-in-one: WireGuard + Web UI.
* Easy installation, simple to use.
* List, create, edit, delete, enable & disable clients.
* Show a client's QR code.
* Download a client's configuration file.
* Statistics for which clients are connected.
* Tx/Rx charts for each connected client.
* Gravatar support.

This app is hosted on Cappuccino as a docker container

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
version: "3.8"
services:
  app:
    environment:
       - WG_HOST=$HOST
       - PASSWORD=$PASSWORD
       - WG_PORT=$PORT_VPN
       #- WG_DEFAULT_ADDRESS=172.1.72.x
       - WG_DEFAULT_DNS=$DNS1, $DNS2
      # - WG_MTU=1420
       - WG_ALLOWED_IPS=10.0.0.0/8
      # - WG_PRE_UP=echo "Pre Up" > /etc/wireguard/pre-up.txt
      # - WG_POST_UP=echo "Post Up" > /etc/wireguard/post-up.txt
      # - WG_PRE_DOWN=echo "Pre Down" > /etc/wireguard/pre-down.txt
      # - WG_POST_DOWN=echo "Post Down" > /etc/wireguard/post-down.txt
      
    image: weejewel/wg-easy:7
    volumes:
      - app:/etc/wireguard
    ports:
      - $PORT_VPN:51820/udp
      - $PORT_WEB:51821/tcp
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.ip_forward=1
      - net.ipv4.conf.all.src_valid_mark=1   
    dns:
      - $DNS1
      - $DNS2
volumes:
  app:
```
{% endcode %}

| Port | Purpose  |
| ---- | -------- |
| 789  | VPN Port |
| 5100 | WebUI    |

| Host Volume | Container Volume | Purpose                          |
| ----------- | ---------------- | -------------------------------- |
| app         | /etc/wireguard   | configuration files and app data |

| Integration | Purpose |
| ----------- | ------- |
|             |         |

\
