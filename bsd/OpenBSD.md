## Man pages
 
https://man.openbsd.org/

## sudo

OpenBSD từ 5.8 không còn dùng sudo, mà được cài đặt sẵn `doas`.

## Run services

    rcctl status service-name

## X

Sau khi cài xong OpenBSD, nếu không có X, thì phải enable và start `xenodm` lên. (Lúc cài có package nhưng quên enable): https://www.openbsd.org/faq/faq11.html#ConfigX

## user add to group

Add user vào group wsrc sẽ cấp quyền ghi vào /usr/src (nơi chứa src của BSD hoặc ports).

    $ ls -alh /usr

    # user mod -G wsrc exampleuser

    ## Check lai 
    # groups exampleuser 

## port tree

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