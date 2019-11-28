After installing/updating vbox new version, you may get the following issue

> the support driver is not installed...

Try:

- Install the dkms

      sudo pacman -S virtualbox-host-dkms

- Reconfig the vbox driver

      sudo vboxreload

- You might have to install the kernel headers: `linux-headers`.
