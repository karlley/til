# searchbar_submit_button

検索バーのサブミットボタンのスタイルについて

## 参照

[\[Bootstrap\] inputとボタンが横に並んだフォームを作る \| きほんのき](http://kihon-no-ki.com/twitter-bootstrap-align-input-with-button)

[Rails4でSubmitボタンにFontAwesomeのアイコンを埋め込む \- Qiita](https://qiita.com/jacoyutorius/items/c73327f049ee5270d505)

[f\.submit でFontAwesomeのアイコンが表示できない \- Qiita](https://qiita.com/Uuya/items/68eb5b088ba62cd3e485)

## ポイント

* サブミットボタンをアイコンにする場合は`f.submit` ではなく`button_tag` を使う
* サブミットボタンをフォーム横に設置する場合はフォームを囲んでいるクラスに注意する

## 比較

メニューバーで使用したサブミットボタン無しバージョン

```Ruby
<%= search_form_for @q, html: { class: "navbar-form navbar-right" }  do |f| %>
  <%= f.search_field :name_or_spot_or_address_cont, placeholder: "旅先を探す", class: "form-control", id: "destination_keyword_search" %>
<% end %>
```

```HTML
<form class="navbar-form navbar-right" id="destination_search" action="/destinations" accept-charset="UTF-8" method="get"><input name="utf8" type="hidden" value="&#x2713;" />
  <input placeholder="旅先を探す" class="form-control" id="destination_keyword_search" type="search" name="q[name_or_spot_or_address_cont]" />
</form>
```

トップページ用サブミットボタン有り

```Ruby
<%= search_form_for @q, html: { class: "input-group" }  do |f| %>
  <%= f.search_field :name_or_spot_or_address_cont, placeholder: "旅先を探す", class: "form-control", id: "destination_keyword_search" %>
  <span class="input-group-btn">
    <%= button_tag type: "submit", class: "btn btn-default" do %>
      <i class="fas fa-search"></i>
    <% end %>
  </span><!-- .input-group-btn -->
<% end %>
```

```HTML
<form class="input-group" id="destination_search" action="/destinations" accept-charset="UTF-8" method="get"><input name="utf8" type="hidden" value="&#x2713;" />
  <input placeholder="旅先を探す" class="form-control" id="destination_keyword_search" type="search" name="q[name_or_spot_or_address_cont]" />
  <span class="input-group-btn">
    <button name="button" type="submit" class="btn btn-default">
      <i class="fas fa-search"></i>
    </button>
  </span><!-- .input-group-btn -->
</form>
```

* `input-group` クラスと`form-control` クラスで横幅のサイズを決定
* `button_tag` で生成した`button` タグでfont-awesome のアイコンを挟んで表示
