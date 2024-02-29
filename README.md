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

