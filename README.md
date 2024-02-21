this is my desktop/gaming/gpu/vfio/windows11/parsec hypervisor box

## Things not covered by this (yet)

+ Initial Disk formatting
+ Uuser creation
+ DHCP/IP Setup
+ br0/network bridging (was done with this:
```bash
nmcli con show
nmcli con add type bridge ifname br0
nmcli con add type bridge-slave ifname enp5s0 master br0
nmcli con up bridge-br0
```

+ As well as rfkill/disabling radios
```bash
# I can't remember if actually used an rfkill command
nmcli radio wifi off
# theres one for wwan too, i just do it via nmtui
```

## ZFS

Pool was created with:
```bash
zpool create tank mirror /dev/sda /dev/sdb cache /dev/sdc
zpool set cachefile=/etc/zfs/zpool.cache
zfs set mountpoint=none tank 
zfs create tank/plex
zfs set mountpoint=/mnt/plex tank/plex
zfs create tank/var-lib-plex
zfs set com.sun:auto-snapshot=false tank/var-lib-plex
zfs set mountpoint=/var/lib/plex tank/var-lib-plex
zfs create tank/borg
zfs set com.sun:auto-snapshot=false tank/borg
zfs set mountpoint=/mnt/borg tank/borg
```

## vdirsyncer

Sync was started with

```bash
yes | vdirsyncer discover
vdirsyncer sync
```

## TODO

+ try non-lvm raid installation
+ fix bizarre 90 second hang when rebooting
  + Maybe masking `efi.mount`? If unused on the new install?
+ Add shell role with bash options (like `set -o vi`)
+ Populate boot entries in install guide
  + add note about /boot in fstab
+ Configure ytdl_sub systemd timer/unit
+ Have a more elegant way of adding variables  (git hook? right now you use `ansible-vault view vars/vault.yml | cat | sed 's/:.*//' > vars/vault.yml.sample`)
