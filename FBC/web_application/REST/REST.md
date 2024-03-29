# REST

* REpresentational State Transfer
* Webサービスの設計モデル
* URIにHTTPメソッドでアクセスすることでデータの送受信を行う
* データ形式はJSON が使われることが多い
* `GET`、`POST`、`PUT/PATCH`、`DELETE`が可能
* 多くのサービスがREST API を公開している
* セッション情報やCookie情報を保存して管理していない

# RESTful

> * アドレス指定可能なURIで公開されていること
> * インターフェース(HTTPメソッドの利用)の統一がされていること
> * ステートレスであること
> * 処理結果がHTTPステータスコードで通知されること

## アドレス指定可能なURIで公開されていること

* 1つのリソースに対して1つのアドレス指定可能
* アドレスが名詞のみで構成されている

## インターフェース(HTTPメソッドの利用)の統一がされていること

* REST で用いられるHTTP メソッドはCRUD 処理と対応している
  * POST:	CREATE
  * GET: READ
  * PUT: UPDATE
  * DELETE:	DELETE
* POST で指定するアドレスのid は自動採番される

## ステートレスであること

* サーバーがクライアントのセッション情報を保持しない
* ステートレスのメリット
  > * クライアントのリクエストはリソース操作に必要十分な情報になる
  > * セッション管理がシンプルになる
  > * スケーラブルなシステムにできる

## 処理結果がHTTPステータスコードで通知されること

* ステータスコード: 200から500までのコードでエラーの内容を表したもの

# RESTを構成する要素

> * 資源（RESOURCE） – URI
> * 行為（Verb） – HTTPメソッド
> * 表現（Representations）

# RESTの特徴

* Uniform: URIで指定されたリソースを統一
* Stateless: Session、Cookie 情報を保存しない、リクエストを処理することに限定する
* Cacheable: HTTP のキャッシュ機能が使用可能
* Self-descriptiveness: URI でデータに何を行うのか理解しやすい
* Client – Server構造: クライアントとサーバーで開発すべき内容が明確
* 階層型構造: セキュリティ、負荷分散、暗号化レイヤなどを柔軟に追加できる

# REST API の設計ガイド

> 1. URIは情報の資源を表現しなければならない。
> 2. 資源の行為は、HTTPメソッド（GET、POST、PUT、DELETE）で表現する。

* リソース名は`名詞`
* リソースの動作はHTTPメソッドで表現する(`動詞`)

> URIはリソースの表現に集中して、操作の定義はHTTPメソッドを使って処理するのが、REST APIの規則です。

# URI 設計時の注意点

* URI の最後に`/` を使用しない
* ハイフン`-` でURI の可読性を向上させる
* アンダースコア`_` はURI に使用しない
* URI には小文字を使用する
* ファイル拡張子をURI に含めない

===

# URL 設計で分からないところ

後で調べる

* リソース中心のパスとは？
* 同一リソースを指すパスは異なる動作でも同一のパスでないといけないのか？ `POST /users/{user_id}/tweet` と`POST /users/{user_id}/tweets/{tweet_id}/reply`


===

# ログインユーザーに対するリソースしか操作できないものにはuser_id は含めない

* `POST /users/{user_id}/tweets` のような形だと他人へのなりすましでtweet できてしまうのでNG
* ログインユーザーの情報はセッションを使って取得するので一般的にパスには含めない

# 削除系の処理はDELETE 以外でも表現できる

* UIや仕様によっては`PUT`や`PATCH` を使ってリソースを更新することで削除処理をすることも可能
* UIや使用に合わせて`DELETE`、`PUT`、`PATCH` を使い分ける必要がある

# 付属するリソースの表現

`/(リソースAの複数形)/(リソースAのID)/(リソースBの複数形)` や`/(リソースAの複数形)/(リソースAのID)/(リソースBの複数形)/(リソースBのID)` とすることで`リソースAのID: XXXに付属するリソースB` を表現できる

* 例1: lists リソースに付属するusers リソース
`/lists/{list_id}/users`

* 例2: lists リソースに付属するusers リソースの特定のuser
`/lists/{list_id}/users/{users_id}`

# どこに属したリソースなのか意識する

どこに属したリソースなのか意識して設計すること

フォロー一覧の表示
* 誰のフォロー一覧なのか分からないのでNG: `GET /following-users`
* 自分以外のユーザーのフォロー一覧も見れるのでOK: `GET /users/{user_id}/following-users`

リスト一覧の表示
* 誰のリスト一覧なのか分からないのでNG: `GET /lists`
* 自分以外のユーザーのリスト一覧も見れるのでOK: `/users/{user_id}/lists`