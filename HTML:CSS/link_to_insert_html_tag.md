# link_to_insert_html_tag

`link_to` メソッドにHTML タグを含める

## 参照

[link\_to \(ActionView::Helpers::UrlHelper\) \- APIdock](https://apidock.com/rails/ActionView/Helpers/UrlHelper/link_to)

## 目標

`link_to` メソッドを使ったリンクの生成にHTML タグを含める

```HTML
<a href="path_to_login"><span>Login</span></a>
```

## 方法

`link_to` のブロック構文を使ってHTML を内包する

```Ruby
# 一般的なlink_to の使い方
<%= link_to "Link Text", path %>

# ブロック構文を使ってHTML を含ませる
<%= link_to path do %>
  含めたいHTML
<% end %>
```

`<a>` タグの中に`<span>` タグを含める場合は下記のように記述する

```Ruby
<%= link_to login_path do %>
  <span>Login</span>
<% end %>
```
