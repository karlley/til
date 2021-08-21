# search_form_for_add_html_class_20210427

ransack を使用した検索フォーム生成時の`search_form_for` メソッドにHTML のクラスを追加する

## 参照

[ransackでRailsアプリのヘッダーに検索機能をつける \- Qiita](https://qiita.com/fujitora/items/b2134bf6abcfda79c47f)

[Railsメモ\(11\) : Ransackで検索機能を追加する \- もた日記](https://wonderwall.hatenablog.com/entry/2015/08/09/213129)

## 実装

```Ruby
# search_form_for の引数にhtml オプションを追加

<%= search_form_for @q, html: { class: "navbar-form navbar-right" }  do |f| %>
  <%= f.search_field :name_or_spot_or_address_cont, placeholder: "旅先を探す", class: "form-control", id: "destination_keyword_search" %>
<% end %>
```
