# Yubikey learning log

Wow, hôm nay nhận được cái key. Shipped via fado (đặt ngày 26/06, nhận ngày 10/07), giá mắc hơn amazon khoảng 400k.

Đầu tiên là yubikey thường được dùng để xài U2F. Cụ thể: Khi gõ username + pwd xong, service provider sẽ yêu cầu plug key vào và touch the button. Nếu ok sẽ cho vào.

Trên Gmail, hướng dẫn cài đặt rất trực quan, làm theo các bước là xong.

_Ghi chú_ Do archlinux phải cài thêm các gói: `libu2f-host` mới có thể dùng chung với browser.

Log:

Đã cài các gói:

- `libu2f-host`: Enable browser connectivity with the physic key.
- `yubico-pam yubikey-manager yubikey-manager-qt`: Loggini on linux?!
- `yubikey-personalization yubikey-personalization-gui`: GUI để customize lại key.

Start service: `pcscd`.

---

Giới thiệu sơ về yubikey.

Có 2 slot. Mặc định slot đầu sẽ là Yubico OTP. Kích hoạt bằng cách chạm vào button 0.3-1.5s. Khi chạm sẽ gởi 1 đoạn các ký tự gì đó xem như là OTP để đăng nhập. Slot 2 sẽ được kích hoạt khi chạm 2-5s.

**Lưu ý khi overwrite config vào slot 1**

Khi xuất xưởng, yubikey được preconfig OTP vào slot 1. Prefix `CC` được dùng cho credentials. Khi overwrite lại `VV` sẽ được dùng. Về mặt kỹ thuật, `VV` hay `CC` đều có mức độ bảo mật như nhau. Nhưng 1 số service provider lại prevent `VV` do họ nghĩ rằng user can thiệp vào thì sẽ ảnh hưởng ít nhiều tới độ bảo mật. (Xem thêm tại [đây](https://wiki.archlinux.org/index.php/YubiKey#Installation))

Suy nghĩ kỹ trước khi thực hiện việc này.

---

Dùng HOTP với lastpass free

1. Cài Yubikey OATH desktop (AUR).
1. Sau đó vào lastpass enable Google Authenticator. View QR.
1. Mở Yubikey OATH desktop và scan QR. QR phải ở cùng 1 mành hình với app.
1. Nó sẽ tự add thêm 1 credential vào.
1. Touch btn để generate code. Paste code này vào chỗ U2F của Lastpass.
1. OK.

Nếu quá rắc rối, chi \$1/month để dùng lastpass premium.
