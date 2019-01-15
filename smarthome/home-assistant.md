# Home assistant learning log

1.  Installation

- Download from the github repo the ready-to-be-burnt image. There are a wide range of available images.
- Use etcher.io to burn the just downloaded image.
- Plug the card to a PC, mount the hass-boot (or something like that). Create a CONFIG folder, then network, then touch a `my-network` file. Bref: `CONFIG/network/my-network`.
- Follow the guide: https://github.com/home-assistant/hassos/blob/dev/Documentation/network.md
- The wifi and static ip network should be configured:

      [connection]
      id=hassos-network
      uuid=72111c67-4a5d-4d5c-925e-f8ee26efb3c3
      type=802-11-wireless

      [802-11-wireless]
      mode=infrastructure
      ssid=MY_SSID
      # Uncomment below if your SSID is not broadcasted
      #hidden=true

      [802-11-wireless-security]
      auth-alg=open
      key-mgmt=wpa-psk
      psk=MY_WLAN_SECRET_KEY

      [ipv4]
      method=manual
      address=192.168.1.111/24,192.168.1.1
      dns=8.8.8.8;8.8.4.4;

      [ipv6]
      addr-gen-mode=stable-privacy
      method=auto

- Insert sd card to the rpi.
- The rpi should boot up and provide an http access from http://the-rpi-ip:8123
- This will update the hass OS (in about 20min, as the banner says)

2. First-time config

- Provide some initial user information: Fullname, username, password...

- Enable
