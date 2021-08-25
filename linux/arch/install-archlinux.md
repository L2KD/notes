This is based on [Installation guide](https://wiki.archlinux.org/index.php?title=Installation_guide) from the official wiki.

0. Make an USB live boot (archiso), downloaded from archlinux home page.
0. Verify boot mode (UEFI or BIOS).
0. Boot on the USB.
0. Connect to internet (wired or wireless). Wired connection with DHCP is automatically set.

    Wireless:

        wpa_passphrase SSID passphrase > /etc/wpa_supplicant/SSID.conf
        wpa_supplicant -B -iwlp1s0 -Dwext -c/etc/wpa_supplicant/SSID.conf

    -B : background
    -i : interface, check yours in `iw dev` or `ip link`
    -D : driver (again, check with your wifi card, more details: lspci)
    -c : config file

    after this command, you re connected to SSID wifi. Get ip by dhclient

    dhclient wlp1s0 (or other interface)

0. Verify your internet connection:

        ping 1.1.1.1

0. (Optional) Update system clock

        timedatectl set-ntp true

0. Partition disk

    Should use LVM [TODO](Need another article inserted here).

    Format partition

        mkfs.btrfs /dev/...

0. Mount filesystem

        mount /dev/... /mnt
        mkdir /mnt/boot
        mount /dev/... /mnt/boot
        
    If you have /usr or /home on other partition, mount them to /mnt/...


0. (Optional) Select mirror

        vi /etc/pacman.d/mirrorlist

    Select your country mirror or the nearest

0. Installation

        pacstrap /mnt base

0. Then generate file system table

        genfstab -U /mnt >> /mnt/etc/fstab

    May have to edit the file for errors-checking.

0. Chroot to new system

        arch-chroot /mnt

0. Timezone

        ln -sf /usr/share/zoneinfo/Region/City /etc/localtime

        hwclock --systohc

0. Locale

    Uncomment en_US.UTF-8 in `/etc/locale.gen`
    
    Then generate locale by

        locale-gen

0. set LANG variable in locale.conf

        vi /etc/locale.conf

        LANG=en_US.UTF-8

0. Hostname

        echo 'Your hostname here' > /etc/hostname

0. `hosts` file

        vi /etc/hosts

        127.0.0.1 localhost
        ::1       localhost

0. If you're using LVM, you may need to provide it in `/etc/mkinitcpio.conf`

        HOOKS=(base udev autodetect modconf block lvm2 filesystems keyboard fsck)

0. Then re build the init ram fs image

        mkinitcpio -p linux

0. Root password

        passwd

0. (Optional) Add new user

        useradd username
        usermod -a -G wheel username
        passwd username

0. (Optional) visudo

        remove # of wheel

    From now on, the username should be able to use sudo command


0. Bootloader: grub or UEFI

0. Exit, umount -R /mnt and reboot


---

0. Next, install GUI (or not if you like adventures).

        #driver
        pacman -S xf86-video-intel 

        # X server
        pacman -S xorg-server xorg-xinit xterm 

        # Display manager
        pacman -S lightdm-gtk-greeter 
        systemctl enable lightdm

        # Window manager
        pacman -S i3-gaps

    After a reboot, your login screen should appear.

0. You might have to Network Config

        ## Edit file /etc/systemd/network/20-wired.network

        [Match]
        Name=enp2s0 (replace by your interface, check by: ip a)

        [Network]
        DHCP=yes
    
    Enable service
    
        # systemctl enable systemd-networkd
        # systemctl start systemd-networkd
        
After installation, some suggestions to continue: https://wiki.archlinux.org/index.php?title=General_recommendations
    
## Other (maybe useful) commands

- Check kernel version `pacman -Q linux`
- Check current uname version `uname -r`
