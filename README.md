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
### Windows側のLAN環境を確認する  
・ipconfigでIPアドレスやネットマスクを調べる  
・DHCP環境の場合、DHCPに割り当てられない空きアドレスも調べておく  
### XM6 TypeG上のHuman68k環境を設定する  
#### CONFIG.SYS  
・"PROCESS=”行を設定する  
・etherL12.sys or etherL12.sys を組み込む  
#### ifconfigとinetdconf  
・ローカルネットワークの空きアドレスを en0 に設定する  
・netmaskはWindows機と同じでOK  
### Windows上のネットワークアダプタのブリッジ設定  
 
## 確認  
・Windows側からHuman68kへpingを打ってみる  
・Human68k側から適当なサイトへpingを打ってみる  
  
