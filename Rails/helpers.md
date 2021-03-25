# helpers

`app/helpers` ディレクトリについて

* view ファイルは基本的に処理は書かない
* view ファイルの分岐処理等を減らす為に使用する
* デフォルトでは全てのview で全てのhelpers ファイルが読み込まれる
* view ファイル毎に読み込むhelpers ファイルを指定する事も可能

## 使い方

helper ファイルで定義したメソッドでview ファイルの表示を分岐させる

```Ruby
# helpers/destination_helper.rb
# view ファイルで使用するヘルパーメソッドを定義
module DestinationsHelper
  # edit ページが表示されていればtrue を返す
  def edit_page?
    @destination.id.present?
  end
end
```

```Ruby
# views/destinations/new.html.erb
# ヘルパーメソッドを使用して表示を分岐させる
  <% if edit_page? %>
    <%= link_to "行き先を削除する", destination_path(@destination), method: :delete, data: { confirm: "Are you sure?" } %>
  <% end %>
```
