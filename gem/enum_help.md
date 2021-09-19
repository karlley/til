# enum_help

enum を使用したセレクトボックスの日本語化に使用

## 参照

[zmbacker/enum\_help: Help ActiveRecord::Enum feature to work fine with I18n and simple\_form\.](https://github.com/zmbacker/enum_help)

## ポイント

* gem `rails-i18n` も必要
* gem `rails-i18n`はRails 2.2以降からRailsに同梱されている

## 導入

インストール

```Ruby
# Gemfile

gem 'enum_help'
```

```Shell
$ bundle install
```

i18n の日本語設定

* app/config/application.rb で設定する場合も有り
* Rails5 はinitializers/locals.rb を推奨

`add_local_setting.md`、`enum_error.md` を参照
