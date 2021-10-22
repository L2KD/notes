# X11 tweaks

## Multi monitors

Edit file `/etc/X11/xorg.conf.d/51-main-monitor.conf`

    Section "Monitor"
        Identifier "LVDS1"
        Option "PreferredMode" "1366x768"
        Option "Primary" "1"
    EndSection

And file `52-multi-monitors.conf`

    Section "Monitor"
        Identifier "HDMI1"
        Option "Rotate" "left"
        Option "PreferredMode" "1920x1080"
        Option "RightOf" "LVDS1"
    EndSection

This will extend a rotated monitor on the right of main screen.

## Tear free graphic

File `/etc/X11/xorg.conf.d/20-intel.conf`

    Section "Device"
    Identifier "Intel Graphics"
    Driver "intel"
    Option "TearFree" "true"
    EndSection

## Remake touchpad on Dell laptop (enable tapping, and tap map left, right, middle = 1 finger, 2 fingers, 3 fingers)

    Section "InputClass"
        Identifier "SynPS/2 Synaptics TouchPad"
        Driver "libinput"
        MatchIsTouchpad "on"
        Option "Tapping" "on"
        Option "TappingButtonMap" "lrm"
    EndSection
    