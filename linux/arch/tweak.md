# Usual tweak

## Recommendation

### Restore configuration

See this repo: https://github.com/voldedore/dotfiles.git

### sudo

Leo thang đặc quyền bằng password có nguy cơ tiềm ẩn, do X11 cho các application được keylog nhau (install `xkbcat` để test).

Như vậy có các giải pháp:

1. Dùng yubikey, config PAM để leo thang. Xem https://github.com/voldedore/notes/blob/master/linux/sudo-use-with-yubikey.md
2. Chuyển tty khi cần leo thang.
3. Xóa khả năng leo thang của non-root user.

### gpg-agent to SSH-ing with yubikey

See: https://github.com/voldedore/notes/blob/master/linux/gpg-agent.md

### Snapshot với btrfs

Cài đặt [`snapper`](https://wiki.archlinux.org/title/Snapper) và (`snap-pac`)[https://wiki.archlinux.org/title/Snapper#Tips_and_tricks] (cần có `cronie`, enable service).

Tạo các subvolume dành riêng cho những vùng không cần thiết, như `/tmp`, `/var/log`.

Xem thêm tại: https://github.com/voldedore/notes/blob/master/linux/btrfs-practices.md

### GPG keys

Import your public keys to use your yubikey

See this https://github.com/voldedore/notes/blob/master/privacy-sec/gpg.md

### Fonts

- For the polybar normally functioning, install the `font-awesome` package.

### Sound

- `alsa-utils`, `pulseaudio-alsa`.

### VIM

Install vundle for plugins.

### Power management (Deep sleep) S3 instead of S2idle

Add a new kernel parameter on booting with GRUB:

    /etc/default/grub

    GRUB_CMDLINE_LINUX_DEFAULT="... mem_sleep_default=deep"

Then re-gen grub cfg (you might want to backup it first):

    # grub-mkconfig -o /boot/grub/grub.cfg

Reboot

Sleep and check it with

    # systemclt suspend

    # journalctl | grep "PM: suspend" | tail

    Mar 16 17:29:32 TGG000923 kernel: PM: suspend entry (s2idle)
    Mar 16 19:06:31 TGG000923 kernel: PM: suspend exit
    Mar 17 07:22:30 TGG000923 kernel: PM: suspend entry (s2idle)
    Mar 17 08:10:35 TGG000923 kernel: PM: suspend exit
    Mar 18 18:12:31 TGG000923 kernel: PM: suspend entry (s2idle)
    Mar 18 19:58:32 TGG000923 kernel: PM: suspend exit
    Mar 18 21:28:39 TGG000923 kernel: PM: suspend entry (deep)
    Mar 18 21:28:54 TGG000923 kernel: PM: suspend exit
    Mar 18 21:29:44 TGG000923 kernel: PM: suspend entry (deep)
    Mar 19 07:34:06 TGG000923 kernel: PM: suspend exit

The system won't drain electric power while sleeping anymore.

## Optional

### Mirror

Cài package `pacman-contrib`.

Lấy 5 mirror nhanh nhất, theo các quốc gia: Vietnam, Thái, Sing, HongKong, protocol https:

```
curl -s "https://archlinux.org/mirrorlist/?country=VN&country=TH&country=SG&country=HK&protocol=https&use_mirror_status=on" | sed -e 's/^#Server/Server/' -e '/^#/d' | rankmirrors -n 5 -
```

Lưu vào /etc/pacman.d/mirrorlist

### Limit size của journal

https://wiki.archlinux.org/index.php/Systemd/Journal#Journal_size_limit

### Compress mkinitcpio

```
# /etc/mkinitcpio.conf

COMPRESSION="xz"
COMPRESSION_OPTIONS=(-0 -T 0)
```

### Fix network interface name

Vì các lý do trời thần nào đó mà network interface của máy có khi nó là enp2s0 có khi nó thành enp2s69, nên các config khác phụ thuộc vào interface name sẽ bị rối loạn lên hết. Để cố định lại, xem https://github.com/voldedore/notes/blob/master/linux/rename-network-interface.md

### Isolation apps

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
