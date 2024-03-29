# Printer driver

## Canon LBP2900

Install dependencies:

- `cups` (enable + start cups.service)
- `libglade` (AUR)
- `lib32-libxml2` (`multilib` repo)

Then install the driver from AUR: https://aur.archlinux.org/packages/capt-src. As this was written, its version is 2.71.

Post installation messages:

    >>> Installation:
    >>>
    >>> 1) Before doing anything, be sure to add your user to the lp group:
    >>> eg.
    >>> gpasswd -a your_user lp
    >>> and then reboot, or relogin
    >>>
    >>> 2) Connect the printer to your computer, turn it on and start CUPS, or restart it if it was already running
    >>> eg.
    >>> systemctl restart cups.service
    >>>
    >>> 3) /usr/bin/lpadmin -p <name> -m <corresponding ppd> -v ccp://localhost:59687 -E 
    >>> eg.
    >>> /usr/bin/lpadmin -p LBP2900 -m CNCUPSLBP2900CAPTK.ppd -v ccp://localhost:59687 -E 
    >>> (you can find ppds in the /usr/share/cups/model/ directory)
    >>>
    >>> 4) 
    >>>  a) For a locally connected printer (USB / Parallel port), check the name of 
    >>>     the device, udev created for you.
    >>> eg. /dev/usb/lp0
    >>> and run: /usr/bin/ccpdadmin -p <name> -o udev_device
    >>> eg.
    >>> /usr/bin/ccpdadmin -p LBP2900 -o /dev/usb/lp0
    >>> (it should show a table with the new printer)
    >>> 
    >>>  b) For a network printer:
    >>> /usr/bin/ccpdadmin -p <Printer name> -o net:<IP address>
    >>> eg. /usr/bin/ccpdadmin -p LBP2900 -o net:192.168.0.10
    >>>
    >>> 5) systemctl start ccpd.service (using systemd)
    >>>
    >>> 6) Check you have two instances of ccpd in memory, then run captstatusui, check it's 
    >>>  telling you it's ready to print and the printer should work.
    >>>  eg. ps awx | grep ccpd
    >>>  or using systemd: systemctl status ccpd.service
    >>>  For captstatusui: /usr/bin/captstatusui -P LBP2900
    >>>
    >>> 7) Make sure cupsd and ccpd are running at boot
    >>> eg.
    >>> systemctl enable ccpd.service
    >>>

Add user to `lp` group:

    # usermod -a -G lp admin
    
Restart cups service

    # systemctl restart cups.service
    
Check ppds

    $ ll /usr/share/cups/model/ | grep 2900
    
Then add new printer

    $ lpadmin -p LBP2900 -m CNCUPSLBP2900CAPTK.ppd -v ccp://localhost:59687 -E
    
Check device

    $ ll /dev/usb
    
Then add print from the device

    # ccpdadmin -p LBP2900 -o /dev/usb/lp0
    
Start ccpd.service

    # systemctl start ccpd.service
    
Tested on LBP2900.
