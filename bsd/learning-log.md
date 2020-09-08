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
