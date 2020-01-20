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
