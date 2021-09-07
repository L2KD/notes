Cài `iwd`. `iwd` là một cái daemon, cung cấp util như `iwctl` để monitor, control các thứ về wireless networking (wifi). Sau khi cài, enable service.

`iwd` plays well with `systemd-networkd`.

Tạo file /etc/iwd/main.conf 

    [General]
    EnableNetworkConfiguration=true

Để có DHCP khi connect.

Để connect 

    $ iwctl
    > device list
    > station wlan0 scan
    > station wlan0 get-networks
    > station wlan0 connect SSID
    > device wlan0 show
    > station wlan0 show
    > known-networks list

Để cố định network interface, xem các notes đã có.
