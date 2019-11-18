# LUKS

## Format

```
cryptsetup luksFormat /dev/mapper/vg0/home

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