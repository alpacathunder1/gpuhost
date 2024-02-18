```bash
wipefs -a /dev/nvme0n1
wipefs -a /dev/nvme1n1
parted /dev/nvme0n1 mklabel gpt
parted /dev/nvme1n1 mklabel gpt
# Say yes to prompt
mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/nvme0n1 /dev/nvme1n1 --metadata=0.90
parted /dev/md0 mklabel gpt
# Create 3 Partitions
# 1GB EF00 EFI
# -   8300
cgdisk /dev/md0
mkfs.fat -F 32 /dev/md0p1
pvcreate /dev/md0p2
vgcreate vg0 /dev/md0p2
lvcreate vg0 -n swap -L 4G
lvcreate -l +100%FREE vg0 --name root
mkfs.ext4 -L arch_os /dev/mapper/vg0-root
mkswap /dev/mapper/vg0-swap
mount /dev/mapper/vg0-root /mnt
swapon /dev/mapper/vg0-swap
mkdir /mnt/boo
mount /dev/md0p1 /mnt/boot
pacstrap /mnt base base-devel bash neovim git efibootmgr intel-ucode mdadm networkmanager openssh linux-lts linux-firmware python3
genfstab -pU /mnt >> /mnt/etc/fstab 
arch-chroot /mnt /bin/bash
```

## In Chroot

```bash
# I needed a slightly older kernel since the current zfs isn't up to date yet
# I got these filese from my old install
pacman -U linux-lts-6.6.16-1-x86_64.pkg.tar.zst  linux-lts-headers-6.6.16-1-x86_64.pkg.tar.zst
hwclock --systohc --utc
echo desktop > /etc/hostname
echo LANG=en_US.UTF-8 >> /etc/locale.conf
echo LANGUAGE=en_US >> /etc/locale.conf
echo LC_ALL=C >> /etc/locale.conf
useradd -m -G wheel -s /bin/bash alex
# Set a password
passwd alex
# Find & uncomment the line below:
# "Uncomment to allow members of group wheel to execute any command"
EDITOR=nvim visudo
# Add 'ext4' to "MODULES" and add 'mdadm_udev lvm2' "HOOKS" *before* 'filesystems'
nvim /etc/mkinitcpio.conf 
mkinitcpio -p linux-lts
# Make sure to populate some entries !!
bootctl install
systemctl enable sshd NetworkManager
```

## Get out of chroot, and 
```
umount -R /mnt
swapoff /dev/mapper/vg0-swap
poweroff
```
