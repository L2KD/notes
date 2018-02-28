Of course without data loss

Đầu tiên, tìm các disks có trong system:

    fdisk -l

Result in

    Disk /dev/sda: 2000.0 GB, 1999999336448 bytes, 3906248704 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x000d3c5b

    Device     Boot    Start       End      Blocks      Id  System
    /dev/sda1   *        2048     1026047      512000   83  Linux
    /dev/sda2         1026048  3906248703  1952611328   8e  Linux LVM

Chỗ `Disk label type: dos` cho biết HDD đang là MBR.

Sử dụng `gdisk` để convert

    gdisk /dev/sda

THằng gdisk sẽ tìm và phát hiện thằng mbr

Sau đó chọn `w`, confirm lại `Y`

Sẽ báo OK.

Khi này vùng MBR sẽ bị ghi chồng lên bằng thằng GPT --> mất grub của linux.

Cần khôi phục lại `grub`.
