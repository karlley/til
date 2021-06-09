# render_@variables

`render` メソッドで@変数をつかったパーシャルの呼び出しについて

## 参照

[【Rails基礎】ややこしい部分テンプレートの省略形について簡単にまとめてみた｜TechTechMedia](https://techtechmedia.com/partial-template-rails/)

[Ruby \- renderメソッドにインスタンス変数をとる書き方について｜teratail](https://teratail.com/questions/85856)

[忘れがちなrenderメソッドの使い方まとめ \[Rails\] \- Qiita](https://qiita.com/hayashino/items/c2a4e7d3edbdcce3cd2a#render%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%82%92%E3%83%93%E3%83%A5%E3%83%BC%E3%81%A7%E4%BD%BF%E3%81%86)

[パーシャルをrenderする際のパフォーマンスに関する注意点 \- Qiita](https://qiita.com/itmammoth/items/612efc6ad3280349b7e1)

[Viewでの繰り返しをコレクションをレンダリングすることで実装する【Rails】 \- Qiita](https://qiita.com/nizi24/items/c2d609c709bbafdfc5c9)

[レイアウトとレンダリング \- Railsガイド](https://railsguides.jp/layouts_and_rendering.html#%E3%82%B3%E3%83%AC%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%92%E3%83%AC%E3%83%B3%E3%83%80%E3%83%AA%E3%83%B3%E3%82%B0%E3%81%99%E3%82%8B)

## ポイント

* `@変数` でのパーシャル呼び出しは省略形である
* 単数形, 複数形で使用用途が異なる
  * 単数形: 部分テンプレートで持っていきたい値がある場合に使用
  * 複数形: 部分テンプレートで繰り返し処理をしたい場合, 繰り返し処理に使いたい値がある場合に使用, `each` を使った繰り返し処理の記述が不要になる
* 複数形は`collection` で`@変数` が呼ばれるので繰り返し処理になる
~~* 見通しが悪くなるのでできるだけ使用しない方が良さそう~~
  * パフォーマンスの観点から@変数を使った使ったコレクションを優先的に使った方が良い
  * 冗長だが`render partial: 'tweets/tweet', collection: @tweets` の形で記述すると見通しが良い
  * パーシャルに渡るローカル変数名は`partial: 'hoge'` にしていた場合は`hoge` がローカル変数名になる

```Ruby
# 単数形の原型
render partial: ‘tweet’, locals: { tweet: @tweet }

# 単数形の省略形
render 'tweets/tweet', tweet: @tweet
render @tweet

# 複数形の原型
render partial: 'tweets/tweet', collection: @tweets

# 複数形の省略形
render partial: ‘tweet’, collection: @tweets
render @tweets
```
