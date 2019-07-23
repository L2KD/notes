This is based on https://jamesthebard.net/archlinux-and-u2f-login/

Đầu tiên là phải có cái PAM lib.

    pacman -S pam-u2f

Sau đó dùng `pamu2fcfg` để gen ra file `/etc/u2f_mappings`

```
pamu2fcfg -ujweatherly > /etc/u2f_mappings
```

File đó có dạng:

jweatherly:ádhjdsjfsd:adfsdgdfdfg:dfdssdgf

Thay jweatherly bằng username mong muốn.

Thay đổi file

```
/etc/pam.d/system-auth
# Thêm vào
auth  sufficient  pam_u2f.so  debug authfile=/etc/u2f_mappings cue

```

Reboot machine.

Lightdm should work. Sudo is elevated.
