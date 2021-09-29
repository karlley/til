# faker_locale_setting

gem faker のlocale 設定

## 参照

[Faker::Config\.locale](https://qiita.com/kyanny/items/00ef3727c7738f2cc26c)

## 状態

```Ruby
# config/initializers/locale.rb

# 元の設定
rails.application.config.i18n.default_locale = :ja

# 元の設定を削除して下記の3つの設定を追加した

# locals ファイル読込ディレクトリ
i18n.load_path += dir[rails.root.join('config', 'locales', '**', '*.{rb,yml}').to_s]

# デフォルト言語
i18n.default_locale = :ja

# アプリケーションで有効化する言語指定
i18n.available_locales = [:ja, :en]
```

```Shell
# 元の設定での言語設定
[1] pry(main)> I18n.locale
:ja
[2] pry(main)> Faker::Config.locale
:en

# 追加した設定でも変化無し
# 元の設定での言語設定
[1] pry(main)> I18n.locale
:ja
[2] pry(main)> Faker::Config.locale
:en
```

## 結果

Faker の言語設定は`:en` になっているけど特にエラーは出ていないのでそのまま進めた

