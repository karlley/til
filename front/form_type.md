# form_type

`form` ヘルパーの種類

## 参照

railstutorial 7章,13章

## 種類

* `form_for`: model 有り(DB に保存する系)
* `form_tag`: model 無し(保存しない, 検索バー系)
* `form_with`: form_for/_tag 両方イケる(記述は異なる)

## 使い方

新しいオブジェクトを作る

<form action="/users" class="new_user" id="new_user" method="post">

* `action`: 送り先
* `method="post"`: 新しいオブジェクトを作成

`/users` + `POST` > createに送信される
