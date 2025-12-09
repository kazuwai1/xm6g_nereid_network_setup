# XM6 TypeG の Nereid のLAN機能を設定してみる(作成中)  
## やりたいこと  
・XM6 TypeG上で動くx68k環境でLANを使えるようにする手順を記録に残す  
  
## 必要なもの(2025.12.08現在)
・Windows用のTAPドライバ  
　→ OpenVPNをインストールするか、OpenVPNのTAPドライバを単独でインストールする  
 　　参考) https://www.choge-blog.com/programming/windows-tapdevice/  
・計測技研のネットワークソフト(TCPPACKA)  
　→ こちらでダウンロードできます http://retropc.net/x68000/software/internet/kg/tcppacka/  
・Neptune-X / Nereid 用のネットワークドライバ  
　→ etherL12.sys : http://retropc.net/x68000/software/hardware/nereid/   
　→ etherL12wa.sys : https://github.com/yunkya2/etherL12  
　　※"etherL12wa.sys"はXM6 TypeG(Ver3.37以前)＋X68030エミュレーション環境専用  
  
## やっておくこと  
・XM6 TypeG上でX68000もしくはX68030環境を動くようにしておく  
  
## 実際の作業  
### WindowsにTAPドライバをインストールしてネットワークアダプタ名をリネームする  
　参考) https://www.choge-blog.com/programming/windows-tapdevice/  
### XM6 TypeGの設定画面でNereidを有効化して、TAPドライバ名を設定する  
　こんな感じで赤丸の個所を適宜設定する  
 ![設定ﾀﾞｲｱﾛｸﾞ](xm6g_setting.png)  
### Windows側のLAN環境を確認する  
・ipconfigでIPアドレスやネットマスクを調べる  
・DHCP環境の場合、DHCPに割り当てられない空きアドレスも調べておく  
### XM6 TypeG上のHuman68k環境を設定する  
#### CONFIG.SYS  
・"PROCESS=”行を設定する  
　→TCPPACKAのドキュメントには "PROCESS=2 10 10"、Nereidのドキュメントには"PROCESS=32 10 100"とあります。とりあえずどちらでも動きます。  
・etherL12.sys or etherL12.sys を組み込む  
#### ifconfigとinetdconf  
・ループバック(localhost)を使えるようにする： ifconfig lp0 up
・ローカルネットワークの空きアドレスを en0 に設定する  
　例) ifconfig en0 192.168.10.100 netmask 255.255.255.0  
　　→この例はウチのWifiルータにぶら下がっているLANが192.168.10.xxxで、DHCPのアドレス割り当て範囲が101-200ということで空きの100を設定した例です  
　　→netmaskはホストPCと同じでいいです(ipconfigで確認できます)  
・inetdconfでゲートウェイアドレスとdnsを指定しておきます  
　例) inetdconf +router 192.168.10.1 +dns 192.168.10.1  
　　→この例は我が家のWifiルータにぶら下がっているLANの例です  
### Windows上のネットワークアダプタのブリッジ設定  
　こんな感じで、コントロールキーを押しながらTAPドライバとホストPCのLANアダプタを選択して右クリックで「ブリッジ接続」を選択します。  
![ブリッジ設定例](./bridge.png)  
 
## 確認  
・Windows側からHuman68kへpingを打ってみる  
・Human68k側から適当なサイトへpingを打ってみる  
  
