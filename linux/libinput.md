grep -e "Using input driver 'libinput'" /path/to/Xorg.0.log

install libinput

xorg-xinput

    # libinput list-devices

/etc/X11/xorg.conf.d/libinput.conf

remap touch pad for dell

Cài libinput

Edit file 

/etc/X11/xorg.conf.d/70-touchpad.conf

    Section "InputClass"
        Identifier "SynPS/2 Synaptics TouchPad"
        Driver "libinput"
        MatchIsTouchpad "on"
        Option "Tapping" "on"
        Option "TappingButtonMap" "lrm"
    EndSection
