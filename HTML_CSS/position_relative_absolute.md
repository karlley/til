# position_relative_absolute

`position: relative;`, `position: absolute;` を使った位置の指定

## 参照

[【初心者】positionは、relativeで範囲を決めてabsoluteで動かそう！ \- Qiita](https://qiita.com/sun19008/items/b18b2dc98443ab988ecb)

[top,left,translate,padding\-topなどに指定するパーセント値が何に対する割合なのかを理解する \- Qiita](https://qiita.com/kazhashimoto/items/bfee95a4180b7304bee0)

## ポイント

* `ralative`, `absolute` は必ずセットで指定する
  * `relative`: 親要素に付与して基準を作る
  * `absolute`: 子要素に付与して親要素からの移動位置を指定
* 親要素の`width`, `height` は指定しないと`top`, `bottom`, `right`, `left` の指定ができない
* `top`, `bottom`, `right`, `left` は`%` でも指定できる
