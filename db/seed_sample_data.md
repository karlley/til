# seed_sample_data

seed に本番でも使用できるリアルなデータを生成する

## 参照

[faker\-ruby/faker: A library for generating fake data such as names, addresses, and phone numbers\.](https://github.com/faker-ruby/faker)

[ランダムな日本語のデータを生成するGemまとめ \- Qiita](https://qiita.com/Peranikov/items/12f1015fc20fa41844b3)

[cooklog\_v2/development\.rb at master · katochan55/cooklog\_v2](https://github.com/katochan55/cooklog_v2/blob/master/db/seeds/development.rb)

[【Ruby】Fakerを利用してテストダミーデータを用意する \| Enjoy IT Life](https://nishinatoshiharu.com/usage-faker/)

[Fakerを使ってダミーデータを作る【Day 8/30 2nd】｜K\.Tawara｜note](https://note.com/kentarotawara/n/n240bf41b5a54)

[簡易版Fakerを作ってGemの作り方の手法を学ぶ \- Qiita](https://qiita.com/Coolucky/items/4f8eaa2ad015e82fab41)

## ポイント

* seed で使用する場合は`require` 等の記述は不要
* seed では`create` ではなく`create!` で作成する事でデバックが容易になる
* seed で日本語データを生成する場合は`Faker::Config.locale = 'ja'` が必要

## コンソールで確認

`rails console` で読込無しで使える

```Shell
$ rails console

# 日本語に設定
> Faker::Config.locale = :ja
:ja

# 日本語の街名を出力
> Faker::Address.city
中田町
```
