# Crowdsec Modules



## Install Crowdsec

Crowdsec is pushed via Ansible, but if you're following along at home... [follow the official guide instead](https://docs.crowdsec.net/docs/v1.0/getting\_started/install\_crowdsec/) :P&#x20;

## Firewall Bouncer

This guide is applicable to Ubuntu 20.04 LTS, which uses nf tables by default

1.  Run the below commands and take note of the API key

    {% code overflow="wrap" %}
    ```bash
    apt install crowdsec-firewall-bouncer-iptables -y
    cscli bouncer add firewall #copy the API key
    ```
    {% endcode %}
2.  Remove and edit the config file

    ```bash
    rm /etc/crowdsec/bouncers/crowdsec-firewall-bouncer.yaml
    nano /etc/crowdsec/bouncers/crowdsec-firewall-bouncer.yaml
    ```
3.  Paste in my config below (please update the variables marked with a $)

    ```yaml
    mode: nftables
    update_frequency: 300s
    log_mode: file
    log_dir: /var/log/
    log_level: info
    log_compression: true
    log_max_size: 100
    log_max_backups: 3
    log_max_age: 30
    api_url: http://127.0.0.1:8080/
    api_key: $APIKeyFromStep1
    insecure_skip_verify: false
    disable_ipv6: false
    deny_action: DROP
    deny_log: false
    supported_decisions_types:
      - ban
    #to change log prefix
    #deny_log_prefix: "crowdsec: "
    #to change the blacklists name
    blacklists_ipv4: crowdsec-blacklists
    blacklists_ipv6: crowdsec6-blacklists
    #type of ipset to use
    ipset_type: nethash
    #if present, insert rule in those chains
    iptables_chains:
      - INPUT
    #  - FORWARD
    #  - DOCKER-USER

    ## nftables
    nftables:
      ipv4:
        enabled: true
        set-only: false
        table: crowdsec
        chain: crowdsec-chain
        priority: -10
      ipv6:
        enabled: true
        set-only: false
        table: crowdsec6
        chain: crowdsec6-chain
        priority: -10

    nftables_hooks:
      - input
      - forward

    # packet filter
    pf:
      # an empty string disables the anchor
      anchor_name: ""

    prometheus:
      enabled: false
      listen_addr: 127.0.0.1
      listen_port: 60601
    ```
4.  Restart the service and review the nft tables

    ```bash
    systemctl restart crowdsec-firewall-bouncer.service
    systemctl restart crowdsec
    nft list tables
    ```
5.  You should see an output similar to below

    ```bash
    table ip nat
    table ip filter
    table ip6 filter
    table ip crowdsec
    table ip6 crowdsec6
    ```

## Cloudflare Bouncer

I've read online that the original bouncer isn't as good as the workers module but I'm a bit nervous to use the worker as, to me, it reads like it reviews every request coming through Cloudflare. This is great because it checks current data against current data but you only get x amount of free worker compute. If you're DDOS'd I imagine the bill would be big. So I've gone with the WAF based bouncer, which is pretty slow to update and can get API limited.

### Generate an API key for Cloudflare

1. Navigate to[ user profile > API keys](https://dash.cloudflare.com/profile/api-tokens)
2. Create a new API token and pick custom token
3.  Give it the following permissions\


    <figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption><p>You can limit the API key to certain domains here but you can also do it in the Crowdsec config file</p></figcaption></figure>
4. Click continue to summary and then create token
5. Copy the API token into your password vault

### Gather your Account and Zone IDs

1. Go to your [Cloudflare dashboard](https://dash.cloudflare.com/) and open one of the domains you wish to protect with Cloudflare
2. Scroll down and locate the API section on the right
3. Take note of your Zone ID (I would recommend formatting it like $ZONEID #my.domain.com)&#x20;
4. Take note of your Account ID
5. Repeat steps 1 - 3 for each domain you wish to protect (the account ID will most likely be the same for each)

### Install the Crowdsec module and bouncer



Example config:

{% code overflow="wrap" %}
```yaml
#Cloudflare Config.
cloudflare_config:
  accounts:
  - id: ACCOUNT_ID
    token: API_TOKEN
    ip_list_prefix: crowdsec
    default_action: managed_challenge
    zones:
    - actions:
      - managed_challenge # valid choices are either of managed_challenge, js_>      zone_id: 182bacc7aaeda7bf1f1e35c79883b3f8 #agamersgrind.com
    - actions:
      - managed_challenge
      zone_id: ZONE_ID1 #my.domain.com
    - actions:
      - managed_challenge
      zone_id: ZONE_ID2 #your.domain.net
      ### Want to add another zone? Copy paste from here...
    - actions:
      - managed_challenge
      zone_id: ZONE_ID3 #grandmas.domain.xyz
      ### ... End copy paste here

  update_frequency: 5m # the frequency to update the cloudflare IP list. I have set this frequency to be very low to reduce risk of API limiting

```
{% endcode %}

## Allow CURL or remote access to Crowdsec Engine

Some tools, such as Docker containers, are considered 'external' to the host device. This causes Crowdsec (and potentially the firewall) to block communications for this app or module.

For example, I had to do this for the Wordpress Crowdsec plugin, which sits inside my Wordpress container.

1. SSH into the server you wish to link back too (generally the host machine)
2.  Input the below command allow Crowdsec through the firewall and to edit the Crowdsec config file

    ```bash
    ufw allow 8080
    nano /etc/crowdsec/config.yml
    ```
3. Locate 127.0.0.1:8080 and change it to 0.0.0.0:8080\
   This allows any device to talk to the Crowdsec engine on that device
4. Save the config file
5.  Input the below command to restart Crowdsec

    ```bash
    systemctl restart crowdsec
    ```
6. You will now be able to CURL the Crowdsec engine
