# add_foregin_key_exisiting_table

既存テーブルの既存カラムを外部キーカラムに変更する

## 参照

[Active Record マイグレーション \- Railsガイド](https://railsguides.jp/active_record_migrations.html)

[【Rails】 アソシエーションを図解形式で徹底的に理解しよう！ \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/association)

[後からreferencesカラムを追加しようとするとdb:migrateできない \- その辺にいるWebエンジニアの備忘録](https://kossy-web-engineer.hatenablog.com/entry/2018/09/03/022121)

[migrateでカラムに外部キーを追加して、モデルを関連づける【Day 4/30 2nd】｜K\.Tawara｜note](https://note.com/kentarotawara/n/n47ab0894b4aa)

[Ruby on Rails カラムの追加と削除 \- Qiita](https://qiita.com/azusanakano/items/a2847e4e582b9a627e3a)

## 流れ

既存カラムを残したまま外部キー用カラムを追加して影響が出ないように修正する

1. 外部キーに変更したい既存カラムを残したまま外部キー用のカラムを追加、モデル間の関連付け
2. 追加した外部キーカラムに合わせて周辺のコードを修正
  1. seed
  2. 参照表示コード
  3. テストコード
3. 動作テストで既存カラムに依存しない形になっているか確認
4. 不要な既存カラムを削除
5. 追加した外部キーのみで正常に動作するかテスト

## 外部キー用のカラム追加、モデル間の関連付け

不要な既存カラムを残したまま、関連付け用の外部キー用カラムを追加、モデル間の関連付けを追加

### 1. 外部キー用カラム(country_id)追加用マイグレーションファイル作成

* テーブル名は複数形
* 追加するカラムには`_id` は不要、自動的に`culumn_name_id` の形でカラムが追加される

```Shell
# Destinations テーブルにcoutnry_id を外部キー制約付きで追加
$ rails g migration AddCountryRefToDestinations country:references
```

* 追加するテーブル: Destinations
* 追加するカラム: country_id
* 追加するオプション: foreign_key

```Ruby
# 生成されたマイグレーションファイル
class AddCountryRefToDestinations < ActiveRecord::Migration[5.2]
  def change
    add_reference :destinations, :country, foreign_key: true
  end
end
```

### 2. seed.rb に外部キー用カラムの値を追加

環境別にseed ファイルを分けているので全てのファイルに外部キーの値を追加

```Ruby
# seed/development.rb
# seed/production.rb
# Destination
10.times do |n|
  Destination.create!(name: Faker::Address.city,
                      description: "This is Faker Destination No.#{n + 1}!",
                      # :country のCSV 表示に合わせて1-249 の数字を生成
                      country: Faker::Number.between(from: 1, to: 249),
                      # ↓これを追加
                      # seed で作成したCountry オブジェクトの中からランダムに国を選択
                      country_id: Country.find(Faker::Number.between(from: 1, to: 249)).id,
      spot: Faker::Address.street_name,
                      ...
```

### 3. マイグレーションとseed データの再生成

db データ削除 > マイグレーション > seed データ生成

```Shell
# db のデータ削除(develop, test, production)
$ rails db:reset

# 外部キー追加用マイグレーション実行
$ rails db:migrate

# マイグレーションが正常に行われたか確認
$ rails db:migrate:status

# seed 生成
$ rails db:seed
```

test 環境 のマイグレーションとseed データ生成(production 環境は今は不要)

```Shell
# test 環境 マイグレーション
$ rails db:migrate RAILS_ENV=test

# マイグレーションが正常に行われたか確認
$ rails db:migrate:status RAILS_ENV=test

# test 環境 seed データ生成
$ rails db:seed RAILS_ENV=test
```

### 4. コンソールで値の確認

* カラムが追加されているか
* seed.rb で作成したデータがカラムに保存されているか
* モデル間のリレーションを設定していないのでモデルを通した取得はまでできない

```Shell
# コンソール起動
$ rails console

# オブジェクトを1件表させる
> Destination.find(1)

# モデルを通して関節的なオブジェクト取得はまだできない
> country = Country.find(1)
> country.destinations
NoMethodError: undefined method `destinations' for #<Country:0x00007fbb7e785d98>
from /Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/activemodel-5.2.3/lib/active_model/attribute_methods.rb:430:in `method_missing'

# test 環境でコンソールを起動
$ rails console -e test

# コンソールの環境を確認
> Rails.env
"test"
```

### 5. モデル間のリレーション設定追加

Country モデルにDestination モデルを関連付ける

* `has_many` の場合は従モデルは複数形
* `has_one` の場合はどちらのモデルも単数形
* `belongs_to` は単数形

```Ruby
# 主になるモデル、主キー(primary key) を持っている
# app/model/country.rb

class Country < ApplicationRecord
  has_many :destinations
end
```

```Ruby
# 従になるモデル、外部キー(foreign key) を保存するカラムが存在するモデル
# app/model/destination.rb

class Destination < ApplicationRecord
  belongs_to :country
end
```

### 6. リレーション設定後のオブジェクト取得が可能か確認

```Shell
$ rails console

# country オブジェクトを取得
> country = Country.find(1)

# 間接的なオブジェクトの取得が可能か確認
$ country.destinatons
Destination Load (0.8ms)  SELECT "destinations".* FROM "destinations" WHERE "destinations"."country_id" = $1 ORDER BY "destinations"."created_at" DESC  [["country_id", 1]]
[]
```

### 7. 周辺のコードを追加した外部キー用カラムに合わせて修正

* model バリデーション
* model メソッド
* helper メソッド
* view
* locals
* controller
* factory
* rspec
* test_helper

factorybot で生成するオブジェクトをコンソールで確認

```Shell
$ rails console
> dest = FactoryBot.build(:destination)
> dest.save
```

### 8. 不要な既存カラムの削除

Destinations テーブルのcountry:string のカラムを削除

```Shell
$ rails g migration RemoveCountryFromDestinations country:string
```

```Ruby
class RemoveCountryFromDestinations < ActiveRecord::Migration[5.2]
  def change
    remove_column :destinations, :country, :string
  end
end
```

```Shell
$ rails db:migrate
```

DB のデータをリセットしてマイグレーション、seed 作成を確認

```Shell
# DB データ削除(test, development), マイグレーション
$ rails db:migrate:reset
$ rails db:migrate:reset RAILS_ENV=test

# マイグレーションが通ったか確認
$ rails db:migrate:status
$ rails db:migrate:status RAILS_ENV=test

# seed 作成
$ rails db:seed
$ rails db:seed RAILS_ENV=test

# seed が作成されているか確認
$ rails console
> Destination.all
> Country.all
$ rails console -e test
> Rails.env
> Country.all
```
