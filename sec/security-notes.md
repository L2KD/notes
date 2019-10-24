Dạo gần đây mình bị ám ảnh bởi mọi thứ liên quan đến bảo mật. Cụ thể hơn là bảo mật thông tin cá nhân. Đành viết vài dòng để lại khi cần thì có summary cho nhanh.

1. SMS is bad

Tắt Preview notification của iOS cho mọi ứng dụng. Hãy nghĩ thử về trường hợp sau:

Attacker biết được username của bạn trên 1 service, và đang nắm trong tay điện thoại của bạn.

Hắn dùng chức năng `Forgot password` (thật là irony vì thực ra hắn không phải `forgot` nhưng lại cả gan dùng cái chức năng được đặt tên như vậy, what a bad boy).

Form của chức năng này thường là nhập vào username, và 1 loạt các option như sms, email... để nhận cái confirmation code.

Không cần mật khẩu phone cũng có thể xem được nội dung tin nhắn. Từ đây hắn có thể reset được password nhờ vào code hiện trên phone (vẫn đang được khóa).

**Cách xử lý:** Tạm thời: Vào Settings > Notifications > Preview > Only when locked. Tốt hơn: Tắt option recovery password via sms. (Why? SMS is bad).
