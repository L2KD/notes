On the terminal,

    telnet ip_of_switch

Enter password

Go to exec mode

    en(able)

Go to config terminal mode

    config t

Monitoring port:

From configuration mode, type

    logging monitor info
    end

then from enable mode type

    term[inal] mon[itor]
    
this will send console messages to your vty session

turn it off

    term no mon
