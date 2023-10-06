# Install Portainer

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>30 Minutes</td></tr><tr><td>Difficulty</td><td>Moderate</td></tr></tbody></table>

## Configuration suggestion

I would suggest configuring a main Portainer with Edge agents and putting no or minimal containers on the main Portainer instance.

This config requires minimum;

* One Portainer instance (VM or Bare Metal)
* One Portainer Edge Agent (VM or Bare Metal)

The advantages of a set up like this are;

* Portainer Edge can browse Docker Volumes. The main Portainer installation cannot browse volumes, thus you cannot download, edit and upload config files from the browser
* Reduced security risk if the public facing instance is breaked
* Segregation
  * A VM for test
  * a VM for prod external services (eg Minecraft server, website)
  * a VM for prod internal only services (eg DNS, PXE)

## Install Portainer

1.  SSH into your Docker host and run the below command to login as root.&#x20;

    ```bash
    sudo -i
    ```

    _Provide your password when prompted_
2.  Copy paste the below commands to install Docker (assumes Ubuntu)

    {% code lineNumbers="true" %}
    ```bash
     sudo apt-get update
     sudo apt-get install ca-certificates curl gnupg
     sudo install -m 0755 -d /etc/apt/keyrings
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
     sudo chmod a+r /etc/apt/keyrings/docker.gpg
    echo \
      "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin
    ```
    {% endcode %}
3.  Run the below command to install Portainer

    ```
    docker run -d --label=com.centurylinklabs.watchtower.enable=true -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer:/data portainer/portainer-ce:latest
    ```

    _I would suggest installing Portainer using this command (rather than the one provided by Portainer themselves) as it adds a WatchTower label to enable auto-updates, which we will set up later in this doco_\

4.  Run the below command and confirm you see that the Portainer container is running

    ```bash
    docker up
    ```

## Configure Portainer

Now that Portainer is installed, we can behind configuring it

### Account Creation

1. Browse to https://YOURSERVERIP:9443
2. Create your credentials, I would suggest following [this guide](../../policies/authentication-access-and-accounts.md)

### (Optional) Install Portainer Edge Agents&#x20;

1. On the left hand pane, click on Environments
2. On the far right, click on 'Add environment'
3. Select 'Docker Standalone' and click on 'Start Wizard'
   1. Click on 'Edge Agent Standard'
   2. Give the instance a name, such as the host name for the server / machine
   3.  The Portainer API server URL should be something like:

       ```
       yourserverIP:9443
       ```

       \
       _If possible, use your servers DNS alias here to make your network more resilient to IP address changes_
   4. Click on Create
   5. Scroll down and locate the 'Docker Standalone' install script and copy this to notepad
      1. Ensure the last line is '`portainer/agent:latest`' and not versioned (eg agent:10.1)
      2.  Replace the the 'docker run' line with the below. This will allow WatchTower to automatically update the Edge Agent

          ```bash
          docker run -d --label=com.centurylinklabs.watchtower.enable=true \
          ```
   6.  SSH into your server that will have the Edge Agent installed and log in as the Root account with the below command

       ```bash
       sudo -i
       ```
   7.  Copy paste the below commands to install Docker (assumes Ubuntu)

       ```sh
        sudo apt-get update
        sudo apt-get install ca-certificates curl gnupg
        sudo install -m 0755 -d /etc/apt/keyrings
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
        sudo chmod a+r /etc/apt/keyrings/docker.gpg
       echo \
         "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
         "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
         sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
       sudo apt-get update
       sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin
       ```
   8. Install the Edge Agent by copy pasting the text copied and altered in step 5
   9. Wait for the container to download and launch
   10. Click on 'Home' and confirm all your agent's show a green Heartbeat
4. Repeat step 3 for any other Edge Agent's you're configuring

### Create Tags & Groups

1. On the left hand panel, click on Environments
2. Click on Tags
3. Create any relevant tags, such as 'Test' or 'Production'
4. On the left, click on Groups
5. Click on Add Group
   1. Provide a name, eg 'Test' or 'Production'
   2. Provide a description, eg 'Test servers'
   3. Click on tags and select the relevant tag
   4. Click on 'Create group'
6. Repeat step 5 for any other groups

#### Apply Tags

1. On the left hand menu, click on Home
2. Click on the pen icon  next to your first Portainer instance
3. under Metadata
   1. Under Group, select the relevant group (eg 'test')
   2. Under Tags, select the relevant tag/s (eg 'test', 'linux')
4. Click on Update environment
5. Repeat for each Portainer instance

### Enable Edge Compute

1. On the left hand panel, click on Settings
2. Click on 'Edge Compute'
3. tick 'Enable Edge Compute features'

### Set up the WatchTower Edge stack

1. On the left, click on 'Edge Stacks' and click on 'Create'
   1. Name your stage 'WatchTower'
   2. Click on Edge Groups and select all your groups
   3. In the web editor, provide the below compose file and hit deploy

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg/blob/main/docker-compose/watchtower-label.yml" %}

2. Take note of the state of your stacks\
   ![](<../../.gitbook/assets/image (44).png>)
3. Click on 'WatchTower' and you will be shown the same screen as step 1. You can make any adjustments to the stack here, such as removing groups or editing the compose file
4. Click on the Environments tab. This will show any server the stack has been deployed too and their state. The stack will be deloyed to any devices in the groups chosen in step 1.2

This stack will enable auto-updates for anything with the `com.centurylinklabs.watchtower.enable=true` label

_Please note: This is NOT a recommended solution for updating containers. This guide will only assist you with updating Portainer, as it is potentially a public facing resource and needs to be patched. The WatchTower auto update may (and probably will) break Portainer at some stage. Keep backups._

