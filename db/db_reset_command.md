# db_reset_command

DB をリセットする`rails db:reset` と`rails db:migrate:reset` の違い

## 参照

[rails db:resetとrails db:migrate:resetの違い \- Qiita](https://qiita.com/d0ne1s/items/80f61608fa4947a337cd)

[db:reset と db:migrate:reset の違いについて、ささいなこと。 \- Qiita](https://qiita.com/mochizukikotaro/items/7ebdfc199cd7b06eac29)

## ポイント

* `rails db:reset`: データ消去、`migrate`、`seed` を実行
* `rails db:migrate:reset`: データ消去、`migrete` を実行
* 環境別に`seed` ファイルを用意している場合は`rails db:migrate:reset` を使用して1つずつ作業する必要あり
