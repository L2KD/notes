Sử dụng gpg-agent thay cho ssh-agent để có thể xài được smart card. Yubikey cũng có thể giả lập được smart card.

Sau khi mọi thiết lập OK (GPG key imported vào yubikey).

Chạy `ssh-add -L` sẽ list các key hiện tại. Nhưng sẽ báo lỗi connection... do gpg-agent không được support ssh theo mặc định. Gpg-agent này được khởi chạy khi boot.

Để fix cái này có thể theo:

1. Chèn vào script bashrc để tắt ssh-agent, bật gpg-agent với option là ssh-support.
2. Đổi lại biến môi trường để gpg-agent khởi chạy mặc định chịu xài ssh.

Thêm config vào `.zshrc`, hoặc nếu zshrc được restore từ dotfiles thì đã có config.

    export GPG_TTY="$(tty)"
    export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
    gpgconf --launch gpg-agent

*Lưu ý*: GPG agent lúc hoạt động sẽ yêu cầu mở khóa GPG, nếu sử dụng smartcard phải có pinentry, nếu dùng pinentry của GTK2 thì phải cài thêm gói `gtk2`.

Cách 2:

Tạo thêm file

```
~/.pam_environment

SSH_AGENT_PID	DEFAULT=
SSH_AUTH_SOCK	DEFAULT="${XDG_RUNTIME_DIR}/gnupg/S.gpg-agent.ssh"
```

Check lại bằng

```
‣ gpgconf --list-dirs agent-ssh-socket
/run/user/1000/gnupg/S.gpg-agent.ssh
‣ echo $XDG_RUNTIME_DIR
/run/user/1000
```

Như vậy file trên đã ok. 

Reboot máy.

Chạy

```
‣ ssh-add -L
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQClY/hhk/B5ViXnsT6J7r0keiSNSjz0fpH49jn48VUqE9wOG0xtQA5u0It4lo6C/x1/rYc9zz7qyMd2tdDm4bfGEul8NM8gOM25Ewfu/9IsRWp4w29coIadl5IcD7dzTB+XE4TH3fIA3RuDlyuemKzQkp/4wuVJpiXN7mJZ+beFbK5b60x4C4uTU6TaZUooJxsgY69LuVPGQzeeUxmHyp8PGcoLmcRt20mEA531lq3lFNP51M/eypFWGwwD0zcEZeBQh8y5w3+rOL4wNOmx44GWUI/JutKUcwNbDo/18S2VWHBG2uc+8tjo8wHDxjOSr63BgFzLB2Ew+Kivfgz1UCkZkB/OB9uiee7jUx51mP1yHJCru/KTkQ0n5F4WUxuYyZGpvFS1NkkYmJCD1O1x3f+mU2vUqF9++BYe34u/q7QakX60Zf6eCKpuoJkdZKcNdjxJHMo7Ifhe5VeCWxEge6ISQbS/GJ3uKaHmYpr55URZ2njSEN1nYGqPLYEIyeJhOJKtN4s0AUqxhip9uUia4xLl6mq1ubx4kXuGnbj+7suf9TEpS7Yo8RH3rE6lTSGNpMocJlPN0JQCSTTC+2ZGg7rvtTem/XfDZyVXOiQo2FkxaX702jShXziZuf1TtczbVDAhCtqzmu4cgHGiz/aNM5rgCGdfocY2l6c4yHF+Y4irOw== cardno:000610320244
```

Chỗ cardno:xxx là yubikey của ta.
