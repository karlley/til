# PostgreSQL

# データベース

* データベース: 大量のデータをコンピュータシステム上で扱えるように集めたもの
* データベース管理システム: データベースを効率よく読み書きするためのソフトウェア
* DBMS: DataBase Management System

# PostgreSQL

> フリーなオープンソースのリレーショナルデータベース管理システム

# リレーショナルデータベース

> あらゆるデータは表であらわす  

* テーブル: 表、リレーション
* カラム: テーブルを縦割りした部分、属性、列、同じ表の中に同じ名前のカラムはNG
* フィールド: テーブルの横割りした部分、レコード、タプル、行

# データベースの特徴

* 1つのフィールドに入れることができるデータは1つ
* フィールドの連結/分割はできない
* フィールド(レコード)の順番には意味は無い
* データ読出し時にデータの順番を並べ替える方法がある

# リカバリシステム

* クラッシュリカバリ: ログを使ってデータベースを最新の状態に復旧する
* アーカイブリカバリ: バックアップとログを保存しておき、これらを使ってデータベースを復元すること

# トランザクション

* アプリケーションにとって意味のある処理の単位で処理を進めること
* 処理の途中で異常が発生した場合は処理の単位でロールバックする
* 複数の処理が並行して行なわれてもデータの整合性が保たれる

# Debian にPostgreSQL をインストールする

インストール対象のLinux: Debian 11

[Debian 8\.7\(Jessie\)にPostgreSQL 9\.6をインストールし、外部接続を許可する \- Symfoware](https://symfoware.blog.fc2.com/blog-entry-1948.html)

上記URL を参考にしてリポジトリの追加、認証キー追加後に下記のインストールコマンドでインストールできなかった

```
# apt-get install postgresql-9.6
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package postgresql-9.6
E: Couldn't find any package by glob 'postgresql-9.6'
E: Couldn't find any package by regex 'postgresql-9.6'
```

バージョン指定無しでインストールしたところインストールできた

```
# apt-get install postgresql
...
# su - postgres
~$ psql
psql (13.5 (Debian 13.5-0+deb11u1))
Type "help" for help.

postgres=#
```

インストールを確認

```
$ apt list --installed | grep postgresql

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

postgresql-13/stable,stable-security,now 13.5-0+deb11u1 amd64 [installed,automatic]
postgresql-client-13/stable,stable-security,now 13.5-0+deb11u1 amd64 [installed,automatic]
postgresql-client-common/stable,now 225 all [installed,automatic]
postgresql-common/stable,now 225 all [installed,automatic]
postgresql/stable,now 13+225 all [installed]

$ psql --version
psql (PostgreSQL) 13.5 (Debian 13.5-0+deb11u1)
```

# `su - postgres` の`-` について

`su` コマンドの`-` の有無で動作が異なる

* `-` 有り: カレントディレクトリ、環境変数がが初期化
* `-` 無し: コマンド実行前のカレントディレクトリ、環境変数を引き継ぎ管理者権限でコマンドを実行する

# DB に接続するまでの流れ

[postgresとして、ログインできないです](https://teratail.com/questions/243773)

* PostgreSQL はデフォルトで`postgres` という名前でデータベース、ロールが作成されている
* `CREATE ROLE` でロールを作成できる
* 作成したロールを使用すれば誰でもDBにアクセスできる
* ロールは各DBに対して操作できるユーザみたいなもの
* `-U` を省略すると現在のユーザ名と同名のロールでDBにアクセスする
* 引数無しで`psql` コマンドを実行した場合はログインユーザと同名のロール、DBにアクセスする
* ロール名、DB名を指定してアクセスする場合は`psql -U ロール名 DB名`

postgres ロールでDB に接続する流れ

1. root ユーザに切り替え
2. postgres ユーザに切り替え
3. `psql` コマンドでDBにアクセス

```
# root ユーザに切り替え
$ su

# root ユーザ からpostgres ユーザに切り替え
root@...# su - postgres

# postgres ユーザでpostgres ロールでpostgres DB に接続
postgres@...# psql -U postgres

# -U を省略すると現在のユーザと同名のロールでpostgres という名前のDBにアクセスする(この場合だとpostgres ロール)
postgres@...# psql postgres

# 引数無しだとユーザ名と同名のロール、DBにアクセスする
postgres@...# psql

# デフォルトで作成されるDB、ロールを確認(postgres DB、postgres ロール が作成されている)
postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(3 rows)

postgres=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```

# PostgreSQL をDebian にインストール

参考URLでのインストールの手順がいまいち理解できなかったので、公式を参考に再度インストール

1. インストール先のDebian のバージョン確認

参照: [LinuxにPostgreSQLをインストールする際のエラーの解決策 \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/questions/769)

```
# Debian のバージョン確認
$ cat /etc/debian_version
11.3
```

2. [PostgreSQLの公式](https://www.postgresql.org/download/linux/debian/) でDebian のバージョンが対応しているか確認(さくらVPSのカスタムOSは`Debian 11 amd64`をインストール済み)

3. リポジトリ追加、公開鍵追加

```
# リポジトリを追加
$ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# リポジトリの追加を確認
$ cat /etc/apt/sources.list.d/pgdg.list
deb http://apt.postgresql.org/pub/repos/apt bullseye-pgdg main
```

4. 公開鍵の追加で`gnupg`、`gnupg2`どちらかのインストールを求められる場合は追加でインストール後、再度公開鍵を追加

参照: [【🐹 \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/reports/38699)

```
# 公開鍵を追加
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
E: gnupg, gnupg2 and gnupg1 do not seem to be installed, but one of them is required for this operation # よく分からない

# 公開鍵を追加でgnupg/gnupg2 どちらかのツールのインストールを求められたので追加
$ sudo apt install gnupg

# gnupg のインストールを確認
$ apt list --installed | grep gnupg

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

gnupg-l10n/stable,now 2.2.27-2+deb11u1 all [installed,automatic]
gnupg-utils/stable,now 2.2.27-2+deb11u1 amd64 [installed,automatic]
gnupg/stable,now 2.2.27-2+deb11u1 all [installed]

# 再度、公開鍵を追加
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
OK
```

5. PostgreSQL インストール 

```
# パッケージ更新
$ sudo apt-get update

# インストール
$ sudo apt-get -y install postgresql

# インストール確認
$ apt list --installed | grep postgresql

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

postgresql-14/bullseye-pgdg,now 14.2-1.pgdg110+1 amd64 [installed,automatic]
postgresql-client-14/bullseye-pgdg,now 14.2-1.pgdg110+1 amd64 [installed,automatic]
postgresql-client-common/bullseye-pgdg,now 238.pgdg110+1 all [installed,automatic]
postgresql-common/bullseye-pgdg,now 238.pgdg110+1 all [installed,automatic]
postgresql/bullseye-pgdg,now 14+238.pgdg110+1 all [installed]

$ psql --version
psql (PostgreSQL) 14.2 (Debian 14.2-1.pgdg110+1)
```

6. psql 起動

```
# インストールで作成されｔらpostgres ユーザに切替
$ sudo su - postgresql

# postgres ユーザでpsql を起動
postgres@...$ psql
psql (14.2 (Debian 14.2-1.pgdg110+1))
Type "help" for help.

postgres=#
```

# PostgreSQL のバージョンを変更したらpsql が起動しなくなった

PostgreSQL のバージョンを変更した際に下記のエラーが発生

原因: 旧バージョンの不要なファイルが残っていた

```
# postgres ユーザに切替
$ sudo su - postgresql

# 接続
postgres@...$ psql
psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: No such file or directory
	Is the server running locally and accepting connections on that socket?
```

psql サーバの起動確認

* 正常: Status が`online`
* 異常: Status が`down`

```
# 削除したはずのバージョン13のステータスが表示されている
$ pg_lsclusters
Ver Cluster Port Status                Owner     Data directory              Log file
13  main    5432 down,binaries_missing <unknown> /var/lib/postgresql/13/main /var/log/postgresql/postgresql-13-main.log
14  main    5433 online                postgres  /var/lib/postgresql/14/main /var/log/postgresql/postgresql-14-main.log
```

不要なバージョンのディレクトリを削除

```
$ sudo rm -rf /etc/postgresql/13/
```

psql サーバ再起動

```
$ sudo service postgresql restart
```

再度、psqlサーバの起動確認

```
# 正常にバージョン14のみが起動している
$ pg_lsclusters
Ver Cluster Port Status Owner    Data directory              Log file
14  main    5433 online postgres /var/lib/postgresql/14/main /var/log/postgresql/postgresql-14-main.log
```

psql に接続

```
# postgres ユーザに切替
$ sudo su - postgres

# 接続
postgres@...$ psql
psql (14.2 (Debian 14.2-1.pgdg110+1))
Type "help" for help.

postgres=#
```

[DebianにPostgreSQLをインストールした後、ユーザーを作成できな \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/questions/985)

[Postgresqlに接続できない時の対処法 \- Qiita](https://qiita.com/Dexctersu/items/3d6bc50bf1d4a294980b)

[PostgreSQL で psql コマンド を実行したら could not connect to database postgres: could not connect to server: No such file or directory と出る場合 \- 約束の地](https://obel.hatenablog.jp/entry/20181027/1540588239)

[service postgresql status](https://femoghalvfems.info/archives/20776)

# PostgreSQL のアンスインストールでの注意点

[UbuntuでPostgresql の削除 \- Qiita](https://qiita.com/ho_soft/items/5136bc0dcee2ca110d8a)

[dpkg \-l \| grep postgres](https://postgresweb.com/uninstall-postgresql-ubuntu2004)

アンインストール時には設定ファイルも含めて削除する

1. 削除対象のファイルを検索
2. 依存ライブラリ、設定ファイルも含めてremove する
3. 削除対象のファイルが削除されているか確認

```
# 削除対象のファイルを検索
$ dpkg -l | grep postgresql
ファイル名

# remove
$ sudo apt remove --purge ファイル名

# 削除されたか確認
$ dpkg -l | grep postgresql
```

# apt の削除コマンドの違い

[apt\-get のpurgeとremoveについて](https://teratail.com/questions/119697)

* `apt remove`: パッケージを削除、設定ファイルは削除しない(再度インストールすると設定が引継がれる)
* `apt purge`: パッケージ、設定ファイルを削除
* `apt remove --purge`: `apt purge` と同じ
* `apt autoremove`: 依存関係の無い不要なパッケージを自動削除

# 外部接続設定

外部接続の設定を下記の2つに追加

* `/etc/postgresql/14/main/postgresql.conf`: listen するIPアドレスを設定
* `/etc/postgresql/14/main/pg_hba.conf`: 接続を許可するIPアドレスを設定

## postgresql.conf

全IPアドレスからlisten する設定を追加

* `*` で全てのIPアドレスからのアクセスを許可
* 変更では無く追記すること

```
$ sudo vim /etc/postgresql/14/main/postgresql.conf
```

```
# /etc/postgresql/14/main/postgresql.conf

#listen_addresses = 'localhost'         # what IP address(es) to listen on;
listen_addresses = '*' # 追記
```

ちなみにポートの設定は変更しなくてok

* コメントアウトされている状態でデフォルトの`5432` が設定されている
* ポートを変更する場合はコメントアウトを外してポート番号を指定する

```
# /etc/postgresql/14/main/postgresql.conf

#port = 5432                             # (change requires restart)
```

## pg_hba.conf 

アクセスするmac のグローバルIPアドレスを調べる

* `ipconfig` で表示されるIPアドレスはローカルIPアドレスなので設定しないこと
* sshで接続していない状態で行う(ローカルのターミナルで実行)

```
# アクセスするmacのIPアドレス
$ curl inet-ip.info
xxx.xxx.xxx.xxx
```

macのIPアドレスからの接続を許可する

* アクセスする側のIPアドレスを指定する 
* ポート番号は`0`
* 全てのIPアドレスからの接続を許可する場合は`0.0.0.0/0` を指定する

```
$ sudo vim /etc/postgresql/14/main/pg_hba.conf
```

```
# /etc/postgresql/14/main/pg_hba.conf 

# IPv4 local connections:
host    all             all             127.0.0.1/32            scram-sha-256
host    all             all             xxx.xxx.xxx.xxx/0       scram-sha-256 # 追記
```

PostgreSQL を再起動

```
$ sudo service postgresql restart
```

# 外部接続を確認

ssh で接続していない状態でさくらVPSのPostgreSQL にアクセス

```
# ホスト名でアクセス
$ psql -U karlley -d postgres -h ik1-215-78392.vs.sakura.ne.jp
Password for user karlley: #外部接続用ロールで設定したパスワード
psql (14.2)
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

# IPアドレスとポート番号でアクセス
$ psql -U karlley -d postgres -h 153.120.41.146 -p 5432
```

# 192.168.x.x とは

[なぜ「192\.168\.x\.x」のアドレスを使う？ \| 日経クロステック（xTECH）](https://xtech.nikkei.com/it/free/NNW/NETHOT/20040624/146335/)

[プライベートIPアドレスとは](https://www.pc-master.jp/internet/private-ip-address.html)

* `192.168.x.x` はプライベート・アドレス
* プライベート・アドレスを割り当てたコンピュータが間違ってインターネットにつながっても何も起こらない
* プライベート・アドレスのホストはインターネット上に存在しない
* プライベートIPアドレス == ローカルIPアドレス
* プライベートIPアドレスの種類
	* `10.0.x.x`: クラスA、大規模ネットワーク向け
	* `172.16.x.x`: クラスB、中規模ネットワーク向け
	* `192.168.x.x`: クラスC、小規模ネットワーク向け、ルータやPCで割当られる

# サブネットマスク

[サブネットマスク \(subnet mask\)とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word11975.html)

> IPアドレスのどこからどこまでが「どのネットワークですよ～」な情報で、どこからどこまでが「どのコンピュータですよ～」な情報かを示す情報

`192.168.1.11` を例にする

* ネットワーク部: `192.168.1` まで
* ホスト部: `11`、各PCや機器に割当られる
* ネットワーク部、ホスト部を組み合わせるルールをサブネットマスクと呼ぶ
* サブネットマスクは大規模、中規模のみで変更する場合が殆ど

# 127.0.0.1 とは

[127\.0\.0\.1とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word17065.html)

[127\.0\.0\.1とlocalhostと0\.0\.0\.0の違い \- Qiita](https://qiita.com/1ain2/items/194a9372798eaef6c5ab)

* 自分の今使っているコンピュータのIPアドレス
* ループバックアドレス
* 同一ホスト内でしか通信を行なわない
* `localhost` と同意義
* `0.0.0.0` とは異なる(全ての通信を表す、ワイルドカードのようなもの)

# pg_hba.conf に設定する認証方式の種類

認証方式にはいくつか種類がある

* `scram-sha-256`: 現在提供されている中では最も安全
* `md5`: 安全性は低い
* `peer`: OSのユーザー名を取得してDBのユーザーとして使う認証方式

# ロールの作成/削除

OSのコマンドでロールの作成/削除を行う

* 作成: `createuser`
* 削除: `dropuser`

```
# ロール作成
$ createuser --pwprompt --interactive hoge
Enter password for new role: # ロールのパスワード
Enter it again: # パスワード再入力
Shall the new role be a superuser? (y/n) y # スーパーユーザに設定

# 接続確認
$ psql -U hoge -d postgres
Password for user hoge:
psql (14.2 (Debian 14.2-1.pgdg110+1))
Type "help" for help.

postgres=#

# ロールの作成を確認
postgres=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
 hoge      | Superuser, Create role, Create DB                          | {}
 karlley   | Superuser, Create role, Create DB                          | {}
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```

```
# ロール削除
$ dropuser hoge
Password: # 管理者のパスワード

# ロールの削除を確認
$ psql -U karlley -d postgres
Password for user karlley:
psql (14.2 (Debian 14.2-1.pgdg110+1))
Type "help" for help.

postgres=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
 karlley   | Superuser, Create role, Create DB                          | {}
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```

psql を立ち上げた状態でSQLでロールの作成/削除を行う場合は下記のSQL文を使用する

* 作成: `CREATE USER`
* 削除: `DROP USER`

# 作成したロールでログイン時に認証キーでエラー

ロール`hoge` でDB `postgres` に接続

```
$ psql -U hoge -d postgres
psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  Peer authentication failed for user "hoge"
```

* ログイン時の認証`Peer` でエラー
* 今回設定した認証方式は`Peer` ではなく`scram-sha-256` を使っている

`/etc/postgresql/14/main/pg_hba.conf` のログイン時の認証方式を修正

```
$ sudo vim /etc/postgresql/14/main/pg_hba.conf
```

```
# /etc/postgresql/14/main/pg_hba.conf

# "local" is for Unix domain socket connections only
#local    all             all                        peer # ここをコメントアウトか削除する
local    all             all                        scram-sha-256 # 追記
```

再起動

```
$ sudo service postgresql restart
```

接続できるようになる

```
$ psql -U hoge -d postgres
Password for user hoge:
psql (14.2 (Debian 14.2-1.pgdg110+1))
Type "help" for help.

postgres=#
```