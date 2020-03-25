# Wireguard

New trend VPN

Unofficial docs: https://letswp.io/cloudflare-as-dynamic-dns-raspberry-pi/

## Installation

Check the official doc

## Key gen

## Config

On 'server', config:

```
[Interface]
PrivateKey = YOUR_PRIVATE_KEY
ListenPort = 51280
SaveConfig = false
Address = 10.0.0.1/24
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

# Peer 1
[Peer]
PublicKey = PUBLIC_KEY_ON_ANDROID
AllowedIPs = 10.0.0.2/32

# Peer 2
[Peer]
PublicKey = PUBLIC_KEY_ON_ANDROID
AllowedIPs = 10.0.0.3/32
...
```

On 'client':

```

[Interface]
PrivateKey = YOUR_PRIVATE_KEY
Address = 10.0.0.2/24
Endpoint = your-domain.com:51820
AllowedIPs = 0.0.0.0/0 # This allow all IPs
```

## Run cmd

Start things:

```
sudo wg-quick up wg0
                  ^
                  |
                  This must be the same with /etc/wireguard/wg0.conf
```

The file must be chmod to 600 because it contains private key

Stop things

```
sudo wg-quick down wg0
```

Or the hard way

```
ip link add ...
ip addr add ...
...
```

Ok this is too complicated, so I recommend to stick with the `wg-quick`, because, as its name tells, it's quick.

Enabling service

```
sudo systemctl enable wg-quick@wg0
```

## Test

- Ping other peer in the network after connect.
- Get the public IP.
  ```
  curl -s http://ipv4.icanhazip.com
  ```

---

# Pi Hole

(almost) the black hole of ads

After installation, access web dashboard

```
localhost/admin
```

## Playing with Wireguard

**Note**: Enable this if you want to route all traffic from wg to pihole

Looking for a configuration like, listen on other interface. Since wg traffic is on the `wg0` interface, and the pihole is from `lan0` or `wlan0`...

---

# Use Cloudflare ddns

- Set A type DNS in the Cloudflare dashboard
- Use cron script to update that sub domain to current public IP of the network (maintain by Pi)

View more at https://letswp.io/cloudflare-as-dynamic-dns-raspberry-pi/
