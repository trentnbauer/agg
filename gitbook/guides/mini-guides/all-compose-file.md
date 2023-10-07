# 'All' Compose File

## The Scenario

We want to push a standard compose file to all machines, which enables some standardization across the board.

We're wanting to standardize;

* Automatic updates for specific containers
* Server monitoring
* Server security

#### Requirements

* NetData cloud account (free)
* Docker installed on all servers
* A way to push the Compose file to each server (I recommend Portainer Edge Stacks)

#### Recommendations

* Portainer for managing docker, refer to [Broken link](broken-reference "mention") for a deployment guide

_Please note that WatchTower is NOT a supported method for updating docker containers and automatic updates can cause issues. Please ensure you have backups of your data in case you need to roll back. I would recommend following my_ [Broken link](broken-reference "mention") _documentation as this includes a much safer solution for updating containers._

_Ignoring that, I am using WatchTower to update Portainer in my environment as I am unable to update it without doing it via the CLI. In general, this is the only time I would suggest this as a solution_

## Solution

We're going to create a Docker Compose file to be installed on all machines (this expects Ubuntu, but may work on other Linux distros)

This compose file will container

* NetData (server monitoring and alerts)
* Fail2Ban (security for SSH)
* WatchTower
  * 1 instance for daily updates
  * 1 instance for weekly updates

Any container that requires updating will need the relevant WatchTower label added to enable updates

### Watchtower Tags

_Please ensure that your container version is 'latest' to enable auto-updates. Enabling a version tag (eg v1.1.1) will not allow for auto updates._ eg `netdata:latest`

#### Docker Compose

Add the below to the compose file, at bottom of each container you wish to update. Ensure that the 'labels:' line is at the same spot as image, ports, volumes etc.

{% code title="Compose, daily" %}
```yaml
    labels:
      - com.centurylinklabs.watchtower.scope=daily
```
{% endcode %}

{% code title="Compose, weekly" %}
```yaml
    labels:
      - com.centurylinklabs.watchtower.scope=weekly
```
{% endcode %}

#### CLI

Add the below line to the end of your `docker run` command

<pre class="language-bash" data-title="CLI, daily"><code class="lang-bash"><strong> --label=com.centurylinklabs.watchtower.scope=daily
</strong></code></pre>

{% code title="CLI, weekly" %}
```bash
 --label=com.centurylinklabs.watchtower.scope=weekly
```
{% endcode %}

#### Portainer

1. Edit your container and scroll down to advanced settings
2. Select Labels
3. Click 'Add label'
4.  Add one of the below\


    <table><thead><tr><th width="373">Name</th><th>Value</th></tr></thead><tbody><tr><td>com.centurylinklabs.watchtower.label</td><td>daily</td></tr><tr><td>com.centurylinklabs.watchtower.label</td><td>weekly</td></tr></tbody></table>

### Compose File

_Don't copy paste the file, there are some entries you need to fill_

{% code title="docker-compose.yml" %}
```yaml
version: '3'
services:
  netdata:
    image: ghcr.io/netdata/netdata:latest
    ports:
      - 19999:19999
    restart: unless-stopped
    cap_add:
      - SYS_PTRACE
    security_opt:
      - apparmor:unconfined
    volumes:
      - netdataconfig:/etc/netdata
      - netdatalib:/var/lib/netdata
      - netdatacache:/var/cache/netdata
      - /etc/passwd:/host/etc/passwd:ro
      - /etc/group:/host/etc/group:ro
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /etc/os-release:/host/etc/os-release:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /etc/hostname:/etc/hostname:ro
    environment:
      - NETDATA_CLAIM_TOKEN= #PROVIDE YOUR TOKEN
      - NETDATA_CLAIM_URL=https://app.netdata.cloud
      - NETDATA_CLAIM_ROOMS=
      - CONTAIMNERS=1
    labels:
      - com.centurylinklabs.watchtower.scope=daily


  fail2ban:
    image: ghcr.io/crazy-max/fail2ban:latest
    network_mode: "host"
    cap_add:
      - NET_ADMIN
      - NET_RAW
    volumes:
      - fail2ban:/data
      - /var/log:/var/log:ro
    environment:
      - TZ= #TZ
      - F2B_LOG_TARGET=STDOUT
      - F2B_LOG_LEVEL=INFO
      - F2B_DB_PURGE_AGE=1d
      - SSMTP_HOST=smtp.gmail.com
      - SSMTP_PORT=587
      - SSMTP_HOSTNAME=gmail.com
      - SSMTP_USER= #GMAIL USERNAME
      - SSMTP_PASSWORD= #GMAIL PASSWORD
      - SSMTP_TLS=YES
    restart: always
    labels:
      - com.centurylinklabs.watchtower.scope=daily


  watchtower-daily:
    image: ghcr.io/containrrr/watchtower:latest
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - TZ= #TZ
      - WATCHTOWER_ROLLING_RESTART=true
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_INCLUDE_STOPPED=true
      - WATCHTOWER_POLL_INTERVAL=86400 #1 day in seconds
    labels:
      - com.centurylinklabs.watchtower.scope=weekly
    command: --scope daily


  watchtower-weekly:
    image: ghcr.io/containrrr/watchtower:latest
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - TZ=Australia/Melbourne
      - WATCHTOWER_ROLLING_RESTART=true
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_INCLUDE_STOPPED=true
      - WATCHTOWER_POLL_INTERVAL=604800 #7 days in seconds
    labels:
      - com.centurylinklabs.watchtower.scope=daily
    command: --scope weekly

      
volumes:
  netdataconfig:
  netdatalib:
  netdatacache:
  fail2ban:
```
{% endcode %}
