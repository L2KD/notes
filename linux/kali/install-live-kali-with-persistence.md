Trước khi tiến hành, chắc chắn rằng usb tối thiểu là 8GB. Do phần kali chiếm tầm 3GB mất rồi.

Cần cài các package sẽ phải dùng trong các command bên dưới. 

Cần quyền root.

1. Sau khi burn image bằng etcher xong, chạy lệnh `fdisk -l` sẽ hiện `sdc1` và `sdc2`.
2. Tiếp theo chạy

    parted /dev/sdc mkpart primary
    start: Gõ đại 1 số tầm 3200	
    end: 7gb

    Lúc này nó sẽ hỏi lại start và end nếu như bị overlay các partition lên nhau. Khi hoàn thành chạy lại `fdisk -l` sẽ thấy sdc3.

3. Mã hóa LUKS

    Chạy lệnh sau:

        cryptsetup --verbose --verify-passphrase luksFormat /dev/sdc3
        cryptsetup luksOpen /dev/sdc3 my_usb


     Khi chạy lệnh luksOpen trên, nó sẽ tạo ra trong `/dev/mapper/` một device tên là my_usb.

4. Tạo ext4 fs, label nó là persistence

        mkfs.ext3 -L persistence /dev/mapper/my_usb
        e2label /dev/mapper/my_usb persistence

5. Mount nó vào và tạo một file persistence.conf có nội dung là `/ union` bên trong.

        mkdir -p /mnt/my_usb
	mount /dev/mapper/my_usb /mnt/my_usb
	echo "/ union" > /mnt/my_usb/persistence.conf
        umount /dev/mapper/my_usb

6. Close cryptsetup

        cryptsetup luksClose /dev/mapper/my_usb
