# nginx

nginx の基礎知識

## nginx とは

* web サーバ
* オープンソース
* リバースプロキシ
* サーバのメモリ使用量を軽減できる
* 静的なコンテンツの配信が可能
* 単体では動的コンテンツの配信は不可能(アプリケーションサーバと連携する事で可能になる)
* モジュールにより機能拡張できる
* シングルプロセス

## プロキシとリバースプロキシ

[リバースプロキシ \(reverse proxy\)とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word1755.html)

* プロキシ
  * 内部ネットワークから外部ネットワークに接続する際の中継サーバ
  * 別名フォワードプロキシ
  * ネットワーク高速化、セキュリティ対策、匿名性の確保などが行える
  * ブラウザの身代わり
* リバースプロキシ
  * 外部ネットワークから内部ネットワークに接続する際の中継サーバ
  * 1つのURL で複数のサーバを振り分けられる
  * フォワードプロキシと同じようなメリットがある
  * web サーバの身代わり

## ssh 接続設定

* 秘密鍵、公開鍵を追加
* ssh-copy-id でパーミッションエラー

### 秘密鍵、公開鍵を追加

クライアントでssh接続用の秘密鍵、公開鍵の作成

```
$ cd
$ mkdir .ssh
$ cd .ssh
$ ssh-keygen
```

さくらVPS管理画面からVNCコンソールを起動してipアドレス、user名 を確認(さくらVPS の管理画面にipアドレスは表示されている)

```
$ su
# hostname -I
ipアドレスが表示
# exit
$ echo $USER
user名が表示
```

クライアントからリモートに秘密鍵を登録(22番ポートは閉じているので`-p`で別ポートを指定)

```
$ ssh-copy-id -p ポート番号 user名@ipアドレス 
```

ssh で接続

```
$ ssh -p ポート番号 user名@ipアドレス
```

### ssh-copy-id でパーミッションエラー

`ssh-copy-id` で`Permission denied (publickey).` が出る場合はリモートのパスワード認証を許可する

[CentOS 7 で ssh\-copy\-id ができなかったときの対処法 \- Qiita](https://qiita.com/POPOPON/items/d154df95be78382f42ce)

```
# etc/ssh/sshd_config
PasswordAuthentication no # 禁止
PasswordAuthentication yes # 許可に変更
```

設定を反映させる

```
$ sudo service sshd restart
```

秘密鍵が登録できたら再度`PasswordAuthentication` を`no` に変更する

## さくらVPSのDebian にnginx をインストール

* `apt` を使って`nginx` をインストール
* リモートにssh で接続した状態で行う
* インストール、起動は`root` で行う

```
# nginx を完全一致検索
$ apt list nginx

# sudo でインストール
$ sudo apt install nginx

# インストールを確認
$ apt list --installed | grep nginx

# コマンド実行を確認
$ nginx -v
nginx version: nginx/1.18.0

# nginx を起動
$ su
# /etc/init.d/nginx start
Starting nginx (via systemctl): nginx.service.
```

この後、さくらVPS のipアドレスにブラウザでアクセスすると`Welcome to nginx` と表示されればok

## nginx インストール後に`command not found` が出る

`command not found` の解決方法

[nginx\-vコマンドが見つかりません\-スーパーユーザー](https://superuser.com/questions/662306/nginx-v-command-not-found)

* `root` ユーザーで実行していない
* ユーザ毎に使えるコマンドが制限されているので権限のあるユーザーで実行する

```
# インストールされている場所を表示
$ pgrep nginx | xargs ps -f -p
UID          PID    PPID  C STIME TTY      STAT   TIME CMD
root         350       1  0 06:37 ?        Ss     0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
```

## HTML ファイルの設置場所の確認

ssh 接続、nginx 起動
```
# ssh でリモートに接続
$ ssh -p 20022 karlley@153.120.41.146

# nginx 起動
$ su
# /etc/init.d/nginx start
Starting nginx (via systemctl): nginx.service.

# 設定ファイルの確認
# less /etc/nginx/nginx.conf
# less /etc/nginx/sites-enabled/default
```

設定ファイルの確認

* `/etc/nginx/nginx.conf`
* `/etc/nginx/sites-enabled/default`

```
# /etc/nginx/nginx.conf
...
# 読み込んでいる設定ファイルのディレクトリ
include /etc/nginx/sites-enabled/*;
```

```
# /etc/nginx/sites-enabled/default
...
# HTML ファイルを設置するルートディレクトリ
root /var/www/example.com;
```

## HTML ファイルの設置

* 管理者権限でないとファイルを作成でいないので`su` でファイル作成する
* `/var/www/html/index.nginx-debian.html` がデフォルトで表示されているHTML
* `/var/www/html/index.html` を設置することでデフォルトの表示HTML を変更できる
* `/var/www/html/sample/index.html` を設置することで`http://153.120.41.146/sample/index.html` を表示できる
