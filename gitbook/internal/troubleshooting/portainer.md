---
coverY: 0
---

# Portainer

## Error 500&#x20;

1. Check the container in Portainer and take note of what ports it uses
2. SSH into VM and `Sudo -i` into the root account
3.  Run the below commands and confirm

    ```
    clear
    sudo lsof -i:PORT
    ```
4. If the ports are in use you will see an output similar to this;\
   <img src="../.gitbook/assets/image (53).png" alt="" data-size="original">\
   _Please note: This screenshot is for port 22, which is used for SSH. This is an EXAMPLE ONLY screenshot. Do NOT disable port 22._
5. Investigate why these ports are in use. Some suggestions;
   1. Reboot VM
   2. Refer to Google (eg looking up whatever is in the 'name' category)

### Error 500 (with GPU passthrough)

Refer to the Docker Compose file for 'runtime: nvidia'

#### Reinstall the Nvidia Drivers

1. SSH into Latte and _Sudo -i_ into the root account
2.  Run the below command and next through the prompts

    ```
    ./NVI
    ```
3. Try starting Plex

#### Remove and re-add the GPU

1. Connect to [Proxmox ](https://pve.xfgn.dev)and log in with an Admin account
2. On the left, Expand Macaroni and select Latte
3. Click on 'Shutdown' and wait for the VM to stop
4. Click on the Hardware tab
5. Remove 2 'PCI Device' devices
   1. Click on the first PCI Device
   2. Click on Remove
   3. Repeat
6. Add the Nvidia GPU back
   1. Click on Add > PCI Device
   2. Under the Devices dropdown, locate 'NVIDIA Corporation'
   3. Click on Add
   4. Repeat for any other 'NVIDIA Corporation' devices
7. Click on 'Start'
8. You may need to reinstall the drivers[ per above](portainer.md#reinstall-the-nvidia-drivers)
