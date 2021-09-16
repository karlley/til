# active_select_box

親カテゴリを選択すると小アイテムが動的に変化するセレクトボックスの実装

## 参照

## ポイント

* gem `ancestry` でカテゴリとアイテムの親子関係化
* seed はCSV でインポート

## 手順

1. gem ancestry 導入
2. Country モデルにancestry 用の設定を追加
3. countries テーブルに`ancestry` カラムを追加
4. インポート用csv ファイルに`ancestry` カラムを追加
5. csv インポート設定を追加
6. seed データ投入
7. 親、子が取得できるか確認
8. json 用ルーティングを追加
9. セレクトボックス表示用アクション追加
10. セレクトボックス表示用view 追加
11. JS で使用するjson 用にjbuilder ファイル追加
12. 子アイテム取得、セレクトボックス挿入用JS ファイル追加
13. region_id DB 保存用のカラム追加
14. region_id をstrong_params に追加
15. collection_select エラー解消

### 1. gem ancestry 導入

```Ruby
gem `ancestry`
```

```Shell
$ bundle install
```

### 2. Country モデルにancestry 用の設定を追加

`has_ancestry` の記述追加

```Shell
# app/model/country.rb

class Country < ApplicationRecord
  has_many :destinations
  # ancestry を使う
  has_ancestry
  .
end
```

### 3. countries テーブルに`ancestry` カラムを追加

`ancestry` カラム追加用マイグレーションファイルを追加

```Shell
# countries テーブルにancestry カラム追加用のマイグレーションファイルを作成

$ rails g migration add_ancestry_to_countries ancestry:string:index
```

```Ruby
# 生成されたancestry 用マイグレーションファイル

class AddAncestryToCountries < ActiveRecord::Migration[5.2]
  def change
    add_column :countries, :ancestry, :string
    add_index :countries, :ancestry
  end
end
```

### 4. インポート用csv ファイルに`ancestry` カラム用データ追加

csv ファイルにancestry カラムに階層情報のデータ追加

```csv
# country.csv
番号,国・地域名,ISO 3166-1に於ける英語名,数,三字,二字,場所,各行政区分,ancestry

# 親カテゴリになるエリアの追加、ancestry カラムへ親カテゴリへのパスを追加
1,アジア,,,,,,,
.
10,アイスランド,Iceland,352,ISL,IS,北ヨーロッパ,ISO 3166-2:IS,3
.
```

### 5. csv インポート設定を追加

* ancestry カラムの読込を追加
* country モデルバリデーション修正

```Ruby
# db/seeds/development.rb
require "csv"

CSV.foreach("db/country.csv", headers: true) do |row|
  Country.create!(
    country_name: row["国・地域名"],
    region: row["場所"],
    ancestry: row["ancestry"]
  )
end
```

`region` カラムの`presence: true` を外す、`ancestry` カラムのバリデーションを追加

```Ruby
# app/model/country.rb
class Country < ApplicationRecord
  has_many :destinations
  has_ancestry
  validates :country_name, presence: true
  validate :region
  validate :ancestry
end
```

### 6. seed データ投入

環境別にmigrate & seed データ再生成

**production は不要っぽい?**

```Shell
# development
$ rails db:migrate:reset
$ rails db:seed

# test
$ rails db:migrate:reset RAILS_ENV=test
$ rails db:seed RAILS_ENV=test
```

### 7. 親、子が取得できるか確認

コンソールで親、子の取得を確認

```Shell
# 子から親を取得
> child = Country.find_by(id: 10)
> child.parent
> child.parent_id

# 親から子を取得
> parent = Country.find_by(id: 1)
> parent.children
> parent.child_ids
```

### 8. json 用ルーティングを追加

destinationsに`get_country_children` アクションのルーティングを追加

* `collection` で`id` を含まないルーティング(new アクション用)
* `member` で`id` を含むルーティング(edit アクション用)
* `format: "json"` で`get_country_children` アクションのリクエストのフォーマットをjson に指定する

```Ruby
# routes.rb

Rails.application.routes.draw do
.
resources :destinations do
  # new Ajax 取得用
  collection do
    get "get_region_countries", defaults: { format: "json" }
  end
  # edit Ajax 取得用
  member do
    get "get_region_countries", defaults: { format: "json" }
  end
end
.
end
```

ルーティングを確認

```Shell
$ rails routes
.
                                  POST   /favorites/:destination_id/create(.:format)       favorites#create
                                  DELETE /favorites/:destination_id/destroy(.:format)      favorites#destroy
                                  POST   /destinations(.:format)                           destinations#create
                                  PATCH  /destinations/:id(.:format)                       destinations#update
                                  PUT    /destinations/:id(.:format)                       destinations#update
                                  DELETE /destinations/:id(.:format)                       destinations#destroy
                      destination GET    /destinations/:id(.:format)                       destinations#show
                     destinations GET    /destinations(.:format)                           destinations#index
                  new_destination GET    /destinations/new(.:format)                       destinations#new
                 edit_destination GET    /destinations/:id/edit(.:format)                  destinations#edit
get_region_countries_destinations GET    /destinations/get_region_countries(.:format)      destinations#get_region_countries {:format=>"json"}
get_region_countries_destination GET     /destinations/:id/get_region_countries(.:format)  destinations#get_region_countries {:format=>"json"}
```

### 9. セレクトボックス表示用アクション追加

* セレクトボックスを表示するアクション(`destinations#new`)に選択肢の初期値設定用の処理を追加
* 親カテゴリ選択後の子アイテム表示用の処理を追加


```Ruby
# destinations_controller.rb

def new
  @destination = Destination.new
  # 地域セレクトボックスの初期値
  @regions = Country.where(ancestry: nil)
  end
end

# 地域選択後の国名表示用データの取得
def get_region_countries
  @region_countries = Country.find("#{params[:region_id]}").children
end

# 地域セレクトボックスの初期値、edit 遷移時の国名セレクトボックス用の初期値
def edit
  @destination = Destination.find(params[:id])
  @regions = Country.where(ancestry: nil)
  selected_region = Country.find_by(id: @destination.country_id).parent
  @countries = selected_region.children
  @airlines = Airline.all
end
```

### 10. セレクトボックス表示用view 追加

親カテゴリ、子アイテム表示用のview を追加

* `#destination_region_id` でイベント発火
* `destination_country` の子要素にjs で国名を追加する

```Ruby
<%= form_with model: @destination, local: true do |f| %>
.
  <!-- 親カテゴリ -->
  <div class="form-group" id="destination_region">
    <%= f.label :region_id %><span class="input-need"> *必須</span>
    <%= f.collection_select :region_id, @regions, :id, :country_name, { prompt: "地域を選択" }, { class: "form-control", id: "destination_region_id" } %>
  </div>
  <!-- 子アイテム -->
  <div class="form-group-wrapper" id="destination_country">
  子アイテム挿入ポイント
  </div>
.
<% end %>
```

```Ruby
# 実際のコード
# app/view/destinations/_destination_form.html.erb

<%= form_with model: @destination, local: true do |f| %>
.
  <div class="form-group" id="destination_region">
    <%= f.label :region_id %><span class="input-need"> *必須</span>
    <%= f.collection_select :region_id, @regions, :id, :country_name, { include_blank: "地域を選択" }, { class: "form-control", id: "destination_region_id" } %>
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

### 11. JS で使用するjson 用にjbuilder ファイル追加

* jbuilder で成形したデータをセレクトボックス書き換え用のJS に送る
* jbuiler を作成するディレクトリは`app/views/destinations/`

```JSON
# app/views/destinations/get_region_countries.json.jbuilder

json.array! @region_countries do |country|
  json.id country.id
  json.country_name country.country_name
end
```

`get_region_countries` アクションでjson でのレスポンスを確認

1. destinations#get_region_countries での親カテゴリの取得を仮に設定
2. ブラウザで`get_region_countries` アクションにアクセス

```Ruby
# destinations_controller.rb
# 親カテゴリを仮設定

  def get_region_countries
    # @region_countries = Country.find("#{params[:region_id]}").children
    # アジアの国一覧を取得
    @region_countries = Country.where(ancestry: 1)
  end
```

`http://localhost:3000/destinations/get_region_countries.json` にアクセスしてアジアの国一覧がjson 形式で表示されればok

### 12. 子アイテム取得、セレクトボックス挿入用JS ファイル追加

* JS ファイル作成ディレクトリは`app/assets/javascripts/`
* gem turbolinks を使用している場合は`turbolinks:load` で囲まないと画面遷移時の`ready` アクションが実行されない

JS での処理内容

* 事前準備系
  * 子アイテムのセレクトボックスのオプション作成
  * 子アイテムのセレクトボックス用HTML 作成
* イベント系
  * 親カテゴリが選択されたらAjax で親カテゴリを元に子アイテムを取得
  * 親カテゴリが再選択されたら子アイテムのセレクトボックスを削除
  * 親カテゴリが初期値になったら子アイテムのセレクトボックスを削除

```JavaScript
# app/assets/javascripts/region_countries.js

// gem turbolinks でready イベントが発火しないのでturbolinks:load
$(document).on('turbolinks:load', function () {
  // 国名のセレクトボックスのoption 作成
  function appendOption(country) {
    var html = `<option value="${country.id}">${country.country_name}</option>`;
    return html;
  }
  // 国名のセレクトボックス用HTML
  function appendCountriesSelectBox(insertHTML) {
    var countriesSelectBoxHtml = '';
    countriesSelectBoxHtml = `<div class="form-group" id="country_select_box">
                                <label for="destination_country_id">国</label>
                                <span class="input-need"> *必須</span>
                                <select class="form-control" id="destination_country_id" name="destination[country_id]">
                                  ${insertHTML}
                                </select>
                              </div>`;
    $('#destination_country').append(countriesSelectBoxHtml);
  }

  // 地域選択後にAjax で国名を取得
  $('#destination_region_id').on('change', function () {
    var selectedRegion = document.getElementById('destination_region_id').value;
    // 初期値以外を選択
    if (selectedRegion != "") {
      $.ajax({
        url: 'get_region_countries',
        type: 'GET',
        data: { region_id: selectedRegion },
        dataType: 'json'
      })
        .done(function (countries) {
          // 地域名の再選択で国名のセレクトボックスを削除
          $('#country_select_box').remove();
          var insertHTML = '';
          countries.forEach(function (country) {
            insertHTML += appendOption(country);
          });
          appendCountriesSelectBox(insertHTML);
        })
        .fail(function () {
          // 国名の取得に失敗した場合
          alert('国名の取得に失敗しました');
        })
    } else {
      // 地域名が初期値に戻ると国名のセレクトボックスを削除
      $('#country_select_box').remove();
    }
  });
});
```

### 13. region_id DB 保存用のカラム追加

`region_id` をDB に保存してedit 時にセレクトボックスを選択済みにする

```Shell
# destinations テーブルにinteger 型のregion_id カラムを追加
$ rails g migration AddRegion_idToDestinations region_id:integer
```

```Ruby
# 生成されたマイグレーションファイル
# db/migrate/20210910080733_add_region_id_to_destinations.rb

class AddRegionIdToDestinations < ActiveRecord::Migration[5.2]
  def change
    add_column :destinations, :region_id, :integer
  end
end
```

```Shell
$ rails db:migrate
```

### 14. region_id をstrong_params に追加

DB に保存する為にstrong_params に追加

```Ruby
# app/controllers/destinations_controller.rb

  private

  def destination_params
    params.require(:destination).permit(:name, :description, :spot, :latitude, :longitude, :address, :country_id, :picture, :season, :experience, :airline_id, :food, :region_id).merge(expense: params[:destination][:expense].to_i)
  end
```

### 15. collection_select エラー解消

* `collection_select` メソッドで引数にインスタンス変数を渡すと`undefined method 'map' for nil:NilClass` が発生
* コントローラでのインスタンス変数の記述を直接view に記述して一時的にエラー解消

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
