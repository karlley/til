# db_reset

rails のdb をリセットについて

1. Local db リセット
2. Local seed データ生成
3. Local test 環境 seed データ生成
4. Heroku db リセット
5. Heroku seed データ生成

## 参照

[本番環境でrake db:seedを使ったデータ投入 \- Qiita](https://qiita.com/Sotq_17/items/a091fe92dd64d3cf429b)

## コマンド

* Local
* Local test
* Heroku
*
### Local

Local db リセット

```Shell
# db のデータを削除
$ rails db:reset

# db のデータを削除 > マイグレーション
$ rails db:migrate:reset
```

Local seed データ生成

```Shell
# 初期データ生成
$ rails db:seed

# rails db:seed が通らない時
$ rails db:setup
```

### Local test 環境

Local test 環境 seed データ生成

```Shell
# test 環境でseed データ生成
$ rails db:seed RAILS_ENV=test

# test 環境でコンソールを起動
$ rails console -e test

# コンソールの環境を確認
> Rails.env
"test"

# 必要に応じてテストデータが生成されているかコンソールで確認
```

### Heroku

Heroku db リセット

```Shell
# heroku db を削除
$ heroku pg:reset DATABASE

# heroku マイグレーション
$ heroku run rails db:migrate
```

Heroku seed データ生成

```Shell
# heroku サンプル用データを生成
$ heroku run rails db:seed
```
