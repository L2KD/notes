# dns issues

Theo thứ tự sau linux sẽ dịch domain name thành ip:

1. files hosts (thường ở /etc/hosts)
2. các dns
3. mymachines
4. myhostname
5. resolve

 Các thử tự này được quy định tại file
 
     /etc/nsswitch.conf
     
vd

```
# See nsswitch.conf(5) for details.                                                                                               passwd: files mymachines systemd                                 group: files mymachines systemd                                  shadow: files                                                                                                                     publickey: files                                                                                                                  hosts: files dns mymachines myhostname resolve [!UNAVAIL=return] networks: files                                                                                                                   protocols: files                                                 services: files                                                  ethers: files                                                    rpc: files                                                                                                                        netgroup: files
```

Thay đổi hosts để lấy thứ tự khác.

