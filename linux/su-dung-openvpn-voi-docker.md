openvpn docker

Image được sử dụng: [kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn)

## Sử dụng docker-compose

    version: '2'
    services:
      openvpn:
        cap_add:
         - NET_ADMIN
        image: kylemanna/openvpn
        container_name: openvpn
        ports:
         - "1194:1194/udp"
        restart: always
        volumes:
         - ./openvpn-data/conf:/etc/openvpn

Init các thứ

     docker-compose run --rm openvpn ovpn_genconfig -u udp://VPN.SERVERNAME.COM
     docker-compose run --rm openvpn ovpn_initpki

## Truy cập vào container

    docker run --rm -it --rm kylemanna/openvpn bash

hoặc

    docker-compose --rm openvpn bash

## Generate config cho server

    docker-compose --rm openvpn ovpn_genconfig

Thông thường container sẽ nhớ cấu hình cũ và thay đổi dựa trên các option cung cấp, nên không cần phải cung cấp tất cả option cùng một command.

## Tạo clients

Tạo clients cert

    export CLIENTNAME="your_client_name"
    # with a passphrase (recommended)
    docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME
    # without a passphrase (not recommended)
    docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME nopass

Lấy file client

    docker-compose run --rm openvpn ovpn_getclient $CLIENTNAME > $CLIENTNAME.ovpn

## Cấu hình bỏ Redirect all traffic to VPN gateway

Khi gen config cho server, thêm option `-d` và `-N` để sử dụng NAT & bỏ cái redirect all traffic via openvpn


Thêm dòng sau vào file conf để chuyển hết traffic vào ip 192.168.1.9/32 vào VPN, tất cả traffic còn lại qua gateway đi internet

    push "route 192.168.1.9 255.255.255.255"

hoặc

    docker-compose run --rm openvpn ovpn_genconfig -N -d -e 'push "route 192.168.1.9 255.255.255.255"'

Cấu hình này sẽ push cái cấu hình đó vào client mỗi khi client connected

## Cấu hình sử dụng OTP cho người dùng

1. Chọn cipher khác

    Truy cập vào container, list các supported ciphers

        openvpn --show-ciphers

    Chọn một ciphers có tính bảo mật cao, vd AES-256-CBC

2. Cấu hình cho server

        docker-compose run --rm openvpn ovpn_genconfig -2 -C "AES-256-CBC"

    -2 để xác định sử dụng OTP   
    -C để xác định ciphers sẽ sử dụng là "AES-256-CBC"
