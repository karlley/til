# http

## http とは

> HTTP（HyperText Transfer Protocol）は、Web のサーバと、クライアント（ブラウザ）の間で、ウェブページを送受信するためのプロトコルです。

## メッセージ構文

* クライアント(ブラウザ)
  * リクエスト行: `メソッド パス名 HTTP/バージョン`
  * ヘッダ: `ヘッダ名: ヘッダ値`
  * 空行
  * メッセージボディ
* webサーバ
  * レスポンス行: `HTTP/バージョン ステータス番号 補足メッセージ`
  * ヘッダ: `ヘッダ名: ヘッダ値`
  * 空行
  * メッセージボディ

## GET とPOST の違い

* `POST` はヘッダ下部に送信する値が挿入されている
* `POST` はブラウザのアドレスバーに送信するパラメータが表示される
* 内部的な送信の形式は`GET` も`POST` も大差ない

## プロキシ

* Proxy: 代理
* インターネットに直接接続できないコンピュータに代わり、インターネットに接続してアクセスを行うサーバ
* 内部と外部のネットワークの中継をする役割を持つサーバ
* webプロキシ、SMTPプロキシ、FTPプロキシ などプロトコル毎にプロキシが存在する

## Referer

[リファラ \(referer\)とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word129.html)

* リンク元のURLの情報
* HTTPリクエスト時に一緒に送られる
* リファラを見る事でどのページから遷移してきたのか分かる
* アクセス解析、セキュリティ対策で確認する項目
* プロキシツール等で簡単に書き換えることができる

## Ajax

* JavaScript を仕様して動的にHTML を書き換える技術
* 画面遷移を行わずにページの更新ができる
* 通常のHTTP 通信とリクエスト内容は殆ど変わらない

## JSONP

* JSON を異なるドメイン間での通信を可能にする技術
* 別のドメインのサイトから自サイトのJavaScript の実行権限を与える
* 悪意のあるサイトからのJavaScript を使った攻撃を受けやすく使用には注意が必要
* WebAPI 等で使用される

## パーセントエンコード

* URL エンコード
* 文字コードを16進数で表し`%xx`の形で変換したエンコード方式
* パーセントエンコードから元の文字列を抽出することをデコード処理と呼ぶ

### パーセントエンコードをデコードする

`nkf` コマンドを使用してデコードする

```Shell
$ echo 文字列変換するパーセントエンコード | nkf --url-input

$ echo %E3%81%82%E3%81%84%E3%81%86%E3%81%88%E3%81%8A | nkf --url-input
あいうえお
```

### 文字列をパーセントエンコードする

`tr` コマンドを使用してパーセントエンコードする

参照: [シェルスクリプトでシンプルにurlエンコードする話 \- Qiita](https://qiita.com/ik-fib/items/cc983ca34600c2d633d5)

* 改行を示す`=` を`%` に置換
* 不要な`=` を削除
* 不要な`\n` を削除

```Shell
$ echo  パーセントエンコードに変換する文字列 | nkf -WwMQ | sed 's/=$//g' | tr = % | tr -d '\n'

$ echo  あいうえお | nkf -WwMQ | sed 's/=$//g' | tr = % | tr -d '\n'
%E3%81%82%E3%81%84%E3%81%86%E3%81%88%E3%81%8A
```

### nkf コマンド

* Network Kanji Filter
* 文字コードと改行コードを変換する

### sed コマンド

* stream editor
* 文字列の置換、行単位の出力を行う

### tr コマンド

* translate
* 文字の置換、削除を行う

## バイナリセーフ

* 制御文字が含まれていてもバイナリとして正しく機能するバイナリ
* 制御文字にはNULL文字、改行文字などがある

## web アプリの脆弱性を狙った攻撃

* SQL インジェクション
  * 能動的攻撃
  * アプリケーションが想定しないSQL を実行することでデータベースを不正に操作すること
* クロスサイトスクリプティング(XSS)
  * 受動的攻撃
  * 攻撃対象のweb アプリやweb サイトに悪意のあるスクリプトを仕込み、ユーザーがクリックするとスクリプトが実行され外部の悪意のあるサイトに誘導する攻撃
* クロスサイトリクエストフォージュリ(CSRF)
  * 受動的攻撃
  * 悪意のあるweb サイトに誘導し、ログイン情報を利用してユーザーが目的としているweb サービス等に意図しないリクエストを送信させる攻撃
* セッションフィクセーション
  * セッションID固定化攻撃
  * ユーザーのセッションID を乗っ取り、正規ユーザーになりすます攻撃

## telnet とは

telnet プロトコルでリモートマシンに接続するコマンド

* mac はhomebrew でインストールすると楽
* `https` には対応していないので`http` で接続する
* `http` に対応したポートを利用する(80番)

## telnet を使った接続

* GET
* POST
* 送信するメッセージボディについて
* Content-Length について

### GET

* 接続するホスト: `dummy-bootcamp-fjord-jp.herokuapp.com`
* 使用ポート: `80`
* 表示するページ: `dummy-bootcamp-fjord-jp.herokuapp.com/articles/1023`

```
# 接続
$ telnet dummy-bootcamp-fjord-jp.herokuapp.com 80
Trying 52.5.82.174...
Connected to va03.ingress.herokuapp.com.
Escape character is '^]'.

# リクエスト行
GET /articles/1023 HTTP/1.1

# ヘッダ行
Host: dummy-bootcamp-fjord-jp.herokuapp.com
ここでEnter を2回押して改行

# レスポンス
HTTP/1.1 200 OK
Server: Cowboy
Date: Mon, 31 Jan 2022 20:04:08 GMT
Connection: keep-alive
X-Frame-Options: SAMEORIGIN
...
342
<!DOCTYPE html>
<html>
  <head>
    <title>Dummy</title>
  </head>
...
</html>

0

Connection closed by foreign host.
```

### POST

* 接続するホスト: `dummy-bootcamp-fjord-jp.herokuapp.com`
* 使用ポート: `80`
* 送信するページ: `/articles` 
* 送信するメッセージボディ: `article[title]=Telnet Test&article[body]=test text`
* 送信するバイト数: `50`

```
# 接続
$ telnet dummy-bootcamp-fjord-jp.herokuapp.com 80
Trying 18.211.231.38...
Connected to dummy-bootcamp-fjord-jp.herokuapp.com.
Escape character is '^]'.

# リクエスト行
POST /articles HTTP/1.1

# ヘッダ行
Host: dummy-bootcamp-fjord-jp.herokuapp.com
Content-Length: 50
ここでEnter を2回押して改行

# メッセージボディ
article[title]=Telnet Test&article[body]=test text

# レスポンス
HTTP/1.1 302 Found
Server: Cowboy
Date: Mon, 31 Jan 2022 21:39:56 GMT
Connection: keep-alive
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Permitted-Cross-Domain-Policies: none
Referrer-Policy: strict-origin-when-cross-origin
Content-Type: text/html; charset=utf-8
Location: http://dummy-bootcamp-fjord-jp.herokuapp.com/articles/1224
Cache-Control: no-cache
X-Request-Id: 1ebb5e0b-8cee-4bc4-8108-fc908751299b
X-Runtime: 0.057965
Transfer-Encoding: chunked
Via: 1.1 vegur

7c
<html><body>You are being <a href="http://dummy-bootcamp-fjord-jp.herokuapp.com/articles/1224">redirected</a>.</body></html>
0

Connection closed by foreign host.
```

### 送信するメッセージボディについて

サンプルサイト下部の[New Article](http://dummy-bootcamp-fjord-jp.herokuapp.com/articles/new) ページのフォームを開発ツールで検証して送信先のname 属性を調べる

画像

上の画像から送信先のname属性は下記の2つ

* article[title]
* article[body]

メッセージボディに2つの値を送信する場合は`&`でメッセージを繋ぐ

```
article[title]=Telnet Test&article[body]=test text 
```
### Content-Length について

バイト数はメッセージボディの文字数を下記サイトでカウントして`Content-Length:` に指定する

[文字数カウント](https://www.luft.co.jp/cgi/str_counter.php)

# Burp Suite

[Burp Suiteの使いかた 1 解説とインストール方法 \| サイバー犯罪とセキュリティ](https://blog.azunyan1111.com/burp-suite%E3%81%AE%E4%BD%BF%E3%81%84%E3%81%8B%E3%81%9F-1-%E8%A7%A3%E8%AA%AC%E3%81%A8%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%96%B9%E6%B3%95/)

# Chrome とBurp Suite の設定

[macOS × Chrome でローカルプロキシツール Burp Suite を利用する](https://zenn.dev/faycute/articles/eb490f8839acda)