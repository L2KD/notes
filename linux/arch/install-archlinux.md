This is based on Installation guide from the official wiki.

Make an USB live boot (archiso), downloaded from archlinux home page.

Verify boot mode (UEFI or BIOS)

Boot on the USB

Connect to internet (wired or wireless)

Wireless:

    wpa_passphrase SSID passphrase > /etc/wpa_supplicant/SSID.conf
    wpa_supplicant -B -iwlp1s0 -Dwext -c/etc/wpa_supplicant/SSID.conf

-B : background
-i : interface, check yours in `iw dev` or `ip link`
-D : driver (again, check with your wifi card, more details: lspci)
-c : config file

after this command, you re connected to SSID wifi. Get ip by dhclient

    dhclient wlp1s0 (or other interface)

Verify your internet connection:

    ping 8.8.8.8

Update system clock

    timedatectl set-ntp true

Partition disk

Should use LVM

Format partition

    mkfs.ext4 /dev/...

Mount filesystem

    mount /dev/... /mnt
    mkdir /mnt/boot
    mount /dev/... /mnt/boot


Select mirror

    vi /etc/pacman.d/mirrorlist

Select your country mirror or the nearest

Installation

    pacstrap /mnt base

If you have /usr or /home on other partition, mount them to /mnt/...

Then generate file system table

    genfstab -U /mnt >> /mnt/etc/fstab

May have to edit the file for errors-checking.

Chroot to new system

    arch-chroot /mnt

Timezone

    ln -sf /usr/share/zoneinfo/Region/City /etc/localtime

    hwclock --systohc

Locale
Uncomment en_US.UTF-8 in `/etc/locale.gen`
Then generate locale by

    locale-gen

set LANG variable in locale.conf

    vi /etc/locale.conf

    LANG=en_US.UTF-8

Hostname

    echo 'Your hostname here' > /etc/hostname

hosts file

    vi /etc/hosts

    127.0.0.1 localhost
    ::1       localhost

If you're using LVM, you may need to provide it in `/etc/mkinitcpio.conf`

    HOOKS=(base udev autodetect modconf block lvm2 filesystems keyboard fsck)

Then re build the init ram fs image

    mkinitcpio -p linux

Root password

    passwd

Add new user

    useradd username
    usermod -a -G wheel username
    passwd username

visudo

    remove # of wheel

From now on, the username should be able to use sudo command


Bootloader: grub or UEFI

Exit, umount -R /mnt and reboot


---

Next, install plasma as DE.

    pacman -S xf86-video-vesa mesa xf86-video-intel #driver
    pacman -S xorg-server xorg-xinit xterm # X server
    pacman -S plasma # plasma KDE
    pacman -S lightdm-gtk-greeter # Display manager
    systemctl enable lightdm

After a reboot, your login screen should appear.

install net manager

    pacman-nm

    systemctl disable net-auto@interface
    systemctl enable NetworkManager

Install from AUR, please check the wiki

check kernel version `pacman -Q linux`
check current uname version `uname -r`
