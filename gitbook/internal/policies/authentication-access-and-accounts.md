# Authentication, Access and Accounts

## Creation of accounts

### Creating Accounts

* Usernames and passwords should be randomly generated
* Username must not be 'Administrator', 'root', 'Admin' etc unless the application forces it
* Where possible, OpenID should be [configured using the AGG / XFGN OAuth Client](../service-overviews/infrastructure-maintenance-and-digital-security/google-openid-auth.md)
  * Enable AutoLogon where possible
* For services behind CloudFlare authentication, local auth can be disabled

### Storage of account details

* Usernames, passwords, TOPT and backup codes to be saved in [Bitwarden](https://bitwarden.com)
* Internal services (eg docker containers) are to be saved in the 'Internal Services' folder, whilst external services (eg Cloudflare login, Namecheap login (domain registrar)) are saved in the 'External Services' folder.

### Why?

This is to ensure that accounts are hard to breach and are backed up and stored appropriately

## Permissions

* Accounts required for integrations between systems (eg Home Assistant monitoring UniFi) must first be created as read-only
* Administrator access to be granted as last ditch
  * Do NOT create new accounts as administrators
  * Log in with new account and test before granting additional access
  * Research app online to figure out what access is required if more is required
  * Administrator access can be granted ONLY IF required

### Why?

This is to ensure that if accounts are breached, they can't do much damage

## Deletion of Accounts

* Credentials to be tested and confirmed working, then saved in Bitwarden before deletion

### Why?

This is to ensure that when accounts are deleted they can be restored if they are in use somewhere we don't know.
