# EFI

Hồi xưa thì các PC xài BIOS để boot up. Các dòng PC sau này chuyển qua xài UEFI để làm chuyện đó. Boot time không cần phải tìm phân vùng được đánh dấu boot nữa, mà BIOS sẽ lục trong danh sách các disk, mỗi disk tìm phân vùng có `type` là EFI. Trong phân vùng đó thường sẽ có chứa 1 cái app (file có đuôi `.efi`), sau đó tiếp tục quy trình boot.

Giả sử hiện trạng máy đang có các disk sau:

```
Device     Boot   Start       End   Sectors   Size Id Type
/dev/sda1          2048   1026047   1024000   1G 83 Linux
/dev/sda2       2099200 468862127 466762928 222.6G 83 Linux
```

Theo fstab thì `/dev/sda1` sẽ mount vào `/boot`. Trong đó chứa grub (version 2).

Bây giờ để chuyển sang PC mới có UEFI. Các bước đã làm:

Backup lại `/boot`.

Dùng fdisk tạo 1 phân vùng mới kiểu `ef`, phân vùng này cắt ra từ `sda1` thành `sda1` và `sda3`. Mỗi phân vùng nhỏ là 500M. `sda1` vẫn như cũ là Kiểu `83`, copy ngược lại data đã backup về chỗ cũ (phòng trường hợp lại quay về máy cũ).

Sau đó mkfs kiểu vfat -F32.

Khi này các disk trong máy sẽ như sau:

```
Device     Boot   Start       End   Sectors   Size Id Type
/dev/sda1          2048   1026047   1024000   500M 83 Linux
/dev/sda2       2099200 468862127 466762928 222.6G 83 Linux
/dev/sda3       1026048   2099199   1073152   524M ef EFI (FAT-12/16/32)
```

Kế tiếp, boot lên live usb của arch linux.

Mount `/dev/mapper/vg0-root` vào `/mnt`.

Mount `/dev/sda3` vào `/mnt/boot`.

Gen lại fstab (`genfstab -U -p /mnt >> /mnt/etc/fstab`), nhớ chỉnh sửa lại cho đúng.

Sau đó chroot vào `/mnt`. (`arch-chroot /mnt`).

Chỉnh lại file `mkinitcpio.conf` để adapt early userspace đúng với hardware hiện tại (các card màn hình là chủ yếu...).

Chạy lệnh `mkinitcpio -p linux` để tạo ra EU mới.

Kế tiếp gen grub với điều kiện sau khi cài package (`efibootmgr`).

```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

Lệnh trên sẽ gen lại grub vào vị trí `/boot`, thư mục con bên trong là `GRUB`.

Khi này bên trong `/boot` sẽ có dạng như sau:

```
‣ sudo tree /boot   
/boot
├── EFI
├── grub
│   ├── fonts
│   ├── grub.cfg
│   ├── grub.cfg.backup.moly
│   ├── grubenv
│   ├── i386-pc
│   ├── themes
│   │   └── starfield
│   └── x86_64-efi
├── initramfs-linux-fallback.img
├── initramfs-linux.img
└── vmlinuz-linux

7 directories, 598 files
```

Lưu ý:

Đối với 1 số máy, phải copy file `.efi` ra 1 vị trí default mới có thể boot được. 

VD

```
cp /boot/EFI/grub/grubx64.efi /boot/EFI/Boot/Bootx64.efi
```

Src: https://wiki.archlinux.org/index.php/GRUB/EFI_examples
