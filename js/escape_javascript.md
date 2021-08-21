# escape_javascript_20210705

Ajax 時に使用する`escape_javascript` について

## 参照

[escape\_javascriptメソッドって何ぞや。 \- Qiita](https://qiita.com/techman/items/8807bb9eb40fd2663e1a)

[\[Rails\] scriptタグ内でRuby/Railsのメソッドを使用する際のescape\_javascriptについて \- Qiita](https://qiita.com/eitches/items/290d3e1a8ca33f849ed6)

## ポイント

* `escape_javascript` のエイリアスは`j`
* `script` タグ内でRails のメソッドを使用する際に必要
* js.erb ファイルでRails のメソッドを使用する際に必要
* `<%= %>` と`''` が必要

```Ruby
# js.erb でrender メソッドを実行する

$("#follow_form").html("<%= escape_javascript(render('users/unfollow')) %>");
```
