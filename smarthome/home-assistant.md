# Home assistant learning log

1.  **Installation**

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

2. **First-time config**

- Provide some initial user information: Fullname, username, password...

- Enable SSH:

  - Go to addons store (Hamburger > Hass.io > Add-ons store). Then look for SSH server.
  - Install it and provide your ssh public key to the middle of the config:

        {
          "authorized_keys": [
            "ssh-rsa adsad== your-key"
          ],
          "password": ""
        }

  - That would add the public key to the root user on the hassio.
  - Then start the SSH service.
  - Test it by `ssh root@ip`
  - After successfully connecting to the hassio, cd to `/config`, this directory contains all the configuration files of the hassos.

3. **configuration.yml**

The file is structure like:

    component_type:
      - some options

Eg

    device_tracker
      - platform: nmap...

Check the file syntax by Hamburger > Config > Check config

After editing the config, the hass os must be restart for enabling the configuration.

4. **Presence detection**

Declare some zone:

configuration.yml

    # Declaration of some zones
    zone:
      - name: 402
        latitude: 10.354004
        longitude: 106.399291
        icon: mdi:home

Enable some `device_tracker` component by adding this to the end (or somewhere else) of the `configuration.yml`

    # Presence detection component
    device_tracker:
      - platform: nmap_tracker
        hosts: 192.168.1.0/24

Some 'random' devices will appears in the homepage. After the `device_tracker` component is enabled, it will automatically create a `known_devices.yml` file in the same location of the configuration file.

5. Connect to Apple HomeKit

The component will transfer all things in the HA to your Home app

- Add the following code to the config file

      # Config for HomeKit component

      homekit:

- Well, there are a lot more option, those are optional though. Check [here](https://www.home-assistant.io/components/homekit/) for more details. Save it and restart the pi.

- In the home page, there should be a HomeKit card displaying 8-digit pin code.

- Open your Home app in the iDevices.
- Add accessory. Select Home Assistant Bridge. Enter pin code.
