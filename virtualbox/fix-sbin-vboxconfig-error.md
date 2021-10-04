After installing/updating vbox new version, you may get the following issue

> the support driver is not installed...
> 
> Run sudo /sbin/vboxconfig

## But why?

This error occurs because virtualbox tries to install required drivers and modules from current latest kernel (which is newer than current installed kernel on the host). So, depending on which style you installed virtualbox (dkms or kernel module), update it to the latest version.

      $ pacman -Qi linux      
      Name            : linux
      Version         : 5.13.4.arch1-1
      Description     : The Linux kernel and modules
      Architecture    : x86_64
      URL             : https://github.com/archlinux/linux/commits/v5.14.8-arch1
      Licenses        : GPL2
      Groups          : None
      Provides        : VIRTUALBOX-GUEST-MODULES  WIREGUARD-MODULE
      Depends On      : coreutils  kmod  initramfs
      Optional Deps   : crda: to set the correct wireless channels of your country
                        linux-firmware: firmware images needed for some devices [installed]
      Required By     : virtualbox-host-modules-arch
      Optional For    : base
      Conflicts With  : None
      Replaces        : virtualbox-guest-modules-arch  wireguard-arch
      Installed Size  : 131.22 MiB
      Packager        : Jan Alexander Steffens (heftig) <heftig@archlinux.org>
      Build Date      : Mon 27 Sep 2021 02:36:15 AM +07
      Install Date    : Mon 04 Oct 2021 06:01:20 PM +07
      Install Reason  : Explicitly installed
      Install Script  : No
      Validated By    : Signature

That shows current installed kernel is `5.13.4-arch1-1` and virtualbox installs modules from `/lib/modules/5.14.8-arch1-1`

      $ ls -alh /lib/modules/
      
      $ ls -alh /lib/modules/5.14.8-arch1-1/extramodules/
      total 244K
      drwxr-xr-x 1 root root   90 Oct  4 17:58 .
      drwxr-xr-x 1 root root  494 Oct  4 18:01 ..
      -rw-r--r-- 1 root root 175K Sep 27 03:05 vboxdrv.ko.xz
      -rw-r--r-- 1 root root  23K Sep 27 03:05 vboxnetadp.ko.xz
      -rw-r--r-- 1 root root  42K Sep 27 03:05 vboxnetflt.ko.xz

## Solution

E.g if using `linux`, update it:

    # pacman -S linux linux-firmware

If using other kernel, update it accordingly (and dkms)...

Reboot, then reconfig the vbox driver

    # vboxreload
