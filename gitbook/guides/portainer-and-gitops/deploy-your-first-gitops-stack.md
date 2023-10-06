# Deploy your first GitOps stack

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>30 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

## What is a Stack?

Stacks is how Portainer handles Docker Compose files, which are infrastructure as code documents for spinning up multiple containers, volumes and networks in 1 go.&#x20;

Most docker container developers now provide example compose files with their projects. Its also possible to Google and find examples online

***

For example, you may have a service that has a separate container for

* _a database_
* _a web app_
* _back end compute_

_The database and back end compute may be on Network 1, while the web app is on Network 2._

_All 3 containers have access to the same 'config' volume, but the database and webapp have their own unique volumes_

_The database and backend need the same credentials to the 2 systems can talk, but the web app has separate credentials for the gui. These are environmental variables_

***

Instead of manually setting up and inputting environmental variables, networks and volumes for each container this can all be written into the compose and and span up in 1 click. This also makes the set up system agnostic.

## Deploy your Stack

In this practice, we're going to deploy the Ombi compose file we created in [create-your-first-gipops-compose-file.md](create-your-first-gipops-compose-file.md "mention")

1. Log into Portainer
   1. If you set up Edge Agents, click on the host you want Ombi to exist on
2. On the left hand menu, select 'Stacks'
3. Click on 'Add stack'
   1. Give your stack a name (eg "ombi")
   2. Build method = Repository
   3. Tick 'Authentication'
      * Username = Github Email
      * [Personal Access Token](set-up-github.md#create-a-private-access-token-for-portainer)
   4. Repository URL = the URL of your repo + .git (eg "https://github.com/trentnbauer/agg.local.git_"_)
   5. Compose path = docker-compose/ombi.yml
   6.  Tick 'Automatic updates'\
       You should see a page similar to this,\


       <figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption><p>Pro tip - you can use the Fetch interval to delay updates on some services. An example I have of this is my redundant Cloudflare Tunnel containers. One server updates every 15 minutes, the other updates every 24hours. They both use the same compose file. This means if an update breaks the 15m instance, then 24h is still live for me to roll back</p></figcaption></figure>
   7.  Scroll down to the 'environment variables' section and add the following variables

       * TZ
       * BASE\_URL
       * PORT\_HTTP

       <figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
   8. Provide your TZ per the ['tz identifier' here](https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones)
   9. Provide a base url, if you are using one (not required)
   10. Provide the port that Ombi will use
   11. Click on 'Deploy the stack'

If you have any issues, refer to the Notifications tab at the top of the page;

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Most Portainer errors can be resolved with a simple Google search but I also have my own [troubleshooting list here](../../troubleshooting/portainer.md)
