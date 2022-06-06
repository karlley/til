# negative_margin

ネガティブマージンを使った要素の位置指定

## 参照

[お役立ちCSSテク！ーネガティブマージン – Web制作会社トライム](https://try-m.co.jp/blog/web-create/354/)

[CSS【 margin 】3 ～ ネガティブマージン \| プログラマカレッジ](https://programmercollege.jp/column/3938/)

## ポイント

* 親要素をはみ出して位置指定が可能
* 左にずらす場合は`margin-left` などずらしたい方向へ指定する

## 実装

[A Web Maker experiment](https://codepen.io/karlley/pen/bGqbKyw)

box の中のitem を下端中央に移動してネガティブマージンでdiv 枠を越えて下と左にズラす

```HTML
<div class="box">
  <div class="item">
  </div>
</div>
```

```CSS
.box {
  width: 200px;
  height: 200px;
  position: relative;
  background: yellow;
}

.item {
  width: 50px;
  height: 50px;
  position: absolute;
  bottom: 0;
  left: 100px;
  background: pink;
  //ネガティブマージン
  margin-bottom: -100px;
  margin-left: -50px;
}
```
