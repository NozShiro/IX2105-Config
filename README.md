# IX2105 Config

自宅用なので簡潔に記しておく。自戒用
### 使用機器 IX Series IX2105  
### Software, Version 10.2.16, 
 
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
logging buffered '4096～ 8388608 ' #logging buffer size settings
logging subsystem all warn #All subsystems message enable Warning conditions
logging timestamp datetime #logging add message datetime
```

## ユーザーの追加と権限付与
```
username 'hoge' password hash 'hogehoge' administrator #hashで暗号化できる
```

## ntpの設定
SNTPもあるが、今回はNTPを使用
```
ntp ip enable #ntpの有効化 
ntp interval 3600 #NTPの時刻同期の間隔
ntp server 133.243.238.163 priority 10 #NTPサーバーはNICTから参照
ntp master #ローカルNW内のNTPサーバーをもたせた
```

## PPPプロファイルの設定
```
ppp profile 'hoge' #
  authentication myname j03bj0hk@one.ocn.ne.jp
  authentication password j03bj0hk@one.ocn.ne.jp fmmc82
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

