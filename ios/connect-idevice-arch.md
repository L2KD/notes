# Làm sao để kết nối idevices trên arch linux mà không cần tới iTune?

## Log

Cài packages:

- ifuse
- libimobiledevice
- gvfs-afc
- gvfs-gphoto2
- thunar

## Mục đích

Trên mạng toàn chỉ cách copy photo đã chụp (nằm ở `/private/var/mobile/Media`) vô PC. Chuyện này có thê làm bằng thunar.

1. Mở Thunar,
2. Ghim ipad.
3. Chọn Trust trên ipad.
4. Đừng pair gì cả. Trong thunar chọn ipad. Sẽ mount được thư mục DCIM.

Mục đích chính là để copy film vào VLC app (kèm sub), thông qua cable. Vì VLC có cung cấp 1 cách copy via wifi nhưng do tốc độ chậm nên muốn dùng usb.



## Để connect (mount), 

1. Dùng ifuse:

        1. pair

        $ idevicepair pair

        2. Mount

                sudo ifuse --documents org.videolan.vlc-ios /run/media/vlc

House arrest là cái gì vậy ông thần?
 iTunes Document Sharing

Toàn những tài liệu cách đây cả thập kỷ.

Mình không đùa đâu. 2010 và 2011

## AFC

AFC (Apple File Conduit) is a service that runs on every iPhone / iPod, which iTunes uses to exchange files with the device. It is jailed to the directory /private/var/mobile/Media, which is on the second (non-OS) partition.

Tren thunar gõ afc://uuid của ipad sẽ ra được chỗ `/private/var/mobile/Media`

## VLC

vlc sharing via wifi 

    http://192.168.99.117/download//private/var/mobile/Containers/Data/Application/F3D95DB8-1279-471B-B85D-4F5B228A968F/Documents/Inbox/Hitting.the.Apex.2015.1080p.BluRay.H264.AAC-RARBG.mp4

Như vậy cái app VLC 
