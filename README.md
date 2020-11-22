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
username 'hoge' password hash 'hogehoge' administrator
```

## ntpの設定
SNTPもあるが、今回はNTPを使用
```
ntp ip enable #ntpの有効化 
ntp interval 3600 #NTPの時刻同期の間隔
ntp server 133.243.238.163 priority 10 #NTPサーバーはNICTから参照
ntp master #ローカルNW内のNTPサーバーをもたせた
```

##
##執筆中

