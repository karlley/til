# text_shot

rails で長い文字列を省略して表示

## 参照

[Rails \- 長い文字列を省略して表示する \- Qiita](https://qiita.com/ishidamakot/items/2e74d980b3a338e4c784)

[truncate \| Railsドキュメント](https://railsdoc.com/page/truncate)

## ポイント

* 何文字以降を省略するかは引数で指定する
* 文字数の引数は必須
* 省略後の文字列を`ommission` で変更可

## 実装

description を50文字で省略して表示

```Ruby
<div class="destination-list-description">
  <p><%= destination.description.truncate(50) %></p>
</div>
```
