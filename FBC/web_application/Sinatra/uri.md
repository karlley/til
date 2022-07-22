# URI設計

* 一覧: `GET /memos`

* 作成画面: `GET /memos/new`
* 作成: `POST /memos`

* 詳細画面: `GET /memos/{memo_id}`

* 編集ボタン: 詳細画面に設置
* 編集画面: `GET /memos/{memo_id}/edit`
* 編集: `PATCH /memos/{memo_id}`

* 削除ボタン: 詳細画面に設置
* 削除: `DELETE /memos/{memo_id}`

# 処理の流れ

各処理の流れ

## 一覧表示

1. GET /memos

## 詳細表示

1. GET /memos/:id

## 作成

1. GET /memos
2. GET /memos/new
3. POST /memos
4. redirect to memos/:id
5. GET memos/:id

## 編集

1. GET /memos
2. GET /memos/:id
3. GET /memos/:id/edit
4. PATCH /memos/:id
5. redirect to /memos/:id
6. GET /memos/:id

## 削除

1. GET /memos
2. GET /memos/:id
3. DELETE /memos/:id
4. redirect to /memos
5. GET /memos
