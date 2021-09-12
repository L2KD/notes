# Graphic

To improve tearing issue (fullscreen Netflix is usual)

For intel graphic cards, install the `libva-intel-driver`, `libva-utils`.

Add a file `/etc/X11/xorg.conf.d/20-intel.conf`

    Section "Device"
        Identifier "Intel Graphics"
        Driver "intel"
        Option "TearFree" "true"
    EndSection

Then look for other Firefox config in https://wiki.archlinux.org/title/Firefox#Hardware_video_acceleration

This improve significantly tearing issue, but not totally.