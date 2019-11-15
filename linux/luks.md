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

Khi này partition đó