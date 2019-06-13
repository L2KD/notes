# Cách tạo một con bot để leech RSS và podcast vào một channel

## Deps

- github.com/mmcdole/gofeed
- github.com/robfig/cron
- gopkg.in/tucnak/telebot.v2
- github.com/mattn/go-sqlite3

Trong đó cái `gofeed` dùng để lấy một link chứa RSS (dạng XML) và parse nó. `cron` dùng để schedule cho phần lấy tin tức sau một khoảng thời gian nhất định nào đó. `telebot`, cơ bản của Telegram Bot by Go. `sqlite` dùng để lưu lại những tin đã đăng.

Vd lấy tinhte làm nguồn, hiện tại tinhte dùng feedburner của google để thực hiện việc cập nhật RSS.

## Instruction

Phần boot-up của bot

1. Đầu tiên dùng một file flag để đánh dấu cho biết chúng ta đã init DB rồi (tạm gọi là init.done)
2. Nếu ko có file này, thì thực hiện init DB, tạo bảng.
3. Nếu có file này, có thể là đã được init, nhưng DB chưa chắc đã OK. Tiếp tục phải check xem có DB chưa.
4. Nếu không có DB, thực hiện init, create tables.
5. Nếu đã có DB, tiến hành connect vào và validate DB. (TODO)

Phần body

1. Cứ mỗi 5 phút, dùng `gofeed` lấy thông tin từ URL đã đặt sẵn, ta được một list các items (là những bài tin được đăng)
2. Từ danh sách, loop từng phần tử đồng thời check xem bài hiện tại đã có trong DB chưa.
3. Nếu chưa thì thực hiện lưu vào DB, sau đó post vào channel.
4. Nếu đã có rồi thì next qua phần tử kế.

## TODO

- [ ] Validate DB.
- [ ] Versioning.
- [ ] Dùng các nguồn khác không phải là RSS.
