this is my desktop/gaming/gpu/vfio/windows11/hypervisor box

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

Pool/datasets were created with:
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
zfs create tank/home-alex-.mozilla
zfs set com.sun:auto-snapshot=false tank/home-alex-.mozilla
zfs set mountpoint=/home/alex/.mozilla tank/home-alex-.mozilla
zfs create tank/home-alex-.local-share-vdirsyncer
zfs set com.sun:auto-snapshot=false tank/home-alex-.local-share-vdirsyncer
zfs set mountpoint=/home/alex/.local/share/vdirsyncer tank/home-alex-.local-share-vdirsyncer
zfs create tank/home-alex-.config-Signal
zfs set com.sun:auto-snapshot=false tank/home-alex-.config-Signal
zfs set mountpoint=/home/alex/.config/Signal tank/home-alex-.config-Signal
zfs create tank/home-alex-appimage
zfs set com.sun:auto-snapshot=false tank/home-alex-appimage
zfs set mountpoint=/home/alex/appimage tank/home-alex-appimage
zfs create tank/var-lib-postgres
zfs set com.sun:auto-snapshot=false tank/var-lib-postgres
zfs set mountpoint=/var/lib/postgres tank/var-lib-postgres
zfs create tank/var-lib-gogs
zfs set com.sun:auto-snapshot=false tank/var-lib-gogs
zfs set mountpoint=/var/lib/gogs tank/var-lib-gogs
```

## TODO

### Fonts
+ install good monospaced japanese font

### Gogs
+ Config directory permissions seem to change
+ Setup pam

### Backups
+ move borg to template

### Desktop
+ add sway config
  + swayidle + swaylock
+ japanese IME

### Shell
+ handy aliases

### Ansible/repo
+ **separate zfs role to setup mounts before everything else**
+ rename repo
+ ansible galaxy installation (`ansible-galaxy install -r requirements.yml`)

### Boot
+ enforce efi boot order
+ enforce fstab efi partition correct umask/permissions

### Video Downloader
+ Configure ytdl-sub systemd timer

### DOOM/emacs
+ **auto import khalel events**
+ Installing fonts

### SSH 
+ newline pubkey

### Editor
+ Auto git push/pull for Desktop notes
