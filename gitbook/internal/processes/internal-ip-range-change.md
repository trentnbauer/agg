# Internal IP Range Change

## IP Range Change

Primary DNS server is Americano and secondary is Espresso, Both are hosted in Macaroni (Static 10.0.0.2)

1. UniFi / Firewall changes
   1. Reserve IPs for
      * Fettuccine
      * Americano
      * Espresso
      * Cappuccino
      * Americano
      * Tea
      * Cocoa
      * Mocha
   2. Update DHCP to
      * hand out Americano and Espresso as DNS servers
      * network boot IP to Cocoa
      * network boot image to `netboot.xyz.kpxe`
      * Set domain to `agg.local`
   3. Reboot firewall to force DHCP update
   4. Update Firewall Rules to new IPs
      1. Port Forwards
         * 32400 to Latte (Plex)
         * 27000-27100,19132 to Mocha (Ptero Wings)
         * 6881 to Fettuccine (qBittorrent)
         * 789 to Americano (WireGuard)
2. Update AdGuard DNS rewrite for Fettuccini in Americano & Espresso
3. Macaroni may need its DNS manually updated in Datacenter > Macaroni > System > DNS&#x20;
4. Portainer changes
   * Cappuccino - Wireguard Stack, update DNS (pull & redeploy)
   * Americano - AdGuard Sync stack, update DNS (pull & redeploy)
5. Reboot Fettuccine
6. Reboot Latte and
   * confirm NFS shares have mounted
     * /nfs/Media (must contain folders)
     * /nfs/downloads (must contain folders)
   * Confirm Plex is working
