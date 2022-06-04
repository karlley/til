# apt

Linux でapt を使ったツールの管理

## apt について

* Debian 用のパッケージ管理システム(MacOS のHomebrew みたいなもの)
* Advanced Packaging Tool
* パッケージ形式はDebian 形式(`.deb`)

### パッケージ管理システムとは

* リポジトリからパッケージをダウンロードしてくる
* ソフトウェアの導入、削除、ソフトウェアやライブラリとの依存関係を管理する
* パッケージ形式はOSの種類に依存する(RPM形式、deb形式、ports形式、pkg形式等)

### フロントエンドとは

* ユーザーがシステムとの入出力等の外部と直接やりとりを行うプログラムやモジュール
* CUIやライブラリなどにキーボード、マウス操作、GUIを提供するソフトウェアを指す場合もある

### sources.list とは

* パッケージのダウンロード先のリポジトリの情報を管理するファイル
* 公式のパッケージのリポジトリ情報は`etc/apt/sources.list` に記述
* サードパーティーのパッケージのリポジトリ情報は`etc/apt/sources.list.d/` に`xxx.list` の形式でファイルを追加

### sources.list の書式について

sources.list の書式は2つある

* deb/deb-src: 一般形式
* deb822: 新しい形式、まだ一般的でない?

```
# 書式の違い

# db/deb-src
deb [ option1=value1 option2=value2 ] uri suite [component1] [component2] [...]
deb-src [ option1=value1 option2=value2 ] uri suite [component1] [component2] [...]

# deb822
Types: deb deb-src
URIs: uri
Suites: suite
Components: [component1] [component2] [...]
option1: value1
option2: value2
```

### deb ファイルのディレクトリ

* `deb` ファイルはパッケージのダウンロード時に実際に取得するファイル
* リポジトリから取得したファイルは`/var/cache/apt/archives/` に置かれる

### apt/apt-get の違い

* 旧コマンドが`apt-get` で問題点を改善したコマンドが`apt`
* 公式では`apt` の使用を推奨している

## 標準パッケージのインストール/アンインストール

apt 標準のパッケージのインストール/アンインストール作業

### 参照

[Linux｜aptコマンドでパッケージ管理 \- わくわくBank](https://www.wakuwakubank.com/posts/276-linux-apt/)

### インストール

vim をインストールする

```Shell
# パッケージを完全一致検索
$ apt search ^vim$
$ apt list vim

# パッケージ検索の部分一致検索
$ apt search vim

# 権限でエラーがでたのでスーパーユーザーに切替
$ su

# インストール
$ apt install vim

# インストール確認
$ apt list --installed
$ apt list --installed | grep vim
$ dpkg -L vim
$ dpkg -l vim

# インストール済みのパッケージ一覧を表示
$ dpkg -l
```

### アップデート

* update: インストール可能なパッケージの更新(インストール前などに行う)
* upgrade: インストール済みのパッケージを最新に更新する(バージョンアップ的な)
* **管理者でないと実行できない**

```Shell
# インストール可能なパッケージのアップデート
$ apt update

# インストール済みのパッケージを更新
$ apt upgrade
```

### アンインストール

**管理者でないと実行できない**

```Shell
# アンインストール
$ apt remove vim

# 設定ファイル等を含めてアンインストール
$ apt purge vim

# 削除されたか確認
$ apt list --installed
$ apt list --installed | grep vim
$ dpkg -L vim
$ dpkg -l vim
```

## 標準以外のパッケージのインストール/アンインストール

標準以外(サードパーティー)のパッケージのインストール/アンインストール作業

### 参照

[apt\-getで見つからないパッケージを追加する方法\(debian, ubuntu両方対応\) \- Qiita](https://qiita.com/kon_yu/items/8ac350f3951f8534c931)

### インストール

Qiita の記事を参考に標準以外のパッケージ(iozone3)をインストール/アンインストール

`apt install` でインストールできない標準以外のパッケージあることを確認

```Shell
$ apt install iozone3
Reading package lists... Done
Building dependency tree
Reading state information... Done
E: Unable to locate package iozone3
```

現状のインストール済みのパッケージを確認

```Shell
$ cat /etc/apt/sources.list
# deb http://ftp.jp.debian.org/debian bullseye main

deb http://ftp.jp.debian.orb/debian bullseye main
deb-src http://ftp.jp.debian.org/debian bullseye main

deb http://security.debian.org/debian-security bullseye-security main
deb-src http://security.debian.org/debian-security bullseye-security main
```

必要なツールのインストール

* `apt-file`: 公式以外のパッケージの情報を取得するのに使用
* `apt-add-repository`: 公式以外のパッケージをインストールする際に`sources.list` にリポジトリ情報を追加するツール
* `software-properties-common`: `apt-add-repository` が含まれるパッケージ

```Shell
# apt-file のインストール
$ apt install apt-file
$ apt list --installed | grep apt-file
apt-file/stable,now 3.2.2 all [installed]

# apt-file を使ってapt-add-repository の情報を取得
$ apt-file update
$ apt-file search apt-add-reporisoty
software-properties-common: /usr/bin/apt-add-repository
software-properties-common: /usr/share/man/man1/apt-add-repository.1.gz

# apt-add-repository をインストールする為にsoftware-properties-common をインストール
$ apt install software-properties-common
$ apt list --installed | grep software-properties-common
software-properties-common/stable,now 0.96.20.1-1.1 all [installed]
```

`apt-add-repository` を使用してiozone3 のリポジトリ情報(non-free)を`sources.list` に追加

```Shell
$ apt-add-repository
$ cat /etc/apt/sources.list
# deb http://ftp.jp.debian.org/debian bullseye main

deb http://ftp.jp.debian.orb/debian bullseye main non-free
deb-src http://ftp.jp.debian.org/debian bullseye main non-free

deb http://security.debian.org/debian-security bullseye-security main non-free
deb-src http://security.debian.org/debian-security bullseye-security main non-free
```

iozone3 をインストール

```Shell
$ apt update
$ apt install iozone3
$ apt list --installed | grep iozone3
iozone3/stable,now 489-1 amd64 [installed]
```

### アンインストール

iozone3 をアンインストール

```Shell
$ apt remove iozone3
$ apt list --installed | grep iozone3
```

## apt の削除コマンドの違い

[apt\-get のpurgeとremoveについて](https://teratail.com/questions/119697)

* `apt remove`: パッケージを削除、設定ファイルは削除しない(再度インストールすると設定が引継がれる)
* `apt purge`: パッケージ、設定ファイルを削除
* `apt remove --purge`: `apt purge` と同じ
* `apt autoremove`: 依存関係の無い不要なパッケージを自動削除
