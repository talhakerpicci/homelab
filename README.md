# My Homelab

Configurations and docker-compose files of my homelab

![dashboard](dashy/my_dashboard.png)

## Server Setup

- Checkout [this](./debian.md) for debian server installation.

- Install and setup rocky: https://rockylinux.org/download (!! BOOT IN UEFI MODE !!)

- Give your user permissions `usermod -a -G wheel rocky`

- Update the config with the following for static ip:

```
sudo nano /etc/sysconfig/network-scripts/ifcfg-enp2s0

TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
IPADDR=192.168.0.26
GATEWAY=192.168.0.1
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=enp2s0
UUID=39e829f8-329b-460d-bef6-05695458de7d
DEVICE=enp2s0
ONBOOT=yes
```

- Update all packages: `sudo dnf upgrade`

- Add EPEL repository: `sudo dnf install epel-release`

- Install essential packages: `sudo dnf install neofetch htop git radeontop`

- Reboot

- Lastly run `sudo radeontop` to see usage stats, and the followings to see all fine:

```
sudo lshw -c video
find /dev -group video
modinfo -V radeon
lspci -nn | grep -E 'VGA|Display'
```

- Docker installation:

```
sudo yum install -y yum-utils

sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io

sudo systemctl start docker

sudo usermod -aG docker $USER

sudo systemctl enable docker.service
```

- Partition the HDD. Guides to follow:

```
https://www.tecmint.com/add-disk-larger-than-2tb-to-an-existing-linux/
https://nixcp.com/format-mount-disk-larger-2tb-linux/
```

- Add the following to `/etc/fstab`:

```
/dev/sda1 /hdd auto nosuid,nodev,nofail,x-gvfs-show 0 0
```

- Change ownership of the /hdd: `sudo chown -R rocky:rocky /hdd`

- Done! Add your services to portainer

## Docker and Portainer

- Portainer installation (checkout: official page for latest version):

```
docker volume create portainer_data

docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:2.11.1
```

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

## Services to Try Later

* gotify
* librespeed
* lidarr
* nginx proxy manager
