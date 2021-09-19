# はじめに

セレクトボックスをenum を使って表示させる為の備忘録です

# 環境

* macOS 10.15.6
* Ruby 2.5.7
* Rails 5.2.3

# 参考URL

<https://qiita.com/ozackiee/items/17b91e26fad58e147f2e>
<https://qiita.com/clbcl226/items/3e832603060282ddb4fda>
<https://pikawaka.com/rails/enum>
<https://atora1992.hatenablog.com/entry/2019/09/04/162433>

# 目標

1. 投稿時の:expense カラムのセレクトボックスの選択肢をenum で登録した値(expense1, expense2, expense3)で表示したい

```Ruby
# 実現したいセレクトボックスの形
<select>
  <option value="1">expense1</option>
  <option value="2">expense2</option>
  <option value="3">expense3</option>
</select>
```
2. DB に保存する値はセレクトボックスのvalue の値(1, 2, 3)をInteger 型で保存したい

```Ruby
# 実現したいDB 保存の形
{
    :name => "name",
    :expense => 1 # enum で登録したvalue値のインデックス番号のInteger 型
}
```

# 実装

投稿時のセレクトボックスでDestinations モデルの:expense カラムをenum で定義した値を保存するという想定です

# 1. DB に:expense カラムを用意

* 新規カラムを作成
* 既存カラムを修正する場合はマイグレーションファイルを作成してdefault オプションを追加

## 新規カラムを作成する場合

```Shell
# 新規カラム作成用のmigration ファイル作成
$ rails g migration create_column_to_destinations
```

```Ruby:作成したmigration ファイル
# 新規カラムのdefault 値に0を追加
class AddColumnToDestinationsForDetail < ActiveRecord::Migration[5.2]
def change
  add_column :destinations, :expense, :integer, default: 0
end
end
```

## 既存カラムを修正してenum で表示する場合

```Shell
# 既存カラムのオプション変更用migratiton ファイル作成
$ rails g migration change_column_default_to_destinations
```

```Ruby:作成したmigration ファイル
# :expense カラムにdefault値に0 を追加
class ChangeColumnDefaultToDestination < ActiveRecord::Migration[5.2]
def change
  change_column_default :destinations, :expense, from: nil, to: "0"
end
end
```

## migration ファイルでmigrate

```Shell
# 作成したmigration ファイルでmigrate
$ rails db:migrate

# migrate の履歴を確認
$ rails db:migrate:status
```

## schema.rb を確認

```Ruby:schema.rb
  create_table "destinations", force: :cascade do |t|
  ...
    t.integer "expense", default: 0
  end
```

# 2. model にenum に名前定を追加

model にenum を使用した名前定義を記述

```Ruby:Destination.rb
class Destination < ApplicationRecord

  enum expense: {
    "---": 0,
    expense1: 1,
    expense2: 2,
    expense3: 3,
  }
end
```

# 3. view ファイル修正

* enum を使用したselect ボックス表示用にview ファイルを修正
* view ファイルに直接書いていたセレクトボックスの記述を修正します

```Ruby:view ファイル
# 修正前
<%= form_with model: @destination |f| %>
  <%= f.label :expense %>
  <%= f.select :expense, [["expense1", 1], ["expense2", 2], ["expense3", 3], include_blank: "---"  %>
<% end %>
```

```Ruby:view ファイル
# 修正後
<%= form_with model: @destination |f| %>
  <%= f.label :expense %>
  <%= f.select :expense, options_for_select(Destination.expenses), {} %>
  <%= f.submit%>
<% end %>
```

```HTML
# 出力されるHTML
<select name="destination[expense]">
  <option value="0">---</option>
  <option value="1">type1</option>
  <option value="2">type2</option>
  <option value="3">type3</option>
</select>
```

# 4. DB に保存される値を確認

実装自体は3. で完成していますので、実際に保存される値を確認します
この時の注意点が2つあります

* enum を使って保存される値はrails 上では文字列(もしくはシンボル)で表示される
* `rails console` と`rails db console` でレコードの表示が異なる

詳しく説明します

## enum で保存される値はrails が自動的に見やすい形に変換された状態で表示される

StackOverFlow にて質問したところ、とても分かりやすく説明して頂きました。ありがとうございました。

>Railsのenumはコードの表記上、文字列(またはシンボル）で透過的に扱えるように実装されています
>SQLを直接叩いたり、DBのGUIクライアントを使って保存されたレコード情報を見るとわかりますが実際は文字列ではなく数値が保存されています。あくまでRailsの上ではDBに保存された1の値がtype1と表示されているだけです

StackOverFlow: (enumでセレクトボックスを表示させてインデックス番号をdbに保存したい)[https://ja.stackoverflow.com/questions/72551/enum-%e3%81%a7%e3%82%bb%e3%83%ac%e3%82%af%e3%83%88%e3%83%9c%e3%83%83%e3%82%af%e3%82%b9%e3%82%92%e8%a1%a8%e7%a4%ba%e3%81%95%e3%81%9b%e3%81%a6%e3%82%a4%e3%83%b3%e3%83%87%e3%83%83%e3%82%af%e3%82%b9%e7%95%aa%e5%8f%b7%e3%82%92db-%e3%81%ab%e4%bf%9d%e5%ad%98%e3%81%97%e3%81%9f%e3%81%84]

* DB に保存される値はセレクトボックスのvalue の値
* 保存された値はrails 上ではenum での名前定義(文字列, シンボル)で保存されている用に見える
* 実際に保存された値を確認するには`attribute_before_type_cast` で確認する

```Ruby
# 出力されえるHTML
<select name="destination[expense]">
  <option value="1">type1</option>
</select>

# DB に保存されるレコード
<select name="destination[expense]">
  <option value="DB に保存されるレコード">セレクトボックスに表示される選択肢(enum で定義した名前定義)</option>
</select>
```

(enum の整数値を取得する)[https://qiita.com/QUANON/items/a53ac9b15cad588c46d1]
(Rails5でenum定義したカラムの元の値を取得)[https://qiita.com/yusuke-matsuda/items/df05c8165e2f084023b0]

## `rails console` と`rails db console` でレコードの表示が異なる

enum を使った場合に保存される値はrails 上とDB 上では見え方が異なります

```Ruby
# rails console で値と型を確認
$ rails console
[1] pry > destination = Destination.new(name: "name", expense: 1)
[2] pry > destination.expense
"type1" # 文字列が返る
[3] pry > destination.expense.class
String < Object # String型
```

```Ruby
# rails db を使用してSQL で値を確認
$ rails db
> select expense from destination;
1 # Integer 型
```

rails 上で実際に保存された値を確認する場合は`attribute_before_type_cast` で確認できます

```Ruby
# attribute_before_type_cast を使って値を確認
$ rails console
[1] pry > destination = Destination.new(name: "name", expense: 1)
[2] pry > destination.expense
"type1" # 文字列が返る
[3] pry > destination.expense_before_type_cast
1 # DB に保存された値が返る
```

## 試したコード

```Ruby
# 削除したview ファイルの記述
  <div class="form-group">
    <%= f.label :expense %><span class="input-need"> *Required</span>
    <%= f.select :expense, options_for_select(Destination.expenses), {}, class: 'form-control', id: 'destination_expense' %>
    <%# <%= f.select :expense, options_for_select(Destination.expenses.keys.map{|k| [k, k]}), {}, class: 'form-control', id: 'destination_expense' %1> %>
    <%# <%= f.select :expense, Destination.expenses.keys, class: 'form-control', id: 'destination_expense' %1><% # value = key, 保存可 %1> %>
    <%# <%= f.select :expense, Destination.expenses.keys, {}, class: 'form-control', id: 'destination_expense' %1><% # value = key, 保存可 %1> %>
    <%# <%= f.select :expense, Destination.expenses.keys.map {|k| [k, k]}, {}, class: 'form-control', id: 'destination_expense' %1> %>
    <%# <%= f.select :expense, Destination.expenses.keys.to_a, class: 'form-control', id: 'destination_expense' %1> %>
  </div>
```

## 結果

違う方法で解決した

`enum_error.md` を参照
