# NetbootXYZ

[Link to App](https://netboot.xfgn.dev)

[Link to GitHub or Website](https://github.com/netbootxyz/netboot.xyz)

netboot.xyz is a convenient place to boot into any type of operating system or utility disk without the need of having to go spend time retrieving the ISO just to run it. [iPXE](http://ipxe.org/) is used to provide a user friendly menu from within the BIOS that lets you easily choose the operating system you want along with any specific types of versions or bootable flags.

This app is hosted on Cocoa as a docker container

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg/blob/main/docker-compose/netbootxyz.yml" %}

| Port | Purpose                       |
| ---- | ----------------------------- |
| 3000 | WebUI                         |
| 69   | DHCP / PXE booting            |
| 80   | File store for bootable media |

| Host Volume | Container Volume | Purpose                          |
| ----------- | ---------------- | -------------------------------- |
| assets      | /assets          | stores downloaded bootable media |
| conf        | /config          | stores configuration files       |

| Integration | Purpose               |
| ----------- | --------------------- |
| UniFi       | Set as PXEBoot server |

\
