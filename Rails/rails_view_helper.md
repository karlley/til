# rails_view_helper

view からhelper を使う

## 参照

[【Rails】Helperを使ってよりDRYなviewを書こう \- Qiita](https://qiita.com/shunsuke227ono/items/21f5968ca7ca0391b583)

[rails helper 基本 \- Qiita](https://qiita.com/yukiyoshimura/items/f0763e187008aca46fb4)

## 注意点

* `post.html.erb` には`post_helper.rb` という風に基本的にはコントローラに対応したヘルパーを使用する
* rails 5 以降はデフォルトで全てのヘルパーを読み込む
* ヘルパーのメソッド名が被らないように注意

## 実装

view にCountry モデルから国名を取得して表示したい

```Ruby
# view
# helper のメソッド呼び出し
<%= get_country_name(destination) %>
```

```Ruby
# helper
# 国名の取得
def get_country_name(destination)
  Country.find_by(id: destination.country).country_name
end
```
