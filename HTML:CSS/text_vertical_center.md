# text_vertical_center

div等のブロック要素の子要素のテキストの上下中央寄せ

## 参照

[\[CSS/HTML\]上下左右中央揃えまとめ。 \- Qiita](https://qiita.com/super-mana-chan/items/0d35a0b9ac1bf97593c8)

[CSS：縦中央揃えにする方法まとめ \| ゆずどっとこむ](https://yu-z.com/vertical-alignment/)

[CSSで中央寄せする9つの方法（縦・横にセンタリング）](https://saruwakakun.com/html-css/basic/centering)

[上下中央揃えのCSSまとめ。Flexboxがたった3行で最も手軽 \- ICS MEDIA](https://ics.media/entry/17522/)

## ポイント

* 親要素の高さを指定できない場合にも有効
* ボックス要素の子要素にのみ有効
* 子要素はボックスでもインラインでもok

## flex を使用して実装

親が`div`, 子が`h`の場合

```HTML
<div class="box">
  <h1>text</h1>
</div>
```

```CSS
.box h1 {
  display: flex;
  align-items: center;
}
```

親が`div`, 子が`p` の場合

```HTML
<div class="box">
  <p>text<p>
</div>
```

```CSS
.box {
  display: flex;
  align-items: center;
}
```


