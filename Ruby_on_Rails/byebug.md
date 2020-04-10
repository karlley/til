### byebug

rails は基本gem の中にbyebug が含まれている

```
$ byebug -v
Running byebug 11.1.1
```

e.g.) values_controller.rb のindexアクションの中身を調べたい

1. 調べたいファイルに`byebug` を追記
2. `rails s` でサーバ起動(-d オプションは付けない)
3. ブラウザで対象アクションを実行させる(/http://127.0.0.1:3000/values/index)
4. サーバ起動画面に`(byebug)` が表示される
5. 調べたい値を入力する
6. `ctr + D` で終了

```
  def index
    @values = Value.all
    @values_count = Value.count
    byebug <= 調べたい場所に追記
  end
```

```
$ rails s
(byebug) @values_count
9
```
** `byebug` の場所を変えたらサーバを再起動しないと動かない **