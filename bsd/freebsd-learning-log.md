# Learning log

FreeBSD là unix-like (giống với linux). 

kqueue vs epoll

## Cài đặt

### Pre built image

Tải về image vhd (virttual box) dùng thử trên trang chủ. Phiên bản đã dùng: 12.1

Tài khoản mặc định là `root` và không có mật khẩu.

Không có X server (khá giống arch).

Không có cả cái tool dùng để cài package (`pkg`). Gõ pkg nó sẽ bootstrap để cài vào.

### Cài đặt từ đầu

Nói chung cứ next next tơi thôi, có chọn GELI để disk encrypting.

## Bật sshd

Thử config nhưng sực nhớ phản khoa học quá.

## Tạo user

```
# adduser
Username: jru
Full name: J. Random User
Uid (Leave empty for default):
Login group [jru]:
Login group is jru. Invite jru into other groups? []: wheel
Login class [default]:
Shell (sh csh tcsh zsh nologin) [sh]: zsh
Home directory [/home/jru]:
Home directory permissions (Leave empty for default):
Use password-based authentication? [yes]:
Use an empty password? (yes/no) [no]:
Use a random password? (yes/no) [no]:
Enter password:
Enter password again:
Lock out the account after creation? [no]:
Username   : jru
Password   : ****
Full Name  : J. Random User
Uid        : 1001
Class      :
Groups     : jru wheel
Home       : /home/jru
Shell      : /usr/local/bin/zsh
Locked     : no
OK? (yes/no): yes
adduser: INFO: Successfully added (jru) to the user database.
Add another user? (yes/no): no
Goodbye!
#
```

Cài sudo

`pkg install sudo`

`visudo` remove # for wheel.

Lưu ý: sau này nên dùng doas thay vì sudo. Về lý thuyết thì doas cũng giống vậy (group wheel, doas.conf tương tự sudo.conf...), nhưng doas có độ bảo mật cao hơn (dc dùng trong OpenBSD).

## Enable sshd:

sysrc sshd_enable="YES"

Lệnh trên sẽ thêm dòng sshd_enable="YES" vào file /etc/rc.conf. File rc.conf là file chứa tất cả config của hệ thống khi boot, run service nào, NIC gì...

## Package mirror

Tạo file

```
sudo cp /etc/pkg/FreeBSD.conf /usr/local/etc/pkg/repos/FreeBSD.conf
```

Sửa lại url là http://pkg0.twn.freebsd.org/ (Taiwan)

## RPI

File image rpi có sẵn 2 users là freebsd và root (pwd giống với un).

SSH vào freebsd user, leo thang bằng `su root`, cài pkg...

## Window manager

Mặc định của FreeBSD sẽ không có X server. Cài bằng pkg xorg. Và xdm. `xdm` là một display manager nhỏ gọn.

Để chạy wm khi boot, tạo file `~/.xsession`. 

Vì sao lại là `~/.xsession`? 

    cat /usr/local/etc/X11/xdm/Xsession
    cat /usr/local/etc/X11/xinit/xinitrc

### dwm

dwm là 1 window manager.

Để cài đặt, nên cài bằng pkg

    # pkg install dwm

Sau đó custom lại config rồi compile ra binary mới. Vì những config của dwm nằm trong file source (config.h).

    $ git clone git://git.suckless.org/dwm

Config lại `config.mk`

Đổi `X11INC` từ `/usr/X11R6/include` thành `/usr/local/include`

Đổi `X11LIB` từ `/usr/X11R6/lib` thành `/usr/local/lib`

Tương tự với phần `FREETYPEINC`.

Sau đó compile 

    $ make

Ok.

Config lại `config.h` (file mẫu là `config.def.h`).

Config lại Mod1 thành Mod4 (Super), sao cho tùy nhu cầu.

Mặc định dwm sẽ dùng app launcher là `dmenu`, terminal là `st`. `st` có sẵn khi cài FreeBSD, còn dmenu nếu chưa có thì cài thêm.

Xong mỗi lần config xong sẽ `make`, rồi cp binary chồng lên cái có sẵn.

    # cp -f dwm /usr/local/bin

Hoặc chạy lệnh

    # make clean install

### Status bar

Ở arch, chúng ta có polybar, i3status... ở FreeBSD, chúng ta có những thứ trên. Nhưng để xài chung với dwm, thử dùng slstatus.

Cũng 1 dạng với những app suckless khác, khi cần config, phải config từ source.

Clone source về. Thay đổi mkfile để chọn đúng path của inc và lib. Thêm `-lkvm` vào LDFLAGS để có thể truy cập vào những thứ liên quan tới hardware thông qua kernel.

Edit lại các component trong Makefile cho phù hợp với nhu cầu.


## Go tieng viet trong freebsd

Cai cac packge sau

- qt5
- zh-fcitx
- zh-fcitx-configtool
- fcitx-unikey
- Init may cai environment variable trong ~/.xsession
