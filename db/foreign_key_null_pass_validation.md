# foreign_key_null_pass_validation

外部キー制約のカラムがnull でバリデーションをスキップする

## 参照

[【Rails】外部キーのカラムにnilを保存する方法 \- Qiita](https://qiita.com/onoblog/items/9ef2f0b60c64a4292b41#%E8%A3%9C%E8%B6%B3)

[optional: trueってなに \- Qiita](https://qiita.com/ryy/items/7e401f9600fb11c20215)

[Railsのbelongs\_toに指定できるoptional: trueとは？ \- そうきたか](https://blog.ryskit.com/entry/2018/01/27/195442)

## ポイント

* デフォルトでは外部キー制約のカラムがnull だとバリデーションで弾かれる
* 親/子の関係が必ず必要なイメージ

## 実装

Destinations テーブルのairline カラムのnull を許可する

```Ruby
# model/destination.rb
class Destination < ApplicationRecord
  # optionnal: ture でairline 未選択も許可する
  belongs_to :airline, optional: true
  validate :airline_id
end
```
