# will_paginate_error

gem will_paginate で`undefined method `total_pages' for ` エラーについて

## 参照

[＃<User :: ActiveRecord\_Relation：0x000000051b4608>の未定義のメソッド \`total\_pages '・問題＃411・mislav / will\_paginate](https://github.com/mislav/will_paginate/issues/411)

[bootstrap\-ruby/will\_paginate\-bootstrap: Integrates the Twitter Bootstrap pagination component with will\_paginate](https://github.com/bootstrap-ruby/will_paginate-bootstrap)

[Railsでwill\_paginateとwill\_paginate\-bootstrapを使った \- light log](http://yamacent.hatenablog.com/entry/201っ/05/19/224703)

## やりたいこと

`@destinations` を使ってindexページでページネーションを表示させたい

```Ruby
# Controller
def index
  # current_userがいいね!したdestinations を取得
  favorite_destinations = current_user.favorite_destinations.paginate(page: params[:page], per_page: 12)
  # パーシャル表示用に代入
  @destinationos = favorite_destinations
end
```

```Ruby
# View
<%= will_paginate %>
```

## エラー内容

```
ActionView::Template::Error - undefined method `total_pages' for #<Favorite::ActiveRecord_Associations_CollectionProxy:0x00007feef4c4f7f8>:
```

* controller からview への値は正常に渡っている
* controller の`paginate` メソッドの引数は正常
* view に渡る`@destinations` は`Destination::ActiveRecord_AssociationRelation < ActiveRecord::AssociationRelation` クラスである
* 表示を確認しているpaginate に渡っている値のクラスは`Destination::ActiveRecord_Relation < ActiveRecord::Relation` である

## 解決方法

view の`will_paginate` に引数でcontroller からのインスタンス変数`@destinations` を渡す

```Ruby
# View
<%= will_paginate @destinations %>
```

## 見解

* `ActiveRecord::AssociationRelation` に対応していないgem will_paginate のバグ？
* gem will_paginate では`<% will_paginate %>` 表記だが、gem bootstrap-will_paginate では`<%= will_paginate @変数 %>` 表記になっているのでどっちが正しいのか不明
