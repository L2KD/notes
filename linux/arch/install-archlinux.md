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

    Typically, you need:
    
    - Use fdisk to split your disk to 2 parts. `sda1` for boot, `sda2` for the arch system, and maybe `sda3` for other OSs,
    - `sda1` FAT32 for boot partition.
    - `sda2` used along with LVM. 
    - A physical volume (`pv`) in `sda2`.
    - A volume group called `arch`.
    - At least 3 logical volumes for `/`, `home`, `swap`. `/usr`, `/var` are optional.

    Should use LVM [TODO](Need another article inserted here).
    
    0. Create `pv`.

            pvcreate /dev/sdXx
            pvcreate /dev/sda2
            
            ## Check it by
            
            pvs
          
    0. Create `vg`.

            vgcreate <volume-group-name> /dev/sdXx
            vgcreate arch /dev/sda2
            
            ## Check by
            
            vgs
            
    0. Create `lv`s.

            ## Create / partition
            lvcreate -L <size> <vg-name> -n <lv-name>
            lvcreate -L 30G arch -n root
            ## After this command, a device will be mapped in /dev/mapper/arch-root or /dev/arch/root
            
            ## Create swap partition
            lvcreate -L 2G arch -n swap
            
            ## Create home partition which take the rest of free space of your vg
            lvcreate -l +100%FREE arch -n home
            
            ## You can create more partitions for /usr, /var if you like to.
            
    0. (Optional but recommended) LUKS.

        Since encrypting the whole disk brings me troubles so I just encrypt home partition.
        
            cryptsetup formatLuks /dev/mapper/arch-home
            ## Enter your passphrase
            
            cryptsetup open /dev/mapper/arch-home crypthome
            ## After this command, a device will appear in /dev/mapper/crypthome which can be mounted to /mnt/home later.
            
            ## To close the encryption:
            cryptsetup close /dev/mapper/crypthome
           
    0. Format partitions

        Root and home partitions are recommended to use btrfs. Swap partition is good to go with ext4.

            mkfs.btrfs /dev/mapper/arch-root
            mkfs.btrfs /dev/mapper/arch-home
            mkfs.ext4 /dev/mapper/arch-swap
            
0. Mount filesystem

        mount /dev/mapper/arch-root /mnt
        mkdir -p /mnt/boot
        mkdir -p /mnt/home
        mount /dev/sda1 /mnt/boot
        mount /dev/mapper/arch-home /mnt/home
        
    If you have /usr or /var on other partition, mount them to /mnt/...

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

0. Config `/etc/mkinitcpio.conf`

     If you're using LVM, you may need to provide it in

        MODULES=(dm_mod)
        HOOKS=(base udev autodetect modconf block lvm2 filesystems keyboard fsck)
        
    Then, install missing things: `lvm`, `cryptsetup`.

0. If you're using LUKS, then you need to provide it in `/etc/crypttab`.

    Append the file with:
    
        crypthome /dev/mapper/arch-home none
        
    With this config, a prompt will appear while booting the system to enter your passphrase to openLuks your home partition.

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


0. Bootloader: grub or UEFI. Install os-prober, grub, then generate to `/boot/grub/grub.cfg`.

    This grub is working well:
    
        menyentry...
        ...
        insmod part_msdos
        insmod btrfs
        insmod ext2
        ...

0. Exit, umount -R /mnt and reboot


---

0. Next, install GUI (or not if you like adventures).

        #driver
        pacman -S xf86-video-intel 

        # X server
        pacman -S xorg-server xorg-xinit 

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

My personal tweaks: https://github.com/voldedore/notes/blob/master/linux/arch/tweak.md
    
## Other (maybe useful) commands

- Check kernel version `pacman -Q linux`
- Check current uname version `uname -r`
