# Proxmox

| Proxmox Virtual Environment         | Proxmox Backup Server               |
| ----------------------------------- | ----------------------------------- |
| [Link to App](https://pve.xfgn.dev) | [Link to App](https://pbs.xfgn.dev) |

## Physical Hardware

<table><thead><tr><th width="185.33333333333331">Name</th><th width="192">Type</th><th>PVE</th><th>PBS</th></tr></thead><tbody><tr><td>Macaroni</td><td>Consumer Hardware</td><td>Yes</td><td>Yes</td></tr></tbody></table>

## Macaroni

### Specifications

<table><thead><tr><th width="234">Component</th><th>Model</th></tr></thead><tbody><tr><td>Case</td><td>Silverstone Alta G1M White</td></tr><tr><td>Motherboard</td><td>MSI B660M Pro</td></tr><tr><td>CPU</td><td>i5 12600</td></tr><tr><td>RAM</td><td>4x32GB Vengeance LP, 3200mhz</td></tr><tr><td>GPU</td><td>GTX 1050</td></tr><tr><td>Power Supply</td><td>Corsair 650w SFX</td></tr><tr><td>Storage</td><td>3x NVME 1TB SDD<br>2x SATA 4TB HDD<br>1x SATA 128GB SSD</td></tr><tr><td>Other</td><td>HomeAssistant SkyConnect dongle<br>ZWave Dongle</td></tr></tbody></table>

### Storage Pools

<table><thead><tr><th width="155">Name</th><th width="155">Type</th><th>Drives</th><th>Purpose</th></tr></thead><tbody><tr><td>SSDRAID</td><td>NFS, RAIDZ</td><td>3x 1TB NVME</td><td>VM Disk storage</td></tr><tr><td>HDD-MIRROR</td><td>NFS, MIRROR</td><td>2x 4TB HDD</td><td>Backup storage</td></tr></tbody></table>

<details>

<summary>Images</summary>

![](<../../../.gitbook/assets/image (13).png>)<img src="../../../.gitbook/assets/image (28).png" alt="" data-size="original">

</details>
