Bài này dịch từ [bài này](https://web.archive.org/web/20150430223035/http://archlinux.me/brain0/2010/02/13/early-userspace-in-arch-linux/).

## Booting Linux Systems

Hồi xưa, khởi động 1 hệ thống linux đơn giản như sau: Bootloader sẽ load kernel. Kernel sau đó sẽ được giải nén (extracted) và init phần cứng. Kernel tiếp theo init cụm điều khiển (controller) hard disk (tức là đĩa cứng), tìm thấy dĩa cứng, thấy root file system, mount nó và chạy `/sbin/init`.

Hiện nay, do có quá xá controllers (vì sự phát triển quá nhanh của công nghệ), các distro luôn cố tỏ ra thân thiện và support hết chúng nó. Nên các dev đã dồn hết công sức ngày đêm để build 1 cục kernel nặng vài MB và hỗ trợ tận răng (nguyên văn bài gốc là tới cái bồn rửa chén cũng được hỗ trợ). À phần ngày đêm là mình nói hơi quá lên thôi chứ không phải nhé.

Tưởng tượng 1 người dùng có 2 SATA controllers, 3 IDE controllers, 7 cái dĩa cứng, 3 cái USB drives và bạn cứ kể thêm đi. Lúc này kernel sẽ detect những thứ trên **1 cách bất đồng bộ** -- và đâu là cái root FS? Nó ở cái đĩa thứ nhất, hay 2, hay 4? Mà cái đĩa nào là thứ nhất cái đã?

Rồi nếu tôi muốn mount root fs trên 1 cái VG LVM bên trong 1 cái container được mã hóa, lại nằm trong 1 cái array chạy RAID thì sao?

Cuộc đời màu hồng giờ đã chuyển sang thâm. Kernel nó rất khôn rằng nó sẽ giả bộ ngu -- hay đơn giản hơn là nó không quan tâm về các nhu cầu của bạn, đặc biệt là giờ nó bắt đầu mập mạp (vì nó phải ăn hết mấy cái driver trên thế giới này).

Giải pháp cho vấn đề này là chuyển hết những thứ đó vào userspace, handling hardware detection, cài mọi thứ rắc rối mà người dùng muốn, mount root fs và chạy luôn cái /sbin/init. "Ồ, làm sao tôi chạy được mấy ứng dụng userspace khi mà root fs chưa được mount?".

Trả lời: "It's magic!"

## initramfs là gì?

Ok, thực ra câu trả lời không phải là magic. Mà là initramfs. Mỗi hệ thống linux có 1 cái ramfs file system (wow, there are lots of fs here you know), được mount và được gọi là `ramfs`. Có thể bạn sẽ không thấy nó bởi vì nó được mount chồng lên bằng cái file system thực khác rồi (/root).

Tuy vậy, kernel có 1 thứ gọi là cpio được nén lại và chèn vào nó, để rồi nó được giải nén thành cái rootfs sau khi khởi động. Xịn hơn là chính bạn cũng có thể nén cái cpio đó lại rồi chèn vào kernel để nó extract ra lúc boot.

Trước khi kernel chạy cái mã `init` đã lỗi thời, nó sẽ check xem trong cái rootfs có chứa file nào tên là `/init` không, nếu có thì nó sẽ bỏ qua cái phương thức truyền thống (mount/init code) mà chạy luôn cái `/init` đó. 

Phần `/init` này sẽ đảm nhận mọi thứ rắc rối mà kernel nó nghĩ là rắc rối. Bằng cách này, chúng ta có thể build 1 cái kernel không cần support hết các thể loại controller ổ cứng hay fs gì cả, thay vì vậy nó nằm trong các module của cái initramfs. (Hiện tại Arch Linux default kernel đang làm như vậy).
