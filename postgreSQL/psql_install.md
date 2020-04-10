# postgreSQL

https://www.postgresql.org/

学習の為にmacローカルにbrewでインストール

## install

```
$ brew update
$ brew install postgreSQL
$ psql --version
psql (PostgreSQL) 12.2
```

## path

```
$ which psql
/usr/local/bin/psql
$ which postgresql
/usr/local/bin/postgres
```
- どちらのコマンドも`/user/local/bin` 
- brew でインストールした場合はPATH設定は不要なのかもしれない
(/usr/local/bin/..でokなのかも)

## start/stop

start

```
$ brew services start postgresql
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
```

status check

```
$ brew services list
Name       Status  User    Plist
postgresql started karlley /Users/karlley/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
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
```

restart

```
$ brew services restart postgresql
==> Successfully stopped `postgresql` (label: homebrew.mxcl.postgresql)
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
```

stop

```
$ brew services stop postgresql
Stopping `postgresql`... (might take a while)
==> Successfully stopped `postgresql` (label: homebrew.mxcl.postgresql)
$ brew services list
Name       Status  User Plist
postgresql stopped
```

- `postgresql` コマンドと`psql` コマンドは微妙に挙動が違うので注意