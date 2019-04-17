# Cách tạo một con telegram bot với golang và heroku

_Mở đầu, giải thích sơ về mô hình hoạt động của telegram bot là nó sẽ cung cấp 1 access token (nên được giữ bí mật), sau đó 1 thư viện sẽ liên lạc với 1 hệ thống API để 1) lắng nghe các query tới, 2) lấy thông tin, 3) xử lý thông tin, 4) trả kết quả và 5) nhiều thứ khác nữa._

_Mọi xử lý sẽ nằm ở phía chúng ta (telegram không hỗ trợ về mọi trí não của con bot). Telegram chỉ hỗ trợ các kết nối từ con BOT, về server của ta, và ngược lại, tức là từ phía server trở qua con bot, đương nhiên, là thông qua hệ thống API của telegram cung cấp._

1.  Đầu tiên phải tạo một con bot

    Bằng cách liên lạc với BotFather (@BotFather), the GodFather of the bots, and maybe the only one without his suffix bot for his name.

    Khi chat với BotFather, command `/create`. Sau đó nhập các thông tin cần thiết. VD GoogleSisBot, 1 con bot nhận query từ user và trả về 1 đoạn audio được phát ra bởi chị google.

    Bước này khá đơn giản với dân trong nghề. **Khi này cần lưu lại token được cung cấp.**

    Telegram Bot có các thể loại như: keyboards, inline, etc. Chỗ này xem official doc chắc ăn là sẽ rõ ràng hơn nói ra ở đây.

    Set inline cho bot: Từ BotFather, gõ

        /setinline

    và nhập các phần cần thiết để bật chế độ inline cho bot.

2.  Sau đó ta sẽ bắt đầu phần trí não của con bot.

    Thật ra thì trí não con bot có thể viết với bất kì ngôn ngữ gì, vì chủ yếu nó liên lạc với nhau qua API (và với cái token được cấp). Ở đây mình chọn Golang, vì đơn giản là 1) nó đang hot (dù được 1 thời gian rồi), 2) heroku nó hỗ trợ golang.

    Với golang thì framework được chọn là `gopkg.in/tucnak/telebot.v2`.

    1.  Tạo một folder, ví dụ GoogleSisBot, bên trong tạo `src/main/main.go`

    2.  main.go

            package main

            import (
                "time"
                "log"
                "fmt"
                "strconv"
                "net/url"
                "os"

                tb "gopkg.in/tucnak/telebot.v2"
            )

            func main() {
                b, err := tb.NewBot(tb.Settings{
                    Token:  os.Getenv("SECRET_TOKEN"),
                    Poller: &tb.LongPoller{Timeout: 10 * time.Second},
                })

                if err != nil {
                    log.Fatal(err)
                    return
                }

                b.Handle("/help", func(m *tb.Message) {
                    b.Send(m.Sender, "Con BÊ Ô TÊ này được viết vào một sáng thứ bảy đầy năng lượng, với 1 cái bàn phím và 1 chút ☕️.")
                })

                b.Handle(tb.OnText, func(m *tb.Message) {
                    b.Send(m.Sender, "This bot is currently not supported interactive mode. Please use the inline mode (@GoogleSisBot your_chat)")
                })

                b.Handle(tb.OnQuery, func(q *tb.Query) {
                    if q.Text != "" {
                        // Declaration of G Translate url (our beloved sister)
                        googleTranslateUrl := "https://translate.google.com.vn/translate_tts?ie=UTF-8&tl=vi&client=tw-ob&q="

                        // List of result for inline query
                        results := make(tb.Results, 1)

                        // The one we need
                        result := &tb.AudioResult{
                            Title: q.Text,
                            URL: googleTranslateUrl + url.QueryEscape(q.Text),
                        }

                        results[0] = result
                        results[0].SetResultID(strconv.Itoa(0))

                        err := b.Answer(q, &tb.QueryResponse{
                            Results: results,
                            CacheTime: 60, // in sec
                        })

                        if err != nil {
                            fmt.Println(err)
                        }
                    }
                })

                b.Start()
            }

    Trong đó mình import 1 số thư viện cơ bản (`fmt`, `os`...), và thư viện chính là `tb` (telegram bot).

    `tb.NewBot()` sẽ tạo một instance kết nối tới con bot thông qua token, ở đây mình không chỉ rõ ra trong code và bạn cũng nên thế, vì cơ bản là commit cái token lên thì chung quy là khá nguy hiểm. Mình sẽ lấy token này từ system env (thông qua package `os`).

    `tb.Handle()` sẽ listen các input của user (thông qua command dạng `/command`), hoặc nếu 1st param là onQuery thì sẽ handle input từ mode inline (kiểu gõ `@GoogleSisBot <your_chat>`)

    `b.Answer` sẽ trả KQ về các chat, nhận param đầu là results (1 mảng các `result []tb.Result`). Result thì trong ví dụ là dạng `AudioResult`, nó lại kế thừa từ `BaseResult`... Chỗ này tham khảo doc của cái framework có list rất rõ.

    Khi mọi việc đã xong. Có thể chạy `go install`. Đáng lẽ nó sẽ sinh ra 1 file ở `GoogleSisBot/bin/main.exe` nhưng lại bị thiếu package dependency. Phải chạy `go get gopkg.in/tucnak/telebot.v2` trước để nó tải về package này nằm trong `GoogleSisBot/src/main/github.com....`

    Một khi đã hoàn thành, `go install` sẽ chạy ra được file `main.exe`. Chạy thử file `main.exe`, và chat với con bot, sẽ thấy nó trả lời mình đúng như trong code bên trên.

    Chuyện còn lại là public lên 1 cái server nào đó. Lúc này heroku là một sự lựa chọn gần như là tốt nhất.

3.  Deploy lên heroku

    Tại sao lại là heroku thì nó lại k nằm trong phạm vi bài viết này.

    Đầu tiên mình cài heroku cli cái đã. Sau đó login

        heroku login

    Nhập passwd và username xong. Tạo một app mới.

        heroku create gsis-bot

    Cái trên sẽ tạo một cái app tên là `gsis-bot`

    Sau đó bật cái buildpack go lên (bên trong setting của app).

    Trong heroku để chỉ định cái app của mình nên được chạy theo cách nào thì có file `Procfile` để làm điều đó.

    Procfile

        web: rail...

    hoặc

        worker: main

    Trong đó type web thì sẽ chạy cái lệnh, vd `node start`... Heroku sẽ public ra http và port gì đó... còn nếu type là worker thì có thể nói đơn giản là nó sẽ tạo ra 1 con worker làm việc background. (đều là dyno free hết nếu trong phạm vi free). Ngoài ra còn type release nữa. Loại này sẽ chạy cái command đó lúc deploy.

    Mình đã chơi ngu chọn `release : main`, khi này nó deploy nó sẽ chạy cái main và đương nhiên là k có hồi kết, vì cái phần này nó được thiết kế để chạy vĩnh viễn mà, trừ phi có lệnh tắt từ user thôi.

    Và nó chạy mãi, làm cái deploy phase trở nên _is deploying..._. Lúc này kill process bằng cách `heroku ps:kill...`

    Sau đó git add remote heroku vào.

    Sau đó push lên và nó sẽ tự build. Lúc này sẽ báo lỗi k có dependency, vì nếu đúng trình tự ở local thì heroku nó nên chạy `go get`... gì đó cho mình trước khi chạy go install chứ đúng k.

    Lúc này `Godeps` vào cuộc. Godeps là 1 cái dependency manager của go. Cài vào local. `go get github.com/tools/godep` hoặc github.com/tools gì đó.

    Ở cái chỗ chạy được go install (`src/main`), chạy `godeps save`. Nó sẽ tự sao chép (hay clone?) từ cái dependencies bên ngoài `main.go` vào một cái folder kêu là `vendor`, và đồng thời tạo một folder `Godeps` cùng với 1 vài config kiếu `config.json`. Lúc này nên exclude hoặc delete luôn đống dependency được tạo ra bởi lệnh `go get`. Thử lại thì k có đống đó vẫn `go install` được (thanks to Godeps).

    Nếu không chạy godep được, zsh báo lỗi command not found thì fix theo https://github.com/tools/godep/issues/373

    Lúc này thử push lại lên heroku và boom. Mọi thứ đã được deploy.

    Một cái directory của heroku golang là (`GoogleSisBot/src/main`)

        - main.go
        - Procfile
        - .gitignore
        - Godeps
        - vendor

    Sau khi deploy được, cần scale 1 worker dyno.

        Heroku ps:scale worker=1

    Đó, lúc này lên sóng. Kiểm tra lại trong heroku GUI sẽ thấy dyno đang chạy.

    Các command khác của heroku:

        heroku ps (-a, -app) app_name

    List các process (của app)

        heroku ps:kill process_id.process_name

    Kill process

---

## Trick

Đối với Golang (hiện tại là ver2), một project phải nằm bên trong folder src. Vì khi chạy `go build` hoặc `go install`, go sẽ tìm trong src và (pkg, vendor, ...) để compile. Trick sau đây sẽ không bắt buộc phải add tay các deps (go get ...) mà sử dụng lại phần vendor của Godeps.

Export var GOPATH về chỗ `/home/thevinh/Projects/Golang`

Bố trí theo tree sau

    /home/thevinh/Projects/Golang
    ├── bin
    └── src
        └── gsis-telegram-bot
            ├── Godeps
            └── vendor
                ├── github.com
                └── gopkg.in

Khi này, bên trong `gsis-telegram-bot`, chạy lệnh go install thì mọi thứ sẽ được compile ra thư mục `bin`

Nếu muốn chạy nhiều bot 1 lúc thì xài go kiểu

    func a(){
        // Everything this bot 1 does
        this1BOt.start()
    }

    func b(){
        // Everything this bot 2 does
        this2BOt.start()
    }

    func main() {
        go a()
        go b()

        select {}
    }

Để giữ main process luôn chạy thì thêm

    select { }

vào cuối.

---

## Để post vào một channel

Dùng `Send`.

	channel, channelGetErr := b.ChatByID("chat_id_here_in_string") // Chỗ này không truyền username vào được, mặc dù Official Doc bảo là được...
	b.Send(channel, "This test message")

Để lấy được ID của chat (a bit tricky)

Forward một message của chat cần lấy id vào @userinfobot

Bot đó phải nhận được tin nhắn mới có thể Send được (tránh spam).

---

