## Centos 7 - Some useful commands

fdisk

    fdisk -l

Result in

    Disk /dev/sda: 2000.0 GB, 1999999336448 bytes, 3906248704 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x000d3c5b

       Device Boot      Start         End      Blocks   Id  System
    /dev/sda1   *        2048     1026047      512000   83  Linux
    /dev/sda2         1026048  3906248703  1952611328   8e  Linux LVM

    Disk /dev/mapper/centos-root: 53.7 GB, 53687091200 bytes, 104857600 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes


    Disk /dev/mapper/centos-swap: 8455 MB, 8455716864 bytes, 16515072 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes


    Disk /dev/mapper/centos-home: 1400.4 GB, 1400381636608 bytes, 2735120384 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes


    Disk /dev/mapper/centos-backup: 536.9 GB, 536942215168 bytes, 1048715264 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes

---

lsblk

    lsblk

Result in

    NAME              MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda                 8:0    0   1.8T  0 disk
    ├─sda1              8:1    0   500M  0 part /boot
    └─sda2              8:2    0   1.8T  0 part
    ├─centos-root   253:0    0    50G  0 lvm  /
    ├─centos-swap   253:1    0   7.9G  0 lvm  [SWAP]
    ├─centos-home   253:2    0   1.3T  0 lvm  /home
    └─centos-backup 253:3    0 500.1G  0 lvm  /backup
    sr0                11:0    1  1024M  0 rom

---

Volume Group Summary?

    vgs

Result in

    VG     #PV #LV #SN Attr   VSize VFree
    centos   1   4   0 wz--n- 1.82t 4.00m

---

Logical Volumes Summary?

    lvs

Result in

    LV     VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
    backup centos -wi-ao---- 500.07g
    home   centos -wi-ao----   1.27t
    root   centos -wi-ao----  50.00g
    swap   centos -wi-ao----   7.88g

---

Volume Group Display

    vgdisplay
Result in

    --- Volume group ---
    VG Name               centos
    System ID
    Format                lvm2
    Metadata Areas        1
    Metadata Sequence No  5
    VG Access             read/write
    VG Status             resizable
    MAX LV                0
    Cur LV                4
    Open LV               4
    Max PV                0
    Cur PV                1
    Act PV                1
    VG Size               1.82 TiB
    PE Size               4.00 MiB
    Total PE              476711
    Alloc PE / Size       476710 / 1.82 TiB
    Free  PE / Size       1 / 4.00 MiB
    VG UUID               0Iq3bb-4lNA-ecvd-OKNE-HDk0-puGk-nC8niE

---

Logical Volume Display

    lvdisplay

Result in

    --- Logical volume ---
    LV Path                /dev/centos/root
    LV Name                root
    VG Name                centos
    LV UUID                part82-N6e3-P1S3-fT4k-qSis-0qxO-qix56m
    LV Write Access        read/write
    LV Creation host, time localhost, 2015-07-07 15:14:26 +0700
    LV Status              available
    # open                 1
    LV Size                50.00 GiB
    Current LE             12800
    Segments               1
    Allocation             inherit
    Read ahead sectors     auto
    - currently set to     256
    Block device           253:0

    --- Logical volume ---
    LV Path                /dev/centos/home
    LV Name                home
    VG Name                centos
    LV UUID                1a0WxJ-9EGd-HjSE-FOZ1-DGIE-8j7j-2J58TX
    LV Write Access        read/write
    LV Creation host, time localhost, 2015-07-07 15:14:29 +0700
    LV Status              available
    # open                 1
    LV Size                1.27 TiB
    Current LE             333877
    Segments               1
    Allocation             inherit
    Read ahead sectors     auto
    - currently set to     256
    Block device           253:2

    --- Logical volume ---
    LV Path                /dev/centos/swap
    LV Name                swap
    VG Name                centos
    LV UUID                6BQ2eL-bLSW-j5DV-yJwo-LPcx-Wel0-rvgfNg
    LV Write Access        read/write
    LV Creation host, time localhost, 2015-07-07 15:15:09 +0700
    LV Status              available
    # open                 2
    LV Size                7.88 GiB
    Current LE             2016
    Segments               1
    Allocation             inherit
    Read ahead sectors     auto
    - currently set to     256
    Block device           253:1

    --- Logical volume ---
    LV Path                /dev/centos/backup
    LV Name                backup
    VG Name                centos
    LV UUID                I3nZxf-Tidq-2wAA-1hxw-x3YK-l7ES-zYsEgP
    LV Write Access        read/write
    LV Creation host, time localhost, 2015-07-07 15:15:10 +0700
    LV Status              available
    # open                 1
    LV Size                500.07 GiB
    Current LE             128017
    Segments               1
    Allocation             inherit
    Read ahead sectors     auto
    - currently set to     256
    Block device           253:3

---

Physical Volume Display

    pvdisplay

Result in

    --- Physical volume ---
    PV Name               /dev/sda2
    VG Name               centos
    PV Size               1.82 TiB / not usable 3.00 MiB
    Allocatable           yes
    PE Size               4.00 MiB
    Total PE              476711
    Free PE               1
    Allocated PE          476710
    PV UUID               iPix6q-QzwL-sW9D-alwV-rYb7-yw3L-TJ8gCb

---

View which RAID controller the system uses:

    lspci -vv | grep -i raid

Result in

    00:1f.2 RAID bus controller: Intel Corporation 631xESB/632xESB SATA RAID Controller (rev 09)

---

View list drives

    lsscsi

Result in

    [2:0:0:0]    cd/dvd  TSSTcorp DVD-ROM TS-H353B BC05  /dev/sr0
    [8:0:0:0]    disk    ATA      WDC WD20EURS-73T 0A80  -
    [8:0:1:0]    disk    ATA      WDC WD20EURS-63S AB51  -
    [8:1:1:0]    disk    LSILOGIC Logical Volume   3000  /dev/sda


---

## LSI RAID Card (Hardware)

LSI Logic / Symbios Logic SAS1064ET PCI-Express Fusion-MPT SAS Card

lspci -knn

    40:00.0 SCSI storage controller [0100]: LSI Logic / Symbios Logic SAS1064ET PCI-Express Fusion-MPT SAS [1000:0056] (rev 08)

Or LSI 3041E. According to the [HP's official support](http://h10032.www1.hp.com/ctg/Manual/c01232490), the XW6600 uses LSI 3041E card and it's a **Hardware-assisted** RAID (a.k.a. BIOS RAID). So no linux utilities to communicate with the RAID card (TBD), and instead, BIOS is where all RAID-related works be done.

### Some notations invented by LSI

- IM - or Integrated Mirroring, is also known as RAID-1.
- IME - or Integrated Mirroring Enhanced,
is RAID-10.
- IS - or Integrated Striping, is RAID-0.

So they use their own notations instead of RAID-1-0-10...

## Quick guide

### Tạo MBR và `/boot`

Thường thì chúng ta phải ghi 1 cái MBR hoặc GPT cho ổ đĩa mới.

Và để không bị rắc rối với Grub (grub2) thì nên cho /boot nó nằm ở ngoài LVM.

Như vậy: Dùng fdisk để làm 2 việc trên.

```
fdisk /dev/sdX

o: Tạo MBR hoặc
g: tạo GPT partition table
n: tạo mới 1 partition
w: ghi thay đổi
d: xóa partition
```

`/boot` chỉ cần khoảng 500MB-1GB, không cần nhiều hơn, chừa những phần còn trống quý giá của ổ cứng cho những những điều khác cao cả không kém.

Lúc này sẽ có 1 ổ nằm ở #1, type 83 (Linux)

Sau đó tạo thêm ổ #2, cũng Linux.

### Tạo PV, VG, LV

Tiếp tục, dùng LVM để tạo lần lượt: Physical Volume, Volume Group, Logical Volume.

```
pvcreate /dev/sdX2 

Do sdX1 là /boot rồi
```

Kiểm tra lại bằng `pvscan` hoặc `pvdisplay`.

Tiếp tục, tạo VG

```
vgcreate vg0 /dev/sdX2

Trong đó vg0 là tùy chọn

```

Tiếp, tạo LV

```
lvcreate -L 8G vg0 -n swap
lvcreate -L 80G vg0 -n root
lvcreate -l 100%FREE vg0 -n home
```

Như vậy ta sẽ có

```
/dev/mapper/vg0/swap có 8GB
/dev/mapper/vg0/root có 80GB
/dev/mapper/vg0/home có phần còn lại của ổ đĩa
```

### Bật lvm (active vg)

```
# modprobe dm_mod
# vgscan
# vgchange -ay
```

### Tạo fs