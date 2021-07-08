# hidden_field_tag

`form` タグ等のパラメータ送信時に使用する`hidden_field_tag` について

## 参照

[f\.hidden\_fieldとhidden\_field\_tagの使い方【Ruby on Rails】 \- SakuraWi \- BLog](https://sakurawi.hateblo.jp/entry/hidden_field)

[\[Rails\]hidden\_fieldとhidden\_field\_tagの違いについて！\[初心者\] \- Qiita](https://qiita.com/jackie0922youhei/items/6c4ba67db446e0c91944)

## ポイント

* フォーム入力無しでパラメータを送信する際に使用
* `hidden_field` はcontroller で生成したインスタンスを渡すのに使用、`hidden_field_tag` はmodel/controller 関係なくパラメータを送信可能
