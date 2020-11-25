# IX2105 Config

自宅用なので簡潔に記しておく。自戒用
### 使用機器 IX Series IX2105  
### Software, Version 10.2.16, 
 
## ホストネームの設定
```IX2015
Router(config)# hostname ix2105 
```

## タイムゾーンの設定
```
IX2105(config)# timezone +09 00 #Japan
```

## ユーザーの追加と権限付与
```
IX2105(config)# username 'hoge' password hash 'hogehoge' administrator #hashで暗号化できる
```

## DHCPの設定
```
IX2105(config)# ip dhcp enable
IX2105(config)# ip dhcp profile lan1 #プロファイルの設定
IX2105(config-dhcp-lan1)# dns-server 192.168.1.1dns-server 192.168.0.1
```
## ログ情報の設定
```
IX2105(config)# logging buffered '4096～ 8388608 ' #logging buffer size settings
IX2105(config)# logging subsystem all warn #All subsystems message enable Warning conditions
IX2105(config)# logging timestamp datetime #logging add message datetime
```

## ntpの設定
SNTPもあるが、今回はNTPを使用
```
IX2105(config)# ntp ip enable #ntpの有効化 
IX2105(config)# ntp interval 3600 #NTPの時刻同期の間隔
IX2105(config)# ntp server 133.243.238.163 priority 10 #NTPサーバーはNICTから参照
IX2105(config)# ntp master #ローカルNW内のNTPサーバーをもたせた
```

## PPPプロファイルの設定
```
IX2105(config)# ppp profile 'hoge' #
  authentication myname 
  authentication password 
```
## インターネット接続 ipv4(PPPoE)
```
interface GigaEthernet0.1 #サブインターフェース0.1に今回は設定
  encapsulation pppoe #データリンクの種類　今回はpppoe
  auto-connect #再接続の設定
  ppp binding 'Provider-name' #ppp profileの割当て
  ip address ipcp #ipcpによるアドレス割当の有効化
  ip tcp adjust-mss auto #TCP-MSS値調整機能の有効化
  ip napt enable #naptの有効化
  ip napt static GigaEthernet0.1 50 #ESP
  ip napt static GigaEthernet0.1 udp 500 #IKE
  ip napt static GigaEthernet0.1 udp 4500 #NAT-T
  no shutdown
```
## static routeの設定
```
ip route default GigaEthernet0.1 
```

##執筆中

