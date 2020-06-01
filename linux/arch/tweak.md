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

## Compress mkinitcpio

```
# /etc/mkinitcpio.conf

COMPRESSION="xz"
COMPRESSION_OPTIONS=(-0 -T 0)
```

## Isolation apps

Install `firejail` & `apparmor`

To enable apparmor on boots:

By grub:

Add kernel param `apparmor=1 lsm=lockdown,yama,apparmor`

```
# /etc/default/grub

GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet apparmor=1 lsm=lockdown,yama,apparmor"
```

```
# /boot/grub/grub.cfg

linux	/vmlinuz-linux root=/dev/mapper/vg0-root rw  loglevel=3 quiet apparmor=1 lsm=lockdown,yama,apparmor
```

Enable apparmor

```
# systemctl enable --now apparmor
```

```
# apparmor_parser -r /etc/apparmor.d/firejail-default
```

```
# firecfg
```

Lệnh trên sẽ tạo trong /usr/local/bin các sym link để chạy app bằng firejail (với default profile).

Tuy vậy, có một số app không chạy tốt khi bị jailed (vd telegram, dù đã thông dbus nhưng vẫn không gõ được bằng ibus).

Chỉ jail 1 số app:

```
# ln -s /usr/bin/firejail firefox
```

VD trước khi chạy firefox trong jail

Truy cập file:///home/admin, ta có quyền vào .ssh/id_rsa...

Thoát FF và chạy lại.

Truy cập file:///home/admin, chỉ có 1 số folder được quyền truy cập.

Tương tự với các trường hợp app khác.

```
# echo XXX > /etc/firejail/firejail.users
```

Ref: https://vez.mrsk.me/linux-hardening.html
