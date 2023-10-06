# Pterodactyl

## Panel

### Nothing has broken

.... yet



## Wings

### Pool overlaps with other one on this address space

{% code title="Wings container logs" overflow="wrap" %}
```log
failed to configure docker environment error=Error response from daemon: Pool overlaps with other one on this address space
Stacktrace: Error response from daemon: Pool overlaps with other one on this address space
```
{% endcode %}

#### Symptoms

* Servers wont start but containers are running
* Wings node shows as offline in Panel
* Unable to navigate to https://wingsnode:443
* Error above in logs for Wings container

This one is pretty easy to solve and can be done proactively (straight after installing Wings)

Wings, by default, choses docker network 172.18.0.0 for its game servers. This is a low range and it only takes pushing 18 stacks to run into this issue.

[Per Pterodactyl's doco](https://discord.com/channels/122900397965705216/493443725012500490/838633961911091201), the best way to work around this is changing the subnet that Wings uses. To do so,\
(_This guide assumes you followed my doco for installing Ptero)_

1. Log into Portainer and access your Wings node
2. On the left, click on Stacks
3. Click on your Wings stack
4. Scroll down to containers and open the 'Wings' container (not DB)
5. Scroll down and note the volume mapped to /etc/pterodactyl
6. On the left, select Volumes
7. Click on 'browse' next to the volume noted in step 4
8. Download the config file
9. Open it in your editor of choice
10. Locate the 'Docker' section and take note of the highlighted information below

    <figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Red = interface IP<br>Yellow - Subnet range<br>Green - Gateway IP</p></figcaption></figure>
11. Go back to Portainer and select the 'Networks tab
12. Confirm that your v4 subnet is used by another stack
13. Update the 3 fields to be a range that is not in use
    * Red and Green field will be the same and must be in the same subnet range as orange (eg 172.172.0.1)
    * Orange must be a valid subnet range, with the last 2 digits as 0 (eg 172.172._**0.0/16**_)\
      \
      _Aka only change the second IP number ( 123.**123**.123.123 )_
14. Upload the config file back into the relevant volume
15. Restart the Wings stack
