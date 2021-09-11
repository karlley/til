# collection_select

DB のデータを使ったセレクトボックスを表示する

## 参照

[collection\_select \| Railsドキュメント](https://railsdoc.com/page/collection_select)

[\[Rails 4\.x\] FormのSelect プルダウンメニューの項目をDBから引っ張ってくる方法 \- Qiita](https://qiita.com/colorrabbit/items/b58888506e41d1370fd1)

[Rails でフォーム送信時のプルダウンメニューの項目をCSV インポートしてDB のデータから表示したい \- Qiita](https://qiita.com/karlley/items/b212994c61fb4a868042)

## 文法

```Ruby
<%= f.collection_select <保存先のカラム名>, <表示用の配列データ>, <保存する値のカラム名>,  <表示用のカラム名>, <オプション>, <HTML属性>, <イベント属性> %>
```

## ポイント

* HTML 属性(id, class)を指定する際はオプションを指定が必須(オプション不要でも`{}` が必要)
* オプション: `include_blank`, `disable`, `selected` のどれか1つ
* HTML 属性: 複数指定可能

## include_blank とprompt の使い分け

どちらのHTML オプションも空の文字列か指定した文字列が表示されるが動作の違いに注意

* `include_blank`: 常にセレクトボックスの先頭に空の文字列、もしくは指定した文字列を表示
* `prompt`: セレクトボックスの値が選択されていない場合のみ空の文字列、もしくは指定した文字列を表示
* 常にセレクトボックス先頭行に文字列を追加したい場合は`include_blank` の方が良い
* `prompt` は公式には記載無し

## コード

```Ruby
<%= form_with model: @destination, local: true do |f| %>
.
  <div class="form-group" id="destination_region">
    <% #edit 時のみ地域セレクトボックスを選択済みで表示 %>
    <% if edit_page? %>
      <%= f.label :region_id %><span class="input-need"> *必須</span>
      <%= f.collection_select :region_id, @regions, :id, :country_name, { include_blank: "地域を選択" }, { class: "form-control", id: "destination_region_id" } %>
    <% else %>
      <%= f.label :region_id %><span class="input-need"> *必須</span>
      <%= f.collection_select :region_id, @regions, :id, :country_name, { include_blank: "地域を選択" }, { class: "form-control", id: "destination_region_id" } %>
    <% end %>
  </div>
  <div class="form-group-wrapper" id="destination_country">
    <% #edit 時のみ国名セレクトボックスを先に表示 %>
    <% if edit_page? %>
      <div class="form-group" id="country_select_box">
        <%= f.label :country_id %><span class="input-need"> *必須</span>
        <%= f.collection_select :country_id, @countries, :id, :country_name, { include_blank: "国を選択" }, { class: "form-control", id: "destination_region_id" } %>
      </div>
    <% end %>
  </div>
.
<% end %>
```
