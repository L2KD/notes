# Cách sử dụng screen

Screen dùng để tách nhiều 'màn hình' trong 1 session làm việc trên terminal, hay được dùng khi ssh vào server để chạy nhiều connection mà k phải reconnect một session khác.

Đương nhiên là phải cài screen, tùy theo distro

Khởi động một session mới:

    screen

Xem danh sách các screen hiện tại:

    screen -ls

Reattach một screen trước đó:

    screen -r

Các lệnh cơ bản khi đang trong một session screen:

    ^A ?  : help
    ^A n  : next screen
    ^A p  : prev screen
    ^A d  : detach screen (thoát ra khỏi screen về với session ban đầu)

Để thuận tiện cho việc reattach một screen, người ta thường đặt tên cho screen.

Cách đặt tên,

### cho một screen đã có:

đang trong một screen, gõ `^A :`, sau đó nhập `sessionname your_wanted_name`, enter.

### cho screen mới:

    screen -S session_name

Check lại: `^A d` để thoát ra, kiểm tra danh sách bằng `screen -ls`

    There is a screen on:
      29860.top	(Detached)

Để vào lại screen, gõ `screen -r top`
