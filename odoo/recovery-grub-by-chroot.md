# Recovery GRUB by `chroot`

Đầu tiên là mount thằng root về `/mnt` (hoặc 1 chỗ khác)

    mount /dev/mapper/centos-home /mnt

Do cái partition này đang xài LVM nên nó nằm trong `/dev/mapper`, một số partition khác (không phải LVM) có thể sẽ nằm ở `/dev/sda1`...

Sau đó, mount 3 thằng `/dev`, `/proc`, `/sys` từ hệ thống hiện tại vào `/mnt`. 3 thằng này có trách nhiệm cung cấp 1 số thứ cần thiết cho lệnh `grub-install` về sau.

    mount -bind /dev /mnt/dev
    mount -bind /proc /mnt/proc
    mount -bind /sys /mnt/sys
