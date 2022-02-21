# My Homelab
 
Configurations and docker-compose files of my homelab

![dashboard](dashy/my_dashboard.png)

## Server Setup

- Install and setup debian: https://www.debian.org/distrib/netinst (amd64)
- Add sudo user
- Delete dhcp by running `sudo apt-get remove dhcpcd5 isc-dhcp-client isc-dhcp-common`
- Update interface with the following for static ip:

```
nano /etc/network/interface

# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto enp2s0
allow-hotplug enp2s0
iface enp2s0 inet static
address 192.168.0.26/24
gateway 192.168.0.1
```

- Install docker
- Partition the HDD. Guides to follow:

```
https://www.tecmint.com/add-disk-larger-than-2tb-to-an-existing-linux/
https://nixcp.com/format-mount-disk-larger-2tb-linux/
```

- When setting up the HDD for other services to use, change ownership to debian:debian

## Docker and Portainer

- Add services as stack
- Change the default ip address in portainer

## Notes

- Qbittorrent: Make sure to set default torrent management mode to automatic
- Qbittorrent: Change the default ui on the web: `/config/VueTorrent`
- Qbittorrent: If required update the qbittorrent.conf with the following:

```
[BitTorrent]
Session\DisableAutoTMMTriggers\CategorySavePathChanged=false
Session\DisableAutoTMMByDefault=false
Session\DisableAutoTMMTriggers\CategoryChanged=false
Session\DisableAutoTMMTriggers\DefaultSavePathChanged=true
```
