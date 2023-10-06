# Patch Day

### This documentation is not applicable anymore

_We have migrated from this process to Ubuntu's "Unattended-Upgrades" software, which updates the OS automatically._



\=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

Patch day is handled by Ansible, pushing the below playbooks, and then sanity checked by an XFGN admin

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg.local/blob/main/ansible/update-apt-packages.yaml" %}

## Patch Day Process

1. First Tuesday 4:30PM arrives,
2. Ansible runs patch day playbooks at 4:30pm on servers
   1. apt-update
   2. apt-upgrade
   3. reboot

### Patchday Checklist

* [ ] Confirm services are online at [the Status Page](https://xfgn.dev)
* [ ] Confirm content can be streamed via Plex
* [ ] Confirm one of the game servers are joinable
* [ ] Confirm Wireguard VPN is working
