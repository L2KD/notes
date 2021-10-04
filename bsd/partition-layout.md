Some suggestions:

OpenBSD manual:

    Since some packages need to be started from a wxallowed filesystem, it is recommended to have a separate /usr/local partition.
    Very small partitions can become troublesome when you need to upgrade.
    A /home partition can be nice. New version of the OS? Leave your /home partition untouched, wipe and reload everything else.
    You may also want to create an altroot partition for backing up your root filesystem.
    A system exposed to the internet should have a separate /var and maybe even a separate /var/log.
    Compiling some ports from source can take huge amounts of space on your /usr and /tmp partitions. 
    

https://man.openbsd.org/disklabel#AUTOMATIC_DISK_ALLOCATION:

Disks >= 10 Gigabytes

    /		 5% of disk.  150M – 1G
    swap		10% of disk.   80M – 2x max physical memory
    /tmp		 8% of disk.  120M – 4G
    /var		13% of disk.   80M – 2x size of crash dump
    /usr		10% of disk. 1500M – 6G
    /usr/X11R6	 3% of disk.  384M – 1G
    /usr/local	15% of disk.    1G – 20G
    /usr/src	 2% of disk. 1300M – 2G
    /usr/obj	 4% of disk.    5G – 6G
    /home		30% of disk.    1G – 300G

\*: `disklabel` was embedded and run in the installation of OpenBSD.
