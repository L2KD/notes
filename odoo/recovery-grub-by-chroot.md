# Recovery GRUB

mount /dev/mapper/centos-home /mnt

then

mount -bind /dev /mnt/dev
mount -bind /proc /mnt/proc
mount -bind /sys /mnt/sys 
