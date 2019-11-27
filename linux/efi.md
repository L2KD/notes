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

Khi này các disk trong máy sẽ như sau:

```
Device     Boot   Start       End   Sectors   Size Id Type
/dev/sda1          2048   1026047   1024000   500M 83 Linux
/dev/sda2       2099200 468862127 466762928 222.6G 83 Linux
/dev/sda3       1026048   2099199   1073152   524M ef EFI (FAT-12/16/32)
```

Kế tiếp, boot lên live usb của arch linux.