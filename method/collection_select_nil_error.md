# collection_select_nil_error

`collection_select` メソッドで引数にインスタンス変数を渡すとエラー

## 参照

[collection\_selectにインスタンス変数を渡すと、undefined method \`map' for nil:NilClass が発生するエラー \- Qiita](https://qiita.com/zorori777/items/07cca6b04de125b812a0)

[undefined method \`map' for nil:NilClass](https://qiita.com/shyamahira/items/30e56d8057edcd85ca2d)

## 状況

* view の`collection_select` の引数にインスタンス変数でコントローラからの値を渡すと`undefined method 'map' for nil:NilClass` が発生
* コントローラでのインスタンス変数の記述を直接view に記述するとエラーは解消する

```Ruby
# view
# エラーが発生する

<%= form_with model: @destination, local: true do |f| %>
.
  <div class="form-group" id="destination_region">
    <%= f.label :region_id %><span class="input-need"> *必須</span>
    <%= f.collection_select :region_id, @regions, :id, :country_name, { include_blank: "地域を選択" }, { class: "form-control", id: "destination_region_id" } %>
  </div>
.
<% end %>
```

```Ruby
# controller
# エラーが発生する

def new
  @destination = Destination.new
  # エリア選択の初期値
  @regions = Country.where(ancestry: nil)
  @airlines = Airline.all
end
```

## 対策

とりあえず一時的にview に直書きして解消する

* `new`, `edit` だけでなく`create`, `update` にもインスタンス変数の値を持たせる必要がある？
* プライベートメソッドでインスタンス変数を保持して`before_action`でプライベートメソッドを呼び出す事で解決するかも

```Ruby
# view
# 直書きしてエラー解消

<%= form_with model: @destination, local: true do |f| %>
.
  <div class="form-group" id="destination_region">
    <%= f.label :region_id %><span class="input-need"> *必須</span>
    <%= f.collection_select :region_id, Country.where(ancestry: nil), :id, :country_name, { include_blank: "地域を選択" }, { class: "form-control", id: "destination_region_id" } %>
  </div>
.
<% end %>
```

```Ruby
# controller
# 直書きしたのでコメントアウト

def new
  @destination = Destination.new
  # エリア選択の初期値
  # @regions = Country.where(ancestry: nil)
  # @airlines = Airline.all
end
```
