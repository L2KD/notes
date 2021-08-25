Change interface name

Note: When changing the naming scheme, do not forget to update all network-related configuration files and custom systemd unit files to reflect the change.

You can change the device name by defining the name manually with an udev-rule. For example:

/etc/udev/rules.d/10-network.rules

    SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="aa:bb:cc:dd:ee:ff", NAME="net1"
    SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="ff:ee:dd:cc:bb:aa", NAME="net0"

These rules will be applied automatically at boot.

A couple of things to note:

To get the MAC address of each card, use this command: cat /sys/class/net/device_name/address

Make sure to use the lower-case hex values in your udev rules. It does not like upper-case.

See more: https://wiki.archlinux.org/title/Network_configuration#Change_interface_name
