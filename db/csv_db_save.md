# csv_db_save

view からファイルを選択してインポートする方法ではなく、コマンドでCSV からDB に保存する方法

## 参照

[RailsでCSVファイルのデータをDBに取り込む \- Qiita](https://qiita.com/Ryuta1346/items/c21cb70b9879c66c8639)

[railsでcsvを読み込んで、DBへ保存する \- Qiita](https://qiita.com/SoarTec-lab/items/50e046ea2a2764c12c21)

[class CSV \(Ruby 3\.0\.0 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/class/CSV.html)

## 流れ

1. CSV ファイルを用意する
2. データ保存用のモデルを作成する
3. db/seeds.rb にインポートするCSV のパスと保存先のカラム等の設定を追加する
4. `rails db:seed` でデータをインポートしDB に保存する

## 実装

* 読み込み元のCSV ファイルの置き場所はとりあえずルートディレクトリに設置した
* 読み込み処理のファイルへのパスは絶対パスで動いた

`db/seeds/production.rb`, `db/seeds/production.rb`, `db/seeds/test.rb` にCSV 読み込み処理を記述

```Ruby
# Country CSV import
require "csv"

# CSV ファイルへのパスは絶対パス
CSV.foreach("country.csv", headers: true) do |row|
  Country.create!(
    country_name: row["国・地域名"],
    region: row["場所"]
  )
end

# Airline CSV import
CSV.foreach("airline.csv", headers: true) do |row|
  Airline.create!(
    airline_name: row["航空会社"],
    country_id: row["国番号"],
    alliance: row["アライアンス"],
    alliance_type: row["アライアンス種別"]
  )
end
```

## 使用コマンド

```Shell
# Country モデル作成
$ rails g model Country country_id:integer country_name:string region:string

# CSV をインポートしてDB に保存
$ rails db:seed

# DB を消去して全て入れ直す
$ rails db:migrate:reset
$ rails db:seed
```

## 備考

seed.rb でインポート処理を行う場合とバッチ処理として`rails runnner` コマンドで実装する方法もある

[rails runnerを使ってみた \- Qiita](https://qiita.com/3yatsu/items/416411c0a8f696dbf99e)

[Railsでバッチを書く時によく使うrails runnerとrake taskの違い \- Qiita](https://qiita.com/rllllho/items/672e336a03335cba6b34)
