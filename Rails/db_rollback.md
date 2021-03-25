# db_rollback

rails のロールバックのやり方

## 参照

[【Rails】$rails db:rollbackしたい時の間違えない手順 \- Qiita](https://qiita.com/s_tatsuki/items/3e1f119c91e21b8f0c33)

[rails db:rollbackって一つずつしか差し戻せないんだよって話 \- Qiita](https://qiita.com/ShYaruki/items/37007e97204705d5e87c)

## 手順

```Shell
# migrate のバージョン確認
$ rails db:version
$ rails db:migrate:status

# 1つ前に戻す
$ rails db:rollback

# 任意のバージョンに戻す(number+1 のバージョンまで戻す)
$ rails db:rollback STEP=number

# 状態確認
$ rails db:migrate:status
```

1. ロールバックできた時点で目的のマイグレーションファイルを修正
2. 再度`migrate` して全てのstatus がup になっていることを確認する

## 注意点

* `rollback` は直近のマイグレーションを修正する程度にする
* マイグレーションを変更したい時は変更用のマイグレーションファイルを作成して対応するのが基本
* `rollback` したままだとエラーが発生する

```Ruby
# rollback したままだと発生するエラー
ActiveRecord::PendingMigrationError - Migrations are pending. To resolve this issue, run:

        bin/rails db:migrate RAILS_ENV=development
```
