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
      + br0/network bridging
      + As well as rfkill/disabling radios

## TODO

+ Read test on a clean arch installation
+ Properly set up NetworkManager bridge through ansible
+ Have a more elegant way of adding variables  (git hook? right now you use `ansible-vault view vars/vault.yml | cat | sed 's/:.*//' > vars/vault.yml.sample`)
