# Làm sao để kết nối idevices trên arch linux mà không cần tới iTune?

## Log

Cài packages:

- ifuse
- libimobiledevice
- gvfs-afc
- gvfs-gphoto2
- thunar

Để connect (mount), dùng ifuse:

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
