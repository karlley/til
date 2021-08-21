# factroybot_callbacks

テスト用データ生成時にコールバックで処理を追加する

## 参照

[FactoryBot\(FactoryGirl\)チートシート \- Qiita](https://qiita.com/morrr/items/f1d3ac46b029ccddd017#%E3%82%B3%E3%83%BC%E3%83%AB%E3%83%90%E3%83%83%E3%82%AF%E3%82%92%E5%8F%97%E3%81%91%E3%82%8B)

[factory\_bot/GETTING\_STARTED\.md at master · thoughtbot/factory\_bot](https://github.com/thoughtbot/factory_bot/blob/master/GETTING_STARTED.md#callbacks)

[FactoryBotでそのモデルの持つメソッドを生成後に実行する \- コード日進月歩](https://shinkufencer.hateblo.jp/entry/2018/05/15/103318)

## コールバックの種類

* `after(:build)`:	ファクトリがビルドされた後
* `before(:create)`:	ファクトリがDBに保存される前
* `after(:create)`:	ファクトリがDBに保存された後
* `after(:stub)`:	スタブとして生成された後

## 使い方

ベースとなるFactory 生成時にコールバック

```Ruby
FactoryBot.define do
  factory :destination do
    name { Faker::Address.city }
    # seed で作成したCountry オブジェクトの中からランダムに国を選択
    country_id { Country.find(Faker::Number.between(from: 1, to: 249)).id }
    sequence(:description) { |n| "Destinaton No.#{n}!" }
    spot { Faker::Address.street_name }
    latitude { Faker::Address.latitude }
    longitude { Faker::Address.longitude }
    address { Faker::Address.full_address }

    # インスタンス作成後にaddress を取得 ←コールバック
    after(:build) do |destination|
      destination.add_address
    end
  end
```

trait で特定の条件の追加時にコールバック

```Ruby
FactoryBot.define do
  factory :destination do
  .
end

trait :excute_callback do
  callback action
end
```
