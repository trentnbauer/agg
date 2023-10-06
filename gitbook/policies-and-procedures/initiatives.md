---
description: Current and future plans
---

# Initiatives

You'll find short blurbs on current and future XFGN / AGG Projects here

## GitOps

Shifting everything to GitOps

## Google Auth Everywhere

The Google Auth Everywhere project is simple - enable Google's OAuth wherever possible. The goal here is to remove the reliance on multiple logins to access 1 tool, such as accessing Portainer.&#x20;

My goal here is to implement something close to SSO. We'll call is Semi-SSO.

#### The current workflow is;

1. Browse to URL
2. Input email that is on the auth allowed list (first cred)
3. Input 6 digit pin from email
4. Log into Portainer with credentials from Bitwarden (second cred)

#### The GAE project would update this process to be;

1. Browse to URL
2. Click on 'Sign in with Google' on Cloudflare sign in page (first cred)
3. Input Google credentials
4. Click on 'Sign in with Google' on Portainer which then auto logs in

### Why Google?

There are plenty of services I can host internally to do this, such as Authentik or FusionAuth as well as other externally hosted services like Auth0. Theres also the question of whether Google is a 'good' company or not, with people now moving to browsers like Brave and away from Googes peering eyes. I'm not really fussed about these issues for my homelab or for AGG. In a work environment - sure. Use something internally managed and hosted, or something more professional (?) like Azure AD.&#x20;

Going down the third party route allows me to offload the security risk of account management off of me and onto these external services. And realistically, hosting it internally would just result in my adding multiple third party auths (eg Google, GitHub and Facebook) which would make the internal tool just aggregate these third parties anyway.

#### Why GOOGLE though? Why not GitHub?

Yeah - great question. GitHub is probably better for my use case - I agree. I guess I'm just used to the _'Sign in with Google'_ button and if I want to bring someone else into the mix they're much more likely to have a Google account vs GitHub.

## The Lungo Project

Lungo is a new Ubuntu server built for server management and security. The goal is to shift away from Netdata, enabling more useful alerts as well as automation for deploying tools (such as docker) and security patching.

### Wazuh

Wazuh is a cybersecurity app that checks your OS configurations against CIS best practice and gives you a report. It also does vulnerability scanning.

#### CIS Scans

Our standard Ubuntu configuration meets 42% of the CIS Ubuntu benchmark. A large portion of the checks are overkill for my environment (such as shifting /tmp to a seperate partition) but part of the goal will be to bring this score up. Wazuh provides guidance on how to do so.

#### Vulnerability Scanning

Wazuh will scan your OS and detect vulnerabilities and provide the CVE code and severity. Any in-progress, backlogged or fixed CVE's will be listed on the [Trello Security Dashboard](https://trello.com/b/qX7rrek9/security-dashboard).

### Ansible Semaphore



### Monitoring Software

I have not made a decision on monitoring software as of yet...

