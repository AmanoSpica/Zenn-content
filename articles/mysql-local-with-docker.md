---
title: "Dockerでお手軽MySQLサーバー (Local)"
emoji: "🎈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [docker, mysql]
published: true
---

MySQLをローカルのDocker上で動かす備忘録です。Windowsでのみ動作確認済。

# ファイル構成
```
.
│  Dockerfile
│  my.conf
```

## Dockerfile
```dockerfile
FROM mysql:5.7

ENV MYSQL_DATABASE sample
ENV MYSQL_ROOT_PASSWORD password
ENV MYSQL_USER test
ENV MYSQL_PASSWORD password

COPY ./my.conf /etc/mysql/conf.d/my.cnf
```

## my.conf
```conf
[mysqld]
character-set-server=utf8mb4
collation-server=utf8mb4_bin

[client]
default-character-set=utf8mb4

[mysqldump]
default-character-set=utf8mb4
```

# ビルド＆起動
```shell
# ビルド
$ docker build -t mysql-local-server-image .

# ボリュームの作成
$ docker volume create mysql-data

# 実行
$ docker run --name mysql-local-server -p 12345:3306 -v mysql-data:/var/lib/mysql -d mysql-local-server-image

# 接続
$ mysql -u default -h 127.0.0.1 --port=12345 -p

# 停止
$ docker stop mysql-local-server

# 再起動
$ docker restart mysql-local-server
```


## ボリュームを作成せずにデータをディレクトリに保存させる場合
```shell
$ docker run --name mysql-local-server -p 12345:3306 -v $PWD/temp:/var/lib/mysql -d mysql-local-server-image
```
tempファイルが作成されてそこにデータが収容されます。
