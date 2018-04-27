openvpn docker

## Truy cập vào container

    docker run --rm -it --rm kylemanna/openvpn bash

hoặc

    docker-compose --rm openvpn bash

## Generate config cho server

    docker-compose --rm openvpn ovpn_genconfig

Thông thường container sẽ nhớ cấu hình cũ và thay đổi dựa trên các option cung cấp, nên không cần phải cung cấp tất cả option cùng một command.

## Cấu hình bỏ Redirect all traffic to VPN gateway

Khi gen config cho server, thêm option `-d` và `-N` để sử dụng NAT & bỏ cái redirect all traffic via openvpn


Thêm dòng sau vào file conf

    push "route 192.168.1.9 255.255.255.255"

hoặc

    docker-compose run --rm openvpn ovpn_genconfig -N -d -e 'push "route 192.168.1.9 255.255.255.255"'

lịnh này sẽ push cái cấu hình đó vào client mỗi khi connected

route sẽ điều hết traffic vào ip 192.168.1.9/32 về cái gateway mặc định, tbd


AES-256-CBC


## Cấu hình sử dụng OTP cho người dùng

1. Chọn cipher khác

    Truy cập vào container, list

Cấu hình cho server
