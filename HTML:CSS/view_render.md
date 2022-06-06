# view_render

view ファイルでの`render` メソッドについて

## 参照

[locals](https://qiita.com/hayashino/items/c2a4e7d3edbdcce3cd2a)

[構文エラーが発生しました。予期しない '：'、Rubyで '='が必要です\-スタックオーバーフロー](https://stackoverflow.com/questions/45614852/getting-syntax-error-unexpected-expecting-in-ruby/45614981)

## 使用例

view から`_template_name.html.erb` テンプレートを呼び出す

* テンプレート名の`_` を省略
* `partial` は省略可

```Ruby
<%= render partial: 'template_name' %>
<%= render 'template_name %>
```

view から`other_dir/_template_name.html.erb` テンプレートを呼び出す

```Ruby
<%= render partial: 'other_dir/template_name' %>
<%= render 'other_dir/template_name' %>
```

view から`_template_name.html.erb` を呼びだし、変数`@valiable` を変数`valiable` としてテンプレートに渡す

* `locals` オプションを使用
* `locals` は省略可
* 変数の指定がRuby のバージョンによってはエラーが出る場合有り

```Ruby
<%= render partial: 'template_name', locals: { valiable: @valiable } %>
<%= render partial: 'template_name', locals: { :valiable => @valiable } %>
<%= render 'template_name', { valiable: @valiable } %>
```

view から`_template_name.html.erb` を呼び出し、変数う`@valiables` を`valiable` でループを回す

* `collection` オプションを使用
* `partial` と`collection` は同時省略可
* 省略時に呼ばれるテンプレートはrails が自動的に決定する

```Ruby
<%= render partial: 'template_name', collection: @valiables %>
<%= render @valiables %>
```





