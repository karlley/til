# calender

* nteractive Ruby の強制終了
* 配列の任意のインデックスから最後までの値の取得
* 文字列の右詰め、左詰め、0埋め
* 文字列の中央寄せ、右詰め、左詰め
* p、puts、print の違い
* コマンドラインのオプション
* パースとは
* 標準出力に色を付ける

## interactive Ruby の強制終了

`irb` と`pry` ではコマンドが異なる

[pryのbinding\.pryでループ処理のデバッグ中に強制終了したいときは\!\!\!が便利\!\!\! \- Qiita](https://qiita.com/kazuph/items/dfbdf957ddca904aeb14)

[rspecでのループ中で止まるbinding\.pryを強制終了させるコマンド](https://zenn.dev/yukito0616/articles/c6f7495bb994a2)

* irb の強制終了: `exit! `
* pry の強制終了: `111`、`exit-program`

## 配列の任意のインデックスから最後までの値の取得

配列の中から任意のインデックスから最後の値を取得する方法

[【ruby】配列の指定の位置から最後までを抜き出す \- Yohei Isokawa](https://blog.yuhiisk.com/archive/2018/05/01/ruby-array-slice-to-last.html)

```Ruby
# 配列の中の4番目から最後までの値を取得
some_array = [1, 2, 3, 4, 5]
puts some_array(3..-1)
#=> [4, 5]
```

## 文字列の右詰め、左詰め、0埋め

`sprintf` で文字列を右詰め、左詰め、0埋めする

[sprintf フォーマット \(Ruby 3\.0\.0 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/doc/print_format.html)

[【C言語】printf で 左詰め 右詰め ゼロ埋めを行う方法と豆知識 \| MaryCore](https://marycore.jp/prog/c-lang/left-right-zero-padding/)


```Ruby
s = 5

# 4桁で右詰め
p sprintf("%4d", s)
#=> "   5"

# 4桁で左詰め
p sprintf("%-4d", s)
#=>> "5   "

# 4桁で0埋め
p sprintf("%04d", s)
#=> "0005"
```

## 文字列の中央寄せ、右詰め、左詰め

`center`、`rjust`、`ljust` で文字列の中央寄せ、右詰め、左詰め

[文字列を中央寄せ・左詰・右詰する \- 逆引きRuby](https://scrapbox.io/rubytips/%E6%96%87%E5%AD%97%E5%88%97%E3%82%92%E4%B8%AD%E5%A4%AE%E5%AF%84%E3%81%9B%E3%83%BB%E5%B7%A6%E8%A9%B0%E3%83%BB%E5%8F%B3%E8%A9%B0%E3%81%99%E3%82%8B)

[\[Ruby\] 文字列操作 \- Life with IT](https://l-w-i.net/t/ruby/string_001.txt)



## p、puts、print の違い

`p`、`puts`、`print` の違い

[【ruby】p pp puts print　違い。 \- Qiita](https://qiita.com/Takahashiq/items/f5d84581d3a301a9c22f)

[「puts、print、p」の違いについて【Ruby入門】](https://zenn.dev/nagan/articles/ae479d26e6d2b0)

[【Ruby超入門】print、puts、pの違い \| yukimasablog](https://yukimasablog.com/ruby-print-puts-p#:~:text=%E4%B8%BB%E3%81%AA%E9%81%95%E3%81%84,%E6%A7%98%E3%81%AA%E7%82%B9%E3%81%8C%E3%81%82%E3%82%8A%E3%81%BE%E3%81%99%E3%80%82&text=%E3%80%8Cputs%E3%80%8D%E3%81%A8%E3%80%8Cp%E3%80%8D%E3%81%AF%E5%87%BA%E5%8A%9B%E3%81%97%E3%81%9F%E5%BE%8C%E3%81%AB%E6%94%B9%E8%A1%8C,%E6%83%85%E5%A0%B1%E3%82%82%E5%87%BA%E5%8A%9B%E3%81%97%E3%81%BE%E3%81%99%E3%80%82)

### p

* オブジェクトを出力
* オブジェクト自身が返り値
* 文字列は`""` で囲まれる
* 改行文字`\n` がそのまま出力される
* デバック用

```Ruby
# 数値
p 123
123
=> 123

# 文字列
p "abc"
"abc"
=> "abc"

# 改行文字
p "a\nbc"
"a\nbc"
=> "a\nbc"
```

### print

* 文字列に変換して出力
* `print`自身の返り値は`nil`
* `""` は出力されない
* 改行文字`\n` は出力されないが改行される

```Ruby
# 数値
print 123
123=> nil

# 文字列
print "abc"
abc=> nil

# 改行文字
print "a\nbc"
a
bc=> nil
```

### puts

* 文字列に変換して出力
* `puts` 自身の返り値は`nil`
* `""` は出力されない
* 改行文字`\n` は出力されない
* 改行を加えて返り値を出力

```Ruby
# 数値
puts 123
123
=> nil

# 文字列
puts "abc"
abc
=> nil

# 改行文字
puts "a\nbc"
a
bc
=> nil
```

## コマンドラインのオプション

コマンドラインにオプションを追加する

* `optparse`と`getopts` の2つの方法がある
* `require optparse` で読み込む
* ショート、ロング、引数有りのオプションが指定可能
* `getopts` ロングオプションでないとデフォルト値が設定できない

## パースとは

> コンピュータプログラムの機能・処理の一つで、一定の書式や文法に従って記述されたデータを解析し、プログラムで扱えるようなデータ構造の集合体に変換すること

[パース（parse）とは \- IT用語辞典 e\-Words](https://e-words.jp/w/%E3%83%91%E3%83%BC%E3%82%B9.html)

データの集合を規則的に分解してデータ構造を変更して違うデータの集合に変換するイメージ

## 標準出力に色を付ける

エスケープシーケンスを使って標準出力に色を付けることができる

[RubyでANSIカラーシーケンスを学ぼう！ \- hp12c](https://keyesberry.hatenadiary.org/entry/20101107/p1)

* 文字色、背景色を指定可能
* 0 - 47 で色を指定する
* `\e[色番号m` で色を指定する
* 色指定は複数指定が可能
* 指定した文字色、背景色はデフォルトに戻さない限り継続的に出力結果に反映される

```Ruby
# テキストに色番号で指定
print "\e[色番号mテキスト"

# 文字を黒、背景を白
print "\e[30m\e[47mテキスト"

# 文字を黒、背景を白、テキスト出力後にデフォルトの色に戻す
print "\e[30m\e[47mテキスト\e[0m"
```
