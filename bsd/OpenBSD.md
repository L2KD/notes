## Man pages
 
https://man.openbsd.org/

## sudo

OpenBSD từ 5.8 không còn dùng sudo, mà được cài đặt sẵn `doas`. Mặc dù lúc mới cài xong chạy doas thì nó nói it is not enabled... ơ kìa?

Tạo thêm file: `/etc/doas.conf`

    permit :wheel

Doas: 

    $ doas pkg_check

OK.

## Run services

    rcctl status service-name

## X

Sau khi cài xong OpenBSD, nếu không có X, thì phải enable và start `xenodm` lên. (Lúc cài có package nhưng quên enable): https://www.openbsd.org/faq/faq11.html#ConfigX

## user add to group

    # user mod -G <group-name> <user>

## port tree

Có 2 cách: 1) là bằng mirror, tải 1 file nén về và giải nén vào (src|ports), 2) là bằng cvs (1 kiểu versioning).

Để tránh việc leo thang đặc quyền, thêm user vào group `wsrc`.

Add user vào group `wsrc` sẽ cấp quyền ghi vào `/usr/src` (nơi chứa src của BSD) hoặc `/usr/ports` (ports).

    $ ls -alh /usr

    # user mod -G wsrc exampleuser

    ## Check lai 
    # groups exampleuser 

### Giải nén file vào từ mirror:

Sau khi thêm user vào wsrc group để write vào /usr/src, tạo các folder: `/usr/ports`.

    # cd /usr
    # mkdir -p   xenocara ports
    # chgrp wsrc xenocara ports
    # chmod 775  xenocara ports

Tải file ports.tar.gz từ mirrors về và giải nén vào /usr/ports

    $ cd /tmp
    $ ftp https://cdn.openbsd.org/pub/OpenBSD/$(uname -r)/{ports.tar.gz,SHA256.sig}
    $ signify -Cp /etc/signify/openbsd-$(uname -r | cut -c 1,3)-base.pub -x SHA256.sig ports.tar.gz

    $ cd /usr
    $ tar xzf /tmp/ports.tar.gz

### CVS 

Chọn lại server và tag cho đúng với nhu cầu. Xem thêm: https://www.openbsd.org/anoncvs.html

    $ cd /usr
    $ cvs -qd anoncvs@<server>:/cvs checkout -r<TAG> -P ports

    $ cvs -qd anoncvs@anoncvs.ca.openbsd.org:/cvs checkout -rOPENBSD_6_9 -P ports

## Funny

    ➜  ~ neofetch 
                                        _    admin@OPENBSD_VB.my.domain 
                                        (_)   -------------------------- 
                |    .                      OS: OpenBSD 6.9 i386 
            .   |L  /|   .          _       Host: innotek GmbH VirtualBox 
        _ . |\ _| \--+._/| .       (_)      Uptime: 1 hour, 38 mins 
        / ||\| Y J  )   / |/| ./             Packages: 15 (pkg_info) 
        J  |)'( |        ` F`.'/        _     Shell: zsh 5.8 
    -<|  F         __     .-<        (_)    Terminal: /dev/ttyp2 
        | /       .-'. `.  /-. L___           CPU: Intel i5-2410M (1) @ 2.286GHz 
        J \      <    \  | | O\|.-'  _        Memory: 77MiB / 1023MiB 
    _J \  .-    \/ O | | \  |F    (_)
    '-F  -<_.     \   .-'  `-' L__                                   
    __J  _   _.     >-'  )._.   |-'                                   
    `-|.'   /_.          \_|   F
    /.-   .                _.<
    /'    /.'             .'  `\
    /L  /'   |/      _.-'-\
    /'J       ___.---'\|
    |\  .--' V  | `. `
    |/`. `-.     `._)
        / .-.\
        \ (  `\
        `.\

Only 11 packages? And 77MB of RAM.