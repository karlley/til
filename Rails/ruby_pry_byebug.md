# ruby_pry_byebug

Ruby 単体のアプリでpry-byebug を使う

[deivid\-rodriguez/pry\-byebug: Step\-by\-step debugging and stack navigation in Pry](https://github.com/deivid-rodriguez/pry-byebug)

[pry\-byebug を使ってRailsアプリをデバックする方法 \- Qiita](https://qiita.com/ryosuketter/items/da3a38d0d41c7e20a2d6)

## インストール

gem を使いインストール

```
$ cd working_dir
$ gem install pry-byebug
```

## 使い方

1. デバッグするファイルに`require 'pry'` を追記
2. ブレイクポイントに`binding.pry` を追記
3. `ruby` コマンドでファイルを動かすとデバッグモードになる

## コマンド

pry-byebug 用のコマンド

```
next
# 次の行を実行

step
# 次の行かメソッド内に入る

continue
# プログラムの実行をcontinueしてpryを終了

finish
# 現在のフレームが終わるまで実行
```
