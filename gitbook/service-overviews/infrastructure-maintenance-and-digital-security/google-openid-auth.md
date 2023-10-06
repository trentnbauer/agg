# Google OpenID Auth

Where possible, all services should be set up to use the Google OAuth client. If the service automatically creates accounts and grants permissions when logging in, the service must be behind a [Cloudflare Application](broken-reference) with the [Bypass & Email Auth rules](cloudflare-tunnel.md#authentication) applied

## How to get the Client ID and Client Secret

1. Log into the [Google Cloud Console](https://console.cloud.google.com/apis/credentials?project=xfgn-and-aag-auth) (this link should take you to the 'XFGN and AGG auth' project
2. On the left, click on Credentials
3. Click on 'A Gamers Grind / XFGN'
4. Take note of the Client ID and Client Secret on the right

## Enabling OpenID Authentication on a Service

Google, google, google.

Google is your best friend in this scenario, but here are the generic Google OAuth details

### Add OAuth / OpenID detials to Application

<table><thead><tr><th width="255">Field</th><th>URL</th></tr></thead><tbody><tr><td>Client ID</td><td>Get from the Cloud Console</td></tr><tr><td>Client Secret</td><td>Get from the Cloud Console</td></tr><tr><td>Authorization URL</td><td><a href="https://accounts.google.com/o/oauth2/auth">https://accounts.google.com/o/oauth2/auth</a></td></tr><tr><td>Access Token URL</td><td><a href="https://accounts.google.com/o/oauth2/token">https://accounts.google.com/o/oauth2/token</a></td></tr><tr><td>Resource URL</td><td><a href="https://www.googleapis.com/oauth2/v1/userinfo?alt=json">https://www.googleapis.com/oauth2/v1/userinfo?alt=json</a></td></tr><tr><td>Redirect URL</td><td>URL of app (refer to apps documentation)</td></tr><tr><td>Username</td><td>email (refer to apps documentation)</td></tr><tr><td>Scope</td><td>openid, email, username, profile (refer to apps documentation</td></tr></tbody></table>

These URLs were current as of 11/06/2023

### Add application redirect URL to Google OAuth app

1. Follow [these steps](google-openid-auth.md#how-to-get-the-client-id-and-client-secret) to access the OAuth settings
2. Add the applications domain to 'Authorized JavaScript origins'
3. Add the applications redirect URL to 'Authorized redirect URIs'
4. Click on Save

### Test Authentication

Log out of the app and try logging in with your Google account. If you have issues, refer to the applications documentation.

_**If signing in with a new Google account automatically creates an account, ensure the app is secured behind a**_ [_**Cloudflare Application**_](broken-reference) _**to reduce the risk of unwanted access**_

