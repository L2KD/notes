# Usual tweak

## Mirror

Cài package `pacman-contrib`.

Lấy 5 mirror nhanh nhất, theo các quốc gia: Vietnam, Thái, Sing, HongKong, protocol https:

```
curl -s "https://www.archlinux.org/mirrorlist/?country=VN&country=TH&country=SG&country=HK&protocol=https&use_mirror_status=on" | sed -e 's/^#Server/Server/' -e '/^#/d' | rankmirrors -n 5 -
```

Lưu vào /etc/pacman.d/mirrorlist

## sudo

Leo thang đặc quyền bằng password có nguy cơ tiềm ẩn, do X11 cho các application được keylog nhau (install `xkbcat` để test).

Như vậy có các giải pháp:

1. Dùng yubikey, config PAM để leo thang.
2. Chuyển tty khi cần leo thang.
3. Xóa khả năng leo thang của non-root user.

## Limit size của journal

https://wiki.archlinux.org/index.php/Systemd/Journal#Journal_size_limit
