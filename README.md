this is my desktop/gaming/gpu/vfio/windows11/parsec hypervisor box

## Required

+ `ansible`
+ `bw-cli` (for ansible-vault)

### If remote

+ Python installation (if the target machine is remote)
+ Initial ssh key copying (so that ansible can run)

## Optional

+ `ansible-lint`
 
## Things not covered by this (yet)

+ Arch Installation
    + Disk formatting
    + Initial user creation
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
    ```
### ZFS

Pool was created with:

```bash
zpool create tank mirror /dev/sda /dev/sdb cache /dev/sdc
zpool set cachefile=/etc/zfs/zpool.cache
zfs set mountpoint=none tank 
zfs create tank/plex
zfs set mountpoint=/mnt/plex tank/plex
zfs create tank/borg
sudo zfs set com.sun:auto-snapshot=false tank/borg
zfs set mountpoint=/mnt/borg tank/borg
```

## vdirsyncer

Sync was started with

```bash
yes | vdirsyncer discover
vdirsyncer sync
```

## TODO

+ locale-gen
+ Add shell role with bash options (like `set -o vi`)
+ Run on a clean arch installation (+ set up RAID1)
  + Test MDADM emails once raid is setup
+ Setup vdirsyncer/bitwarden
  + Add `source ~/.bw-session  && vdirsyncer sync -v `#XYZ` cronjob
+ Set up smartd (ignoring nvme drives) 
+ Populate boot entries in install guide
+ Configure ytdl_sub systemd timer/unit
+ Have a more elegant way of adding variables  (git hook? right now you use `ansible-vault view vars/vault.yml | cat | sed 's/:.*//' > vars/vault.yml.sample`)
