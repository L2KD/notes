Trước đây (pre-unix), các hệ thống máy tính được dùng chung (vì chúng quá đắt đỏ). Việc sử dụng này được đặt lịch trước. Chúng có những thiết bị đầu cuối (gọi là terminals) được gắn kết vào hệ thống. Một thời gian sau đó, để khỏi phải đứng trước một hệ thống lớn mà làm việc, các máy tính thường được kết nối với các máy teleprinters điện từ, hoặc các teletypewriters. Các hệ thống sẽ gởi tín hiệu đến những teletypewriters này, và chúng sẽ thực hiện việc in các ký tự output của máy tính đến vị trí của người dùng (một cách vật lý). 

Đó là cách mà các hệ thống mạng unix được xây dựng, về mặt cơ sở hạ tầng vật lý. Sau này, người ta tạo ra các terminals bằng phần mèm (giả lập lại thiết bị đầu cuối vật lý), nhưng vẫn giữ nguyên các nguyên lý trước đây, để tránh phá vỡ hệ thống cơ sở hạ tầng hoặc code cũ. Các máy tính hiện đại vẫn giữ nguyên tên gọi nguyên thủy mặc dù đã sử dụng màn hình để hiển thị output của máy tính (console), và những console như vậy được gọi là console ảo (virtual console) thay vì vật lý.

Trong các hệ linux phổ biến, có 6 virtual console như vậy, nằm từ 1-6. Đổi qua đổi lại bằng phím `Ctrl + Alt + Fn` với `n = 1..6`. Một số distro dùng tty1 để chạy window manager (login screen), một số thì dùng số 7.

Như vậy, mỗi 1 tty là một terminal.

Lý do đó những app tên Konsole, urxvt được gọi là terminal emulators.