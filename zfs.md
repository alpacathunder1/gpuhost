## Pool/datasets were created with:
 
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
zfs create tank/home-alex-.config-Bitwarden\ CLI
zfs set com.sun:auto-snapshot=false tank/home-alex-.config-Bitwarden\ CLI
zfs set mountpoint=/home/alex/.config/Bitwarden\ CLI tank/home-alex-.config-Bitwarden\ CLI
```
