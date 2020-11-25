# IX2105 Config

自宅用なので簡潔に記しておく。

https://jpn.nec.com/univerge/ix/Manual/index.html?#crm コマンドリファレンスから引用

### 使用機器 IX Series IX2105  
### Software, Version 10.2.16, 

## 特権モードに入る
```
Router# enable-config #enでも入れる
```
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

## sshの有効化
telnetも設定できるが今回はsshも使えるので割愛
```
IX2105(config)# ssh-server ip enable #このコマンドだけなら追加したユーザー名とパスワードでログインできる
#やってもやらなくてもよい
IX2105(config)# ssh-server ip access-list 'ACCESSLIST-NAME' #アクセスリストを使ってsshできる端末を絞る
IX2105(config)# ssh-server ip port PORT　'number' #外部に公開するときは一応ルーター側もポート番号を変更しておく。
```
## web-serverの有効化
```
IX2105(config)# http-server ip enable
IX2105(config)# http-server username 'hoge' secret-password 'hogehoge' #secret passwprdで暗号化できる。
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

## NetMeisterの設定
登録しておくと何かと楽である。
```
nm ip enable 
nm account 'GROUP ID' password secret 'password'
nm sitename 'SITENAME' #拠点名
   
```
## PPPプロファイルの設定
```
IX2105(config)# ppp profile 'hoge' 
  IX2105(config-ppp-hoge)# authentication myname 'User' 
  IX2105(config-ppp-hoge)# authentication password 'user' 'Password'
```

## LANの設定
```
interface GigaEthernet1.0 
  ip address 192.168.0.1/24
  ip proxy-arp
  ip dhcp binding lan1 #先ほど設定したDHCPプロファイルをマウント
  no shutdown #portの有効化
```
## インターネット接続 ipv4(PPPoE)
```
IX2105(config)# interface GigaEthernet0.1 #サブインターフェース0.1に今回は設定
  IX2105(config-GigaEthernet0.1)# encapsulation pppoe #データリンクの種類　今回はpppoe
  IX2105(config-GigaEthernet0.1)# auto-connect #再接続の設定
  IX2105(config-GigaEthernet0.1)# ppp binding 'Provider-name' #ppp profileの割当て
  IX2105(config-GigaEthernet0.1)# ip address ipcp #ipcpによるアドレス割当の有効化
  IX2105(config-GigaEthernet0.1)# ip tcp adjust-mss auto #TCP-MSS値調整機能の有効化
  IX2105(config-GigaEthernet0.1)# ip napt enable #naptの有効化
  IX2105(config-GigaEthernet0.1)# ip napt static GigaEthernet0.1 50 #ESP
  IX2105(config-GigaEthernet0.1)# ip napt static GigaEthernet0.1 udp 500 #IKE
  IX2105(config-GigaEthernet0.1)# ip napt static GigaEthernet0.1 udp 4500 #NAT-T
  IX2105(config-GigaEthernet0.1)# no shutdown #portの有効化
```
## static routeの設定
```
ip route default GigaEthernet0.1 
```

OSPF,ddns,access-list,VPN,syslog,SNMP,IPv6などの設定はしているが今回は最低限使える感じでの設定なので後日書くかもしれないです。

