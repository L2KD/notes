openvpn docker

chon -d -N để sử dụng NAT & bỏ cái redirect all via openvpn


trên conf, thêm vào

    push "route 192.168.1.9 255.255.255.255"

lịnh này sẽ push cái cấu hình đó vào client mỗi khi connected

route sẽ điều hết traffic vào ip 192.168.1.9/32 về cái gateway mặc định, tbd


AES-256-CBC
