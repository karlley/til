# render_as_option

`render` 使用時の`as:` オプションについて

## 参照

[Rails 部分テンプレートの使い方 \- Qiita](https://qiita.com/shizuma/items/1c655dadd2e04b3990a8)

[Rails　パーシャル\(部分テンプレート\)へローカル変数を渡したいとき \- Qiita](https://qiita.com/takannporo/items/a8ff93109afc3bc3bab4)

[Railsでrender collectionを使う方法とメリットを解説！変数名を変更したい場合はasを使う – himakuroブログ](https://himakuro.com/rails-render-collection)

## ポイント

* `as: '変数名'` でオプションを指定する
* `collection` を使用したループ表示のみ使用可能、ループ表示でない場合は`locals` オプションを使用してローカル変数を指定可能

## 使い方

`collection` を使いパーシャル内で変数をループ表示する際にローカル変数名を変更する際に使用する

```Ruby
# posts/index.html.erb
#  パーシャルarticle に@articles を渡して、ローカル変数toukou でループを回す

<%= render partial: 'posts/article', collection: @articles , as: 'toukou' %>
```

```Ruby
# posts/_article.html.erb
# パーシャル内でローカル変数toukou を使いコンテンツを表示する

<p>パーシャルarticle 内でcollection でループ表示しているコンテンツは<%= toukou.content %>です。</p>
```

```Ruby
# posts/_article.html.erb
# as: オプションを使わない場合はパーシャル名がローカル変数になる

<p>パーシャルarticle 内でcollection でループ表示しているコンテンツは<%= article.content %>です。</p>
```
