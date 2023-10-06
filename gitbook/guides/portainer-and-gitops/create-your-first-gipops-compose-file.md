# Create your first GipOps Compose file

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>30 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

## Create your first Compose file

In this example, we're going to massage the LinuxServer.io's Ombi image to match our best practices, per below

### Create your Compose file

1. Open your GitHub repo and enter the docker-compose folder
2. Click on 'Add file' > 'Create new file'
3.  Name the file 'ombi.yml', and copy the compose file [from here](https://docs.linuxserver.io/images/docker-ombi)\
    It should look something like this:

    <figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>
4. Refer to the best practices and tips documentation below and edit the file
5. Hit 'Commit changes...'
6. hit 'Commit changes'

Oh, you want **me** to give you the answer to step 4?

As of writing, here is the ombi.yml compose file

<pre class="language-yaml" data-title="original-ombi.yml" data-line-numbers><code class="lang-yaml">---
version: "2.1"
services:
  ombi:
    image: <a data-footnote-ref href="#user-content-fn-1">lscr.io/linuxserver/ombi:latest</a>
    <a data-footnote-ref href="#user-content-fn-2">container_name: ombi</a>
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=<a data-footnote-ref href="#user-content-fn-3">Etc/UTC</a>
      - BASE_URL=/<a data-footnote-ref href="#user-content-fn-4">ombi</a> #optional
    volumes:
      -<a data-footnote-ref href="#user-content-fn-5"> /path/to/appdata/config:/config</a>
    ports:
      - <a data-footnote-ref href="#user-content-fn-6">3579</a>:3579
    restart: unless-stopped
</code></pre>

Here is my tweaked file

<pre class="language-yaml" data-title="bestpractice-ombi.yml" data-line-numbers><code class="lang-yaml">---
version: "2.1"
services:
  ombi:
    image: <a data-footnote-ref href="#user-content-fn-7">ghcr.io/linuxserver/ombi:4.39.1</a>
    <a data-footnote-ref href="#user-content-fn-8">#container_name: ombi</a>
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=<a data-footnote-ref href="#user-content-fn-9">$TZ</a>
      - BASE_URL=/<a data-footnote-ref href="#user-content-fn-10">$BASE_URL</a> #optional
    volumes:
      - <a data-footnote-ref href="#user-content-fn-11">config:/config</a>
    ports:
      - <a data-footnote-ref href="#user-content-fn-12">$PORT_HTTP</a>:3579
    restart: unless-stopped
<a data-footnote-ref href="#user-content-fn-13">volumes:</a>
<a data-footnote-ref href="#user-content-fn-14">  config:</a>
</code></pre>

### Best Practices and tips

#### Variables

To keep your compose files as system agnostic as possible, its best to use variables where you can. Some suggestions are

* Ports (eg $HTTP\_PORT, $DB\_PORT, $API\_PORT)
* Mounted volumes (eg $CONFIG, $DATA)
* Timezones (eg $TZ)

These variables can then be set in Portainer when creating the stack

#### Don't name containers unless there is a requirement for it

Some compose files may name a database 'db', which will then block you from using that name anywhere else. Its best to let Portainer manage names of containers.

#### Container Image versioning

Do not use the 'latest' tag, as it does not allow Renovate to update the compose file

When versioning your compose files, locate the 'latest' tag on the DockerHub, GitHub etc and use the relevant version number.

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

This is harder to do on DockerHub. Per the screenshot below, we've located the 'tatest' tag and used the Digest to locate the correct version.

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption><p>Take note of the OS/ARCH as well - you can see that tag 2.12.5 contains both AMD64 and ARM64 containers, meaning this compose file can be used on both ARM and x86 machines.</p></figcaption></figure>

#### Container Image locations

As Docker has now started rate limiting non-paying users, a lot of container creators are putting their containers on GitHub or other services

Its quite easy to figure out if a container exists on GitHub;

1.  If the 'Packages' option exists, the container is on GitHub\


    <figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
2.  The container URL is underlined\


    <figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

[^1]: Using the Latest tag

[^2]: does the container actually need a name? what if this name is taken?

[^3]: Environment variable that could be a variable instead

[^4]: manually specified

[^5]: Direct path, not a docker volume. This path won't exist on your machine either

[^6]: Port is specified in the compose file. What if this port is in use?

[^7]: ```
    moved to github hosted image, with version number specified for 'latest' release
    ```

[^8]: commented out container name, this will be ignored by compose

[^9]: timezone is now a variable. this compose file can be used anywhere in the world without issues

[^10]: base URL is now a variable. Maybe we want to use /request instead of /ombi

[^11]: instead of a direct path, we're now using a volume (refer to last 2 lines)

[^12]: we can now run this on a different port without having to edit the config file

[^13]: create volume "config"

[^14]: create volume "config"
