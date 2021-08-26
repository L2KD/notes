Dạo gần đây mình bị ám ảnh bởi mọi thứ liên quan đến bảo mật. Cụ thể hơn là bảo mật thông tin cá nhân. Đành viết vài dòng để lại khi cần thì có summary cho nhanh.

Sau đây là 1 số nhận định được tổng hợp lại từ nhiều nguồn (là các chuyên gia, các blog hoặc bài viết về privacy được đánh giá cao):

0. Luôn dùng Password Manager (LastPass, 1Password, Bitwarden...).
1. Luôn dùng 2FA. Một số cách phổ biến: TOTP, HOTP, YubiKey...
1. Tránh dùng các thứ liên quan tới SMS. SMS is bad.
1.

Tắt Preview notification của iOS cho mọi ứng dụng. Hãy nghĩ thử về trường hợp sau:

Attacker biết được username của bạn trên 1 service, và đang nắm trong tay điện thoại của bạn.

Hắn dùng chức năng `Forgot password` (thật là irony vì thực ra hắn không phải `forgot` nhưng lại cả gan dùng cái chức năng được đặt tên như vậy, what a bad boy).

Form của chức năng này thường là nhập vào username, và 1 loạt các option như sms, email... để nhận cái confirmation code.

Không cần mật khẩu phone cũng có thể xem được nội dung tin nhắn. Từ đây hắn có thể reset được password nhờ vào code hiện trên phone (vẫn đang được khóa).

**Cách xử lý:** Tạm thời: Vào Settings > Notifications > Preview > Only when locked. Tốt hơn: Tắt option recovery password via sms. (Why? SMS is bad).

---

Go further:

https://www.nytimes.com/2019/10/04/smarter-living/10-tips-internet-privacy-crowdwise.html
