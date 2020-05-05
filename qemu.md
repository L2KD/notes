# Log

Installed:

- qemu
- virt-manager
- libvirt
- ebtables
- dnsmasq
- spice-vdagent

Start services:

- libvirtd
- virtlogd

Add user to libvirt group

    # usermod -a -G libvirt `whoami`

https://getlabsdone.com/10-easy-steps-to-install-windows-10-on-linux-kvm/
