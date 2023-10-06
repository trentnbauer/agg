# Creation and Managment of Servers or Services

## Creation of Servers

To create a new VM, follow [create-new-virtual-machine.md](../processes/create-new-virtual-machine.md "mention")

* The naming scheme is to be followed
  * Physical hardware: Pasta styles / shapes
  * Virtual prod: Hot drinks
  * Virtual test: Herbs
  * Offsite prod: Cold drinks
  * Refer to UniFi console for names in use
* Use Ubuntu Server or Windows server
* If VM, add relevant tags (eg DNS, WebServer) in Proxmox
* Refer to the Server and Service Monitoring procedure

#### Why?

This is to ensure that each VM is set up with a similar approach, with the same software and can be easily identified by hostname

## Creation and Management of Services

* Use Docker where possible
* If VM, update tags in Proxmox
* If major changes, create a snapshot or backup if possible
* Create documentation
  * Refer to the BookStack Templates\

* Create workflows on draw.io if the system is complicated
* Refer to the Server and Service Monitoring section
* Refer to the Github and Docker section

#### Why?

This is to ensure our documentation is current and standardized and that changes can be reverted. Proxmox tags allow for quick glancing at what each server does

## Linking servers and services together

* Link servers by DNS alias, not IP (unless forced by application)
  * If application forces IP, update the [IP Range Change doco](broken-reference) with instructions on what and how to change it if the IP changes on the server

#### Why?

This is to ensure the network is resilient to IP changes

## Deletion of Servers and Services

* Create backup or snapshot of server or services
* Switch off server or service and wait a week\

* Test high priority services are still functioning&#x20;

#### Why?

This is to reduce the potential impact of deleting a production service

## Backups

* If Incremental backups are possible, backup 6 hourly
* If full backups are the only option, back up 24 hourly
* Test servers don't need regular backups
* Where possible, backup retention is
  * 3 hourly
  * 6 daily
  * 3 weekly
  * 2 monthly
* If server cannot be backed up through conventional methods, use [Google Drive](../service-overviews/other-adhoc-apps/google-drive-sync.md)

#### Why?

This is to ensure that dead servers can be fixed or bad changes can be reverted

## Github and Docker

Where possible, GitOps ("Github code as infrastructure") should be used to manage apps and servers

### Docker Compose Files

* All compose files must be placed in the docker-compose folder
* All compose files must not contain any identifiable or usable information, such as API keys
* All compose files should be written to be re-usable format where possible
  * $PORT variable on host port (left side)
  * $TZ variable for Timezone
  * Refer DNS aliases where possible\

* All compose files should use Docker volumes
* Compose files should only use 'host' networking when required
* Don't use 'latest' or 'dev' tag for docker containers - always use version numbers
* Use github docker containers where possible

### Portainer Stacks

* All containers should be created as stacks
* When using a Github stack
  * Tick Authentication
  * Tick Automatic Updates
  * Open Bitwarden and prefill the Portainer admin account (this will prefill github creds etc)
* Update the docker-compose/\*.yml to include the correct yaml file
* Do not migrate stacks between hosts

### Portainer Edge Stacks

Similar to stacks as above, but can be applied over a group of servers, such as 'prod'. Please note that the compose files are NOT updated with GitHub and will need to be manually updated.

### Portainer Agent

All docker servers should have a Portainer agent installed to enable central management

### Renovate bot

The aGG github repository is monitored by the Renovate bot. This bot checks version numbers in containers

#### Why?

This is to follow the 'infrastructure as code' and/or 'GitOps' standards. When a docker container requires changing, the Github is updated and that syncs down to Portainer. This is significantly easier to manage updating and changing containers vs running straight containers. This also makes the containers agnostic to the host software (Portainer in this case). If Portainer dies, the contents of the compose files aren't lost.

Running applications as containers makes them host agnostic and does not affect the functioning of the host machine. This means that installing application B next A won't install dependancies that break A.

_These policies do not need to be followed for the test environment_
