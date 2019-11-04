Why Never SSL?

Bài này không đề cập đến SSL là gì, tại sao phải dùng chúng và những tác hại khi không dùng chúng. Ngược lại, có 1 lý do mà trang web này lại không sử dụng SSL (aka TLS).

## HSTS header

HSTS header là một HTTP header từ Web server (WS) báo cho client (ở đây là các web browser) biết nên redirect qua HTTPS. Đại khái vậy.

Khi người dùng truy cập vào web via HTTP, WS sẽ cố gắng thêm HSTS header để browser này sẽ không cố gắng dùng HTTP nữa. Bởi vì, HTTPS is secure.

## Captive portals

Captive portal là những portal đăng nhập khi dùng public wifi. Các portal này không dùng SSL (thường là locally hosted in the network).

## Why neverssl.com

Như vậy, khi connect vào 1 public network. Người dùng thường gõ fb.com hoặc gg.com hoặc whatever có dùng https. Khi này browser sẽ bị redirect về captive portal để log on, nhưng lại dính HTTPS.

Nếu người dùng cố đổi thành http://fb.com thì vẫn bị chuyển qua https (do HSTS header).

Như vậy neverssl.com sinh ra để giải quyết chuyện đó.

Không có TLS, không có HSTS header. Gõ vào đây sẽ văng về trang portal của network để logon.

## Alternative

Một số trang tương đồng cùng 1 chức năng (tuy có thể có những chức năng thừa khác hoặc mục đích tồn tại không giống 100%).

- http://example.com (trang này là tên miền được đặt riêng, nên sẽ không bao giờ được bán lại cho bên khác, tuy vậy không có lời hứa về HSTS hoặc HTTP only).

- http://perdu.com (in FR).

- http://purple.com (K.O.; Hiện đã bị bán cho một hãng kinh doanh nệm).

_Source_ https://superuser.com/questions/1490795/why-does-neverssl-com-redirects-you-to-a-random-subdomain
