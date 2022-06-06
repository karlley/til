# render_file_name

`render` 使用時のファイル名について

## 参照

[【Rails】 部分テンプレートの使い方を徹底解説！ \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/partial_template#collection%E3%82%AA%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3)

[レイアウトとレンダリング \- Railsガイド](https://railsguides.jp/layouts_and_rendering.html#%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E5%A4%89%E6%95%B0)

## ポイント

* パーシャル側で使用するローカル変数名を意識してファイル名を決める
* `collection` 使用時のローカル変数はパーシャルのファイル名で決まる
* `collection` 使用時のローカル変数は`as:` オプションで変更できる

```Ruby
# product パーシャルに使用するローカル変数はproduct になる

#_product.html.erb
<%= render partial: "product", collection: @products %>
```

```Ruby
# product パーシャルに使用するローカル変数をitem に指定

#_product.html.erb
<%= render partial: "product", collection: @products, as: :item %>
```
