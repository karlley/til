# route_collection_member

ルーティングで使用する`collection` と`member` について

## 参照

[Rails のルーティング \- Railsガイド](https://railsguides.jp/routing.html#%E3%83%A1%E3%83%B3%E3%83%90%E3%83%BC%E3%83%AB%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0%E3%82%92%E8%BF%BD%E5%8A%A0%E3%81%99%E3%82%8B)

[railsのroutes\.rbのmemberとcollectionの違いは? \- Qiita](https://qiita.com/k152744/items/141345e34fc0095217fe)

[Railsのルーティングを極める \(後編\)｜TechRacho（テックラッチョ）〜エンジニアの「？」を「！」に〜｜BPS株式会社](https://techracho.bpsinc.jp/baba/2020_11_20/15619#1-6)

## ポイント

* `resouces` で自動生成される7つのルーティング(`index, create, new, edit, show, update, destroy`) 以外のアクションをルーティングに追加したい場合に使用
* `collection`、`member` でルーティングを設定するとurl ヘルパーも自動生成される
* `member` はルーティングに`id` が含まれる
* `collection` はルーティングに`id` が含まれない
* `member` は特定のデータへのアクション追加時に使用
* `collection` は全体のデータへのアクション追加時に使用
