# db_reset

rails のdb をリセットについて

1. local db リセット
2. local seed データ生成
3. local test 環境 seed データ生成
4. Heroku db リセット
5. Heroku seed データ生成

## 参照

[本番環境でrake db:seedを使ったデータ投入 \- Qiita](https://qiita.com/Sotq_17/items/a091fe92dd64d3cf429b)

## コマンド

* local
* local test
* Heroku

### local

local db リセット(developmant, test がリセットされる)

```Shell
# db のデータを削除
$ rails db:reset

# db のデータを削除 > マイグレーション
$ rails db:migrate:reset
```

local seed データ生成(development のみ)

```Shell
# 初期データ生成
$ rails db:seed

# rails db:seed が通らない時
$ rails db:setup
```

### local test 環境

local test 環境 マイグレーション(リセットはdevelop と同時に実行済みの場合)

```Shell
$ rails db:migrate RAILS_ENV=test
```

local test 環境 seed データ生成

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
