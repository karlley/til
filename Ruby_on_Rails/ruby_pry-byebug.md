# Ruby pry-byebug

https://github.com/deivid-rodriguez/pry-byebug

https://qiita.com/ryosuketter/items/da3a38d0d41c7e20a2d6

## install

```
$ cd working_dir
$ gem install pry-byebug
```

## how to use

1. デバッグするファイルに`require 'pry'` を追記
2. ブレイクポイントに`binding.pry` を追記
3. `ruby` コマンドでファイルを動かすとデバッグモードになる

## command

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