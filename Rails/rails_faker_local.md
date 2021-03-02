# rails_faker_local_20210225

Facker gem のlocal 変更

## 参考URL

[Faker を i18n locale :ja で使うときは Faker::Config\.locale を :en にセットしなおすべし \- Qiita](https://qiita.com/kyanny/items/00ef3727c7738f2cc26c)

[Railsでfakerのバージョンを上げたらspecが壊滅したけど設定で乗り越えたメモ \- 一から勉強させてください](https://dangerous-animal141.hatenablog.com/entry/2014/10/28/234314)

[Railsの初期設定に関して気をつけること（config/initializers/） \- Qiita](https://qiita.com/ryosuketter/items/03f841538aca5c7e7e83)

[Rails\.application\.config を config/initializers/\*\.rb で書き換えても反映されないケース \- Qiita](https://qiita.com/labocho/items/9ca9185cf82b824c8308)

## ポイント

* I18n gem 導入時にconfig/initializers/locale.rb にRails.application.config.i18n.default_locale = :jaを記述時点でFaker のlocale は:ja に変更される
* config/initializers/faker.rb を新規作成しFaker gem の設定を追加する必要がある

```Ruby
# config/initializers/faker.rb
# Faker のロケールをen に変更
Faker::Config.locale = :en
```
