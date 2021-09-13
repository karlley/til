# add_db_column_option

既存テーブルに`default`、`null` オプションを追加する

## 参照

[RailsでNotNull制約を後からカラムに追加する方法2種類 \- Qiita](https://qiita.com/MOssan-32/items/89afc9e4375215f8b5d2)

## 手順

1. マイグレーションファイル生成
2. マイグレーションファイル記述
3. マイグレーション

```Shell
# マイグレーションファイル生成
# destinations テーブルexpense カラムにdefault, null オプションを追加

$ rails g migration AddNotnullAndDefaultToExpenseOnDestinations
```

```Ruby
# 生成されたマイグレーションファイルは空なので追記

class AddNotnullAndDefaultToExpenseOnDestinations < ActiveRecord::Migration[5.2]
  def change
    change_column :destinations, :expense, :integer, default: 0, null: false
  end
end
```

```Shell
# マイグレーション

$ rails db:migrate
```
