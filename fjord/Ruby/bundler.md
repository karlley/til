# Bundler

Bundler について

## Bundler とは

* gem同士の互換性を保ちながら導入/管理を行う
* `Gemfile` にインストールするgem を記述して管理する
* Bundler 以外のgem はBundler で管理するのが良い
* Ruby 2.6 以降はBundler が標準添付になった

## Bundler の導入

```Shell
$ gem install Bundler
$ bunder -v
```

## Bundler の使い方

[Bundlerの使い方 \- Qiita](https://qiita.com/oshou/items/6283c2315dc7dd244aef)

基本コマンド

```Shell
# Gemfile を生成
$ bundle init

# パス指定無しでインストール
$ bundle install

# パス指定で一括インストール(非推奨)
$ bundle install --path vendor/bundle

# Bundler のgem を使ってプログラムを実行
$ bundle exec ruby ファイル名 
```

* `Gemfile` の先頭に`source "rubygems"` を付ける
* 一度`--path vendor/bundle` を付けてインストールしたら、2回目以降はパス指定しなくて良い
* gem インストール後に`gemfile.lock` が自動生成される

実行ファイルへの記述

```Ruby
# gem を読み込む
require "rubygems"

# bundle でインストールしたgem を使う
require "bundler/setup"
```

## Bundler を素のRuby で使う

[【Day39】rubocop の使い方を知る \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/reports/45239)

* Bundler を使ってgem を管理した方が良い
* `bundle init` は各プロジェクト毎に行いプロジェクト毎に`Gemfile` を設置する

## --path vendor/bundle での警告

[bundle install時に"\[DEPRECATED\] The \`\-\-path\` flag is deprecated"という警告が発生した場合の対応手順 \- Qiita](https://qiita.com/jnchito/items/62adbea043abf72fa7cc)

`bundle install --path vendor/bundle` で`flag is deprecated..` という警告が発生した

```Shell
# --path vendor/bundle に対する警告
[DEPRECATED] The `--path` flag is deprecated because it relies on being remembered across bundler invocations, which bundler will no longer do in future versions. Instead please use `bundle config set --local path 'vendor/bundle'`, and stop using this flag
```

* 原因: `--path` オプションはパスを記憶させるので非推奨
* 対策:  `bundle install` 実行前に`bundle config` で明示的にパス指定する方法が推奨されている

```Shell
# プロジェクト単位でインストールパスをvender/bundle に指定
# 従来の--path vender/bundle と同一
$ bundle config set --local path 'vendor/bundle'

# マシン全体でインストールパスをvender/bundle に指定
$ bundle config set path 'vendor/bundle'
```

## --path vendor/bundle の必要性について

[bundle install時に\-\-path vendor/bundleを付ける必要性は本当にあるのか、もう一度よく考えてみよう \- Qiita](https://qiita.com/jnchito/items/99b1dbea1767a5095d85)

> 様々なバージョンの様々なgemがグローバルにインストールされている状況下でも、Bundlerがうまくgem同士の依存関係をハンドリングしてくれるはずです。

> Bundlerが十分に普及した現在でも「おまじない」として生き伸びているのかもしれません。

> 僕はこれまで何百というgemをインストールしてきたはずですが、僕が記憶している限り、問題が起きたのはこの2件だけです。

> デフォルトのまま使った方が、余計な手順が増えず、シンプルだから

* Bundler を導入しているプロジェクトであればgem の依存関係の問題は解消してくれる
* パス指定しないことによる問題が起こる確率はかなり低い
* オプションの付け忘れ、チーム間での統一がしやすい

上記の理由から`--path vendor/bundle` オプションは付けない方が良さそうだと感じました。
