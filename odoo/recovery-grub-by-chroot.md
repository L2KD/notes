# Recovery GRUB by `chroot`

Đầu tiên là mount thằng root về `/mnt` (hoặc 1 chỗ khác)

    mount /dev/mapper/centos-home /mnt

Do cái partition này đang xài LVM nên nó nằm trong `/dev/mapper`, một số partition khác (không phải LVM) có thể sẽ nằm ở `/dev/sda1`...

Sau đó, mount 3 thằng `/dev`, `/proc`, `/sys` từ hệ thống hiện tại vào `/mnt`. 3 thằng này có trách nhiệm cung cấp 1 số thứ cần thiết cho lệnh `grub-install` về sau.

    mount -bind /dev /mnt/dev
    mount -bind /proc /mnt/proc
    mount -bind /sys /mnt/sys

Sau đó mount thằng `/boot` vào, vì `grub-install` nó mặc định sẽ bụp vào thằng `/boot`

    mount /dev/sda3 /mnt/boot

Thằng sda3 này là `/boot` với flags `bios_partition` khi disk dạng GPT, hoặc flag boot khi disk dạng MBR.

Khỏi mount thằng `swap`, `home`, hoặc `/var/` vì k liên quan.

Sau đó `chroot`

    chroot /mnt

Lúc này hệ thống sẽ hiểu cái root là `/mnt` thay vì `/`, mấy ông đã mount sẽ được ghi dữ liệu vào ổ cứng đang được mount thay vì ổ cứng của hệ thống hiện hành.

Tiến hành `grub-install`

    grub-install /dev/sda

hoặc trên centos 7

    grub2-install /dev/sda

Nó báo successful là ok.

Reboot hệ thống.
