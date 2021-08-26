# Learning log

FreeBSD là unix-like (giống với linux). 

kqueue vs epoll

Tải về image vhd (virttual box) dùng thử trên trang chủ. Phiên bản đã dùng: 12.1

Tài khoản mặc định là `root` và không có mật khẩu.

Không có X server (khá giống arch).

Không có cả cái tool dùng để cài package (`pkg`). Gõ pkg nó sẽ bootstrap để cài vào.

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
