# db console

* DB の中身を直接見れる
* SQL を実行できる
* DB の種類によって操作が異なる
* MySQL, PostgreSQL, SQLite, SQLite3 をサポートしている

## 起動/終了

```Shell
# 起動
$ rails db
$ rails dbconsole

# 終了
>\q
>exit
```

DB の環境を指定して実行できる

```Shell
# test 環境を指定
$ rails db test

# production 環境を指定
$ rials db productionj
```

## PostgreSQL での操作

```Shell
# DB一覧
> \l

# ユーザ一覧
> \du

# テーブル一覧
> \dt
> \d

# データ一覧
> \d table_names
```

## SQL での操作

**SQL 文の最後にセミコロン(;)が必要**

```SQL
# カラム表示
> select column_name1, column_name2 form table_name;
```
