```bash
wipefs -a /dev/nvme0n1
wipefs -a /dev/nvme1n1
parted /dev/nvme0n1 mklabel gpt
parted /dev/nvme1n1 mklabel gpt
mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/nvme0n1 /dev/nvme1n1 --metadata=0.90
parted /dev/md0 mklabel gpt
cgdisk /dev/md0
# Create 3 Partitions
# 100M EF00 ESP
# 8G   8200 swap
# -    8300 root
mkfs.fat -F 32 /dev/md0p1
mkswap /dev/md0p2
parted /dev/md0p3 mklabel gpt
mkfs.ext4 -L arch_os /dev/md0p3
mount /dev/md0p3 /mnt
swapon /dev/md0p2
mkdir /mnt/efi
mount /dev/md0p1 /mnt/efi
pacstrap /mnt base base-devel bash neovim git efibootmgr intel-ucode mdadm networkmanager openssh linux-lts linux-firmware python3
genfstab -pU /mnt > /mnt/etc/fstab 
arch-chroot /mnt /bin/bash
```

## In Chroot

```bash
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
# Add 'ext4' to "MODULES" and add 'mdadm_udev' to "HOOKS" *before* 'filesystems'
nvim /etc/mkinitcpio.conf 
mkinitcpio -p linux-lts
mkdir -p /efi/loader/entries
echo "title Arch Linux" >> /efi/loader/entries/arch.conf
echo "linux /vmlinuz-linux-lts" >> /efi/loader/entries/arch.conf
echo "initrd /intel-ucode.img" >> /efi/loader/entries/arch.conf
echo "initrd /initramfs-linux-lts.img" >> /efi/loader/entries/arch.conf
echo "options root=\"LABEL=arch_os\" rw" >> /efi/loader/entries/arch.conf
bootctl install
systemctl enable sshd NetworkManager
```

## Get out of chroot, and 
```
umount -R /mnt
swapoff /dev/md0p2
poweroff
```
