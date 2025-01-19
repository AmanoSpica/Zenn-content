---
title: "[Ubuntu 22.04] systemctl(systemd)でシェルスクリプトを自動実行"
emoji: "☀️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ubuntu", "shell"]
published: true
published_at: "2025-01-19 21:00"
---

Ubuntu起動時にシェルスクリプトを自動実行する手順の備忘録です。bashシェルを想定しているためzshなどを使用している方は適宜読み替えてください。

# 手順
## 1. シェルスクリプトを作成
任意の場所でシェルスクリプトを作成します。
```sh:sample.sh
#!/bin/bash

# 例) pythonスクリプトを実行
/home/ubuntu/.pyenv/versions/3.12.7/bin/python3.12 /home/ubuntu/example/main.py
```

- ファイルは絶対パスで表記
- カレントディレクトリを移動する必要がある場合は`cd /home/ubuntu... && ...`のようにする

実行権限を付与
```bash:bash
chmod 744 sample.sh
```


## 2. 設定ファイルを作成
サービスのファイルを配置する場所に移動
```bash:bash
cd /etc/systemd/system
```

このディレクトリ内の`<service-name>.service`がすべてサービスとして認識されます。

任意の名称で`.service`ファイルを作成
```bash:bash
sudo vim sample.service
```

```ini:sample.service
[Unit]
Description=This is sample service  # serviceの説明
After=network.target  # ①

[Service]
User=<User Name>  # 実行するユーザー名
ExecStart=/home/ubuntu/sample.sh  # シェルスクリプトの場所(絶対パス)
Restart=always  # ②
type=simple  # ④

[Install]
WantedBy=multi-user.target
```

### ①[Unit] - After
ここで指定したサービスが終了した後にこのサービスを実行します。
`network.target`は**ネットワークが利用可能になった後**ということです、

### ②[Service] - Restart
プロセスが停止したときのプロセス再起動の条件です。
- always: 常に再起動
- no: 再起動しない
- on-success: 終了コードが0のときに再起動
- on-failure: 終了コードが0以外の時に再起動

### ④[Service] - Type
サービスプロセスの起動完了の判定方法を指定します。
- simple (default): コマンドを実行したタイミングで起動完了
  （フォアグラウンドで実行を継続するコマンド）
- forking: 実行したコマンドが終了した時点で起動完了

サービスをリロード
```bash:bash
sudo systemctl daemon-reload
```

自動起動を有効化
```bash:bash
sudo systemctl enable sample.service
# sample.serviceは設定ファイル名
```

サービス起動
```bash:bash
sudo systemctl start sample.service
```

状態確認
```bash:bash
sudo systemctl status sample.service
```
![](/images/ubuntu-systemctl-service/systemctl-status.png)
画像のオレンジ枠内のように`Active`になっていれば成功です。

再起動後にもう一度`systemctl status`を実行して`Active`であることを確認してください。


## トラブルシューティング
### code=exited, status=203/EXEC
![](/images/ubuntu-systemctl-service/systemctl-status-failed.png)

`systemctl status`コマンドでは原因がわからないのでおとなしくlogを見ましょう
```bash:bash
less /var/log/syslog
```

たいてい、`Permission denied`か`No such file or directory`エラーが発生しています。

`Permission denied`: シェルスクリプトの実行権限を744に指定
`No such file or directory`: すべて絶対パスで指定しているかもう一度確認

# 参考文献
https://note.com/ipa713/n/ndf1b4941362d
https://qiita.com/sinsengumi/items/24d726ec6c761fc75cc9
