# nginx ssl

nginx をSSL化する

# SSL とは

[SSL/TLSとは？簡単説明 \| GMOグローバルサインカレッジ](https://college.globalsign.com/ssl-pki-info/ssl-tls/)

データ通信の暗号化を行う仕組み

* Secure Sockets Layer
* SSL からTLS に移行している
* SSL という呼称が浸透しているのでTLS移行後もSSL と呼ばれることばある
* SSL/TLS と併記される場合もある

# TLS とは

[SSLとTLSの違い \| GMOグローバルサインカレッジ](https://college.globalsign.com/ssl-pki-info/tls_ssl_brittle/)

SSL より新しい暗号化技術

* Transport Layer Security 
* SSL とひとくくりにして呼ばれる場合がある

# 暗号化のメカニズム

[\[Think IT\] 第6回：SSLの基本を押さえる \(1/3\)](https://thinkit.co.jp/free/article/0706/3/6/)

* 鍵と計算方法を組み合わせて行う
* 計算方法が同じでも鍵が異なると暗号化の結果は異なる

# 暗号化の種類

SSL/TLS の暗号化には3つの方式がある

## 共通鍵暗号化方式

* 暗号化、復号化を同一の鍵を利用する
* 共通鍵暗号化アルゴリズムの種類
  * DES（Data Encryption Standard）
  * 3DES（トリプルDES）

## 公開鍵暗号化方式

* 暗号化、復号化を異なる鍵を利用する
* 公開鍵、秘密鍵の2つの鍵が必要
* 正しいペアの鍵でないと復号できない
* 復号に掛かる時間は共通鍵よりもかなり長い
* 公開鍵暗号化アルゴリズムの種類
  * RSA（Rivest Shamir Adleman）
  * EPOC（Efficient Probabilistic Public-Key Encryption）

## ハイブリット方式

* 共通鍵暗号化方式、公開鍵暗号化方式を組み合わせた方法
* 公開鍵暗号化方式の復号に掛かる時間を短縮できる

# 秘密鍵、公開鍵のイメージ

[イケメンとラブレターで学ぶSSLの仕組み \- give IT a try](https://blog.jnito.com/entry/2013/05/26/064803)

> 実現したいことは、誰からも見えないように宝箱に手紙を入れ、鍵をかけて郵送しあうこと

> 南京錠の特徴は、施錠と解錠の方法が異なることです。
施錠は誰でもできますが、解錠は鍵を持っている人しかできません。

> 毎回公開鍵方式でやりとりすると、暗号化と復号化の手間がかかります。
なので、安全に共通鍵を受け渡しできたら、それ以後は手間があまりかからない共通鍵方式でデータの暗号化と復号化を行ないます。

# TLS/SSL証明書(サーバ証明書)

通信相手に偽りが無いことを保証するもの

* 公正な第三者機関であるSSL認証局(CA = Certificate Authority)から発行される
* 認証のレベル毎に3つの種類がある
* SSL通信を開始する際にサーバ側からクライアント側に送信される
* SSL通信を開始する際にサーバ側の秘密鍵で暗号化して送信される
* 証明書には公開鍵が含まれている

## 種類

* ドメイン認証(DV): Domain Validation、ドメインを管理していることを証明、商用には利用不可
* 組織認証(OV): Organization Validation、組織として法的に実在していることを証明、公共/商用での標準
* EV認証(EV): Extended Validation、DV + OV + 物理的に組織が存在していることを証明、最高レベルの認証

# CA

[認証局とは \| GMOグローバルサインカレッジ](https://college.globalsign.com/ssl-pki-info/certification_authority/)

> ウェブサイトの運営者やシステムなどの利用者に対し、正しい人物や機器が本物であるかどうかの認証を行います

* Certificate Authority
* 自己の正当性を自分で証明できるCAはルート証明局(ルートCA)と呼ばれる
* 自己の正当性を自分で証明できないCAは中間証明局(中間CA)と呼ばれる

# CSR

* Certificate Signing Request
* サーバ証明書を発行してもらう為の署名要求

# 中間証明書

* 中間CAに認証してもらったサーバ証明書
* 自己の正当性を自分で証明できないCAの場合は、上位のCA(ルートCA)に正当性を証明してもらう必要がある

# CAの種類

* ルートCA: 自己の正当性を自分で証明できるCA、発行したサーバ証明書はルート証明書と呼ばれる
* 中間CA: 自己の正当性を自分で証明できないCA、サーバ証明書には中間証明書が必要

# SSL化する手順

[イケメンとラブレターで学ぶSSLの仕組み \- give IT a try](https://blog.jnito.com/entry/2013/05/26/064803)

> CA(=市役所)にサーバー証明書(=印鑑証明書)を作ってもらい、サーバー証明書とサーバーの秘密鍵(=伊藤さん特注の南京錠)をWebサーバーに置けば良い。

## 必要なもの

* CSR
* サーバ証明書
* 自分で作成した公開鍵
* サーバが発行した秘密鍵(サーバ証明書に含まれる)
* 中間証明書(中間CAでのサーバ証明書の場合のみ必要)

## 手順

1. サーバ管理者がCAにCSRを送信(秘密鍵も同時に作成される)
2. CAからサーバ証明書、中間証明書が発行される
3. サーバ管理者がサーバ証明書、中間証明書、秘密鍵をサーバにアップロードする

# openssl コマンド

[opensslコマンドの使い方 \- hana\_shinのLinux技術ブログ](https://hana-shin.hatenablog.com/entry/2022/01/29/184741)

[opensslコマンドの使い方: UNIX/Linuxの部屋](http://x68000.q-e-d.net/~68user/unix/pickup?openssl)

* CSR作成、公開鍵作成、暗号化/復号化等が行えるコマンド
* Linux には標準でインストールされている？

```sh
# 使用可能なコマンドの一覧を表示
$ openssl help

# 各コマンドのヘルプを表示
$ openssl コマンド名 -help
```

# 自作のサーバ証明書を発行する

[オレオレ証明書をopensslで作る（詳細版） \- ろば電子が詰まつてゐる](https://ozuma.hatenablog.jp/entry/20130511/1368284304)

## 作成するもの

サーバ証明書を作成する為に下記の3つを作成する

* 秘密鍵(Private Key): `server.key`
* 証明書署名要求(CSR): `server.csr`
* サーバ証明書(CRT): `server.crt`

## 作成手順

1. 秘密鍵を作成
2. 秘密鍵を元にCSRを作成(同時に`openssl req`コマンドが秘密鍵から公開鍵も作成してくれる)
3. 自分自身で作成したCSR に署名してサーバ証明書を作成
4. 作成した3つのファイルを設置

```sh
# 秘密鍵作成
$ openssl genrsa 2048 > server.key

# 秘密鍵の中身を確認
$ openssl rsa -text < server.key

# 秘密鍵からCSR と公開鍵を作成
$ openssl req -new -key server.key > server.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:JP # 日本
State or Province Name (full name) [Some-State]:FUKUOKA # 福岡
Locality Name (eg, city) []:FUKUOKA-SHI # 福岡市
Organization Name (eg, company) [Internet Widgits Pty Ltd]: # 入力無し
Organizational Unit Name (eg, section) []: # 入力なし
Common Name (e.g. server FQDN or YOUR name) []:karlley.com # VirtualHost のドメイン名
Email Address []:karlley.jp@gmail.com # メールアドレス

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []: # 入力無し
An optional company name []: # 入力無し

# CSR の中身を確認
$ openssl req -text < server.csr

# 作成したCSR に自分自身で署名してサーバ証明書を作成(有効期限は10年に設定)
$ openssl x509 -req -days 3650 -signkey server.key < server.csr > server.crt  

# サーバ証明書の中身を確認
$ openssl x509 -text < server.crt

# 作成した3つのファイルを/etc/ssl/ に移動
$ sudo mv server.key server.csr server.crt /etc/ssl/.
```

# 複数ファイルをmv コマンドで移動する

[【Linuxコマンド mv】複数ファイルを同じフォルダに一括移動させる \- よちよちpython](https://chayarokurokuro.hatenablog.com/entry/2021/08/04/235221)

> 「/.」を付加することで、そのディレクトリ直下にファイルを移動することを示すコマンドとなりますので、ディレクトリが存在しないと移動することはできません。

```
$ mv ファイル1 ファイル2 移動先のディレクトリ/.
```

# VirtualHost にSSL を反映させる

1. VirtualHost の設定ファイルにSSL化する為の記述を追加
2. 設定ファイルが正しく記述されているか確認
3. 設定ファイルを反映させる為にnginx を再起動
4. ブラウザでHTTP とHTTPS の両方の表示を確認

## 1. VirtualHost の設定ファイルにSSL化する為の記述を追加

VirtualHost の設定ファイル`/etc/nginx/sites-available/karlley.com` にSSL の設定を追加

`$ sudo vim /etc/nginx/sites-available/karlley.com`

下記の設定を追加

```
server {
  listen 80;
  server_name karlley.com;
  access_log /home/karlley/public_html/karlley.com/log/access.log;
  error_log /home/karlley/public_html/karlley.com/log/error.log;
  location / {
    root   /home/karlley/public_html/karlley.com/public/;
    index  index.html;
   }
}
server {
  listen 80;
  server_name www.karlley.com;
  access_log /home/karlley/public_html/www.karlley.com/log/access.log;
  error_log /home/karlley/public_html/www.karlley.com/log/error.log;
  location / {
    root   /home/karlley/public_html/www.karlley.com/public/;
    index  index.html;
  }
}

# 下記を追記
server {
  listen 443 ssl;
  server_name karlley.com;
  access_log /home/karlley/public_html/karlley.com/log/access.log;
  error_log /home/karlley/public_html/karlley.com/log/error.log;
  location / {
    root   /home/karlley/public_html/karlley.com/public/;
    index  index.html;
  }
  ssl_certificate /etc/ssl/server.crt;
  ssl_certificate_key /etc/ssl/server.key;
}
server {
  listen 443 ssl;
  server_name www.karlley.com;
  access_log /home/karlley/public_html/karlley.com/log/access.log;
  error_log /home/karlley/public_html/karlley.com/log/error.log;
  location / {
    root   /home/karlley/public_html/www.karlley.com/public/;
    index  index.html;
  }
  ssl_certificate /etc/ssl/server.crt;
  ssl_certificate_key /etc/ssl/server.key;
}
```

記述をまとめたバージョン

```
server {
  listen 80;
  listen 443 ssl;
  server_name karlley.com;
  access_log /home/karlley/public_html/karlley.com/log/access.log;
  error_log /home/karlley/public_html/karlley.com/log/error.log;
  location / {
    root   /home/karlley/public_html/karlley.com/public/;
    index  index.html;
   }
  ssl_certificate /etc/ssl/server.crt;
  ssl_certificate_key /etc/ssl/server.key;
}
server {
  listen 80;
  listen 443 ssl;
  server_name www.karlley.com;
  access_log /home/karlley/public_html/www.karlley.com/log/access.log;
  error_log /home/karlley/public_html/www.karlley.com/log/error.log;
  location / {
    root   /home/karlley/public_html/www.karlley.com/public/;
    index  index.html;
  }
  ssl_certificate /etc/ssl/server.crt;
  ssl_certificate_key /etc/ssl/server.key;
}
```

## 2. 設定ファイルが正しく記述されているか確認

VirtualHost の設定ファイル`/etc/nginx/sites-available/karlley.com` が正しく記述されているか`nginx -t` コマンドで確認する

* `syntax is ok` と`test is successful` と表示されればok
* 警告やエラーが表示された場合は修正する

```sh
$ sudo nginx -t
[sudo] password for karlley:
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

## 3. 設定ファイルを反映させる為にnginx を再起動

`nginx -t` がパスした状態で設定を反映させる

```sh
$ sudo /etc/init.d/nginx stop
$ sudo /etc/init.d/nginx start 
```

## 4. ブラウザでHTTP とHTTPS の両方の表示を確認

下記URLを全て表示してHTML を確認

* `http://kalley.com`
* `http://www.karlley.com`
* `https://karlley.com`
* `https://www.karlley.com`

下記が設定されているか確認

* `http://kalley.com` と`http://www.karlley.com` が異なるHTML が表示されている
* `https://kalley.com` と`https://www.karlley.com` にアクセスするとアドレスバーに`保護されていない通信`が表示されHTTPS 通信に設定されている
* `https://kalley.com` と`https://www.karlley.com` にアクセスしエラー画面で`thisisunsafe` とタイプするとHTML が表示されるので以下のHTML の表示を確認する
  * `http://kalley.com` と`https://kalley.com` が同一のHTML
  * `http://www.karlley.com`と`https://www.karlley.com` が同一のHTML

# CSR作成時のCommon Name に入力するドメイン名はwww有り/無しどちらでもOK

* `openssl` コマンドでCSR作成時に`Common Name` を入力するドメイン名は`www` 有り/無しどちらでも大丈夫だった
* 全く違うドメインの場合は別のCSR を用意する必要がありそう

```
# 秘密鍵からCSR と公開鍵を作成
$ openssl req -new -key server.key > server.csr
...
Common Name (e.g. server FQDN or YOUR name) []:www.karlley.com # www無しでもok
```

# listen ディレクティブのdefault について

[nginx の設定で気をつける事（個人用メモ） \- Qiita](https://qiita.com/white_aspara25/items/bc9d9b9b2dc0a673169a#default_server)

[nginxでデフォルトのバーチャルホストを設定する方法 \- Linux入門 \- Webkaru](https://webkaru.net/linux/nginx-default-server/)

* `default_server` は古いバージョンの記述なので`default` を使用する
* `default` を記述しない場合は一番最初の設定を使用する
* 最初の設定のサーバをデフォルトサーバに設定したくない場合場合のみ設定する(デフォルトにしたいサーバーに`default` を付ける)

#  a duplicate default server for 0.0.0.0:443 エラーについて

[How nginx processes a request](http://nginx.org/en/docs/http/request_processing.html)

[Configuring HTTPS servers](http://nginx.org/en/docs/http/configuring_https_servers.html)

VirtualHost の設定ファイル`/etc/nginx/site-abailable/karlley.com` の修正時に`[emerg] a duplicate default server for 0.0.0.0:443` とエラーが出た

```sh
$ sudo nginx -t
nginx: [emerg] a duplicate default server for 0.0.0.0:443 in /etc/nginx/sites-enabled/karlley.com:16
nginx: configuration file /etc/nginx/nginx.conf test failed
```

* `/etc/nginx/site-abailable/karlley.com` 内のlisten ディレクティブの`default` の記述が原因
* `default` はポート毎に設定するので同ポートにそれぞれ設定するのは間違い
* `default` は必要な場合のみ設定する

```sh
# 修正前
server {
  listen 80;
  listen 443 default ssl; # ここが原因
  server_name karlley.com;
  ...
}
server {
  listen 80;
  listen 443 default ssl; # ここが原因
  server_name www.karlley.com;
  ...
}
```

```sh
# 修正後
server {
  listen 80;
  listen 443 ssl; # default を削除
  server_name karlley.com;
  ...
}
server {
  listen 80;
  listen 443 ssl; # default を削除
  server_name karlley.com;
  ...
}
```

#  the “ssl” directive is deprecated, use the “listen … ssl” directive instead の対策

[\[warn\] the “ssl” directive is deprecated, use the “listen … ssl” directive instead](http://psychedelicnekopunch.com/archives/1569)

[2\. use the listen \.\.\. ssl directive instead \- AWSのnginx を更新→ssl directive is deprecated](http://manualkun.com/warn_ssl/2/)

[nginx連載6回目: nginxの設定、その4 \- TLS/SSLの設定 \- インフラエンジニアway \- Powered by HEARTBEATS](https://heartbeats.jp/hbblog/2012/06/nginx06.html)

VirtualHost の設定ファイル`/etc/nginx/site-abailable/karlley.com` の修正時に`[warn] the “ssl” directive is deprecated, use the “listen … ssl” directive instead` と警告が表示された

```sh
# 設定ファイルをテストすると警告が出る
$ sudo nginx -t
nginx: [warn] the "ssl" directive is deprecated, use the "listen ... ssl" directive instead in /etc/nginx/sites-enabled/karlley.com:24
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

* > listenディレクティブにsslパラメータを付けると、そのポートでSSLを有効にして待ち受けるようになります。そのポートに関して後述するsslディレクティブをonにしたのと同じ動作になるため、sslディレクティブの記述は不要になります。

* `/etc/nginx/site-abailable/karlley.com` 内の`ssl on` の記述は非推奨になっている
* `ssl on` の代わりに`listen ポート番号 ssl` を使用する

## 対策

`ssl on` の記述をコメントアウト、または削除する

```
# 修正前
server {
  listen 443;
  ssl on;
  ...
}
```

```
# 修正後
server {
  listen 443 ssl; #ssl を追加
  # ssl on; # 削除しても良い
  ...
}
```

## HTTP とHTTPS のどちらにも対応する際の注意点

`ssl on` の記述にyum さんの日報に解説があり、理解することができました。

[【33日目】SSL対応サイトの作成・SQL基礎 \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/reports/50538#ssl-1)

> sslディレクティブをonにすると全アクセスに対してSSL通信が適用されてしまうためHTTP通信がうまくいきません。

* `ssl on` でserver ディレクティブ内の`server_name` で指定されているドメインの全てのポートに対してSSL化される
* 特定のポートのみ(443番など)SSL化したい場合は`listen 443 ssl;` のように設定する

# 自己証明書でSSL化したサイトをChrome で確認する方法

自己証明書でSSL化したサイトをChrome で強制表示して確認する

[オレオレ証明書で「この接続ではプライバシーが保護されません」と出る件について \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/questions/989)

[Chromeで自己証明書（などの不正証明書）を使用したサイトにアクセスする方法 \- Qiita](https://qiita.com/sugiyasu-qr/items/1dce0742af960b3e618f)

* 強制表示をon: `この接続ではプライバシーが保護されません` という表示が出た画面で`thisisunsafe` とタイプしEnter
* 強制表示をoff: アドレスバーの`保護されていない通信` をクリックし、`警告をオンにする`をクリック

# VirtualHost で2つのサイトをSSL化 する場合のディレクトリ構成

ドメイン毎のHTML、アクセスログ、エラーログ

```
$ tree /home/karlley/
/home/karlley/
└── public_html
    ├── karlley.com
    │   ├── backup
    │   ├── log
    │   │   ├── access.log
    │   │   └── error.log
    │   ├── private
    │   └── public
    │       └── index.html # ドメイン1のHTML(HTTP、HTTPS共通)
    └── www.karlley.com
        ├── backup
        ├── log
        │   ├── access.log
        │   └── error.log
        ├── private
        └── public
            └── index.html # ドメイン2のHTML(HTTP、HTTPS共通)
```

CRT、サーバ証明書、秘密鍵

```
$ tree /etc/ssl/ -L 1
/etc/ssl/
├── server.crt # CRT
├── server.csr # サーバ証明書
└── server.key # 秘密鍵
```

設定ファイル

```
$ tree /etc/nginx/
/etc/nginx/
├── nginx.conf # nginx の設定ファイル、include でsites-enabled から読み込むファイルを指定
├── sites-available
│   ├── default
│   └── karlley.com # Virtual Host の設定ファイル
├── sites-enabled # Virtual Host の設定ファイルが入っている
     ├── default -> /etc/nginx/sites-available/default 
     └── karlley.com -> /etc/nginx/sites-available/karlley.com
```

nginx 実行ファイル

```
$ tree /etc/init.d/
/etc/init.d/
├── nginx # 実行ファイル
```

# ログをリアルタイムに確認する方法

`tail` コマンドを使う

[tail コマンド \| コマンドの使い方\(Linux\) \| hydroculのメモ](https://hydrocul.github.io/wiki/commands/tail.html)

```
$ tail -f ファイル名
```

```
# www.karlley.com のアクセスログを確認する
$ tail -f /home/karlley/public_html/www.karlley.com/log/access.log 
```
