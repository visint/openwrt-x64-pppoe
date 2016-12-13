# openwrt-x64-pppoe
ipk 文件可以不分平台安装
标准配置是
opkg updateopkg install rp-pppoe-server

pppoe 服务器启动命令
pppoe-server -k -I br-lan -L 10.10.10.1 -R 10.10.10.100-200


root@OpenWrt:~# vim /etc/ppp/chap-secrets
#USERNAME  PROVIDER  PASSWORD  IPADDRESS
guojing * 123456 *
huangrong * 123456 *
yangguo * 123456 *
xiaolongnv * 123456 *
zhaomin * 123456 *
zhangwuji * 123456 *
wifitest017 * 834645 *
wifitest019 * 872364 *
wifitest020 * 345943 *
wifitest021 * 287346 *
wifitest022 * 439857 *
wifitest023 * 283759 *
wifitest024 * 394579 *
wifitest025 * 893475 *
wifitest003 * 237493 *
guojing@pifii.com * 123456 *
huangrong@pifii.com * 123456 *
yangguo@pifii.com * 123456 *
xiaolongnv@pifii.com * 123456 *
zhaomin@pifii.com * 123456 *
zhangwuji@pifii.com * 123456 *



root@OpenWrt:~# vim /etc/ppp/pppoe-server-options
# PPP options for the PPPoE server
# LIC: GPL
require-pap
require-chap
login
lcp-echo-interval 10
lcp-echo-failure 2
netmask 255.255.255.255
ms-dns 114.114.114.114
logfile /tmp/pppoe.log
~


root@OpenWrt:~# vim /etc/init.d/pppoe-server
#!/bin/sh /etc/rc.common
# Copyright (C) 2006-2011 OpenWrt.org

START=50

DEFAULT=/etc/default/pppoe-server

start() {
        #[ -f $DEFAULT ] && . $DEFAULT
        #service_start /usr/sbin/pppoe-server $OPTIONS
        pppoe-server -k -I br-lan -L 10.10.10.1 -R 10.10.10.100-200
        iptables -t nat -A POSTROUTING -s 10.10.10.0/24 -j MASQUERADE

        iptables -I FORWARD -i ppp0 -j ACCEPT
        iptables -I FORWARD -o ppp0 -j ACCEPT
        iptables -I FORWARD -i ppp1 -j ACCEPT
        iptables -I FORWARD -o ppp1 -j ACCEPT
        iptables -I FORWARD -i ppp2 -j ACCEPT
        iptables -I FORWARD -o ppp2 -j ACCEPT
        iptables -I FORWARD -i ppp3 -j ACCEPT
        iptables -I FORWARD -o ppp3 -j ACCEPT
}

stop() {
        service_stop /usr/sbin/pppoe-server
        iptables -t nat -D POSTROUTING -s 10.10.10.0/24 -j MASQUERADE
}





