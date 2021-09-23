# validation_error_message_japanese

バリデーションエラーのメッセージを日本語化する

## 参照

[【Rails】バリデーションのエラーメッセージを取得・表示・日本語化する方法を完全解説！｜TechTechMedia](https://techtechmedia.com/error-messages-validation-rails/)

[【Rails】バリデーションエラーメッセージの日本語化〜ja\.yml〜 \- Qiita](https://qiita.com/bon_eng/items/87085891f4ed5f27eb9d)

## エラー確認

コンソールでエラーの詳細を確認

```Shell
$ rails console

# 空でDestination オブジェクトを作成
> destination = Destination.new

# バリデーション確認
> destination.valid?
false

# エラー内容確認
> destination.errors
#<ActiveModel::Errors:0x00007fd0be7a8c08 @base=#<Destination id: nil, name: nil, description: nil, user_id: nil, created_at: nil, updated_at: nil, picture: nil, spot: nil, latitude: nil, longitude: nil, address: nil, expense: nil, season: nil, experience: nil, food: nil, country_id: nil, airline_id: nil, region_id: nil>, @messages={:user=>["を入力してください"], :country=>["を入力してください"], :user_id=>["を入力してください"], :name=>["を入力してください"], :region_id=>["を入力してください"], :country_id=>["を入力してください"], :expense=>["は一覧にありません"], :season=>["を入力してください"]}, @details={:user=>[{:error=>:blank}], :country=>[{:error=>:blank}], :user_id=>[{:error=>:blank}], :name=>[{:error=>:blank}], :region_id=>[{:error=>:blank}], :country_id=>[{:error=>:blank}], :expense=>[{:error=>:inclusion, :value=>nil}], :season=>[{:error=>:blank}]}>

# エラーメッセージ確認
> destination.errors.full_messages
[
    [0] "Userを入力してください",
    [1] "Countryを入力してください",
    [2] "Userを入力してください",
    [3] "行き先名を入力してください",
    [4] "地域を入力してください",
    [5] "国を入力してください",
    [6] "費用は一覧にありません",
    [7] "シーズンを入力してください"
]

# country カラムのエラーメッセージ確認
> destination.errors.full_messages_for(:country)
[
    [0] "Countryを入力してください"
]

# user カラムのエラー詳細確認
> destination.errors.details[:user]
[
    [0] {
        :error => :blank
    }
]
```

```
# yml のcountry_id を削除した結果
[4] pry(main)> destination.errors.full_messages
[
    [0] "Userを入力してください",
    [1] "Countryを入力してください",
    [2] "Userを入力してください",
    [3] "行き先名を入力してください",
    [4] "地域を入力してください",
    [5] "Countryを入力してください",
    [6] "費用は一覧にありません",
    [7] "シーズンを入力してください"
]
```

## 原因

* 関連付け済みのモデルの外部キー(user_id, country_id)に`presense: ture` でバリデーション指定する事でエラー時のメッセージに関連付け済みのモデル名(user, country) が表示される為
* ja.yml の`attributes` に関連付け済みのモデル名の翻訳情報が無い

[【Rails】 Railsのバリデーションの使い方をマスターしよう！ \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/validation)

> 色々調べるとrails5からアソシエーションを定義しているとusersテーブルとjobsテーブルとの結びつけをするjob_idというカラムにデフォルトで入力必須のバリデーションが定義されてしまうためだということがわかりました。
この場合アソシエーションの定義であるbelongs_toのバリデーションに引っかかるとjob_idではなく、モデル名であるjobがエラーメッセージ内に表示されるようです。

[Ruby on Rails 5 \- 外部キーのバリデーション、エラーメッセージが重複してしまう｜teratail](https://teratail.com/questions/268530)

>validateionを書かないでも presence のvalidationが定義されています。
validationを書いたので二回行うことになり、二回出力された次第

## 対策

* 関連付け済みのモデルにバリデーションを設定すると2重でバリデーションする事になるが、エラー項目へのマーキングを自動で行ってくれるので外部キーへの`presense: true` を設定する
* ja.yml の`attributes` へのモデル名の翻訳情報を追加

```Ruby
# app/models/destination.rb

class Destination < ApplicationRecord
  belongs_to :user
  belongs_to :country
  # optionnal: ture でairline 未選択も許可する
  belongs_to :airline, optional: true
  has_many :favorites, dependent: :destroy
  has_many :comments, dependent: :destroy

  default_scope { order(created_at: :desc) }
  mount_uploader :picture, PictureUploader

  # 外部キーでのバリデーション追加
  validates :user_id, presence: true
  validates :name, presence: true, length: { maximum: 50 }
  validates :region_id, presence: true
  validates :country_id, presence: true
  # 費用をenum で管理する
  enum expense: {
    till_50000: 1,
    till_100000: 2,
    till_200000: 3,
    till_300000: 4,
    till_500000: 5,
    till_700000: 6,
    till_1000000: 7,
    over_1000000: 8,
  }
  # enum 定義以外の値を許可しない
  validates :expense, inclusion: { in: Destination.expenses.keys }
  validates :season, presence: true
  validate :airline_id
  .
end
```

```Ruby
# config/local/ja.yml

---
ja:
  activerecord:
    models:
      user: ユーザー
      destination: 行き先
      comment: コメント
    attributes:
      destination:
        # バリデーションエラーで表示される関連付けたモデル名
        user: ユーザー
        country: 地域/国
        # ここまで
        id: ID
        user_id: ユーザー
        name: 行き先名
```
