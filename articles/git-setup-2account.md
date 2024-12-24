---
title: "[備忘録] 複数Githubアカウントのセットアップ"
emoji: "🐱"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

今記事ではGit/GitHubで**ディレクトリごとに**アカウントを分ける設定方法を記載しています。

## ssh-keyを作成
```shell
# 1つ目のアカウント
$ ssh-keygen -t rsa -f <ファイル名A>

# 2つ目のアカウント
$ ssh-keygen -t rsa -f <ファイル名B>
```
ファイル名A, Bにはお好みの名前を指定してください。
※必ず別のファイル名を指定してください。


## ssh設定ファイル (~/.ssh/config) を編集
```yaml
# Github Main Account
Host github github-main
    HostName github.com
    IdentityFile <ファイル名A>
    User git
    Port 22
    TCPKeepAlive yes
    IdentitiesOnly yes

# Github Sub Account
Host github-sub
    HostName github.com
    IdentityFile <ファイル名B>
    User git
    Port 22
    TCPKeepAlive yes
    IdentitiesOnly yes
```


## Githubに公開鍵を登録
作成した公開鍵(`<ファイル名>.pub`) をGithubに登録します。それぞれのアカウントで行ってください。

1. catコマンドまたはメモ帳で `<ファイル名>.pub` を開いて中身をコピーしてください。

2. ブラウザでGithubを開き、Settings > SSH and GPG keys にアクセスします
![](/images/git-setup-2account/github-sshkey.png)

3. 「New SSH key」をクリックして`Key`のところに先ほどコピーした公開鍵の中身を貼り付けます。<br>`Title`は何でも構いません。`Key Type` は `Authentication Key` になっていることを確認してください。
![](/images/git-setup-2account/github-new-sshkey.png)


# Githubとssh接続を確認
```shell
# Main Account
$ ssh -T github
$ ssh -T github-main

# Sub Account
$ ssh -T github-sub
```

以下のメッセージが帰ってくれば正しく接続できています。

```
Hi {yourname}! You've successfully authenticated, but GitHub does not provide shell access.
```


# .gitconfigを編集