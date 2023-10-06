# Managing your production compose files

## Updating Containers

Updates are managed via the renovate bot and automatically pulled by Portainer

### Updating the Compose file

When a new update is released, the renovate bot will create a pull request in your repo;

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption><p>the 'home page' of a pull request</p></figcaption></figure>

We can see that this pull request is updating the 'package', 'mariadb'. It is a minor update, going from version 10.5 to 10.11

Clicking on the 'Files changed' tab will list any files that will be updated;

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p>this pull request will replace the red line with the green line</p></figcaption></figure>

If we are happy with this update, we can click on the 'Merge pull request' button at the bottom of the homepage

### Updating Portainer

Portainer will check in with GitHub and download the updated compose file and update the container image.

You can tell Portainer to manually pull by;

1. Browse to the stack
2. Click on 'Pull and redeploy'

## Changing Compose Files

Compose files can be changed in your GitHub repo. These will sync automatically to Portainer
