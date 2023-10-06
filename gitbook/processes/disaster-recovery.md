# ❔ Disaster Recovery

As much as we don't want it to happen, somethings things die. Sometimes power runs out.

## What is a 'Disaster'

This documentation can be followed for the below scenarios

* Power outage
* Server hardware failure

## Procedure

### Confirm equipment is OK

1. Check all infrastructure is powered on (look for power lights)
   1. If equipment isn't powered on, press the power button
2. Confirm [Proxmox VE](https://pve.xfgn.dev) is OK
   * [ ] WebUI is loading
   * [ ] All storage pools show
   * [ ] VM's show
3. Confirm [Proxmox Backup Server](https://pbs.xfgn.dev) is OK
4. Confirm the [NAS ](https://nas.xfgn.dev)is accessible
   * [ ] Log in with account from Vault
   * [ ] Open Storage Manager and confirm 'system is healthy'

### Confirm Software has loaded

???
