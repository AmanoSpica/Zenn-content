---
title: "Discord発RaspberryPi経由WoL線 (Discordを経由して外出先からPCの電源をONにする)"
emoji: "🚄"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["discord", "discordpy", "raspberrypi", "wol"]
published: false
---

https://github.com/AmanoSpica/WoL-Bot.git
# UseCase
![](/images/raspberrypi-wol-bot/usecase.jpg)
**「これを使えば外出先からPCの電源をONにできるよ！！」**

# 動作フロー
![](/images/raspberrypi-wol-bot/flow.jpg)

Wake on Lanはルーター越えが難しいという問題点がありましたが、このシステムはラズパイ(または常時ONのPC)さえあれば環境に依存せず簡単にWoLでPCを起動させることができます。


# 導入方法
### 事前準備
こちらの記事の "WoLの利用手順" - "「起動される側のPCの設定」"を行ってください。
https://zenn.dev/headwaters/articles/317fa82c78e077

## 1. [Windows] ファイアウォールの設定
Windowsのファイアウォールはデフォルト状態ではpingのレスポンスを返さない設定になっています。PCの状態確認のために使用するので有効化します。

検索から「セキュリティが強化されたWindows Defenderファイアウォール」を起動します。
受信の規則から「ファイルとプリンターの共有 (エコー要求 - ICMPv4 受信)」を有効化してください。
![](/images/raspberrypi-wol-bot/WindowsDefender.png)

## 2. [Raspberry Pi] 下準備
### A. 使用するソフトをインストール
```bash:bash
sudo apt install ethtool wakeonlan
```

### B. WoLを有効化
WoLを設定したいネットワークデバイス名を確認してください。
```bash:bash
$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    ...
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    ...
```

今回は`eth0`に設定します。
```bash:bash
$ sudo ethtool eth0
Settings for eth0:
        ...
        Supports Wake-on: pumbag
        Wake-on: d
        ...
```

`Wake-on`が`d`なら無効、`g`なら有効です。

有効化します。
```bash:bash
$ sudo ethtool -s eth0 wol g
$ sudo ethtool eth0
        ...
        Supports Wake-on: pumbag
        Wake-on: g
        ...
```

再起動すると無効化されてしまうので、再起動後に自動で有効化するように設定します。
```bash:bash
$ sudo sh -c "echo '#! /bin/sh
### BEGIN INIT INFO
# Provides:	wakeonlan
# Required-Start:
# Required-Stop:
# Should-Start:
# Default-Start:	2 3 4 5
# Default-Stop:
# Short-Description:	wakeonlan
### END INIT INFO
sudo ethtool -s eth0 wol g
exit' > /etc/init.d/wakeonlan"
```
```bash:bash
sudo chmod +x /etc/init.d/wakeonlan
sudo update-rc.d wakeonlan defaults
```

設定ができたか確認します。
```bash:bash
$ ls -l /etc/rc*.d/*wakeonlan
lrwxrwxrwx 1 root root 19 11月 15 21:42 /etc/rc2.d/S01wakeonlan -> ../init.d/wakeonlan
lrwxrwxrwx 1 root root 19 11月 15 21:42 /etc/rc3.d/S01wakeonlan -> ../init.d/wakeonlan
lrwxrwxrwx 1 root root 19 11月 15 21:42 /etc/rc4.d/S01wakeonlan -> ../init.d/wakeonlan
lrwxrwxrwx 1 root root 19 11月 15 21:42 /etc/rc5.d/S01wakeonlan -> ../init.d/wakeonlan
```

再起動して`Wake-on: g`になっていることを確認してください。

## 3. Discord Botを作成
下記リンクにアクセスしてください。
https://discord.com/developers/applications
![](/images/raspberrypi-wol-bot/discord-bot-1.png)
![](/images/raspberrypi-wol-bot/discord-bot-2.png)
![](/images/raspberrypi-wol-bot/discord-bot-3.png)
![](/images/raspberrypi-wol-bot/discord-bot-4.png)
:::message
トークンは後に使用しますので控えていてください。
:::
![](/images/raspberrypi-wol-bot/discord-bot-5.png)
:::message alert
必ず非公開に設定してください
:::
![](/images/raspberrypi-wol-bot/discord-bot-6.png)
![](/images/raspberrypi-wol-bot/discord-bot-7.png)
:::message
Botの権限は以下を有効にしてください。
- Embed Links
- Send Messages
:::
![](/images/raspberrypi-wol-bot/discord-bot-8.png)

## 4. [Raspberry Pi] WoL-Botをインストール
### 実行環境
- Ubuntu 24.04
- Python 3.12.7

Raspberry Pi OSでも動作しますが一部コマンドが異なる場合があります。

### 手順
**⓪Pythonをインストール**
pyenv経由でインストールするのがおすすめです。
https://zenn.dev/hr0t15/articles/8ae3564bde2cce

**①リポジトリをクローン**
```bash:bash
git clone https://github.com/AmanoSpica/WoL-Bot.git
```

**②ライブラリをインストール**
```bash:bash
pip install -r requirements.txt
```

**③環境変数を設定**
```:.env
DISCORD_BOT_TOKEN="コピーしたdiscord botのトークン"
```

**④実行**
```bash:bash
python main.py
```

バックグラウンドで実行する場合↓
```bash:bash
nohup python main.py &
```
または
https://zenn.dev/amano_spica/articles/ubuntu-systemctl-service

## 5. Discord上での設定

<!-- TODO -->


# 参考文献
https://zenn.dev/headwaters/articles/317fa82c78e077
https://vild.hatenablog.com/entry/2019/09/14/154813
https://qiita.com/Sakathon/items/a01adbf17d846beb4605
