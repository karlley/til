# height_100

`height: 100%` 使用の注意点

## 参照

[height:100%が効かない！CSSのheightの使い方について l NatsukiMemo なつ記メモ of WEBデザインTIPS](https://natsukimemo.com/css-height-100)

["height: 100%" の要素が画面の高さ100%にならない場合の対応方法 \- Qiita](https://qiita.com/shouchida/items/205fed63b886681661bd)

## ポイント

* `%` は相対指定なので親要素に高さ指定が必須
* `vh` は表示領域に対しての相対指定なので親要素の高さ指定は不要
