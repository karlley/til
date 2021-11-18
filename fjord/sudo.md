# sudo

スーパーユーザーや他のユーザーで一時的にコマンドを実行するコマンド

## sudo のメリット

* `sudo` コマンドでの実行ログが残るので不具合の発生時に追跡しやすい
* `root` アカウントを無効化することでセキュリティを強化できる
* `su` コマンドよりも細かくセキュリティを設定できる

## su とsu - の違い

* `su`: ホームディレクトリは切り替えずにユーザーのみ切り替える、環境変数は引き継がれない
* `su -`: ホームディレクトリを`/root` に切り替える、環境変数を切り替えたユーザーを引き継ぐ
* 環境変数に依存した操作(ユーザー固有のコマンド等)を行いたい場合に`su -` を使う

## 導入時のポイント

* コマンドの実行権限を`/etc/sodoers`に記述する
* `etc/sodoers` の編集には`visudo` コマンドを使用する
* `visudo` コマンドは保存時に構文チェックが行われる
* `su` ではなく`su -` でroot ユーザーに切り替えないと`visudo` コマンドが実行できない

## visudo の起動エディタをnano からvi に変更

`visudo` コマンドのデフォルトエディタがnano になっていたのでvim に変更

[Ubuntuのvisudoエディタが変nanoになる \- それマグで！](https://takuya-1st.hatenablog.jp/entry/20110423/1303585363)

[Vim の種類 \(Vim family\) \- Qiita](https://qiita.com/b4b4r07/items/f7a4a0461e1fc6f436a4)

```Shell
# デフォルトのエディタを確認
$ ls -l /etc/alternatives/editor
.../etc/alternatives/editor -> /bin/nano

# エディタを変更
$ su
$ update-alternatives --config editor
使用したいエディタの番号を選択してエンター

# エディタが変更されたか確認
$ ls -l /etc/alternatives/editor
.../etc/alternatives/editor -> /usr/bin/vim.basic

# visudo でvim が起動するか確認
$ su-
$ visudo
vim が起動すればok
```

## sudo インストール

apt 経由でインストール


```Shell
$ apt list sudo
$ su
$ apt install sudo
$ apt list --installed | grep sudo
sudo/stable,now 1.9.5p2-3 amd64[installed]
```

権限設定

**`su` ではなく`su -` でないと`visudo` が実行できない**

[【Debian 8 Jessie】sudoコマンドをインストールする \- Qiita](https://qiita.com/osktak/items/f1746d64797a6a4dd6ae#sudo%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B)

```
$ su -
# visudo
user名 ALL=(ALL:ALL) ALL を追加
```

権限の確認

```
# sudo 無しでシャットダウンできないことを確認
$ shutdown -r now
-bash: shutdown: command not found

# sudo 有りでシャットダウンできることを確認
$ sudo shutdown -r now
OS が再起動される
```
