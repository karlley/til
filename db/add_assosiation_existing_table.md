# add_association_existing_table

既存テーブルに関連付けを追加する

## 参照

[migrateでカラムに外部キーを追加して、モデルを関連づける【Day 4/30 2nd】｜K\.Tawara｜note](https://note.com/kentarotawara/n/n47ab0894b4aa)

[Active Record の関連付け \- Railsガイド](https://railsguides.jp/association_basics.html)

[Active Record マイグレーション \- Railsガイド](https://railsguides.jp/active_record_migrations.html)

[Railsの外部キー制約とreference型について \- Qiita](https://qiita.com/ryouzi/items/2682e7e8a86fd2b1ae47)

[Ruby on Rails 5 \- 【Rails　関連付け】referencesはカラムを自動生成するのか？｜teratail](https://teratail.com/questions/169685)

## 手順

1. 子テーブルへの外部キー追加用マイグレーションファイル 生成
2. マイグレーションファイルに既存テーブルに外部キーを追加する記述を追記
3. マイグレーション
4. モデル間の関連付けを各モデルファイルで追加
5. 関連付いたオブジェクトが取得可能か確認

## 外部キー追加マイグレーション

外部キー追加用マイグレーションファイル生成

```Shell
# 子テーブル(counties, airlines) に外部キー(destinations_id) を追加

$ rails g migration AddDestinationRefToCountries destination:references
$ rails g migration AddDestinationRefToAirlines destination:references
```

生成されたマイグレーションファイル

```Ruby
# countries
class AddDestinationRefToCountries < ActiveRecord::Migration[5.2]
  def change
    add_reference :countries, :destination, foreign_key: true
  end
end

# airlines
class AddDestinationRefToAirlines < ActiveRecord::Migration[5.2]
  def change
    add_reference :airlines, :destination, foreign_key: true
  end
end
```

マイグレーション実行

```Shell
$ rails db:migrate
```

schema.rb で関連付けの構造の確認

* 子テーブルに親テーブルの主キー用のカラムが追加されているか？
* 子テーブルに親テーブルの主キーのインデックスが貼られているか？

```Ruby
# countries
  create_table "countries", force: :cascade do |t|
    t.string "country_name"
    t.string "region"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
    t.bigint "destination_id"
    t.index ["destination_id"], name: "index_countries_on_destination_id"
  end

# airlines
  create_table "airlines", force: :cascade do |t|
    t.string "airline_name"
    t.integer "country_id"
    t.string "alliance"
    t.integer "alliance_type"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
    t.bigint "destination_id"
    t.index ["destination_id"], name: "index_airlines_on_destination_id"
  end
```