# position_relative_absolute

`position: relative;`, `position: absolute;` を使った位置の指定

## 参照

[【初心者】positionは、relativeで範囲を決めてabsoluteで動かそう！ \- Qiita](https://qiita.com/sun19008/items/b18b2dc98443ab988ecb)

[top,left,translate,padding\-topなどに指定するパーセント値が何に対する割合なのかを理解する \- Qiita](https://qiita.com/kazhashimoto/items/bfee95a4180b7304bee0)

[CSSのpositionを総まとめ！absoluteやfixedの使い方は？](https://saruwakakun.com/html-css/basic/relative-absolute-fixed)

## ポイント

~~* `ralative`, `absolute` は必ずセットで指定する~~
* `absolute` で基準要素を指定したい場合に`relative` をセットで指定する
  * `relative`: 親要素に付与して基準を作る
  * `absolute`: 子要素に付与して親要素からの移動位置を指定
* `absolute` で`ralative` を指定しない場合は一番近い親要素、無い場合は画面左上が基準点になる
* `relative` は`z-index` が指定できない
* `absolute` は`z-index` が指定可能
* 親要素の`width`, `height` は指定しないと`top`, `bottom`, `right`, `left` の指定ができない
* `top`, `bottom`, `right`, `left` は`%` でも指定できる
