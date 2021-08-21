# delete_column

カラムを削除する

## 参照

[Ruby on Rails カラムの追加と削除 \- Qiita](https://qiita.com/azusanakano/items/a2847e4e582b9a627e3a)

[カラムを追加・削除・編集する方法 ❏Rails❏ \- Qiita](https://qiita.com/ITmanbow/items/2bf4dfa4d9ca705b97df)

## 手順

1. 削除用マイグレーションファイル生成
2. マイグレーション

## 実行

削除用マイグレーションファイル生成

```Shell
$ rails g migration Removeカラム名Fromテーブル名 カラム名:型
```

Destinations テーブルのString 型のcountry カラムを削除

```Shell
$ rails g migration RemoveCountryFromDestinations country:string
```

生成されたマイグレーションファイル

```Ruby
class RemoveCountryFromDestinations < ActiveRecord::Migration[5.2]
  def change
    remove_column :destinations, :country, :string
  end
end
```

マイグレーションのステータスを確認

* 生成したマイグレーションが存在している
* 生成したマイグレーションが`down` になっている

```Shell
$ rails db:migrate status
$ rails db:migrate:status RAILS_ENV=test
```

マイグレーション

```Shell
$ rails db:migrate
$ rails db:migrate RAILS_ENV=test
```

再度マイグレーションのステータスを確認

* 全てのマイグレーションが`up` になっている
* develop, test どちらの環境も確認

```Shell
$ rails db:migrate status
$ rails db:migrate:status RAILS_ENV=test
```

db/schema.rb でカラムが削除されているか確認
