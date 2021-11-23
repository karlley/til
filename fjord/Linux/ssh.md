# ssh とは

ネットワークを介して他のコンピュータにログインして操作するソフトウェア

* Secure Shell
* 暗号化された通信経路を構築してから接続を行う

## sshd(SSH Deamon)

外部からの接続を受け付ける常駐プログラム

## ssh_config とsshd_config の違い

* ssh_config: クライアント側の設定
* sshd_config: サーバ側の設定

[sshd\_configとssh\_config \| kamotora](https://kamotora.net/system/linux/sshd_config-ssh_config/)

# ネットワークについて

ネットワークに関する基礎知識

## scp

接続したコンピュータと手元のコンピュータの間でファイルをコピーするコマンド

* secure copy protocol
* ssh の機能の一つ
* UNIX コマンドの`cp` と同じような操作感で使用できる

## TCP/IP

世界標準的に利用されている通信プロトコル

* TCP: Transmission Control Protcol
* IP: Internet Protcol
* アプリケーション層、トランスポート層、ネットワーク層、ネットワークインターフェース層の4層構造

## TCP

コネクション確立を確認しながら通信を行うプロトコル

* Transmission Control Protocol
* ステートフル
* 安全性を重視

## UDP

コネクション確立を行わずに通信を行うプロトコル

* User Datagram Protcol
* ステートレス
* 速度を重視

## ステートフル/ステートレス

通信を行う際にコネクションの確立を確認しながら通信するかどうか

* ステートレス
    * コネクション確立の確認無し
    * 例: HTTP、UDP、IP、DNS
* ステートフル
    * コネクション確立の確認無し
    * 例: FTP、TCP、SSH

## FTP

TCP/IP ネットワークでファイル転送を行うことができるプロトコル

* File Transfer Protocol
* FTP サーバ、FTP クライアントの2つのソフトウェアを使う
* デフォルトは制御に21番ポート、データように20番ポートを使用する
* セキュリティが甘いので現在は殆ど使用されていない
* FTPS、SFTP に移行していった

## FTPS

トランスポート層の暗号化プロトコルにSSL/TLS を使用して通信経路を暗号化しFTP 形式でファイル転送を行うこと

* FTP over SSL/TLS
* TCP/IP にSSL/TLS での暗号化を組み合わせている
* FTP のセキュリティ強化版
* デフォルトは990番ポートを使用する

## SFTP

ssh を使用してファイルを送受信するプロトコル、またはコマンド

* SSH File Transfer Protocol
* ssh パッケージに標準で付属
* FTP に似ている

## OpenSSH

> SSHプロトコルを使用する為のソフトで、SSHクライアントとSSHサーバーの両方が含まれている。

[インフラエンジニアじゃなくても押さえておきたいSSHの基礎知識 \- Qiita](https://qiita.com/tag1216/items/5d06bad7468f731f590e)

* Linux にはほぼデフォルトでインストールされている
* 主要コマンドは`ssh`、`scp`、`ssh-keygen`

## ウェルノウンポート

著名なサービスやプロトコルが使用するTCPやUDPのポート番号

[ポート番号 一覧　危険な ポート番号　閉じておいた方が良い ポート番号　\-　Windows ＆ IT Tips](https://abhp.net/it/IT_Port_No_100000.html)

* WELL KNOWN PORT
* 0番 - 1023番

# ssh インストール

apt 経由でssh をインストール

```Shell
$ apt list ssh
$ apt update
$ sudo apt install ssh
$ apt list --installed | grep ssh
```

# ssh での接続設定

接続先(リモート)と接続元(クライアント)で分けて作業する

1. 【クライアント】接続用の秘密鍵/公開鍵の生成
2. 【クライアント】公開鍵を接続先(リモート)に渡す
3. 【リモート】 OpenSSH で接続に関する設定
4. 【クライアント】ssh でリモートへの接続の確

## 1. 【クライアント】接続用の秘密鍵/公開鍵の生成

[インフラエンジニアじゃなくても押さえておきたいSSHの基礎知識 \- Qiita](https://qiita.com/tag1216/items/5d06bad7468f731f590e#%E5%85%AC%E9%96%8B%E9%8D%B5%E8%AA%8D%E8%A8%BC%E6%96%B9%E5%BC%8F%E3%81%A7%E3%81%AE%E6%8E%A5%E7%B6%9A)

クライアントで`ssh-keygen`コマンドで使い秘密鍵/公開鍵を生成

* `.pub` がついているファイルが公開鍵、リモート側に渡す鍵
* `.pub` 無しが秘密鍵、クライアントに保管する鍵

```Shell
# .ssh ディレクトリに移動
$ cd .ssh
$ pwd
Users/user_name/.ssh

# 鍵があるか確認
$ ls -a

# 鍵の生成
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/user_name/.ssh/id_rsa): # 鍵を生成するディレクトリ
Enter passphrase (empty for no passphrase): # パスフレーズ(任意)
Enter same passphrase again: # パスフレーズ再入力(任意)
Your identification has been saved in /Users/user_name/.ssh/id_rsa.
Your public key has been saved in /Users/user_name/.ssh/id_rsa.pub.

# 鍵が生成されたか確認
$ ls -a
id_rsa  id_rsa.pub
```

## 2. 【クライアント】公開鍵を接続先(リモート)に渡す

1. リモート側のip アドレスを確認
2. クライアント側で`ssh-copy-id` コマンドでリモートに鍵を登録
3. リモート側で鍵の追加を確認(authorized_keys の追加を確認)

[ssh\-copy\-idで公開鍵を渡す \- Qiita](https://qiita.com/kentarosasaki/items/aa319e735a0b9660f1f0)

```Shell
# リモート側でip アドレスを確認
$ hostname -I
ip アドレスが表示される
```

```Shell
# クライアント側でリモートに鍵を登録
$ ssh-copy-id user_name@ipアドレス
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/user_name/.ssh/id_rsa.pub"
...
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes #yes と入力
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
user_name@ipアドレス's password: #パスワード入力

Number of key(s) added:        1

Now try logging into the machine, with:   "ssh 'user_name@ipアドレス'"
and check to make sure that only the key(s) you wanted were added.
```

```Shell
# リモート側で鍵の追加を確認
$ ls -a /home/user_name/.ssh
authorized_keys
```

## 3.【リモート】OpenSSH 設定

設定ファイルのバックアップ

```Shell
$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.org
$ ls -l /etc/ssh/
```

sshd_config に以下の設定追加

* パスワード認証禁止
* root ログイン禁止
* 22番ポート禁止

```Shell
# sshd_config をvi で開く
$ sudo vi /etc/ssh/sshd_config
```

```
# ssh_config

# 22番ポート禁止
# Port 22 を変更
Port xxxxx

# パスワード認証禁止
# PasswordAuthentication yes を変更
PasswordAuthentication no

# root ログイン禁止
# permitRootLogin prohibit-password を変更
PermitRootLogin no
```

sshd_config の設定を反映させる為に再起動

```Shell
$ sudo service sshd restart
```

## 4. 【クライアント】OpenSSH の設定の確認

以下3点の設定を確認

* パスワード無し、鍵認証、22番ポート以外でのログインを確認
* パスワード認証禁止
* root ログイン禁止
* 22番ポート禁止

[ssh コマンドでパスワード認証を指定して接続するオプション \- Qiita](https://qiita.com/dounokouno/items/2e9086fef19ff20fb3f9)

```Shell
# パスワード無し、鍵認証、22番ポート以外でのログインを確認
$ ssh -p 22番以外のポート user_name@ipアドレス
Linux ik1-215-78392 5.10.0-9-amd64 #1 SMP Debian 5.10.70-1 (2021-09-30) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Nov 23 08:10:53 2021 from 180.16.6.5

# パスワード認証禁止を確認(パスワード認証を指定してログインを試す)
$ ssh user_name@ipアドレス -o PreferredAuthentications=password
user_name@ipアドレス: Permission denied (publickey). #ログインできないのを確認

# root ログイン禁止を確認
$ ssh root@ipアドレス
root@ip アドレス: Permission denied (publickey). #ログインできないのを確認

# 22番ポート禁止を確認(22番ポートを指定してログインを試す)
$ ssh -p 22 user_name@ipアドレス
ssh: connect to host ipアドレス port 22: Connection refused #ログインできないのを確認
```

ちなみに下記コマンドで開いているポートを確認できる

```
$ sudo lsof -i -P -n
COMMAND   PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    14860    root    3u  IPv4 251086      0t0  TCP *:20022 (LISTEN)
sshd    14860    root    4u  IPv6 251097      0t0  TCP *:20022 (LISTEN)
sshd    14924    root    4u  IPv4 251744      0t0  TCP 153.120.41.146:20022->180.16.6.5:34849 (ESTABLISHED)
sshd    14930 user_name  4u  IPv4 251744      0t0  TCP 153.120.41.146:20022->180.16.6.5:34849 (ESTABLISHED)
```
