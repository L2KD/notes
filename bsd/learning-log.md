mount:

    mount -t ext2fs -o rw /dev/ada0s2 /mnt
    mount -t ufs -o rw /dev/ada0s3b /mnt

Có 3 phân vùng

    freebsd-ufs /
    freebsd-swap
    freebsd-zfs /usr

Home sẽ được link vào `/usr/home`

Phần bootloader nằm trong `/boot`
