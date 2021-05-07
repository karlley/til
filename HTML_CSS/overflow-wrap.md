# overflow-wrap

テキストの折返し

## 参照

[overflow\-wrap: break\-word; や word\-break: break\-all; が万能の改行処理だったなら、こんなに苦労していない \- Qiita](https://qiita.com/akane_kato/items/2b1385574e1a1babdde1)

## プロパティの種類

* `overflow-wrap: break-word;` :幅優先, 幅より長い単語は強制折返し, テーブル使用不可
* `word-wrap: break-word;` :`overflow-wrap` の前身, 後方互換目的でセットで指定する方が良い, テーブル使用不可
* `word-break: break-all;` :幅最優先, 強制改行, テーブル使用不可
* `word-break: break-word;` :幅優先, 禁則処理は保たれる, 幅より長い単語は強制折返し, テーブル使用可

## 実装

div からはみ出した文字を折り返す

```HTML
<div class="box">
  <p>text text text text text text text text text text text text</p>
</div>
```

```CSS
.box {
  width: 200px;
  background: yellow;
  //後方互換も考慮して2つのプロパティを指定
  overflow-wrap: break-word;
  word-wrap: break-word;
}
```


