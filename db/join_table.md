# join_table

中間テーブル(join table/junction table) を使ったデータの管理について

## 参照

[【初心者向け】丁寧すぎるRails『アソシエーション』チュートリアル【幾ら何でも】【完璧にわかる】🎸 \- Qiita](https://qiita.com/kazukimatsumoto/items/14bdff681ec5ddac26d1#%E3%81%8A%E6%B0%97%E3%81%AB%E5%85%A5%E3%82%8A%E6%A9%9F%E8%83%BD%E3%82%92er%E5%9B%B3%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E8%A8%AD%E8%A8%88%E3%81%97%E3%82%88%E3%81%86)

## ポイント

* テーブル同士の関係性がM対N(多対多) の場合に使用する
* テーブル同士のid のみを保存するテーブル
* 中間テーブルは接続する2つ以上のテーブルに対してどちらにも`belongs_to`になる
* 中間テーブルのデータを表示するにはmodel に`has many through` を追記して直接アソシエーションから取得する
