# link_to_button_20210605

`link_to` をボタン化

## 参照

[【Rails】link\_toをボタンにする。 \- Qiita](https://qiita.com/maru_katy/items/5d352951161ebd9b2e3a)

[【Rails基礎】link\_toメソッドをボタン化して使う方法 \| FREE SWORDER](https://freesworder.net/rails-link-to-button/)

[【Rails】 \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/button_to)

## ポイント

* `form` タグ内部に`link_to` でリンクを設置
* `link_to` へのBootstrap クラスの追加でリンクのスタイルのみボタン化する(タグ自体はaタグ)
* `button_to` で実装する方法もある

## 実装

link_to で作成したリンクをBootstrap でボタン化して他のボタンとスタイルを揃える

```Ruby
# view

<%= form_with model: @destination, local: true do |f| %>
  <%= f.submit class: "btn btn-primary destination-form-submit-btn" %>
  <% if edit_page? %>
    <div class="destination-form-delete-wrapper">
    <%= link_to "削除する", destination_path(@destination), method: :delete, data: { confirm: "削除してよろしいですか?" }, class: "btn btn-danger destination-form-delete-btn" %>
  <% end %>
    </div>
<% end %>
```

```Ruby
# CSS

.destination-form-submit-btn {
  //delete ボタンを他のフォームとスタイルを揃えて下部マージン
  margin-bottom: 15px;
}

.destination-form-delete-btn {
  //delete をsubmit のスタイルに合わせる
  width: 100%;
}
```
