# factorybot_trait

factorybot で特定の条件毎にインスタンスを生成する

## 参照

[factory\_bot/GETTING\_STARTED\.md at master · thoughtbot/factory\_bot](https://github.com/thoughtbot/factory_bot/blob/master/GETTING_STARTED.md#traits)

[FactoryBot Traits \- Qiita](https://qiita.com/ogomr/items/935da1072301ddc1aeaf)

## ポイント

*
* trait を指定のインスタンス作成はtrait 名のシンボルを第2引数に渡す

## 使い方

インスタンス作成用のfactory に条件毎のtrait を追加して呼び出す

```Ruby
# 旅先のインスタンス作成用のfactory
FactoryBot.define do
  factory :destination do
    name { Faker::Address.city }
    # seed で作成したCountry オブジェクトの中からランダムに国を選択
    country_id { Country.find(Faker::Number.between(from: 1, to: 249)).id }
    sequence(:description) { |n| "Destinaton No.#{n}!" }
    spot { Faker::Address.street_name }
    latitude { Faker::Address.latitude }
    longitude { Faker::Address.longitude }
    # address { Geocoder.search([latitude, longitude]) }
    address { Faker::Address.full_address }
    # :expense のenum 用に1-8 の数字を生成
    expense { Faker::Number.between(from: 1, to: 8) }
    season { Faker::Number.between(from: 1, to: 12) }
    experience { "Your Experience!" }
    # seed で作成したAirline オブジェクトの中からランダムに国を選択
    airline_id { Airline.find(Faker::Number.between(from: 1, to: 90)).id }
    food { Faker::Food.dish }
    association :user
    created_at { Time.current }
  end

# 条件を追加したfactory ← trait
  # 国番号指定
  trait :region_asia do
    name { "Region_Asia" }
    # 日本
    country_id { 153 }
  end

  # 体験指定
  trait :experience_activity do
    name { "Experience_Activity" }
    experience { "アクティビティ" }
  end

  # 航空会社指定
  trait :alliance_star_alliance do
    name { "Alliance_Star_Alliance" }
    # 全日空
    airline_id { 2 }
  end
  ```

## コンソールで使う

```Shell
# trait 無しでインスタンス作成 > 保存
> destination = FactoryBot.build(:destination)
> destination.save

# trait 指定してインスタンス作成
# 国番号指定
> destination = FactoryBot.build(:destination, :region_asia)
> destination.save
```
