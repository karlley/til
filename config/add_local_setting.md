# add_local_setting

Rails を多言語化する設定の追加方法

## 参照

[【Rails5】initializers/locale\.rbを使ったWebアプリの多言語化設定について、駆け出しVimmerが説明してみた](https://zenn.dev/yukihaga/articles/83a8f1e07ed193)

[【Rails】i18nによる日本語化 \- Qiita](https://qiita.com/tai_tai_work/items/3af41931958456a73211)

## ポイント

* 多言語化の設定には2通りの方法がある
  * パターン1: `config/application.rb` に記述
  * パターン2: `config/initializers/local.rb` に記述
* rails 5 はパターン2 での設定方法を推奨されている
* 多言語化用のロケールファイルの作成には2通りの方法がある
  * パターン1: gem `rails_i18n` を使用
  * パターン2: ダウンロードして作成

## 実装

Rails 5 なのでinitializers で設定

```Ruby
# config/initializers/locale.rb

# locals ファイル読込ディレクトリ
I18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}').to_s]

# デフォルト言語
I18n.default_locale = :ja

# アプリケーションで有効化する言語指定
I18n.available_locales = :ja
```

コード記述後に`config/locales/` に各言語のロケールファイルを設置する

## コンソールで確認

* 日本語化されているかコンソールで確認
* `テーブル名.カラム名複数形_i18n` で翻訳情報が出力される

```Ruby
$ rails console
> Destination.expenses_i18n
{
      "till_50000" => "￥10,000 ~ ￥50,000,",
     "till_100000" => "￥50,000 ~ ￥100,000,",
     "till_200000" => "￥100,000 ~ ￥200,000,",
     "till_300000" => "￥200,000 ~ ￥300,000,",
     "till_500000" => "￥300,000 ~ ￥500,000,",
     "till_700000" => "￥500,000 ~ ￥700,000,",
    "till_1000000" => "￥700,000 ~ ￥1000,000,",
    "over_1000000" => "￥1000,000 ~,"
}
```
