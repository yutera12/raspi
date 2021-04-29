# raspi起動から初回ログインまで

* https://www.raspberrypi.org/downloads/raspberry-pi-os/ から Raspberry Pi OS (32-bit) Lite をダウンロード(`2021-03-04-raspios-buster-armhf-lite.img`)
* Etcher を使ってSDカードへ書き込み

* raspberrypiにキーボード、モニターを接続し起動
* ID:pi, PASS:raspberry

# wifi,ssh接続まで
#### wifi
```
$ sudo nano /etc/wpa_supplicate/wpa_supplicant.conf
```
を実行し以下を追記
```
country=JP
network={
ssid="ssid"
psk="password"
}
```
再起動することで、wifiに接続できる。

#### ssh

```
$ sudo systemctl start ssh.service
$ sudo systemctl enable ssh.service

```
* PCから`ssh pi@raspberrypi.local`で入れるようになる。

#### wifiパスワードの暗号化
```
$ wpa_passphrase ssid password
```

を実行すると、暗号化済みパスワードが表示される。それを

```
$ sudo nano /etc/wpa_supplicate/wpa_supplicant.conf
```

で書き込む

# パッケージの更新

```
$ sudo apt update
$ sudo apt upgrade
```

# ユーザ追加&piユーザ削除

```
$ sudo useradd -m -s /bin/bash -g pi -G sudo [userName]
$ sudo passwd [username]
$ sudo userdel -r pi
```

# ホスト名変更

```
$ sudo nano /etc/hostname
$ sudo nano /etc/hosts
$ sudo reboot
```
`ssh [ユーザ名]@[変更後ホスト名].local`でログインできるようになる。


# ssh鍵の作成と登録

PCで

```
$ ssh-keygen -t rsa -b 4096  # PC
$ ssh-copy-id -i ~/.ssh/id_rsa.pub [username]@[hostname].local
```

これでパスワードなしでログインできるようになる。

# タイムゾーン変更

```
$ sudo cp /usr/share/zoneinfo/Japan /etc/localtime
$ sudo nano /etc/timezone # Japanと記載
$ sudo tzselects
$ sudo dpkg-reconfigure tzdatalo
```

# ロケール変更
```
$ sudo nano /etc/locale.gen # ja_JP.UTF-8 UTF-8 をコメントアウト
$ sudo locale-gen
$ sudo update-locale LANG="ja_JP.utf8"
```

# ネットワーク関係のパッケージインストール
```
$ sudo apt install dnsutils
```

# Xの実験

* Xserverの起動（PC側）

  PCでVcXsrvをインストール
  XLaunchアイコンをクリックし起動
  あとは流れに沿って。ただし、Disable access controlはチェックを入れる。

* Xclientアプリの起動（ラズパイ側）

  ```
  $ sudo apt install x11-apps
  $ export DISPLAY=IPアドレス:0.0
  $ xeyes &
  ```

* PC側でxeyesの画面が見えているはず

# デスクトップのインストールとVNC接続

```
$ cd /tmp
$ wget https://www.realvnc.com/download/file/vnc.files/VNC-Server-6.7.2-Linux-ARM.deb
$ chmod u+x VNC-Server-6.7.2-Linux-ARM.deb
$ sudo apt install ./VNC-Server-6.7.2-Linux-ARM.deb
$ sudo apt install xserver-xorg
$ sudo apt install xinit
$ sudo apt-get install raspberrypi-ui-mods
$ systemctl get-default
$ sudo systemctl set-default multi-user.target
$ vncserver
```
