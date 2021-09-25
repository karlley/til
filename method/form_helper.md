# form_helper

セレクトボックス等を生成するフォームヘルパーについて

## 参照

[Action View フォームヘルパー \- Railsガイド](https://railsguides.jp/form_helpers.html#%E3%82%BB%E3%83%AC%E3%82%AF%E3%83%88%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9%E3%82%92%E7%B0%A1%E5%8D%98%E3%81%AB%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)

## ポイント

* Rails の基本的なフォームヘルパーはヘルパー名が`_tag` になっている
* 基本的なフォームヘルパー(`_tag` で終わる)は1つの`input` 要素を生成する
* 検索フォーム生成時にはGETを使いURL パラメータにクエリを含むようにする
* model を扱うフォームヘルパーにはヘルパー名に`_tag` が付いていない
* ヘルパーに渡すのはインスタンス変数の名前を渡す(:person、"person"等)
* `method` オプションを使用してPOST、GET以外のPATCH、PUT、DELETE 等のメソッドが使える

## 分類

form 系

* `form_tag`: form タグ生成、POST
* `form`: model を扱うform タグ生成
* `form_for`: formとmodel を簡単に結びつける、フォームビルダーオブジェクト`f` でループを使用してフォームタグを生成

select 系

* `select_tag`: select タグ生成、オプションの文字列でセレクトボックス生成
* `select`: model を使用したselect タグ生成、DB から値を取得するので選択済みの項目が`selected` になる
* `collection_select`: model の情報を元にselect タグを生成

option 系

* `options_for_select`: option タグを生成、`value` 値がコントローラにに送信される、`value` と`text` はmap メソッドで配列化して呼び出す必要がある
* `options_from_collection_for_select`: model を元に`value`と`text` を呼び出してオプションタグを生成
