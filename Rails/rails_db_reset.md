# rails_db_reset_20210225

rails のdb をリセットする方法

## コマンド

Local

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

Heroku

```Shell
# heroku db を削除
$ heroku pg:reset DATABASE

# heroku マイグレーション
$ heroku run rails db:migrate

# heroku サンプル用データを生成
$ heroku run rails db:seed
```
