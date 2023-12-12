# Portainer

[Link to App](https://portainer.xfgn.dev)

[Link to GitHub or Website](https://www.portainer.io/)

Portainer's _hybrid & multi-cloud container management software_ supports Kubernetes, Docker, Swarm in any Data Center, Cloud, Network Edge or IIoT Device.

The main instance of Portainer is hosted on Espresso but each other Docker host also has the Portainer Edge Agent installed, which enabls central management.

### Flowchart

<figure><img src="../../.gitbook/assets/portainer (1).jpg" alt=""><figcaption></figcaption></figure>

As Portainer needs to be installed BEFORE we can use GitOps compose files, we do not use have a Compose file for it.

```bash
docker run -d --label=com.centurylinklabs.watchtower.enable=true \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /var/lib/docker/volumes:/var/lib/docker/volumes \
  -v /:/host \
  -v portainer_agent_data:/data \
  --restart always \
  -e EDGE=1 \
  -e EDGE_ID=REPLACE WITH ID \
  -e EDGE_KEY=REPLACE WITH KEY \
  -e EDGE_INSECURE_POLL=1 \
  --name portainer_edge_agent \
  portainer/agent:latest
```

_The above run command also enables the Watchtower to update Portainer. Watchtower (only enabled for containers with the watchtower enabled label) is deployed to all hosts as part of the 'all' edge stack. This ensures that Portainer is always up to date, as Portainer cannot update or manage itself._

## Instances

### Portainer

| Port | Purpose   |
| ---- | --------- |
| 9443 | SSL WebUI |
| 8000 | API Port  |

| Host Volume          | Container Volume     | Purpose                         |
| -------------------- | -------------------- | ------------------------------- |
| /var/run/docker.sock | /var/run/docker.sock | Management of docker containers |
| portainer            | /data                | configuration                   |

| Integration  | Purpose               |
| ------------ | --------------------- |
| Google OAuth | Enable authentication |

### Edge Agent

| Port | Purpose  |
| ---- | -------- |
| 8000 | API Port |

| Host Volume          | Container Volume     | Purpose                         |
| -------------------- | -------------------- | ------------------------------- |
| /var/run/docker.sock | /var/run/docker.sock | Management of docker containers |

| Integration | Purpose            |
| ----------- | ------------------ |
| Portainer   | Central management |

## Managing Portainer

### Tags and Groups

Applying a tag to a Portainer instance allows us to organize instances into groups which makes identifying each individual servers function easier as well as some automation.

Currently, we have 4 tags;

* Production
* Production Bare Metal
* Production Synology
* Test

Assigning one or any of these tags to a Portainer instance will add it to the group by the same name

### Stacks

Stacks are Portainers take on Docker Compose. The Compose file can be managed directly in Portainer or via a third party service, such as GitHub. Refer to the [GitOps documentation](gitops-github.md) for more information

### Edge Stacks

Edge Stacks are stacks that are assigned to groups, which are then pushed to any Portainer instance in that group.

Unfortunately, Edge Stacks cannot be managed centrally via GitHub but instead centrally managed in Portainer.
