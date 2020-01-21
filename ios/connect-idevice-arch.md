# Làm sao để kết nối idevices trên arch linux mà không cần tới iTune

Nói sơ qua về App trên ios. Mỗi app sẽ được phân cho 1 vùng nhớ riêng (gọi nôm na là House, tức là cái nhà). Các app đó có thể dùng House của mình để lưu dữ liệu mà không cần quan tâm dữ liệu của mình có bị ảnh hưởng tới các app không, và ngược lại, cũng không sợ các app khác làm gì tới dữ liệu của mình.

Như vậy về góc độ bảo mật có thể nói đây là 1 design ok. Nhưng design thì ngược lại với thuận tiện.

## Các packages có thể cần cài

- ifuse
- libimobiledevice
- gvfs-afc
- gvfs-gphoto2
- thunar

## Mục đích

Mục đích chính là để copy film vào VLC app (kèm sub), thông qua cable. Vì VLC có cung cấp 1 cách copy via wifi nhưng do tốc độ chậm nên muốn dùng usb.

Trên mạng toàn chỉ cách copy photo đã chụp (nằm ở `/private/var/mobile/Media`) vô PC. Chuyện này có thê làm bằng thunar.

1. Mở Thunar,
2. Ghim ipad.
3. Chọn Trust trên ipad.
4. Đừng pair gì cả. Trong thunar chọn ipad. Sẽ mount được thư mục DCIM.

## Để connect

1. Dùng ifuse: (mount)

    1. Pair

            $ idevicepair pair

    2. Mount

            sudo ifuse --documents org.videolan.vlc-ios /run/media/vlc

2. AFC

AFC (Apple File Conduit) is a service that runs on every iPhone / iPod, which iTunes uses to exchange files with the device. It is jailed to the directory /private/var/mobile/Media, which is on the second (non-OS) partition.

Khi ghim ipad vào máy tính (có trust không?), usbmux nó sẽ tự trao đổi với service được chạy trong ipad.

Trên thunar gõ `afc://uuid` của ipad sẽ ra được chỗ `/private/var/mobile/Media`.

Tuy vậy những thứ của VLC nó lại nằm bên trong `/private/var/mobile/Containers/Data/Application/F3D95DB8-1279-471B-B85D-4F5B228A968F/Documents/Inbox/Hitting.the.Apex.2015.1080p.BluRay.H264.AAC-RARBG.mp4`.

## House arrest

Hay còn gọi là iTunes Document Sharing. Thực ra nó là 1 cái vỏ bọc các app lại để chúng không vượt ra khỏi cái nhà của chúng (house). Tất cả những thứ của cái app (data...) nó sẽ nằm riêng biệt với nhau.

Như vậy, theo bên trên thì app VLC đó có thể có cái Documents bên trong

```shell
/private/var/mobile/Containers/Data/Application/F3D95DB8-1279-471B-B85D-4F5B228A968F/
```

Trong đó sẽ có

```
Documents/Inbox/Hitting.the.Apex.2015.1080p.BluRay.H264.AAC-RARBG.mp4
```

Chứng mình:

```
‣ idevicepair pair
SUCCESS: Paired with device ddf10c8a9ab9e4843370addd5202c9404fe7e26b

sudo ifuse --documents org.videolan.vlc-ios /run/media/vlc

ls

.
..
/Inbox

cd Inbox
ls

.
..
Aladdin.mp4
Hitting.the.Apex.2015.1080p.BluRay.H264.AAC-RARBG.mp4
```


----

Toàn những tài liệu cách đây cả thập kỷ.

Mình không đùa đâu. 2010 và 2011
