# environment_variable

環境変数について

## 参照

[【Rails】 dotenv\-railsを使って環境変数を管理しよう \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/dotenv-rails)

## 環境変数とは？

>環境変数とは、「全てのプログラムに共有して使うことが出来るWindows・MacOS・LinuxなどのOSが提供する変数のこと」です。

## ポイント

* コマンドで設定した環境変数はターミナルを閉じると消去される
* 通常は`.zshrc` や`.bashrc` などの設定ファイルに環境変数を定義する
* rails で使用する環境変数はgem `dotenv-rails` で管理すると変数の管理が楽になる
* ローカル環境とリモート環境では環境変数をそれぞれ設定する必要有り

## コマンド

環境変数の確認

```Shell
# mac 内に定義されている全ての環境変数を表示
$ printenv

# 変数毎に表示
$ echo $変数名
```

環境変数を追加

```Shell
# VAR_NAME 変数に値var_value を追加
$ export VAR_NAME = "var_value"
$ echo $VAR_NAME
var_value
```
