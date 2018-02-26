###### Môi trường: Centos 7

fdisk

    fdisk -l

KQ

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

KQ

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

KQ

    VG     #PV #LV #SN Attr   VSize VFree
    centos   1   4   0 wz--n- 1.82t 4.00m

---

Logical Volumes Summary?

    lvs

KQ

    LV     VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
    backup centos -wi-ao---- 500.07g
    home   centos -wi-ao----   1.27t
    root   centos -wi-ao----  50.00g
    swap   centos -wi-ao----   7.88g

---

Volume Group Display

    vgdisplay
KQ

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

KQ

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

KQ

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

KQ

    00:1f.2 RAID bus controller: Intel Corporation 631xESB/632xESB SATA RAID Controller (rev 09)

---

View list drives

    lsscsi

KQ

    [2:0:0:0]    cd/dvd  TSSTcorp DVD-ROM TS-H353B BC05  /dev/sr0
    [8:0:0:0]    disk    ATA      WDC WD20EURS-73T 0A80  -
    [8:0:1:0]    disk    ATA      WDC WD20EURS-63S AB51  -
    [8:1:1:0]    disk    LSILOGIC Logical Volume   3000  /dev/sda
