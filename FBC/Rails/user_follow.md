# user follow memo

## redirect_toメソッドのnoticeの2つの書き方

[【Rails】redirect\_toでのnoticeの2つの書き方 \- 時々とおまわり](https://karlley.hatenablog.jp/entry/2023/01/06/175548)

## request.refererとは？

[【Rails】request\.refererとは？ \- 時々とおまわり](https://karlley.hatenablog.jp/entry/2023/01/06/194930)

## 関連付けたモデルの外部キーへのpresense: trueは必要ない

[【Rails】関連付けたモデルの外部キーへのpresense: trueは必要ない \- 時々とおまわり](https://karlley.hatenablog.jp/entry/2023/01/07/081205)

## orderメソッドの使い方

[【Rails】orderメソッドの使い方 \- 時々とおまわり](https://karlley.hatenablog.jp/entry/2023/01/08/185104)

## 外部キー制約とは？

[【Rails】外部キー制約とは？ \- 時々とおまわり](https://karlley.hatenablog.jp/entry/2023/01/10/165450)

## rails g controllerで作成するファイルのディレクトリを指定する

[rails g controllerで作成するファイルのディレクトリを指定する \- 時々とおまわり](https://karlley.hatenablog.jp/entry/2023/01/10/172852)

## DHH流のコントローラ分割について

[【Rails】DHH流のコントローラ分割について \- 時々とおまわり](https://karlley.hatenablog.jp/entry/2023/03/31/104410)

===

## render

`render` を使って遷移する場合は`flash.now` を使用しないとフラッシュメッセージが表示されない点に注意。

[flashとflash\.nowの違いを検証してみた \- Qiita](https://qiita.com/taraontara/items/2db82e6a0b528b06b949)

`render` と`redirect_to` の違いは？

## データ保存の失敗時の分岐の方法

* 成功: `redirect_to`
* 失敗: `render`

[レイアウトとレンダリング \- Railsガイド](https://railsguides.jp/layouts_and_rendering.html#render%E3%81%A8redirect-to%E3%81%AE%E9%81%95%E3%81%84%E3%82%92%E7%90%86%E8%A7%A3%E3%81%99%E3%82%8B)

[【Rails】データ保存の失敗時には、'redirect\_to'ではなく'render'を利用した方が良い理由 \- Qiita](https://qiita.com/yuki-n/items/2e64a179838c9086ab30)

## flashとflash.nowの違い

[flashとflash\.nowの違いを検証してみた \- Qiita](https://qiita.com/taraontara/items/2db82e6a0b528b06b949)

## resouces、resouce、member、collectionの違い

[index、create・new・edit・show・update・destroy](https://techracho.bpsinc.jp/baba/2020_11_20/15619)

[Railsのルーティングの種類と要点まとめ \- Qiita](https://qiita.com/senou/items/f1491e53450cb347606b)

```ruby
# member
followings_user GET    /users/:id/followings(.:format)           users#followings
followers_user  GET    /users/:id/followers(.:format)            users#followers

# resourceで修正
user_follower  GET     /users/:user_id/follower(.:format)        followers#show
user_following GET     /users/:user_id/following(.:format)       followings#show

# resoucesで修正
user_followers  GET     /users/:user_id/followers(.:format)      followers#index
user_followings GET     /users/:user_id/followings(.:format)     followings#index
```

* `resources`: index、create、new、edit、show、update、destroy、id有り
* `resource`: create、new、edit、show、update、destroy、index無し、id無し
* `member`: 7つ以外のルーティングを追加、id有り
* `collection`: 7対外のルーティングの追加、id無し

## 階層になったパーシャルの読み込み方

[\[Rails\] partialが自身からの相対パスで他partailを参照する場合の注意 \- Qiita](https://qiita.com/eduidl/items/5ea74df2db471e15b4cd)

## DBの制約とバリデーションで異なる例外が搬出される

* DBの制約: ActiveRecord::RecordNotUnique
* バリデーション: ActiveRecord::RecordInvalid 

[validates メソッドでユニーク制約を行うと ActiveRecord::RecordNotUnique ではなく、 ActiveRecord::RecordInvalid が raise される \- Qiita](https://qiita.com/ikamirin/items/1a197fe13f09ae863d08)

https://qiita.com/ikamirin/items/1a197fe13f09ae863d08

## createは成否に関わらずモデルオブジェクトを返す

[【Rails】 createメソッドの使い方とは？new・saveメソッドとの違い \| Pikawaka](https://pikawaka.com/rails/create)

## 例外に関して
### 例外を使ったエラーハンドリングについて

* 例外を使ってエラーハンドリングすることで、処理の失敗の原因を明確にできる
* Active Recordのトランザクションは例外の送出をロールバックのトリガーになっているので例外を扱う必要がある
* ガード節が増える、処理のネストが深い場合は例外を使った方がスッキリ書ける
* ブール値を使うだけで十分な場合は無理に例外を使わなくてもok

###  例外処理の Rails.loggerについて

* 例外処理に`Rails.logger` は必須ではない
* 問題を記録、分析に使う場合は必要
  * 基本的に起きてほしくないことだけどたまに起きうることがある、ただユーザーの操作は続行させたいので例外を捕捉してロギングしつつ、処理を続行する場合など
* ログを残す目的が明確な場合に限ってロギング処理を追加する

## byebugでソースを読む

[初心者でもカンタンにRailsの中身のコードをコードリーディングする方法 \- Qiita](https://qiita.com/jabba/items/8a9ac664eb2a0e61e621)

[printデバッグにさようなら！Ruby初心者のためのByebugチュートリアル \- Qiita](https://qiita.com/jnchito/items/5aaf323ab4f24b526a61#byebug%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%81%A7%E3%82%B3%E3%83%BC%E3%83%89%E5%86%85%E3%81%AB%E3%83%96%E3%83%AC%E3%83%BC%E3%82%AF%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%82%92%E5%9F%8B%E3%82%81%E8%BE%BC%E3%82%80)

[til/rubygems\.md at master · karlley/til](https://github.com/karlley/til/blob/master/FBC/Ruby/rubygems.md)

1. byebug gemが入っているか確認
2. `byebug` を該当コードに挿入
3. `rails s` 
4. byebugでstep実行でソースコードを掘っていく


