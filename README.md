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
nmcli radio wwan off
```

### NOTE

You had some bridging issues on the most recent clean reinstall, you had to
+ Delete the unneeded connections
```bash
# This will kick you out if remote
nmcli connection delete Wired\ connection\ 1
```
...and recreate the above bridge, which had `enp6s0` for the name. NetworkManager takes a bit to come up too.  This might be a bug, but you have networking working now.


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

+ cleanup libvirt vm to conform with how it gets imported
+ gogs
+ japanese IME
+ handy aliases
+ ansible galaxy installation (`ansible-galaxy install -r requirements.yml`)
+ add sway config
+ rename repo
+ require a mount for plex role to be run 
+ enforce efi boot order
+ enforce fstab efi partition correct umask/permissions
+ Configure ytdl_sub systemd timer/unit
+ Have a more elegant way of adding variables  (git hook? right now you use `ansible-vault view vars/vault.yml | cat | sed 's/:.*//' > vars/vault.yml.sample`)
