# pry-byebug

pry-byebug の使い方

* デバッグツール
* Gem でインストール

## 参照

[deivid\-rodriguez/pry\-byebug: Step\-by\-step debugging and stack navigation in Pry](https://github.com/deivid-rodriguez/pry-byebug)

## インストール

プロジェクトのGemfile に追記してインストール

```Ruby
# 開発環境, テスト環境で使用する
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  # 下記を追加
  gem 'pry-byebug'
end
```

```Shell
$ bundle install
```

## 使い方

デバッグしたいところに`binding.pry` を挿入

```:user_controller.rb
  def index
    @users = User.all
    # 下記を挿入
    binding.pry
  end
```

サーバを起動してpry で値を確認しながらデバッグ

`binding.pry` を挿入した箇所で処理が停止するのでpry 上で値を確認する

```Shell
$ rails s
.
From: /Users/karlley/Workspace/gyakuten/crud_sample/app/controllers/users_controller.rb @ line 29 UsersController#update:

    24: def update
    25:   user = User.find(params[:id])
    26:   user.update(user_params)
    27:   redirect_to :action => "index"
    28:   binding.pry
 => 29: end

[1] pry(#<UsersController>)> user
=> #<User:0x00007fe365928ab0
 id: 5,
 name: "resources",
 age: 1000,
 created_at:
  Thu, 02 Jan 2020 20:39:17 UTC +00:00,
 updated_at:
  Thu, 02 Jan 2020 20:39:26 UTC +00:00>
.
```
