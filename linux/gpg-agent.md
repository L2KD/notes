Sử dụng gpg-agent thay cho ssh-agent để có thể xài được smart card. Yubikey cũng có thể giả lập được smart card.

Sau khi mọi thiết lập OK (GPG key imported vào yubikey).

Chạy `ssh-add -L` sẽ list các key hiện tại. Nhưng sẽ báo lỗi connection... do gpg-agent không được support ssh theo mặc định. Gpg-agent này được khởi chạy khi boot.

Để fix cái này có thể theo:

1. Chèn vào script
