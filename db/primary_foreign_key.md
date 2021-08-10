# primary_foreign_key

関連付けで使用する主キー(primary key)と外部キー(foreign key)について

## 参照

[Ruby on Rails \- Railsで独自に設定した主キーを外部キーに設定できない｜teratail](https://teratail.com/questions/234943)

## ポイント

* 主キーとは`親テーブル名の単数形_id` として自動で割り当てられるカラムに保存される値
* 外部キーとは子テーブルに保存される親モデルの主キーの値
* 主キーは`親テーブル名の単数形.id` で取得可能
* 外部キーは`子テーブル名の単数形.親テーブル名の単数形_id` で取得可能

```Ruby
# 主キーの取得
# 親: users, 子 posts

user.id
post.user_id
```

