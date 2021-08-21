# factorybot

FactoryBot について

## 参照

[thoughtbot/factory\_bot\_rails: Factory Bot ♥ Rails](https://github.com/thoughtbot/factory_bot_rails)

## コンソールで使う

`gem 'factory_bot_rails'` がdevelopment で使えるかコンソールで確認

```Ruby
group :development, :test do
.
gem 'factory_bot_rails', "~> 4.10.0"
.
end
```

コンソールで使う

```Shell
# ユーザー作成
> user = FactoryBot.build(:user)
> user.save

# 行き先作成
> dest = FactoryBot.build(:destination)
> dest.save
```

## インスタンス生成時の注意点

* 生成のタイミングはlet を使い事前に生成する方法, テスト実行時に生成する2パターン有る
* FactoryBot に渡す引数は代入した値を渡すこと
* インスタンスの種類によっては必要な引数を渡さないとインスタンスを生成してくれない
* 省略形で書いていることを意識しておく
* 省略形で書くには`rails_halper.rb` に設定が必要

省略形

```Ruby
# 略さない書き方
user = FactoryBot.create(:user)

# 省略形
let!(user) { create(:user) }
```

省略する為の設定

```Ruby
# rails_helper.rb

RSpec.configure do |config|
  config.include FactoryBot::Syntax::Methods
end
```

インスタンス生成

```Ruby
require 'rails_helper'

RSpec.describe "Destinations", type: :system do
# FactroyBot で事前にインスタンスを生成
let!(:user) { create(:user) }
let!(:destination) { create(:destination, :picture, user: user) }

describe "Destination Search ページ" do
  context "検索機能" do
    it "feed から検索ワードに該当する結果が表示されること" do
      # FactoryBot のインスタンスに渡す為の変数
      search_word = "東京"
      # 変数を使いインスタンスを生成
      # 変数を使わず直接値を渡すとインスタンスが生成されなかった
      # user: user の引数を渡さないとインスタンスが生成されなかった
      create(:destination, name: search_word, user: user)
      fill_in "q_name_cont", with: search_word
      click_button "Search"
      expect(page).to have_css "h1", text: "\"#{search_word}\" Search Results : 1"
      within(".destinations") do
        expect(page).to have_css "li", count: 1
      end
    end
  end
```

## 複数のインスタンスを生成

FactoryBot で複数のインスタンスを生成する方法

```Ruby
# 30件のuser インスタンスを生成する
create_list(:user, 30)

# 重複しない連番でインスタンスを生成する
sequence(:email) { |n| "example#{n}@example.com" }
```

## FactoryBot で生成した値でCSV からデータを取得する

* FactoryBot で生成した値はmodel_name.column_name で取得できる
* インスタンスの取得 > データの取得まではメソッドチェーンで繋げれる

FactroyBot で生成したcountry の値でCSV から国名を取得する

```Ruby
# FactoryBot でランダムな値を生成
FactoryBot.define do
  factory :destination do
    name { Faker::Address.city }
    # :country のCSV 用に1-249 の数字を生成
    country { Faker::Number.between(from: 1, to: 249) }
  end
```

```Ruby
# 生成したランダムな値を使いCSV の中のデータを取得
require 'rails_helper'

RSpec.describe "Destinations", type: :system do
  let!(:destination) { create(:destination) }
  let!(:country) { create(:country) }

  describe "Destination ページ" do
    context "ページレイアウト" do

      it "行き先情報が表示されること" do
        expect(page).to have_content destination.name
        # FactroyBot で生成した値から国のデータを取得
        country_data = Country.find_by(id: destination.country )
        # 取得した国のデータから国名を取得
        country_name = country_data.country_name
        expect(page).to have_content country_name
      end
    end
  end
end
```

メソッドチェーンでリファクタリング

```Ruby
      it "行き先情報が表示されること" do
        expect(page).to have_content destination.name
        # FactroyBot で生成した値から国のデータを取得 > 国名を取得
        country_name = Country.find_by(id: destination.country ).country_name
        expect(page).to have_content country_name
      end
```
