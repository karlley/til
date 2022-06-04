# Cookie

## Cookie とは

* web サーバからユーザーのコンピューターに送られる情報の一部
* HTTP Header の中で送信される
* ブラウザ上に内部的に保存される
* ユーザーの認証(ログイン等)に使用される
* 最大4KB まで送信できる
* 実行コードは含めることはできない
* 有効期限が設定されていない場合はブラウザを閉じた時点で破棄される

## HTTP Cookie に保存される情報

* `NAME=VALUE`: Cookie の値、必須項目、日本語を扱う場合にはURLエンコードが必要
* `Expires=DATE`: Cookie の有効期限
* `Max-Age=DATE`: Cokkie の残存期間を秒数で指定
* `Domain=DOMAIN_NAME`: Cookie が有効なドメイン名
* `Path=PATH`: Cookie が有効なパスの指定
* `Secure`: 通信チャンネルの指定
* `HttpOnly`: Cookie をJavaScript からアクセスできないように制限する

## HTTP Cookie の種類

* Persistent Cookie: 長期的、自動ログイン等に使用、設定した期間を過ぎると破棄される
* Session Cookie: 一時的、ログイン時に使用、サーバに対するセッションが終わった時点で破棄される

## Cookie の為のヘッダフィールド

* `Set-Cookie`: サーバからセッション開始時に送られる、クライアントに保存してほしい値を送信するフィールド
* `Cookie`: クライアントからサーバに送る値を送信するフィールド
