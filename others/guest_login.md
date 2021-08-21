# guest_login

ポートフォリオ用のゲストログイン

## 参照

[Railsアプリ　ゲストログイン機能を実装（devise不使用） \- Qiita](https://qiita.com/Hiroaki_jr/items/86f028a1a60b3a667475)

[簡単ログイン・ゲストログイン機能の実装方法（ポートフォリオ用） \- Qiita](https://qiita.com/take18k_tech/items/35f9b5883f5be4c6e104)

[Ruby on Rails 5 \- 条件によりtext\_fieldにreadonlyオプションを付与したい｜teratail](https://teratail.com/questions/169215)

## ポイント

* devise 不使用
* devise を使ったゲストログインには通常ログインもdevise を使って実装されている必要有り
* ゲストユーザー削除の対策として`find_or_create_by` メソッドを使用
* ゲストユーザー用に別に動作の制限を与える必要有り(users_edit 等)

## 実装

1. `guest_sessions_controller.rb` を作成
2. `guest_sessions_controller.rb` にゲストユーザーログイン用の記述追加
3. `guest_sessions#create` 用のルーティング設定

```Shell
# guest_sessions_controller を作成
$ rails g rails g controller guest_sessions
```

```Ruby
# guest_sessions_controller.rb
class GuestSessionsController < ApplicationController
  # log_in, remember メソッドを使えるようにする
  include SessionsHelper

  def create
    user = User.find_or_create_by(email: "guest@example.com") do |u|
      u.password = SecureRandom.urlsafe_base64
      u.name = "ゲストユーザー"
    end
    log_in user
    remember user
    flash[:success] = "ゲストユーザーでログインしました"
    redirect_to root_url
  end
end
```

```Ruby
# routes.rb
Rails.application.routes.draw do
.
  post "guest_login", to: "guest_sessions#create"
end
```

