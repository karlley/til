# rails_db_reset_migrate_seed

db のreset, migrate, seed の手順

## 参照

[RailsのDBとmigrationファイルとschemaの関係 \- Qiita](https://qiita.com/kakiuchis/items/2ed1604557ee29bbcbf7)

[rails generate migrationコマンドまとめ \- Qiita](https://qiita.com/zaru/items/cde2c46b6126867a1a64)

[Rails カラム名変更方法 \- Qiita](https://qiita.com/libertyu/items/93acd8733e34b1d0a63c)

## 手順

```Shell
# db のデータを削除
$ rails db:reset
# db のデータを削除 > マイグレーション
$ rails db:migrate:reset
# サンプル用データを生成
$ rails db:seed
# rails db:seed が通らない時
$ rails db:setup
```

```Shell
# Heroku
# heroku db を削除
$ heroku pg:reset DATABASE
# heroku マイグレーション
$ heroku run rails db:migrate
# heroku サンプル用データを生成
$ heroku run rails db:seed
```

rails db:migrate やり直し

```Shell
$ rails db:rollback #一つ前に戻る
$ rails db:migrate VERSION=0 #一番最初に戻る
```

rails レコード 全削除

```Shell
$ rails db:migrate:reset
```

