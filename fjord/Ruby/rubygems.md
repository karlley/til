# ruby gems

ライブラリの作成や公開、インストールを助けるシステム

## gem とは

* ライブラリの作成や公開、インストールを助けるシステム
* バージョン1.9 以降は標準添付されている

## gem の検索

`gem search` で検索できる

* `^文字列`で前方一致
* `文字列$`で後方一致
* `^文字列$`で完全一致
* `-d` で詳細情報を見る
* `-a` で前バージョン表示

```Shell
# byebug を完全一致で検索
$ gem search -r ^byebyg$

# -d で詳細情報を見る
$ gem search -r ^byebug$ -d

# -a で全バージョン表示 
$ gem search -r ^byebug$ -a
```

## インストール済みのgem の確認

* `gem list` でインストール済みのgem を表示
* `gem which` でインストール済みのgem のパスを表示
* `^文字列`で前方一致
* `文字列$`で後方一致
* `^文字列$`で完全一致

```Shell
# インストール済みgem の表示
$ gem list

# インストール済みgem からbyebug を完全一致検索
$ gem list ^byebug$

# インストール済みのbyebug のインストールのパス表示
$ gem which byebyg
```

## gem のインストール

`gem install` でgem をインストール

```Shell
# byebug をインストール
$ gem install byebug
```

## byebug とは

* `irb` 上で動くデバッガ
* `pry` 上で動く`pry-byebug` もある
* ブレークポイントを追加して途中で処理を止めることができる

## byebug の使い方

byebug からプログラムを起動

```Shell
# byebyg コマンドで実行
$ byebug fizzbuzz.rb

# ruby コマンドで実行
$ ruby -rbyebyg fizzbuzz.rb
```

* `continue`, `c`: ブレイクポイントまで処理を続行
* `continue 行番号`: 行数まで処理を進める
* `step`, `s`: ステップ実行、1行ずつ処理を進める
* `finish`, `fin`: 実行中のスタックを全部実行し終わった直後まで戻る
* `restart`: プログラムを最初からやり直す
* `next`, `n` : ステップオーバー、メソッドの中に入らずに処理を進める
* `break ファイル名:行番号`: ブレークポイントを追加 
* `break ファイル名:行番号 if 条件`: 条件付きでブレークポイントを追加
* `info breakpoints`: ブレークポイントの一覧を表示
* `disable/enable breakpoints <ID>`: <ID> のブレークポイントの無効化、ID 無しで全ブレークポイントの無効化
* `delete <ID>`: <ID> のブレークポイントの削除、ID 無しで全ブレークポイントを削除
* `list`: 停止中の周辺コードを表示
* `quit`, `q!`: プログラムの停止、プログラムの強制停止
* 変数名がbyebug のコマンドとぶつかる場合は`eval` メソッドを使う
* リターンキーで入力したコマンドを再度実行できる










