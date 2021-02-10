# ransack add refind search

ransack について

[ransack](https://github.com/activerecord-hackery/ransack)

## 参考URL

[[Rails]ransackを利用した色々な検索フォーム作成方法まとめ](https://qiita.com/nishina555/items/2c1f8bae980e426519bc)

[Ransackのススメ](https://qiita.com/nysalor/items/9a95d91f2b97a08b96b0)

[【Rails】ransackを使って検索機能がついたアプリを作ろう！](https://pikawaka.com/rails/ransack)

[Ransackで簡単に検索フォームを作る73のレシピ](http://nekorails.hatenablog.com/entry/2017/05/31/173925)

## ransack 単語

* `search_form`: 検索フォームを作る, inputタグに`type="search"` が追加される, 「x」ボタンをクリックすると入力された内容をリセットできる
* `params[:q]`: 検索パラメータ
* `ransack`: 検索パラメータから検索するメソッド
* `result`: 検索結果をActiveRecord_Reration オブジェクトに変換する
* `_cont`: 検索したワードが含まれているレコードを取得するためのメソッド, あいまい検索
* `_eq`: 検索したワードが含まれているレコードを取得するためのメソッド, 完全一致検索
* `_or_`: 複数のカラムを同時検索

## ransack モード

ransack の2つのモードについて

1. シンプル

* カラム名,述語(Predicate) でSQL を組み立てる方法
* Predicate == `:*_cont`

```Ruby
> Destination.ransack(name_cont: "福岡").result.to_sql
"SELECT \"destinations\".* FROM \"destinations\" WHERE \"destinations\".\"name\" ILIKE '%福岡%' ORDER BY \"destinations\".\"created_at\" DESC"
```

2. アドバンス

* 入力値と述語どちらも自由に設定できる為複雑な検索が可能
* 内部的にはシンプルモードと変わらない
