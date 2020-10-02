* https://www.raspberrypi.org/downloads/raspberry-pi-os/ から Raspberry Pi OS (32-bit) Lite をダウンロード
* Etcher を使ってSDカードへ書き込み
* raspberrypiにキーボード、マウス、モニターを接続し起動
* ID:pi, PASS:raspberry
* sudo raspi-config
  * 1. password変更
  * 5. Interface Options -> P2 SSH -> YES
  * 2. Network Options -> N2 Wireless LAN
*  ssh pi@raspberrypi.localで入れるようになる。

  
  
