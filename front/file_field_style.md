# file_field_style

ファイル選択画面の`file_field` ボタンにスタイルを当てる方法

## 参照

[\[Rails\]\[file\_field\]画像のアップロードのボタンデザインを変更する \- Qiita](https://qiita.com/Yukina_28/items/4a8332354f6cb7c7a6f6)

## 実装

画像のアップロードボタンにスタイルを当てる

1. `label` をボタン化, ボタンのスタイルを追加
2. `file_field` を非表示

```Ruby
# 修正前
  <span class="picture">
    <%= f.label :picture %><span class="input-unneed"> *Optional</span>
    <%= f.file_field :picture, accept: "image/jpeg, image/png", class: "form-control", id: "destination_picture" %>
    <% if edit_page? && have_picture? %>
      <%= image_tag(@destination.picture.thumb400.url) %>
    <% end %>
  </span>
```

```Ruby
# 修正後
  <span class="picture">
# ラベルをbtn btn-primaryでボタン化
    <%= f.label :picture, "画像を選択", class: "btn btn-primary" %>
    <%= f.file_field :picture, accept: "image/jpeg, image/png", class: "picture-select-form", id: "destination_picture" %>
    <% if edit_page? && have_picture? %>
      <%= image_tag(@destination.picture.thumb400.url) %>
    <% end %>
  </span>
```

```CSS
# 選択ボタンの非表示
.picture-select-form {
  display: none !important;
}
```



