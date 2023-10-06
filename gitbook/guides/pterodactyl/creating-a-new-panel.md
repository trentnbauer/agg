# Creating a new Panel

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>30 Minutes</td></tr><tr><td>Difficulty</td><td>Moderate</td></tr></tbody></table>

## Installing the Panel

### Setting up the Portainer stack

Create your Portainer stack using the below compose and env file

_I would recommend deploying GitOps (have a look at the_ [portainer-and-gitops](../../service-overviews/portainer-and-gitops/ "mention") _guide) as this will shift your compose file off of your server and into GitOps and manage container updates for you_

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg/blob/main/docker-compose/pterodactyl-panel.yml" %}

{% code title=".ENV File" %}
```editorconfig
MYSQL_PASS=
MYSQL_PASS_ROOT=
PORT_DB=
PORT_HTTP=
MAIL_FROM=
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=youremail@gmail.com
MAIL_PASS=
PTERO_PANEL_URL=https://panel.example.com #your cloudflare subdomain
TZ=
```
{% endcode %}

### Confirm the Panel is running

Check the Portainer logs for the panel container, you should see something similar to below

```
external vars exist.
Checking if https is required.
Checking if letsencrypt email is set.
No letsencrypt email is set using http config.
Removing the default nginx config
Checking database status.
Waiting for database connection...
database (172.28.0.3:3306) open
Migrating and Seeding D.B
   INFO  Nothing to migrate.  
   INFO  Seeding database.  
  Database\Seeders\NestSeeder ........................................ RUNNING  
  Database\Seeders\NestSeeder ................................... 2.34 ms DONE  
  Database\Seeders\EggSeeder ......................................... RUNNING  
*********************************************
*     Updating Eggs for Nest: Minecraft     *
*********************************************
Updated Paper
Updated Bungeecord
Updated Vanilla Minecraft
Updated Sponge (SpongeVanilla)
Updated Forge Minecraft
*************************************************
*     Updating Eggs for Nest: Source Engine     *
*************************************************
Updated Counter-Strike: Global Offensive
Updated Garrys Mod
Updated Team Fortress 2
Updated Ark: Survival Evolved
Updated Insurgency
Updated Custom Source Engine Game
*************************************************
*     Updating Eggs for Nest: Voice Servers     *
*************************************************
Updated Teamspeak3 Server
Updated Mumble Server
****************************************
*     Updating Eggs for Nest: Rust     *
****************************************
Updated Rust
  Database\Seeders\EggSeeder .................................. 175.67 ms DONE  
Starting cron jobs.
Starting supervisord.
2023-06-13 13:41:50,854 CRIT Server 'unix_http_server' running without any HTTP authentication checking
```

#### Confirm the Login page loads

Browse to http://yourserver:port and confirm you see the below

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

## Create your Admin user

1. Open up Portainer and navigate to the Panel container
2. Click on Console and change the command to '/bin/sh'
3. Hit Connect
4. Input the below command and next through the prompts (set account as administrator)

```sh
php artisan p:user:mak
```

5. Log into Pterodactyl with your newly created administrator account

## Configure the Panel

### Enforce 2FA

1. Click on the Settings cog in the top right hand corner
2. Click on Settings
3. Set 'Require 2FA authentication' to 'All Users' and hit Save
4. Click on 'Enable 2FA' and follow the steps
5. Save your backup codes somewhere

### Create Node Locations

1. Click on Locations, then 'create new'
2. Create 2 locations,
   1. a location for 'On Prem' nodes
   2. a location for 'Off Prem' nodes

## Set up your Proxy

1. Refer to the [Cloudflare Proxy](../cloudflare/tunnel/create-a-proxy-public-hostname.md) and [Authentication ](broken-reference)guides

* Your subdomain and domain needs to match the PTERO\_PANEL\_URL variable set above
* Type is HTTP, pointing at yourserver:port

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption><p>panel.example.com proxies to yourserver:port</p></figcaption></figure>

2. navigate to your proxy url and confirm you see the login page
