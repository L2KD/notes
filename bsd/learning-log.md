## mount:

    mount -t ext2fs -o rw /dev/ada0s2 /mnt
    mount -t ufs -o rw /dev/ada0s3b /mnt

Có 3 phân vùng

    freebsd-ufs /
    freebsd-swap
    freebsd-zfs /usr

Home sẽ được link vào `/usr/home`

Phần bootloader nằm trong `/boot`

ZFS

`zpool import`: lệnh này sẽ chekc hết storage media, và tìm 1 ZFS signature valid để lấy pool name và list ra. Lệnh này không làm gì ngoài việc detect và list pool.
Sau khi có tên pool rồi, mount bằng cách

    zpool import -fR /mnt <tên pool>

Trong đó f là force (do zfs khá thông minh, nó nhớ history mount, cần force khi thay đổi môi trường).

Sau khi mount xong, lấy thêm thông tin như mountpoint, canmount...

    zfs get mountpoint,canmount <tên pool>

    NAME   PROPERTY    VALUE       SOURCE
    zroot  mountpoint  /           local
    zroot  canmount    on          default

Thông thường, 1 hệ thống sẽ mount fs thông qua file `/etc/fstab`, hoặc manually bằng command. Tuy vậy, đối với zfs thì lại không dùng `/etc/fstab`, mà chính fs nó theo dõi việc mount này tự thân nó. Vậy nên `-R` option trên để ép hệ thống nhận ra fs được access qua path khác với cái path được thiết lập trong zfs.

    zfs list

Lệnh trên list ra các pool mà hệ thống đã import.

    zfs mount <tên pool>

Lệnh tren sẽ mount các fs vào đung mountpoint.

    zfs set mountpoint=/myspecialfolder mypool

Lệnh trên đơn giản quá, nhìn hiểu rồi.

Ngoài ra còn zpool cache. File này nằm ở `/boot/zfs/zpool.cache`. Một khi import pool, nó sẽ mount vào mount point và ghi lại file cache. Khi hệ thống ở mode RO, nó không có quyền ghi vào file này, dẫn đến lần boot kế tiếp không mount được.

https://forums.FreeBSD.org/threads/how-to-mount-a-zfs-partition.66603/post-393889

## Single user read only escape:

Boot vào single user mode, sau đó mount lại `/` bằng `mount -urw /`, khi này sẽ có quyền write.

## Boot loader

Bản thân freebsd có boot loader riêng của nó (`/boot`). Để tránh mất các boot loader cho các os khác, tôi dùng grub2 tạm.

Trong grub.cfg, thêm entry

    menuentry "BSD" {
        set root="hd0,msdos3"
        chainloader +1
    }

Việc này nó sẽ chain load cái boot loader grub, thẩy qua cho thằng boot loader mà nó tìm thấy trong cái disk msdos3 kìa.

Ngoài ra còn có thể boot trực tiếp từ grub, mà chưa thành công.

## su

Thêm người dùng vào group wheel

    pw usermod sysadmin -G wheel

Leo thang

    su -
    Nhập pwd của root

## Sau khi cài xong

- pkg update, pkg upgrade
- Add người dùng vào group wheel.
- Cài sudo, chỉnh visudo thêm wheel vào sudo.
- NTP. Enable service & sync on startup.
- Firewall.
- Instal shell.
- Install X system (follow official doc).

## Port collections

Sau khi cài OS, nếu có tùy chọn cài port col thì cài thêm. Còn không thì cài vào sau bằng cách:

To download a compressed snapshot of the Ports Collection into /var/db/portsnap:

    # portsnap fetch

When running Portsnap for the first time, extract the snapshot into /usr/ports:

    # portsnap extract

After the first use of Portsnap has been completed as shown above, /usr/ports can be updated as needed by running:

    # portsnap fetch
    # portsnap update

When using fetch, the extract or the update operation may be run consecutively, like so:

    # portsnap fetch update`

Để cài port

    $ cd /usr/port/<tên port>
    # make install

## Remove user

    # rmuser

## Install kernel source

Một vài port cần phải có src của kernel để có thể cài đặt được. Kernel source có thể được cài lúc cài OS (bsdinstall), hoặc nếu không thì cài sau bằng:

    fetch ftp://ftp.freebsd.org/pub/FreeBSD/releases/amd64/10.2-RELEASE/src.txz

    tar -C / -xzvf src.txz

    10.2-RELEASE MUST be replaced with correct version of your OS.

    One can find version using command: freebsd-version -k

    The minor versions should be ignored to fetch from the above URL. For ex: if it is 10.2-RELEASE-p1, just use: 10.2-RELEASE

## Install `xorg` & `xfce`

- Install `xorg` from pkg.
- Install `xfce`.
- Create `~/.xinitrc` & `~/.xsession`
- `/etc/rc.conf`

        clear_tmp_enable="YES"
        sendmail_enable="NONE"
        hostname="LLED-BSD"
        wlans_iwn0="wlan0"
        ifconfig_wlan0="WPA DHCP"
        sshd_enable="YES"
        ntpd_enable="YES"
        ntpd_sync_on_start="YES"
        zfs_enable="YES" # Set dumpdev to "AUTO" to enable crash dumps, "NO" to disable
        dumpdev="AUTO"
        slim_enable="YES"
        dbus_enable="YES"
        hald_enable="YES"
        xdm_enable="YES"
        kld_list="/boot/modules/i915kms.ko"

- Install intel driver (from port `/graphics/drm-kmod`)
- Add this line to /boot/loader.conf to enable vt(4):

        kern.vty=vt

## Yubikey

To enable gpg card functionalities.

- Install `yubikey-manager-qt`.
- Enable `pcscd` service.

GPG import/export

- gpg --export -a --output public_gpg.txt <key>
- On the freebsd machine:
- gpg --import public_gpg.txt

## VPN Cisco

Cài port tên vpnc 

    cd /usr/port/security/vpnc
    sudo make install
    
Tạo file tên `/usr/local/etc/vpnc/mekong.conf` có nội dung 

    IPSec gateway x.x.x.x
    IPSec ID vpn
    IPSec secret 123456
    Xauth username u
    Xauth password p
    Domain ""

Chạy `sudo vpnc mekong`. 

Disconnect chạy vpnc-disconnect.
