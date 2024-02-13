this is my gaming/gpu/vfio/windows11/parsec hypervisor box

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
## TODO

+ Test zfs mail
+ Setup ytdl_sub + system timer/unit
+ Read test on a clean arch installation
+ Set up NetworkManager bridge & static ip through ansible
+ Have a more elegant way of adding variables  (git hook? right now you use `ansible-vault view vars/vault.yml | cat | sed 's/:.*//' > vars/vault.yml.sample`)
