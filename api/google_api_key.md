# Google API Key

Google アカウントを使ってAPI Key を取得する

* プロジェクト作成
* API 有効化
* API Key 作成
* 請求情報追加

## 参照

[Google Maps API を使ってみた \- Qiita](https://qiita.com/Haruka-Ogawa/items/997401a2edcd20e61037)

[Rails5でGoogleMapを表示してみるまで \- Qiita](https://qiita.com/tiara/items/4a1c98418917a0e74cbb)

## 流れ

1. 新しいプロジェクトを作成
2. 作成したプロジェクトを開く
3. APIとサービス > ライブラリ
4. 必要なAPI を検索して有効化する
5. Maps JavaScript API, Geocodeing API の2つを有効化する
6. APIとサービス > ダッシュボード > 対象のAPI を選択 > 認証情報 > 同意画面を構成
7. OAuth 同意画面 に遷移する > 外部を選択 > 作成
8. OAuth 同意画面 > アプリケーション名のみ入力 > 保存
9. APIとサービス > 認証情報 > 認証情報を作成 > APIキー > API Key が作成される
10. APIとサービス > 認証情報 > 作成したAPI を開く > API 名, HTTP リファラー選択, localhost を追加 > 保存
11. お支払い > 同意して続行 > 必要な情報を入力して保存

## 注意点

* OAuth とAPI キーは別なので同意画面の流れは不要かも?
* 請求先の登録は最初の1回だけでok
* 請求情報を追加しないと動作しない
