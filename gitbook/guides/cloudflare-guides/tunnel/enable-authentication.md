# Enable Authentication

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>Time Required</td><td>5 Minutes</td></tr><tr><td>Difficulty</td><td>Low</td></tr></tbody></table>

_Please note this will not work without a_ [_public hostname being created first_](create-a-proxy-public-hostname.md)

## Adding auth and blocking access to Tunnel

1. Log into Cloudflare and click on Zero Trust
2. Navigate to Access > Applications
3. Click on 'Add an application' and select 'Self hosted'
4.  Fill in the relevant details & click on 'nex

    <figure><img src="../../../.gitbook/assets/image (42).png" alt=""><figcaption><p><em>You can add multiple domains under 1 application</em></p></figcaption></figure>
5.  Replicate these settings and click next

    <figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption><p>I've set this application to require the 'email auth' access group to be satisfied to allow user access</p></figcaption></figure>
6. Accept the defaults, scroll down and click 'Add application'

Now anyone within the 'Email Auth' access group can access the sit
