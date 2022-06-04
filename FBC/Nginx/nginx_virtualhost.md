# nginx virtualhost

VirtualHost を使い複数のドメインのサイトを立ち上げる

## ネームサーバ

* インターネット通信時にドメイン名をIPアドレスに変換するサーバー
* ネームサーバ == DNSサーバ
* ドメイン名とIPアドレスを紐付ける
* サーバを変更した場合にネームサーバの情報を変更する必要がある
* 階層化されており最上位のネームサーバをルートサーバとも呼ぶ

## DNS

* Domein Name System
* インターネット通信時にドメイン名をIPアドレスに変換する仕組み
* 名前解決とも呼ばれる
* キャッシュサーバを利用して処理の高速化を行っている

## DNS レコード

[DNSレコードとは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word12284.html)

* ドメインとIPアドレスを紐づけの一覧表をゾーンファイルと呼ぶ
* ゾーンファイルの1行毎の情報
* 主にドメインに対応するIPアドレス、ドメインのエイリアス等の情報が含まれる

## キャッシュサーバ

* DNS サーバに問い合わせを行うためのサーバ
* DNS サーバの負荷分散を行うことが目的
* 名前解決に必要な情報を一定期間保持している(DNS キャッシュ)
* DNS キャッシュの保持時間は指定できる(TTL == Time To Live)

## お名前.com でドメインの取得

* さくらVPSのネームサーバを使う場合はお名前.comでのネームサーバの申し込みは不要
* ドメイン取得後にドメイン情報認証メールが届き、URLにアクセスして認証する必要がある

# nginx の設定ファイル

* 設定ファイルは`/etc/nginx/nginx.conf`
* `include` を使って他の設定ファイルを読み込む(例: `include /etc/nginx/conf.d/*.conf;`)
* 設定ファイルはモジュールのカテゴリ毎に`core`、`event`、`http`、`mail` の順で記述されている
* バーチャルサーバの設定ファイルは`/etc/nginx/conf.d/` ディレクトリに作成する
* `nginx.conf` 内の`include /etc/nginx/conf.d/*.conf;` で`/etc/nginx/conf.d` ディレクトリにある拡張子がconfであるファイルを読み込む

## VirtualHost の設定

* VirtualHost の設定ファイルは`/etc/nginx/sites-available/` ディレクトリに作成する
* リクエストヘッダの`Host:` を見てアクセスするIPアドレスを切り替える
* DNS サーバの設定でアクセスするIPアドレスを指定する
* アクセス先を直接IPアドレスを指定した場合はデフォルトの設定にアクセスされる
* DNS でサブドメインでアクセスするIPアドレスを設定できる
* listen ディレクティブとserver_name ディレクティブによりどのバーチャルサーバが適応されるか判断される
* root ディレクティブでドキュメントのルートを指定
* location ディレクティブ に個々のドキュメントのルートを指定可能
* 正規表現を使いlocation で指定したドキュメントを切り替えが可能
* index ディレクティブでアクセスしたURL がディレクトリだった場合に表示するドキュメントを指定可能

## お名前.com で取得したドメインをさくらVPS のネームサーバに設定

[お名前\.comで取得したドメインをサクラVPSに設定する \- mfks17's blog（Life is Good \!\!）](https://mfks17.hateblo.jp/entry/20130320/1363730585)

[さくらVPSにお名前\.comで取得した独自ドメインを適用する手順 – Rainbow Engine](https://rainbow-engine.com/onamae-domain-sakuravps-apply/)

> 取得したドメインの「権限」を上位の権威DNSサーバ（ゾーン情報を保持し、自身で問い合わせに応答できるサーバ）から「委譲」してもらうための手続き

1. お名前.com の`ドメインのDNS設定` にアクセス
2. ネームサーバをさくらVPSのネームサーバに変更
3. さくらVPSの`DNS登録` にアクセス
4. さくらVPS でドメイン登録(お名前.com で取得したドメインを入力)
5. さくらVPS で登録したドメインのゾーン設定(簡単設定でさくらVPS のIPアドレスを入力)

## 名前解決できているか確認

[nslookup【コマンド】とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word12665.html)

[nslookupコマンドの使い方、初心者に必要なことだけを説明する – ノンネットワークエンジニアのネットワークハンドブック](https://network-beginners-handbook.com/nslookup/)

[コマンドプロンプト【nslookup】の使い方,見方,オプション～権限のない回答,unknown,逆引き,DNSサーバ指定方法,キャッシュについて～ \| SEの道標](https://milestone-of-se.nesuke.com/sv-basic/windows-basic/nslookup/)

ドメインとIPアドレスが紐付いているか確認できる

```
# 取得したドメインが反映されているか確認
$ nslookup karlley.com
Server:		133.242.0.3
Address:	133.242.0.3#53

Non-authoritative answer:
Name:	karlley.com # 取得したドメイン
Address: 153.120.41.146 # さくらVPS のIPアドレス
```

```
# ネームサーバが変更されているか確認
$ nslookup -type=ns karlley.com
Server:		133.242.0.3
Address:	133.242.0.3#53

Non-authoritative answer:
karlley.com	nameserver = ns2.dns.ne.jp. # 変更したネームサーバ
karlley.com	nameserver = ns1.dns.ne.jp. # 変更したネームサーバ

Authoritative answers can be found from:
```

## Virtural Host でHTML を表示する

1. Virtural Hostのデータを入れるディレクトリを作成

```
$ mkdir /home/karlley/public_html
```

2. ドメイン毎のサブディレクトリを作成

* `/home/karlley/public_html/karlley.com/public/`
* `/home/karlley/public_html/karlley.com/private`
* `/home/karlley/public_html/karlley.com/log`
* `/home/karlley/public_html/karlley.com/backup`

```
$ mkdir -p /home/karlley/public_html/karlley.com/{public,private,log,backup}
```

3. トップに表示させるHTML を作成

```
$ touch /home/karlley/public_html/karlley.com/public/index.html
```

```
# index.html
<html>
  <head>
   <title>karlley.com</title>
  </head>
  <body>
    <h1>karlley.com</h1>
  </body>
</html>
```

* デフォルトのHTML は`/var/www/html/index.nginx-debian.html`
* 前プラクティスで追加したHTMLは`/var/www/html/index.html`
* 前プラクティスで追加したサブディレクトリのHTMLは`/var/www/html/sample/index.html`

4. Virtual Host の設定ファイルを作成

* `/etc/nginx/sites-available/` ディレクトリにVirtual Host の設定ファイルをドメイン毎に作成する
* `/etc/nginx/sites-available/default` がVirtual Host のデフォルトの設定ファイルとして設置されている
* `sudo` で実行する必要がある
* `location` の`index index.php` はセキュリティーホールの原因になるので書かない

```
$ sudo touch /etc/nginx/sites-available/karlley.com
```

```
# /etc/nginx/sites-available/karlley.com 

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
```

5. Virtual Host の設定ファイルからシンボリックリンクを作成

* `/etc/nginx/sites-enabled/` ディレクトリ内にVirtual Host の設定ファイル`/etc/nginx/sites-available/karlley.com` へのシンボリックリンクを作成
* デフォルトである`/etc/nginx/sites-enable/default` も同様に`/etc/nginx/sites-available/default` へのシンボリックリンクが貼られている
* `sudo` で実行する必要有り

```
# シンボリックリンク作成
$ sudo ln -s /etc/nginx/sites-available/karlley.com /etc/nginx/sites-enabled/karlley.com

# シンボリックリンクが作成されたか確認
$ ls -n /etc/nginx/sites-enabled/
total 0
lrwxrwxrwx 1 0 0 34 Feb  9 06:24 default -> /etc/nginx/sites-available/default
lrwxrwxrwx 1 0 0 38 Mar 22 08:14 karlley.com -> /etc/nginx/sites-available/karlley.com
```

6. nginx の再起動

* nginx 起動時に`/etc/nginx/sites-enabled` を読み込むので設定を反映させるために再起動が必要
* `sudo /etc/init.d/nginx restart` だと起動しないことがあるので`stop/start` で行う

```
$ sudo /etc/init.d/nginx stop
$ sudo /etc/init.d/nginx start
```

7. nginx 再起動の確認

[【Webサーバー】Nginxが正常に起動されているか確認する](https://zerofromlight.com/blogs/detail/5/)

[nginxのworkerプロセスのチューニング \| ネットdeカガク](https://netdekagaku.com/nginx-workerproces/)

* `master` と`worker` 2つのプロセスがあればok
  * `master`: rootで動作する、`worker` プロセスを起動して設定の読み込み、ソケットの管理等を行う
  * `woker`: クライアントからのリクエストを処理する
* 環境にもよるがプロセスが複数存在していてもok

```
$ ps ax | grep nginx
130171 ?        Ss     0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
130173 ?        S      0:00 nginx: worker process
131804 pts/0    S+     0:00 grep nginx
```

ポートの確認

* HTTP: `80`
* HTTPS: `443`
* SSH: `20022` (22番から変更済み)

```
$ ss -natu | grep LISTEN
tcp   LISTEN   0      511           0.0.0.0:80            0.0.0.0:*
tcp   LISTEN   0      128           0.0.0.0:20022         0.0.0.0:*
tcp   LISTEN   0      511              [::]:80               [::]:*
tcp   LISTEN   0      128              [::]:20022            [::]:*
```

8. HTML の表示を確認

* `karlley.com` にブラウザでアクセスしてHTML が表示されていればok
* ブラウザでアクセス後にアクセスログを確認

```
$ cat /home/karlley/public_html/karlley.com/log/access.log
...
180.16.6.5 - - [23/Mar/2022:04:45:04 +0900] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36"
```

## Virtural Host で複数のドメインのHTML を表示する

`karlley.com` と`www.karlley.com` の2つのドメインで別々のHTML を表示する

1. 追加するドメインのサブディレクトリを追加

* `/home/karlley/public_html/www.karlley.com/public/`
* `/home/karlley/public_html/www.karlley.com/private`
* `/home/karlley/public_html/www.karlley.com/log`
* `/home/karlley/public_html/www.karlley.com/backup`

```
$ mkdir -p /home/karlley/public_html/www.karlley.com/{public,private,log,backup}
```

2. 追加するドメインで表示するHTML を追加

```
$ touch /home/karlley/public_html/www.karlley.com/public/index.html
```

```
# index.html
<html>
  <head>
   <title>www.karlley.com</title>
  </head>
  <body>
    <h1>www.karlley.com</h1>
  </body>
</html>
```

3. Virtual Host の設定ファイルに追加するドメインの設定を追加 

`/etc/nginx/sites-available/karlley.com` に追記

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

# 下記を追記
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
```

4. 追加したドメインの名前解決を確認

* `Server:` にIPv6 のアドレスが表示されている
* `Address`: にIPV6 のアドレス + `#xx` 等の番号が表示されている
* ドメイン毎に`Name:` に指定したドメインが表示されている
* Virtual Host で1つのホストに2つのドメインを表示させるので`Address:` が2つのドメイン共通のIPアドレスになっている

```
$ nslookup
> karlley.com
Server:		2400:4152:181:4200:225:36ff:fe9a:4fb8
Address:	2400:4152:181:4200:225:36ff:fe9a:4fb8#53

Non-authoritative answer:
Name:	karlley.com
Address: 153.120.41.146

> www.karlley.com
Server:		2400:4152:181:4200:225:36ff:fe9a:4fb8
Address:	2400:4152:181:4200:225:36ff:fe9a:4fb8#53

Non-authoritative answer:
Name:	www.karlley.com
Address: 153.120.41.146
```

4. nginx の再起動、プロセスの確認

```
$ sudo /etc/init.d/nginx stop
$ sudo /etc/init.d/nginx start
$ ps ax | grep nginx
```

## ドメイン名のwww有り/無しのDNS設定について

[ネームサーバ利用申請 – さくらのサポート情報](https://help.sakura.ad.jp/206207381/)

* さくらVPS VPSの簡単設定では`mail`や`www`なども自動でゾーン設定をしてくれる
* `www` 有りのDNS設定は簡単設定を行った場合は不要、nginx の設定ファイルで表示させるHTMLの設定を追加するのみ
* `www` 有りが上手く表示されていない場合は`nslookup` コマンドで名前解決できているか再確認する
* `www` のゾーン設定はタイプは`CNAME`、データは`@` でok

## DNSゾーン

[DNSゾーンとは何ですか？ \| Cloudflare](https://www.cloudflare.com/ja-jp/learning/dns/glossary/dns-zone/)

* ドメイン名の管理において、DNSサーバ(ネームサーバ)が管理するドメイン範囲
* ゾーンの情報はゾーンファイルに保存される

## ゾーンファイル

> ゾーンファイルは、DNSサーバーに保存されているプレーンテキストファイルで、ゾーンの内容が記載され、ゾーン内のすべてのドメインのすべてのレコードが含まれています。

## レコードの種類

[ゾーンファイル \(zone file\)とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word12283.html)

* SOA: ゾーンに関する情報
* NS: DNSサーバの名前
* A: ドメインをIPアドレスに置き換えるレコード、基本的なレコード
* MX: メールエクスチェンジの略、メールサーバのホスト名を記載されている
* CNAME: キャノニカルネームの略、ドメインを別のドメインに置き換えるレコード、特定のドメインを別のドメインに転送する場合に使用

## nginx、Virtural Host のディレクトリ構成

nginx 全体のディレクトリ構成

```
/
├── etc
│  ├── init.d
│  │  ├── nginx # nginx の実行ファイル
│  ├── nginx
│      ├── nginx.conf # nginx の設定ファイル、include でsites-enabled から読み込むファイルを指定
│      ├── sites-available # Virtual Host の設定ファイル
│      │   ├ default 
│      │   ├ karlley.com # 設定ファイルの実体
│      ├── sites-enabled # sites-available 内の設定ファイル読込用のシンボリックリンクのディレクトリ
│           ├ default 
│           ├ karlley.com # 設定ファイルへのシンボリックリンク
├── home
│  └── karlley # ユーザ
│      └── public_html
│          ├── karlley.com # ドメイン1
│          │   ├── backup
│          │   ├── log
│          │   │   ├── access.log
│          │   │   └── error.log
│          │   ├── private
│          │   └── public
│          │       └── index.html # ドメイン1で表示するHTML
│          └── www.karlley.com # ドメイン2
│              ├── backup
│              ├── log
│              │   ├── access.log
│              │   └── error.log
│              ├── private
│              └── public
│                  └── index.html # ドメイン2で表示するHTML
├── var
│  ├── www
│      ├── html
│          ├── nginx-debian.html # デフォルトのHTML
│          ├── index.html # デフォルトから変更したHTML
│          ├── sample
│              └── index.html # 小ディレクトリのHTML
```

Virtual Host のディレクトリ構成

```
/home/karlley
└── public_html
    ├── karlley.com # ドメイン1
    │   ├── backup
    │   ├── log
    │   │   ├── access.log
    │   │   └── error.log
    │   ├── private
    │   └── public
    │       └── index.html # ドメイン1 のHTML
    └── www.karlley.com # ドメイン2
        ├── backup
        ├── log
        │   ├── access.log
        │   └── error.log
        ├── private
        └── public
            └── index.html # ドメイン2 のHTML
```

## Debianにtree コマンドをインストール

Debian に`tree` コマンドを`apt` 経由でインストール

```
$ apt list sudo
$ sudo apt install tree
$ apt list --installed | grep tree
```

ディレクトリの確認

```
$ cd dir_name
$ tree
```

ディレクトリの深さを指定して確認

```
$ tree -L 深さを数値で指定
```
