Bài này dịch từ [bài này](https://web.archive.org/web/20150430223035/http://archlinux.me/brain0/2010/02/13/early-userspace-in-arch-linux/).

## Booting Linux Systems

Hồi xưa, khởi động 1 hệ thống linux đơn giản như sau: Bootloader load kernel. Kernel sẽ được giải nén (extracted) và init phần cứng. Kernel init cụm điều khiển (controller) hard disk (tức là đĩa cứng), tìm thấy dĩa cứng, thấy root file system, mount nó và chạy `/sbin/init`.

Hiện nay, do có quá xá controllers (vì sự phát triển quá nhanh của công nghệ)