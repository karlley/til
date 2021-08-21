# factorybot_association

FactoryBot で関連モデルのインスタンスも同時に生成する

## 参照

[【FactoryBot】associationの使い方 \- Qiita](https://qiita.com/Ryoga_aoym/items/741c57e266a9d811a2d4)

[RSpec \- FactoryBotを導入し、効率良く意味のあるデータを作成する \- fv17の日記 \- Coding Every Day](https://forest-valley17.hatenablog.com/entry/2018/09/28/075644)

## ポイント

* 関連モデルのインスタンスも同時に生成したい時に使用する
* 関連モデルがあるからといって必ず指定しないといけない訳ではない
* `cleate_list` でのインスタンス生成時には`favorite` がループするので`user` もループ分生成される

Favorite モデルはuser, destination に関連付けられている

```Ruby
# Favorite model

class Favorite < ApplicationRecord
  belongs_to :user
  belongs_to :destination
end
```

destination.user に関連付けれたdestination インスタンスをfavorite で同時に生成する

```Ruby
# spec/factories/favorite.rb

FactoryBot.define do
  factory :favorite do
    association :destination
    user { destination.user }
  end
end
```

## 注意点

`association` の指定を関連モデルどちらにも設定すると重複したインスタンスが生成されてしまう

```Ruby
# 重複するuser が生成されてしまう書き方
# spec/factories/favorite.rb

FactoryBot.define do
  factory :favorite do
    association :destination
    association :user
  end
end
```


