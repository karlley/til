# Linux

Linux について

## サーバーとクライアント

* サーバーとクライアントは「サービスを提供する側」と「サービスを受ける側」
* HTTPサーバー、HTTPクライアントと呼ぶ
* 例 HTTPサーバー: nginx、HTTPクライアント: Google Chrome

## サーバーへの接続

* サーバーへの接続はsshを使う
* sshでの接続ができない場合にはVNC コンソールを使いサーバーを操作する

## さくらVPSにDebian をインストール

学習用にさくらVPSにオープンソースLinuxのDebianをインストール

## 参照

[Debian 9 / Debian 10 — さくらの VPS マニュアル](https://manual.sakura.ad.jp/vps/os-reinstall/custom/de9-10.html)

にDebian10 Busterをインストール \- asmp78 blog](https://asmp78.hatenablog.com/entry/2020/01/16/072133#:~:text=sakura.ad.jp-,Debian10%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB,%E3%81%97%E3%81%A6%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%82%92%E5%AE%9F%E8%A1%8C%E3%80%82))

### 契約情報、初期インストール

* 初期インストールOSではDebianを選択できないのでVPSサーバ立ち上げ後、再度カスタムOSでOS を再インストールする
* 初期インストールOSは再インストールするので何を選択しても良い
* サーバ選択
  * ゾーン: 石狩第1
  * プラン: 512M
  * ストレージ: 25GB
* サーバ設定
  * インストールOS: Ubuntu
  * OSバージョン: 最新
  * スタートアップスクリプト: 利用しない
  * サーバー接続のSSHキーの登録: 登録しない

### Debian を再インストール

**Chrome拡張機能vimmiumを使うとVNCコンソールの挙動がおかしくなるのでoff**

1. サーバー一覧で`OS再インストール`を選択
2. `カスタムOS`、インストールOSは`Debian 11 amd64`を選択
3. VNCコンソールを起動
4. `root password`、`full name`、`user`、`user password` を設定
5. パーティーションの設定で`Guided – use entire disk`を選択、確認画面で`Yes`を選択
6. OSインストール終了後に`Continue`を選択するとVNCコンソールが終了する 

### Chrome 拡張機能vimnium を停止

VNCコンソールで誤作動を起こすので一時的に停止する

### VNCコンソールのキー配列設定

* USキーボードを使用する場合は設定が必要
* 設定変更にはサーバーをシャットダウンする必要がある

1. サーバー一覧からキー配列を変更するサーバーを選択
2. `コンソールを設定する`を選択
3. VNCコンソールキー配列を`ja`から`en-us`に変更

### サーバー起動

1. サーバー一覧画面で起動するサーバを選択して`起動する`を選択
2. `稼働中`が表示されサーバーが起動する

### VNCコンソールでログイン

1. サーバー一覧から起動するサーバーを選択
2. サーバーパネルから`コンソール`を選択、`VNCコンソール`を選択
3. VNCコンソールに`user`と`user password` を使いログイン
 
### OSのバージョン確認

OS毎にディレクトリ構造が異なる

```
$ cat /etc/*release
$ cat /etc/*version
```

