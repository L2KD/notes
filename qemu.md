# Log

Installed:

- qemu
- virt-manager
- libvirt

Start services:

- libvirtd
- virtlogd

Add user to libvirt group

    # usermod -a -G libvirt `whoami`
