# flexbox_lower_item

flexbox を使って親要素に下付きでアイテムを配置する

## 参照

[Flexboxを使って ボタンを下側に揃えて配置する方法 \| たねっぱ！](https://taneppa.net/flexbox_btn01/)

[A Pen by karlley](https://codepen.io/karlley/pen/PopJPpd)

[【保存版】コピペでOK！Flexboxで作る頻出レイアウトの構造解説 – 東京のホームページ制作 / WEB制作会社 BRISK@新卒採用22年新卒採用中](https://b-risk.jp/blog/2021/01/flexbox/)

[【CSS】flexboxで要素やボタンを下端（底辺）に揃えたい時のやり方 \| でざなり](https://dezanari.com/css-flex-item-bottom/)

## ポイント

* flexbox の縦並びは親要素への`flex-direction: column;`
* 下付きにしたい要素へ`margin-top: auto;` を指定
* 親要素の高さ指定が無い場合は`amrgin-top: auto;` が効かない
