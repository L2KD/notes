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

