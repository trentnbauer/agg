# Set up GitHub



<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>10 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

## Set up your Repo

### Create  a Private Repo

_If you have a pre-existing repo you wish to use, skip the creation part._

[Follow GitHubs documentation on creating a repo](https://docs.github.com/en/get-started/quickstart/create-a-repo)

### Ensure Repo is Private

[Ensure your repo is set to private, or switch it to private](https://docs.publishing.service.gov.uk/manual/make-github-repo-private.html)

### Somethings to think about

#### Private vs Public repo

I would recommend creating a Private repo so you can store private information, such as API keys in it.

If you're going to obfuscate your files with variables or secrets (which you will need to store elsewhere), you could make the repo Public. But in my opinion, its better to be safe than sorry.

_Please note that the version history is also public, so if you accidentally save a password or API key its saved forever._

#### What to name your repo

Your repo should be named something you remember and unique to you. It doesn't really matter though. I would suggest something short so your repo url is short

## Create a Private Access Token for Portainer

As your repo is private, you will need to create a PAC for Portainer to use and access the repo

1. Click on your profile in the top right, then select Settings
2. On the left, click on Developer Settings
3. Click on Personal Access Tokens, then 'Tokens (classic)'
4. Click on 'Generate new token', then select 'Classic'
5.  Input the below information\


    <figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>You can expire the credential if you want, though this may break Portainer</p></figcaption></figure>
6. Scroll down and click on 'Generate Token'
7. Save your PAC somewhere safe and smart

## Install Renovate Bot

The Renovate Bot watches for dependancies and automatically creates merge requests to update the contents of your Repo. This allows you to update your containers outside of Portainer as well as review changes made etc.

### Get your Repo ready

1. Create a folder '.github' in the root of your repo
2. Create a folder 'docker-compose' in the root of your repo
3.  In the '.github' folder, create a file 'renovate.json5' with the below contents



    ```json5
    {
        "$schema": "https://docs.renovatebot.com/renovate-schema.json",
        "extends": [
            "config:base",
            ":disableRateLimiting"
        ],
        "docker-compose": {
            "fileMatch": ["docker-compose/.+\\.ya?ml$"]
        }
    }
    ```

_This code block tells the bot to watch any '.yml' or '.yaml' file in the 'docker-compose' folder_

### Install the Bot

1. Follow [this link ](https://github.com/marketplace/renovate)to install the bot
2. You can set renovate to run on all repo's you own, or only your repo created in this doco\
   _This is up to you. If you actually use GitHub for development, it may be best to select only this repo_

### Confirm the bot is installed

1. Browse to your GitHub repo
2. Click on 'Issues', you should see the Dependency Dashboard\
   ![](<../../.gitbook/assets/image (38).png>)

