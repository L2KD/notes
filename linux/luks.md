# LUKS

## Format

```
cryptsetup luksFormat /dev/mapper/vg0/home

```
hoặc

```
cryptsetup luksFormat /dev/sdc1
```

Cung cấp password.

Khi này phân vùng đó sẽ được mã hóa với passphrase cung cấp, với các tùy chọn mã hóa mặc định (LUKS 2 nếu package >= 2.x, chỗ này xem official doc của arch linux để biết thêm chi tiết).

## Open

```
cryptsetup open /dev/mapper/vg0/home crypthome
```

Gõ vào password.

Khi này partition đó sẽ được mở và available tại /dev/mapper/crypthome

Sau đó có thể tiến hành mount và sử dụng như bình thường. 

VD

```
mkdir -p /home
mount /dev/mapper/crypthome /home
```

Hoặc tạo fs mới để sử dụng, vd:

```
mkfs.ext4 /dev/mapper/microsd
```

Sau đó mount và sử dụng.

## Close

umount trước.

```
cryptsetup close microsd
```

## Dùng khi boot

Do 1 số trường hợp cần phải mount partition lúc boot, 1 cách tự động chứ không phải mount bằng tay sau khi log in.

Cách cấu hình:

```
# /etc/fstab

# /dev/mapper/crypthome
UUID=uuid	/home     	btrfs     	rw,relatime,ssd,space_cache,subvolid=5,subvol=/	0 0

# /etc/crypttab

home /dev/vg0/home none
```

Khi boot, sẽ phải nhập passphrase để unlock partition.

Có thể phải thêm module sau vào `/etc/mkinitcpio.conf`

```
MODULES=(dm_mod)
```

More at: https://www.golinuxcloud.com/mount-luks-encrypted-disk-partition-linux/
