# rails_query_method

レコードからデータを取得するクエリメソッドについて

## 参照

[【Rails】findとfind\_byとwhereの使い分け \- Qiita](https://qiita.com/morikuma709/items/abbaf8f25b3336e0436b)

[Active Record クエリインターフェイス \- Railsガイド](https://railsguides.jp/active_record_querying.html)

[【Rails】find・find\_by・whereについてまとめてみた \- Qiita](https://qiita.com/nakayuu07/items/3d5e2f8784b6f18186f2)

## クエリメソッドの種類

* `find`: 主キーに対応するレコードを取り出す
* `find_by`: 与えられた条件にマッチするレコードのうち最初のレコードだけ返す
* `where`: 与えられた条件にマッチするレコードをすべて返す

## ポイント

* 使用用途にあったメソッドを使用する
