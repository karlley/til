# add_column

DB にカラムを追加する

## 参照

[Railsでデータベースカラムの追加を行う方法 \- Qiita](https://qiita.com/ryosuketter/items/1aba45161bfffefed68b)

[Ruby on Railsでテーブルのカラムを追加・削除する方法 \- Reasonable Code](https://reasonable-code.com/rails-column/)

[【Rails】ターミナルでテーブル内容確認【PostgreSQL】 \| データは友達ドットコム](https://data-is-friend.com/%E3%80%90rails%E3%80%91%E3%82%BF%E3%83%BC%E3%83%9F%E3%83%8A%E3%83%AB%E3%81%A7%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E5%86%85%E5%AE%B9%E7%A2%BA%E8%AA%8D%E3%80%90postgresql%E3%80%91/)

## ポイント

* テーブル名は複数形
* カラム名は単数形

## 手順

1. カラム追加用マイグレーションファイル作成
2. マイグレーション実行
3. カラムが追加されているかテーブル確認

```Shell
# マイグレーションファイル作成
$ rails g migration Addカラム名Toテーブル名 カラム名:データ型

# destinations テーブルにinteger 型のregion_id カラムを追加
$ rails g migration AddRegion_idToDestinations region_id:integer
```

```Ruby
# 生成されたマイグレーションファイル
# db/migrate/20210910080733_add_region_id_to_destinations.rb

class AddRegionIdToDestinations < ActiveRecord::Migration[5.2]
  def change
    add_column :destinations, :region_id, :integer
  end
end
```

```Shell
$ rails db:migrate
```

```Shell
# マイグレーションステータス確認
$ rails db:migrate:status
```

```Shell
# dbconsole でカラムの追加を確認
# dbconsole 起動
$ rails dbconsole

# destinations テーブルの情報を表示
> departure_development-# \d destinations;
```
