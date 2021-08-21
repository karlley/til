# factorybot_foreign_key

factorybot を使用したテストデータ作成時に外部キー保存用のカラムに保存する値について

## 参照

[FactoryBotで外部キーを使用する場合のやり方　〜別解 \- Qiita](https://qiita.com/TerToEer_sho/items/3f7958503fe81aedd6d1)

[FactoryGirl\.createのパラメータにActiveRecordのオブジェクトを設定するとActiveRecord::AssociationTypeMismatchが発生した \- Qiita](https://qiita.com/adebadayo/items/430ccaa3bda4d8ade239)

[Railsの外部キー制約とreference型について \- Qiita](https://qiita.com/ryouzi/items/2682e7e8a86fd2b1ae47)

[Ruby On Railsで質問に対してのBA機能 \- スタック・オーバーフロー](https://ja.stackoverflow.com/questions/26959/ruby-on-rails%E3%81%A7%E8%B3%AA%E5%95%8F%E3%81%AB%E5%AF%BE%E3%81%97%E3%81%A6%E3%81%AEba%E6%A9%9F%E8%83%BD)

## ポイント

* 外部キー用カラムには関連付けされているモデルのオブジェクトでなければならない
* 外部キー用カラムに正しい値が生成されないと`ActiveRecord::AssociationTypeMismatch:` が出る

## 外部キー制約付きカラムを追加した場合

`country` カラムから`country_id` カラムを外部キー制約付きで追加した場合

* Country モデルはseed でCSV インポート済み(オブジェクト作成済み)
* `country_id` はランダムな番号を生成したい

```Ruby
# 外部キー制約無しのカラムの場合は以下でok
FactoryBot.define do
  factory :destination do
    # 1 - 249 までのランダムな数字を生成
    country { Faker::Number.between(from: 1, to: 249) }
  end
end
```

```Ruby
# 外部キー制約を付けたカラムを追加後のテストデータ
FactoryBot.define do
  factory :destination do
    # seed で作成したCountry オブジェクトの中からランダムに国を選択
    country_id { Country.find(Faker::Number.between(from: 1, to: 249)).id }
  end
end
```
