# user_follow

## 1. Friendshipモデル作成

中間テーブルであるFriendshipsテーブルを作成。

* `following_id`: フォローしている`user_id`、integer型
* `follower_id`: フォローされている`user_id`、integer型

```shell
$ rails g model Friendship following_id:integer follower_id:integer
```

作成されたmigrationファイルを下記のように修正。

```ruby
# db/migrate/〇〇_create_friendships.rb

class CreateFriendships < ActiveRecord::Migration[6.1]
  def change
    create_table :friendships do |t|
      t.integer :following_id, null: false
      t.integer :follower_id, null: false

      t.timestamps
    end
    add_index :friendships, :following_id
    add_index :friendships, :follower_id
    add_index :friendships, [:following_id, :follower_id], unique: true
  end
end
```

* `null: false`: 空データの保存を禁止する
* `add_index`: データ検索を高速化する
* `unique: true`: テーブル内で重複するデータを禁止する

`add_index :friendships, [:following_id, :follower_id], unique: true` の部分は`following_id` と`follower_id` の組み合わせが必ずユニークになるようにする為に指定します(複合キーインデックス)。

[第12章 ユーザーをフォローする \- Railsチュートリアル](https://railstutorial.jp/chapters/following_users?version=4.2#sec-a_problem_with_the_data_model)

修正後、`migrate` して反映させます。

```shell
$ rails db:migrate
```

## 2. User、Friendshipの2つのモデルに関連付け、バリデーションを追加

2つのモデルへ関連付け、バリデーションを追加します。

```ruby
# app/models/friendship.rb

# frozen_string_literal: true

class Friendship < ApplicationRecord
  belongs_to :following, class_name: 'User'
  belongs_to :follower, class_name: 'User'

  validates :following_id, presence: true
  validates :follower_id, presence: true
end
```

```ruby
# app/models/user.rb

# frozen_string_literal: true

class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable

  has_one_attached :avatar

  # どのユーザーをフォローするか
  has_many :active_friendships, class_name: 'Friendship', foreign_key: 'follower_id', dependent: :destroy, inverse_of: :follower
  # どのユーザーにフォローされるか
  has_many :passive_friendships, class_name: 'Friendship', foreign_key: 'following_id', dependent: :destroy, inverse_of: :following

  # フォローする全ユーザー
  has_many :followings, through: :active_friendships, source: :following
  # フォローされる全ユーザー
  has_many :followers, through: :passive_friendships, source: :follower
end
```

### class_name

> 関連付けの相手となるオブジェクト名を関連付け名から生成できない事情がある場合、class_nameオプションを用いてモデル名を直接指定できます。

関連先のUserモデルのオブジェクト名を`following`、`follower` として元のモデル名と異なる名前に指定しています。
`class_name` オプション無しだと`following`、`follower` と同一名のテーブルを探しに行ってしまうので、Userモデルを参照するために`class_name: 'User'` が必要。

[Active Record の関連付け \- Railsガイド](https://railsguides.jp/association_basics.html#belongs-to%E3%81%AE%E3%82%AA%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3-class-name)

[アソシエーションにおけるclass\_nameの定義！ \- Qiita](https://qiita.com/wacker8818/items/eccdf0a63616feb14a70)

### foreign_key

参照するテーブルの外部キーを指定する。

* `active_friendships`: 「どのユーザーをフォローするか」なのでフォローする対象ユーザーの`follower_id` に設定
* `passive_friendships`: 「どのユーザーにフォローされるか」なのでフォローされる対象ユーザーの`following_id` に設定

### inverse_of

> Active Recordは、:throughや:foreign_keyオプションを使う双方向関連付けを自動認識しません。関連付けの反対側でカスタムスコープが使われていると、同様に自動認識しなくなります。

* `foreign_key` オプションを使用した双方向の関連付けの為、明示的に記述することが必要。
* `inverse_of` オプションを付与しない場合、rubocopで`Rails/InverseOf: Specify an :inverse_of option.` の警告が出る。

[Active Record の関連付け \- Railsガイド](https://railsguides.jp/association_basics.html#%E5%8F%8C%E6%96%B9%E5%90%91%E9%96%A2%E9%80%A3%E4%BB%98%E3%81%91)

[RuboCopの Rails/InverseOf について調べた \- sometimes I laugh](https://sil.hatenablog.com/entry/rubocop-rails-inverse-of)

[\[ActiveRecord\] 双方向関連付けとinverse\_of](https://zenn.dev/igaiga/books/rails-practice-note/viewer/ar_inverse_of)

### source

>  :source オプションには:throughで指定したモデルから、取得したいモデルへたどるための関連名を書きます。

`through` で指定したモデル(`active_friendships`、`passive_friendships`)を通して取得したいオブジェクト名を指定します。

* フォローしている人(`followings`): フォロー関係(`active_friendships`)の`following` カラムから対象ユーザーのフォロー関係を探す。
* フォローされている人(`followers`): 被フォロー関係(`passive_friendships`)の`follower` カラムから対象ユーザーの被フォロー関係を探す。

[\[ActiveRecord\] through, source](https://zenn.dev/igaiga/books/rails-practice-note/viewer/ar_through_source)

