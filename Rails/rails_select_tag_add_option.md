# rails_select_tag_add_option

form タグ内のselectにclass/id などのHTML オプションを追加する

## 参照

[railsでf\.selectにclassを設定する \- Qiita](https://qiita.com/nakanoyoshiki/items/e87a6238f8febbeb208a)

[Railsのf\.selectにclassが設定できなかった \- Qiita](https://qiita.com/Nawada/items/014c2f59f5bdc364b582)

[select \(ActionView::Helpers::FormOptionsHelper\) \- APIdock](https://apidock.com/rails/ActionView/Helpers/FormOptionsHelper/select)

[select \| Railsドキュメント](https://railsdoc.com/page/select)

[rails/form\_options\_helper\.rb at 71c7fd101324046995d8f7e51e78475c0e37ec1a · rails/rails](https://github.com/rails/rails/blob/71c7fd101324046995d8f7e51e78475c0e37ec1a/actionview/lib/action_view/helpers/form_options_helper.rb#L776)

## 目標

ransack を使った検索フォームに下記を追加したい

* オプション(`include_blank: "何月ですか?"`)
* HTMLオプション(`class: "form-control", id: "destination_season_search"`)

## 方法

* `f.select` にオプションとHTMLオプションを`{}` で閉じて渡す
* オプションとHTML オプションを別のものと解釈する必要がある

```Ruby
f.select(メソッド名, 要素(配列 or ハッシュ), { オプション }, { HTML オプション }
```

オプションを追加せずにHTML オプションのみを追加したい場合は空カッコ`{}` を使いオプションをスキップする必要がある

```Ruby
f.select(メソッド名, 要素(配列 or ハッシュ), {}, { HTML オプション }
```

## 実装

メソッド名と要素のみ出力して月を選択できるフォーム

```Ruby
# 修正前
<%= search_form_for @q do |f| %>
  <%= f.label :season_refind %>
  <%= f.select :season_eq, [["1月", 1], ["2月", 2], ["3月", 3], ["4月", 4], ["5月", 5], ["6月", 6], ["7月", 7], ["8月", 8], ["9月", 9], ["10月", 10], ["11月", 11], ["12月", 12]] %>
  <%= f.submit "Search", class: "btn btn-primary" %>
<% end %>
```

オプションとHTML オプションを追加

```Ruby
# 修正後
<%= search_form_for @q do |f| %>
  <%= f.label :season_refind %>
  <%= f.select :season_eq, [["1月", 1], ["2月", 2], ["3月", 3], ["4月", 4], ["5月", 5], ["6月", 6], ["7月", 7], ["8月", 8], ["9月", 9], ["10月", 10], ["11月", 11], ["12月", 12]], { include_blank: "何月ですか?" }, { class: "form-control", id: "destination_season_search" } %>
  <%= f.submit "Search", class: "btn btn-primary" %>
<% end %>
```

HTML オプションのみを追加

```Ruby
# 修正後
<%= search_form_for @q do |f| %>
  <%= f.label :season_refind %>
  <%= f.select :season_eq, [["1月", 1], ["2月", 2], ["3月", 3], ["4月", 4], ["5月", 5], ["6月", 6], ["7月", 7], ["8月", 8], ["9月", 9], ["10月", 10], ["11月", 11], ["12月", 12]], {}, { class: "form-control", id: "destination_season_search" } %>
  <%= f.submit "Search", class: "btn btn-primary" %>
<% end %>
```
