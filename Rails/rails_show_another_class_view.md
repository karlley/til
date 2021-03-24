# rails_show_another_class_view

`@feed_items` でのパーシャル呼び出しが分かりづらい

## 参照

[【実践編④】ポートフォリオ作成のロードマップ〜料理の投稿〜｜こうだい＠フルリモートエンジニア｜note](https://note.com/kodai_0122/n/nfbbe6df50b6a)

> @feed_itemsの各要素はDishクラスを持つため、renderすることで対応するパーシャル（_dish.html.erb）を呼び出せたというわけです。

* `@feed_items` にはDestination クラスの要素を保有
* `render` で保有クラスに対応するパーシャルを自動で呼び出す

## まとめ

* インスタンス変数の保有しているクラス名で自動的にview ファイルが選択される
* 明示的に使用するパーシャルを明記する方法の方が良さそう
