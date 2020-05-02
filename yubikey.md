# Yubikey learning log

Wow, hôm nay nhận được cái key. Shipped via fado (đặt ngày 26/06, nhận ngày 10/07), giá mắc hơn amazon khoảng 400k.

Đầu tiên, một trong các chức năng của yubikey là nó thường được dùng để xài 2FA (2nd factor authentication). Use case thường thấy: Khi gõ username + pwd xong, service provider sẽ yêu cầu plug key vào và touch the button. Nếu ok sẽ cho vào.

Mặc định yubikey được cài đặt sẵn chức năng này (gọi là Yubico OTP) ở slot 1. Mua về xài liền cũng được. Trên Gmail, hướng dẫn cài đặt rất trực quan, làm theo các bước là xong.

Log:

Đã cài các gói:

- `libu2f-host`: Enable chromium browser connectivity with the physic key.
- `yubico-pam yubikey-manager yubikey-manager-qt`: PAM để login, cái ykman để dùng các application được hỗ trợ (vd totp gen, smartcard...).
- `yubikey-personalization yubikey-personalization-gui`: GUI để customize lại key, ghi config vào key.

Service này cần chạy để OS biết và nói chuyện với yubikey: `pcscd`.

## Giới thiệu sơ về yubikey.

Có 2 slots. Mặc định slot đầu sẽ là Yubico OTP. Kích hoạt bằng cách chạm vào button 0.3-1.5s. Khi chạm sẽ gởi 1 đoạn các ký tự gì đó xem như là OTP để đăng nhập. Slot 2 sẽ được kích hoạt khi chạm 2-5s.

**Lưu ý khi overwrite config vào slot 1**

Khi xuất xưởng, yubikey được preconfig OTP vào slot 1. Prefix `CC` được dùng cho credentials. Khi overwrite lại `VV` sẽ được dùng. Về mặt kỹ thuật, `VV` hay `CC` đều có mức độ bảo mật như nhau. Nhưng 1 số service provider lại prevent `VV` do họ nghĩ rằng user can thiệp vào thì sẽ ảnh hưởng ít nhiều tới độ bảo mật. (Xem thêm tại [đây](https://wiki.archlinux.org/index.php/YubiKey#Installation))

Suy nghĩ kỹ trước khi thực hiện việc này.

## Dùng HOTP với lastpass free

Thực ra LastPass Free xài TOTP cho việc 2FA, nên các ứng dụng như Authy, Google Authenticator đều support. Enable 2FA của lastpass rồi scan QR là xong.

## Cách thêm một TOTP service vào yubikey.

Giao diện:

1. Cài Yubikey OATH desktop (AUR).
1. Sau đó vào service enable Google Authenticator. View QR.
1. Mở Yubikey OATH desktop và scan QR. QR phải ở cùng 1 mành hình với app.
1. Nó sẽ tự add thêm 1 credential vào.
1. Touch btn để generate code. Paste code này vào chỗ U2F của service.
1. OK.

Mobile:

1. Cài đặt app yubico authenticator.
1. Trong service, chọn enable 2FA. Hiện QR.
1. Ở màn hình chính, chọn `+` để thêm service. Chọn scan QR.
1. Scan QR và đưa yubikey vào vùng đọc NFC của điện thoại.
1. Paste code vừa gen vào service.
1. OK

CLI:

1. TBD

Trên linux:

    ykman oath add <servicename> key

Trong đó key lấy từ QR code (dùng app khác để scan lấy key) hoặc có 1 số service show key lúc enabling 2FA via OTP.

    ykman oath add lastpass dfguhsldflkdf

Ngoài ra còn có nhiều option như:

- `-t`: Touch khi lấy code.
- `-o`: HOTP hay TOTP (mặc định là TOTP).
- `-a`: Mặc định là SHA1.
- `-i`: Issuer.

Xem thêm: `ykman oath add -h`.

Một số service có thể cung cấp full các thông tin trên thông qua QR code. Hoặc chỉ cung cấp 1 vài thông tin required (các option khác sẽ là mặc định).

## `ykman`

Help:

`ykman -h`

Với mỗi application sẽ có section help tương ứng. Ví dụ để xem help của application oath thì

`ykman oath -h`

## OATH application

App này dùng để sinh ra OTP (HOTP & TOTP) (Tương tự với standard đang dùng cho các app như Google Authenticator...).

Để add thêm service, có nhiều cách:

1. Dùng CLI (`ykman oath`).
2. Dùng GUI (của yubico).
3. Dùng mobi app (Authenticator trên iOS) với NFC featured keys.

Tất cả các cách trên đều sẽ add service vào key. Key được dùng để lấy OTP.

### List services:

App iOS: Kéo xuống và áp key vào điện thoại **(iPhone 8 nằm ở phía đầu của điện thoại)**.

Linux:

```
$ ykman oath list
Bitbucket:voldedore
Epic Games:voldedore@Epic Games
LastPass:voldedore@gmail.com
```

### Generate code

iOS: Xem phần trên

Linux:

```
$ ykman oath code LastPass:voldedore@gmail.com
LastPass:voldedore@gmail.com  982977
```

Tùy vào lúc add service có thểm option `-t` hay không, có thể phải touch device.

Lưu ý: `-t` chỉ có thể sử dụng trên PC. Do key áp vào điện thoại không có điện vào nên không touch được key. (Physically touchable but nothing happens).
