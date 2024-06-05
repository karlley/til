# js npm

npmの使い方

## npm

- JavaScript 系のパッケージ管理ツール
- Node Package Manager
- Ruby の bundler みたいなもの

## npm のインストール

nodeと一緒にインストールされている

```sh
$ npm -v
9.6.4
```

## npm を使ってパッケージをインストールする

プロジェクト毎にパッケージをインストールする

- パッケージ管理用ファイルは`package.json`
- `package.json` は Ruby の Gemfile みたいなもの
- `npm init` で`package.json` が作成される

2 種類のパッケージのインストール方法がある

- ローカル: カレントディレクトリ内の`node_modules/` にインストール
- グローバル: システムディレクトリ内の`node_modules/` にインストール
  - Node.js のインストール方法によってディレクトリが異なる

```sh
$ cd project_name
$ npm init # package.json作成
# 全てenter
$ npm install パッケージ名 # パッケージのインストール
$ npm install パッケージ名@x.y.z # パッケージをバージョン指定でインストール
$ npm install -g # パッケージをグローバルインストール
```

`npm init -y` とすると問い合わせをスキップして`package.json` が作成できる

## インストール時に package.json への追記オプション

- `package-lock.json` は Rubyの`Gemfile.lock` のようにインストール後のnpmの状態が記述される
- 追記オプションを追加することで`package-lock.json` への追記内容を制御できる

- `--no-save`: 書き込まない
- `--save`: 依存パッケージは書き込む
- `--save-optional`: 依存するが必須ではない場合に書き込む
- `--save-dev`: 開発者が使用するパッケージの場合に書き込む
  - 開発環境のみで使用する場合のオプション
  - [【npm 初心者】なんんとなく使っていた npm install の \-\-save\-dev ついて調べてみた](https://zenn.dev/hrkmtsmt/articles/5f4a0e5c79b77a)

```sh
$ npm install パッケージ --no-save
$ npm install パッケージ --save
$ npm install パッケージ --save-optional
$ npm install パッケージ --save-dev
```

## npm のコマンド

```sh
$ npm ls # インストール済みパッケージ一覧
$ npm ls -g # グローバルインストール済みパッケージ一覧
$ npm outdated # 新しいバージョンの確認
$ npm outdated -g # 新しいバージョンの確認(グローバルインストール済み)
$ npm update # パッケージアップデート
$ npm uninstall パッケージ # パッケージをアンインストール
$ npm uninstall -g パッケージ # グローバルパッケージのアンインストール
$ npm search パッケージ # パッケージ検索
$ npm info パッケージ version  # 最新バージョンを表示
$ npm info パッケージ versions  # インストール可能なバージョン一欄
```

## emoj パッケージのインストール

例としてemojパッケージをインストールする手順

```sh
$ npm search emoj
$ npm install --global emoj
$ npm ls
$ emoj 'i love unicorns'
$ npm uninstall -g emoj
$ npm ls # 表示されていないのを確認
```

## 参照

[npm 入門 \- とほほの WWW 入門](https://www.tohoho-web.com/ex/npm.html)

[fbc\_js\_npm/README\.md at main · karlley/fbc\_js\_npm](https://github.com/karlley/fbc_js_npm/blob/main/README.md)
