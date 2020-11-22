# IX2105-Sample-config

自宅用なので簡潔に記しておく。自戒用

 IX Series IX2105 (magellan-sec) Software, Version 10.2.16, RELEASE SOFTWARE
 
## ホストネームの設定
```IX2015
hostname ix2105 
```

## タイムゾーンの設定
```
timezone +09 00 #Japan
```

## ログ情報の設定
```
logging buffered '4096～ 8388608 ' #logging buffer
logging subsystem all warn
logging timestamp datetime
```

##執筆中

!
username shirokus password hash 59d34EAF3aeB94C2A609d4F2be7594@ administrator
!
ntp ip enable
ntp server 133.243.238.163 priority 10
ntp server 210.173.160.27
ntp server 133.243.238.164
ntp master
!
!
ip ufs-cache enable
ip route default GigaEthernet0.1
ip route default 153.205.238.213
ip dhcp enable
ip access-list l2tp-list permit ip src any dest any
ip access-list management permit ip src 192.168.30.0/24 dest any
!       
!       
!       
ike nat-traversal
!       
ike proposal l2tp-ike1 encryption aes-256 hash sha group 1024-bit
ike proposal l2tp-ike2 encryption aes hash sha group 1024-bit
ike proposal l2tp-ike3 encryption 3des hash sha group 1024-bit
!       
ike policy l2tp-ike-policy peer any key shirokus l2tp-ike1,l2tp-ike2,l2tp-ike3
!       
ipsec autokey-proposal l2tp-sec1 esp-aes-256 esp-sha
ipsec autokey-proposal l2tp-sec2 esp-aes esp-sha
ipsec autokey-proposal l2tp-sec3 esp-3des esp-sha
!       
ipsec dynamic-map l2tp-ipsec-map l2tp-list l2tp-sec1,l2tp-sec2,l2tp-sec3
!       
!       
!       
!       
!       
!       
!       
proxy-dns ip enable
!       
!       
ssh-server ip enable
!       
http-server username shirokus secret-password cJZnRPlgsOclvcUw8Br35Ivv
http-server ip enable
!       
!       
ddns enable
!       
nm ip enable
nm account shirokus password secret ZESDJbkA2H3x9SOKoBf2cjvHA94hoVQMIkpzuNryrdRboYu4YWr3uIvv
nm sitename adachi
!       
!       
!       
ppp profile flets-v4
  authentication myname j03bj0hk@one.ocn.ne.jp
  authentication password j03bj0hk@one.ocn.ne.jp fmmc82
!       
ppp profile flets-v6
!       
ppp profile l2tp
  authentication request chap
  authentication password nz-shiroko-bou.com NsRi336
  lcp pfc
  lcp acfc
  ipcp ip-compression
  ipcp provide-remote-dns 8.8.8.8
  ipcp provide-ip-address range 192.168.30.30 192.168.30.50
!       
ip dhcp profile lan1
  dns-server 192.168.30.1
!       
device GigaEthernet0
!       
device GigaEthernet1
!       
interface GigaEthernet0.0
  no ip address
  ip napt static GigaEthernet0.0 50
  ip napt static GigaEthernet0.0 udp 500
  ip napt static GigaEthernet0.0 udp 4500
  no shutdown
!       
interface GigaEthernet1.0
  ip address 192.168.30.1/24
  ip proxy-arp
  ip dhcp binding lan1
  no shutdown
!       
interface GigaEthernet0.1
  encapsulation pppoe
  auto-connect
  ppp binding flets-v4
  ip address ipcp
  ip tcp adjust-mss auto
  ip napt enable
  ip napt static GigaEthernet0.1 50
  ip napt static GigaEthernet0.1 udp 500
  ip napt static GigaEthernet0.1 udp 4500
  no shutdown
!       
interface GigaEthernet1.1
  encapsulation pppoe
  auto-connect
  no ip address
  no shutdown
!       
interface GigaEthernet1.2
  encapsulation pppoe
  auto-connect
  no ip address
  no shutdown
!       
interface Loopback0.0
  no ip address
!       
interface Null0.0
  no ip address
!       
interface Tunnel0.0
  ppp binding l2tp
  tunnel mode l2tp-lns ipsec
  ip unnumbered GigaEthernet1.0
  ip tcp adjust-mss auto
  ipsec policy transport l2tp-ipsec-map
  no shutdown
