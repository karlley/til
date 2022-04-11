# sql

SQL の基本

# SQL とは

* Structured Query Language
* リレーショナルデータベース(RDBMS)を操作する為の言語
* 標準SQL: ISO(国際標準化機構)で定められた標準規格を満たしたSQL
* SQL には方言のようなものが存在する(標準SQL以外)

# SQL の機能の分類

* DDL
  * Data Definition Language
  * データ定義言語
  * テーブルを作成、削除
* DML
  * Data Manipulation Language
  * データ操作言語
  * データの挿入、更新、削除
* DCL
  * Data Control Language
  * データ制御言語
  * トランザクション、権限関係

# SQL の記述ルール

* 最後に`;` を付ける
* 大文字/小文字の区別無し
* キーワード間にスペース、タブ、改行が使える
* 全角スペースは使えない
* 定数は`'` で囲む

# PostgreSQL をmac にインストール

`brew` 経由でインストール

```
# インストール
$ brew update
$ brew install postgresql
$ psql --version
psql (PostgreSQL) 14.2

# path 確認
$ which psql
/usr/local/bin/psql
$ which postgresql
/usr/local/bin/postgres

# 起動
$ brew services start postgresql
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
$ brew services list
Name       Status  User    File
postgresql started karlley ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

# DB確認
$ psql -l
                           List of databases
   Name    |  Owner  | Encoding | Collate | Ctype |  Access privileges
-----------+---------+----------+---------+-------+---------------------
 postgres  | karlley | UTF8     | C       | C     |
 template0 | karlley | UTF8     | C       | C     | =c/karlley         +
           |         |          |         |       | karlley=CTc/karlley
 template1 | karlley | UTF8     | C       | C     | =c/karlley         +
           |         |          |         |       | karlley=CTc/karlley
(3 rows)

# 再起動
$ brew services restart postgresql
Stopping `postgresql`... (might take a while)
==> Successfully stopped `postgresql` (label: homebrew.mxcl.postgresql)
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)

# 停止
$ brew services stop postgresql
Stopping `postgresql`... (might take a while)
==> Successfully stopped `postgresql` (label: homebrew.mxcl.postgresql)
$ brew services list
Name       Status User File
postgresql none
```

# psql コマンド実行時のエラー

postgresql インストール直後に`psql` コマンドで以下のエラーが発生

```
$ psql
psql: error: connection to server on socket "/tmp/.s.PGSQL.5432" failed: FATAL:  database "karlley" does not exist
```

[PostgreSQL: Documentation: 14: psql](https://www.postgresql.org/docs/current/app-psql.html)

[PostgreSQLの環境構築ができないFATAL: database "t" does not exist](https://teratail.com/questions/cgzz8whsbg0tc6)

> psql コマンドを引数なしで実行するとユーザー名と同じ名前のデータベースに接続しようとする

* `psql` コマンドを引数無しで実行するとユーザ名のデータベースに接続しようとする > ユーザ名と同じデータベースが無いのでエラーが発生
* `psql` コマンドに引数でデータベース名を渡すことでエラーが解消する
* ユーザ名と同名のデータベースを作成すると`psql` コマンドに引数無しでユーザ名と同名のデータベースに接続するのでエラーが解消する
* 引数無しで`psql` コマンドを実行した際のデフォルトのデータベース、ユーザ名等は環境変数を設定することで指定可能

```
$ psql
psql: error: connection to server on socket "/tmp/.s.PGSQL.5432" failed: FATAL:  database "karlley" does not exist

# ユーザ名と同名のデータベースを作成
$ createdb karlley

# ユーザ名と同名のデータベースに接続するのでエラーが発生しない
$ psql
psql (14.2)
Type "help" for help.

# データベースの作成を確認(一番上が作成したデータベース)
karlley=# \l
                           List of databases
   Name    |  Owner  | Encoding | Collate | Ctype |  Access privileges
-----------+---------+----------+---------+-------+---------------------
 karlley   | karlley | UTF8     | C       | C     |
 postgres  | karlley | UTF8     | C       | C     |
 template0 | karlley | UTF8     | C       | C     | =c/karlley         +
           |         |          |         |       | karlley=CTc/karlley
 template1 | karlley | UTF8     | C       | C     | =c/karlley         +
           |         |          |         |       | karlley=CTc/karlley
 testdb    | karlley | UTF8     | C       | C     |
(5 rows)
```

# データベースに接続

ユーザ `karlley` でデータベース`template1` に接続

* postgres ユーザは無いっぽい?
* スーパーユーザであればプロンプトが`#` になる
* 一般ユーザであればプロンプトは`=>` になる

```
$ psql -U karlley template1
psql (14.2)
Type "help" for help.

template1=#
```

# psql の抜け方

以下のどちらかでpsql を抜けれる

* `ctl + D`
* `\q`

# バックスラッシュコマンド

バックスラッシュコマンドの基本操作

* psql 終了: `\q`
* バックスラッシュコマンドのヘルプ: `\?`
* SQL のヘルプ: `\h`
* DB 一覧: `\l`
* ユーザ一覧: `\du`

# データベース作成

postgresql でデータベースを作成する方法は2つある

* postgresql にログインして`CREATE DATABASE`
* ターミナルで`createdb` コマンドを使う

## postgresql にログインしてデータベースを作成

データベース`testdb` を作成する

1. psql を起動する為にデータベース`postgres` に接続(接続するデータベースは他でもok)
2. データベース作成
3. データベースが作成されているか確認
4. 作成したデータベースに切り替える 

```
# データベースpostgres に接続
$ psql -d postgres

# データベース作成
# CREATE DATABASE testdb;
CREATE DATABASE

# データベースの作成を確認
# \l
                           List of databases
   Name    |  Owner  | Encoding | Collate | Ctype |  Access privileges
-----------+---------+----------+---------+-------+---------------------
 postgres  | karlley | UTF8     | C       | C     |
 template0 | karlley | UTF8     | C       | C     | =c/karlley         +
           |         |          |         |       | karlley=CTc/karlley
 template1 | karlley | UTF8     | C       | C     | =c/karlley         +
           |         |          |         |       | karlley=CTc/karlley
 testdb    | karlley | UTF8     | C       | C     |
(4 rows)

# 作成したtestdb に切り替え

```

## ターミナル上でcreatedb コマンドでデータベースを作成

[PostgreSQL: Documentation: 14: createdb](https://www.postgresql.org/docs/14/app-createdb.html)

```
$ createdb データベース名
```
