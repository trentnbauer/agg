# ❔ Create new Virtual Machine

_Please read_ [creation-and-managment-of-servers-or-services.md](../policies-and-procedures/creation-and-managment-of-servers-or-services.md "mention") _documentation for policies around naming schemes etc before following this guide_

## Process

1. Create a new VM in Proxmox
   1. ???
2. SSH into VM and confirm credentials are working
3. Create a new environment in Portainer
   1. ???
4. Push base config via [Ansible](https://ansible.xfgn.dev/project/1/inventory)
   1. Navigate to Inventory and update 'zzNew VM Setup' to point at new server\
      ![](<../.gitbook/assets/image (27).png>)
   2. Edit the Production & Test group and add the machine as root@hostname\
      ![](<../.gitbook/assets/image (7).png>)
   3. Navigate to Key Store and update 'zzNew VM Setup'\
      ![](<../.gitbook/assets/image (30).png>)
   4. Navigate to Task Tempates and select the 'New VM's tab
   5. Run the 'Install SSH Key' playbook
   6. Run the other playbooks in the New VM's tab
5. Update Ansible environment
   1. ???
