# kaminari

## kaminari gemについての雑なまとめ

[kaminari gemについての雑なまとめ \- karlley's tech blog](https://karlley.hatenablog.jp/entry/2022/08/17/150918)

## Railsのto_sqlメソッドは生成するSQLを確認できる

`to_sql`でRailsが自動生成するSQLを確認できます。

```ruby
$ rails c
> Book.all.to_sql
=> "SELECT \"books\".* FROM \"books\""
```

[to\_sql \| Railsドキュメント](https://railsdoc.com/page/to_sql)

## SQLを生成するメソッドを呼ぶ順番は生成するSQLに影響しない

[SQLを生成するメソッドを呼ぶ順番は生成するSQLに影響しない \- karlley's tech blog](https://karlley.hatenablog.jp/entry/2022/08/17/151912)
