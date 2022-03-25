MR. 沙先生沒有標準的 SOP，但是卻有不怎麼樣的經驗談。
Mr. 礦物男人
作品集
Facebook
關於我
Mac 用 SSH 建立 iOS 的 USB 通道 usbmuxd
2015-06-30 IOS, MACOS
每次在用 Mac 連線的時候都要透過 WiFi 來 SSH iOS 設備，必須要使用當地的 Wifi 不然就是要過熱點，明明就可以接 USB 為什麼要脫褲子放屁從 WiFi 連呢？

 

在查詢了一下果然有人直接用 USB 來在 iOS 作業

 

就是 usbmuxd 這個工具，目前出到 usbmuxd 1.1.0 版本，但因為使用方式的不同，我還是喜歡 1.0.6

 

1.1.0 的版本是必須 compile 才能使用，但 1.0.6 解壓出來就有現成的 python script 可以用，非常方便

usbmuxd 下載


 

 

本篇紀錄 1.0.6 的版本使用

 

Step.1 下載 usbmuxd 1.0.6 版本

本站 usbmuxd-1.0.6 備用載點

$ curl -O http://pkgs.fedoraproject.org/repo/pkgs/usbmuxd/usbmuxd-1.0.6.tar.bz2/c8909cfd9253d8d1a5e26f2ff7e5908b/usbmuxd-1.0.6.tar.bz2
$ tar jxvf usbmuxd-1.0.6
 

Step.2 直接執行 script 來連接 iOS 裝置，記得 iOS 必須安裝 OpenSSH

$ cd usbmuxd-1.0.6/python-client
$ chmod +x tcprelay.py
$ ./tcprelay.py -t 22:2222

Forwarding local port 2222 to remote port 22
 

Forwarding local port 2222 to remote port 22 代表通訊完成！透過本機 2222 port Tunnel 到 iOS 裝置的 22 port

 

Step.3 再來用 Ctrl + Z 放到背景，用 bg 來 Running

$ jobs
[1]+  Stopped                 ./tcprelay.py -t 22:2222

$ bg %1
[1]+  Running                 ./tcprelay.py -t 22:2222 &
 

 

Step.4 測試 SSH 連線

$ ssh -p 2222 root@localhost

Incoming connection to 2222
Waiting for devices...
Connecting to device <MuxDevice: ID 1 ProdID 23a8 Serial '90080ac6119eb4586e5737546' Location 0x12300000>
Connection established, relaying data
