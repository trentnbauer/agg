# Creating a new Wings node

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>30 Minutes</td></tr><tr><td>Difficulty</td><td>Moderate</td></tr></tbody></table>

_I am installing Wings on my test VM, 'Basil', for writing this documentation. Any references to Basil will need to be changed to your server. Once this documentation has been finalized, Wings will be deleted from Basil._

## Installing Wings and set up Reverse Proxy

### Configure the 'Pterodactyl Wings' Portainer stack

Refer to the [Portainer](broken-reference) and [GitOps](broken-reference) documentation for configuring a new Portainer stack using the AGG Pterodactyl Wings Docker Compose file

* Name the stack something simple, like 'wings'
* Ensure you fill all the variables from the ENV file

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg/blob/main/docker-compose/pterodactyl-wings.yml" %}

{% code title=".ENV file" %}
```editorconfig
PORT_SFTP=2022
TZ=
SQL_PASS=
SQL_PASS_ROOT=
PORT_DB=3306
PORT= #This is the port Cloudflare will talk too
HEALTHCHECK= #this will be your hostname+PORT, eg http://myserver:80
```
{% endcode %}

_I suggest leaving the env pre-filled fields as they are_

### Configure the Reverse Proxy

Refer to the [Create a Proxy](../cloudflare/tunnel/create-a-proxy-public-hostname.md) Cloudflare Tunnel documentation for creating a Proxy, using hostname:port for the url. You could also use IP:port

My hostname is "basil.agg.local" and port "443" (the $port variable set above)

here is the proxy I created;

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption><p>https://basil.xfgn.dev reverse proxies to http://basil.agg.local:443</p></figcaption></figure>



## Configure Panel & Wings

### Set up a Node on the Panel

1. Log into Pterodactyl with an administrator account
2. Click on the Settings cog in the top right
3. Click on Nodes
4. Click on Create New in the top right and
   1. Name your Node (I normally give them the same name as the server
   2. Provide the FQDN of your Node (this is the externally available reverse proxy address configured here [#configure-the-reverse-proxy](creating-a-new-wings-node.md#configure-the-reverse-proxy "mention"))
   3. Tick 'Use SSL Connection'
   4. Tick 'Behind Proxy'
   5. Provide RAM & over allocation of the machine or VM
   6. Provide the disk space & over allocation of the machine or VM
   7. Set your Daemon port to 443 (As Cloudflare uses 443 for its proxy)
   8.  Click on 'Create Node'\


       <figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

### Upload Configuration file to Wings

1. In Pterodactyl, click on 'Nodes' on the left
2. You should see your new Node, with a red heart\
   ![](<../../.gitbook/assets/image (39).png>)\

3. Click on your new node, then the Configuration tab and confirm that the highlighted lines are the same as mine\
   ![](<../../.gitbook/assets/image (41).png>)\
   _I would also recommend changing the upload limit to 1024_\

4. Copy the contents of the file and save it as 'config.yml'
5. Log into Portainer and click on the Wings host
6. Click on 'Stacks' and select the 'wings' stack
7. Click on 'Stop stack'
8. Click on 'Volumes'
9. Locate the Wings 'config' volume - it will be named after your stack (eg wings\_config) and click on Browse
10. Click on the Upload button, then select your config.yml file
11. Click on Stacks and open your Wings stack
12. Restart the stack and wait 30 seconds
13. Refresh the Pterodactyl Nodes page and it should now be connected\
    ![](<../../.gitbook/assets/image (34).png>)

### Assigning Ports to the Node

1. Click on your new node and click the 'Allocations' tab
2. On the right hand panel, input
   1. IP address: 0.0.0.0
   2. IP Alias: Your domain or subdomain that points to your servers public IP
   3. Ports: The port range you will forward to this VM\
      ![](<../../.gitbook/assets/image (11).png>)
   4. Refer to the [Port Forwarding](../unifi/port-forwarding.md) documentation to forward those ports to the host

## Why do I do it this way?

My assumption here is that the Pterodactyl dev's want us to set up and use SSL certs on the Wings host and make it publicly available without a proxy. I can't be bothered generating certs and managing their expirations in my Homelab. My configuration offloads the certificate management to Cloudflare (or any other reverse proxy - I used to use NGINX Proxy Manager for this) which means that I don't need to worry about cycling certificates.

Offloading this task to the Cloudflare tunnel does create a couple of potential issues,

* If the tunnel is down / crashes, the Panel can't talk Wings
* If your hosting the Panel and Wings in your _home_lab, the connection is reliant on the internet being up
* Cloudflare outages may break the Panel / Wings communication
* Pterodactyl Discord does not provide support for proxied panel/wings connections

Please note that I don't have SFTP working with this set up, though I imagine changing the SFTP port and forwarding that port to the host would function fine.
