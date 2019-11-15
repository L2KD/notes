# LUKS

## Format

```
cryptsetup luksFormat /dev/mapper/vg0/home

```

Cung cấp password.

## Open

```
cryptsetup open /dev/mapper/vg0/home crypthome
```

Gõ vào password.

Khi này partition đó sẽ được mở và available tại /dev/mapper/crypthome

Sau đó có thể tiến hành mount và sử dụng như bình thường.

## Dùng khi boot