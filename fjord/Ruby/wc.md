# wcコマンドとは

* `wc ファイル名` でテキストファイルの`行数`、`単語数`、`バイト数`、`ファイル名`が表示される
* 複数のファイル名を引数に渡すことができる
* 複数のファイル名を渡した場合は出力最終行に`total` で`行数`、`単語数`、`バイト数`の合計を表示
* `単語数`は半角スペース、タブ、改行で区切られた文字の集まりで判断
* 語源は`word count`？
* `l` オプションで各入力ファイルの行数のみを標準出力で表示
* ファイル名を指定しない場合は標準入力から読み込む(標準入力をテキストとしてカウント？)

# 終了条件

* `l` オプション必須
* 標準入力からも受け取れるようにする
* `|`で`ls` コマンドを繋げて実行できる
* ノーブレークスペースを含むマルチバイト文字を1単語多くカウントする機能は不要
* 単語の区切りの空白文字は半角スペース、タブ、改行(全角スペースは含まない)

# 入力/出力

* 入力
  * `ファイル名`
  * `lsコマンドの出力結果`
* 出力
  * 単一ファイル指定: `行数`、`単語数`、`バイト数`、`ファイル名`
  * 複数ファイル指定: 各ファイル毎に`行数`、`単語数`、`バイト数`、`ファイル名`、最終行に`行数計`、`単語計`、`バイト計`、`total`
  * 標準入力: `行数`、`単語数`、`バイト数`

# ビットとバイト

[バイト数 \(Byte数\)とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word16629.html)

* ビット: `0`、`1` が入る箱
* ビット数: `0`、`1`が入る箱が何個あるか
* バイト: ビットが8つ集まったもの(1バイト == 8ビット)
* バイト数: ビットが8つ集まったものが何個あるか(2バイト == 16ビット)
* ひらがな、漢字は3byte(中には4byte もある)

# ヒアドキュメントとは

長い文字列をプログラムに埋め込む為の記述方法

* `<<` の後に開始と終了を表す識別子を書く
* 識別子(統一されれば他の文字列でもok)
  * `EOS`（End Of String）
  * `EOL`（End Of Line）
  * `TEXT` 等
* 展開しない場合は`''`で識別子を囲う(`<<'EOS'`)
* `<<-識別子` で識別子をインデントできる
* `<<~識別子` で先頭のインデントを無視
* ヒアドキュメントに対してメソッドを呼べる(`<<EOF.upcase`)

[Rubyでヒアドキュメントを使う \- Qiita](https://qiita.com/mogulla3/items/3e114e9c4697f0dea84c)

```
# 文字列を代入
text = <<EOS
aaa
EOS
=> "aaa\n"

text
=> "aaa\n"
```

```
# 文字列展開
text = <<"EOS"
aaa
EOS
=> "aaa\n"

"文字列は#{text}"
"文字列はaaa\n"
```

```
# 先頭のインデントを無視
text = <<~EOS
    aaa
EOS
=> "aaa\n"

text
=> "aaa\n"
```

# 複数の引数の受け取る

[Object::ARGV \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Object/c/ARGV.html)

* `ARGV` で配列で引数を受け取る
* 引数に渡された値が配列として`ARGV` に代入される
* `ARGV[n]` の形で引数を取得できる

```
# test.rb

puts ARGV
```

```
# コマンド実行時に複数の引数を渡す
$ ruby test.rb aaa bbb ccc
aaa
bbb
ccc
```

# 標準入力から値の受け取り

* `STDIN`: 標準入力が格納される、組み込み定数
* `$stdin`: 標準入力が格納される、グローバル変数
* `gets`などのIOクラスのメソッドは、`$stdin`からデータを受け取っている(`gets == $stdin.gets`)

[IOクラスについて\($stdin, $stdout, $stderr, etc\.\.\)\[Ruby\] \- Qiita](https://qiita.com/takuyanin/items/d1d5f860a30716baac5b)

# irb がSwitch to inspect mode. で終了する

[パイプで標準入力を渡すと binding\.irb がinspect mode \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/questions/1203)

[Switch to inspect mode\. と表示され \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/questions/946)

## binding.irb のinspect mode とは

* `Inspect`: 検査、点検
* `irb --inspect [raw|p|pp|yaml|marshal|...]` でinspect mode でirb を起動(何に使うか分からない)

## 分かったこと

* `binding.irb`、`byebug`、`binding.pry` でも発生する
* `tty` を通してターミナルは出力を行っている
* パイプで`stdin` を渡した場合は`tty` に繋がっていない
* パイプで`stdin` を渡した場合はパイプの前のコマンドの`stdout` が繋がっている
* `irb` の標準入力は`tty` を通して入力を受け取っている
* `irb` を一時的に`tty` に繋ぎ直すことでパイプで受けた入力でデバッグが可能になる

## 原因

* `irb` の入力は`tty` を通してキーボードの入力を受け取る仕様
* `irb` が起動したプロセスが`tty` に繋がっていない場合は標準入力からのテキストを素のRuby コードとして評価して実行する
* `binding.irb` を記述したファイルに対してパイプで標準出力を渡した場合は`tty` に繋がっていないので`irb` で停止せずに終了した

## 解決法

`irb` の起動プロセスの入力を`tty` を通した入力に一時的に切り替えるメソッドを実行するコードに追加する

```
# 追加するメソッド
def prompt_with_tty
  $original_stdin = STDIN.dup
  STDIN.reopen("/dev/tty", "r")
  yield
  STDIN.reopen($original_stdin)
end

# デバッグ対象のコード
text = $stdin.gets
puts "input: #{text}"

# binding.irb の代わりに下記のコードを挿入
prompt_with_tty { binding.irb }
```

実行例

```
# ls` コマンドの標準出力をパイプでtest.rb に渡す
❯ ls | ./test.rb
input: Gemfile

From: ./test.rb @ line 12 :

     7:   STDIN.reopen($original_stdin)
     8: end
     9:
    10: text = $stdin.gets
    11: puts "input: #{text}"
 => 12: prompt_with_tty { binding.irb }

irb(main):001:0> text
=> "Gemfile\n"
```

## tty とは

> 「制御端末」のことです。何をしているかというと、私たちがターミナルのプログラムに対してキーボードでの入力をしたい時に、キーボードなどからの入力をいい感じに受け取ってプログラムに渡してくれる役割をしています。

## ファイルディスクリプタ

ファイルディスクリプタ(file discriptor): OS が入出力を識別するための識別子

* 標準入力(`stdin`): 0
* 標準出力(`stdout`): 1
* 標準エラー出力(`stderr`): 2

## STDIN と$stdin

[Object::STDIN \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Object/c/STDIN.html)

* `STDIN`: プロセス起動時の標準入力、ファイルディスクリプタ`0` を表す
* `$stdin`: 初期値の値は`STDIN` と同じ
* リダイレクトを行いたい場合は`$stdin` を使う？

## irb でのキーボードでの入力終了を入力する

[IRB::Context\#ignore\_eof= \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/IRB=3a=3aContext/i/ignore_eof=3d.html)

* `ctrl + D`: 入力終了を入力できる、`EOF` を入力する動作に近い
* `ctrl + C`: 実行する処理の強制終了、入力終了を行う際には使用しない

# IOクラスから文字列、バイナリを読み込む

[class IO \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/class/IO.html#S_FOR_FD)

[Ruby　標準入力から値を受け取る方法 \- Qiita](https://qiita.com/Hayate_0807/items/2e9705091b181a104621)

* テキスト読み込み、バイナリ読み込みメソッドの2種類が存在する
* IOオブジェクトは内部エンコーディング、外部エンコーディングの2種類のエンコーディングが存在する
* テキスト読み込みメソッドは引数に`chomp` を渡すことで`\n` 等を取り除くことができる

## テキスト読み込みメソッド

* `gets`: 1行読み込み、1つの配列を返す
* `getc`: 1文字読み込み、文字列を返す
* `readline`: 1行読み込み、文字列を返す
* `readlines`: 全行読み込み、1つの配列を返す、引数に`|` に続くコマンドの出力結果を渡せる
* `foreach`: 引数のファイルの各行をブロックにしてループを実行、引数に`|` に続くコマンドの出力結果を渡せる
* `each`: IO の現在位置から1行ずつ文字列として読み込み 

## バイナリ読み込みメソッド

* `read`: 指定した読み込みバイト数を読み込み、文字列を返す、読み込みバイト数が指定されない場合はテキスト読み込みとして機能する

# マルチバイト文字

[マルチバイト文字とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word1809.html)

> 2バイト以上のデータで表現する（1バイトでは表現できない）文字のこと

* 日本語の文字列 
* `\n`、`\r`、`\t` 等のメタ文字

# 入れ子の配列を平坦化する

[Array\#flatten \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Array/i/flatten.html)

* 空配列`[]` は削除される
* 平坦化されない場合は`nil` を返す

```
[1,[2],3].flatten
=> [1, 2, 3]

[1,[],2].flatten
=> [1, 2]

[1,2,3].flatten!
=> nil
```

# count メソッドでマッチする文字列の数を取得する

[Enumerable\#count \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/count.html)

`文字列.count('数えたい文字列')` で数えたい文字列の数を取得できる

```
# 'b' の数を取得
text = 'abbc'
text.count('b')
=> 2
```

# 文字列のシングルクオーテーション、ダブルクオーテーションの違い

[シングルクォーテーションとダブルクォーテーションの違い（Rubyの文字列） \- Qiita](https://qiita.com/ryosuketter/items/ddad508cb0124e4fe378)

[Ruby で文字列をシングルクォートで囲むかダブルクォートで囲むか問題 – どうのこうの](https://dounokouno.com/2017/07/04/ruby-string-quotation/)

* 特殊文字(`\n`、`\t` など)を埋め込む場合には`""` で囲う
* 式展開(`#{}` )を埋め込む場合には`""` で囲う
* `''` は特殊文字、式展開が無いことを明示する場合に使用する
* Rubocop の初期値は`""` に設定されている
* 基本的には`''` を使い、特殊文字と式展開がある場合のみ`""` を使うようにすれば良いのかな？

```
# 特殊文字を使った場合の'' と"" の違い
text_1 = 'aaa\n'
=> "aaa\\n"

text_2 = "aaa\n"
=> "aaa\n"

text_1 == text_2
=> false
```

# VSCode でタブを入力する

[VSCode \| タブキーを押したときにスペースに変換するのかに関する設定](https://www.javadrive.jp/vscode/setting/index9.html)

* デフォルトでタブ入力はスペースに変換されるようになっている
* `Editor: Insert Spaces` を`Disable` にすることタブ入力が可能
* タブサイズは`Editor: Tab Size` で設定できる、デフォルトは4文字

# 文字列を囲うのに''と""のどちらを使うか

[【Day111】wcコマンド作る \#14 \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/reports/50023)

> 基本的には'' を使い、特殊文字と式展開がある場合のみ"" を使うようにすれば良いのかな？
それでいいと思うし、なんならつねに""でもいいと思います！デメリットも特にないので。

* `""` を常用することにデメリットはない
* Rubocop のデフォルトは`""`
* 性能的にも`''`と`""`に差は無い
* 意識的に`''`と`""`を使い分ける手間も省けるので`""` を常用した方が良さそう

# 初期化した変数にループのブロック変数の値を加算する

代入する変数を先に初期化して`each` でブロック変数の値を加算する

```
array = [1,1,1]
result = 0

array.each do |i|
  result += i
end
#=> [1,1,1] # each の戻り値

puts result
#=> 3 # array の合計
```

# 代入演算子の種類

[Ruby入門 \- 演算子 \- とほほのWWW入門](https://www.tohoho-web.com/ruby/operators.html)

代入演算子の種類

```
a = b    # a に b を代入する
a += b   # a = a + b
a -= b   # a = a - b
a *= b   # a = a * b
a /= b   # a = a / b
a %= b   # a = a % b
a **= b  # a = a ** b
a &= b   # a = a & b
a |= b   # a = a | b
a ^= b   # a = a ^ b
a <<= b  # a = a << b
a >>= b  # a = a >> b
a &&= b  # a && (a = b)
a ||= b  # a || (a = b)
```

# ヒアドキュメントの識別子の種類

ヒアドキュメントに使用する識別子の種類と特徴

[リテラル \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/doc/spec=2fliteral.html)

* `<<識別子`: 基本、囲んだ部分を文字列リテラルとする
* `<<-識別子`: 終端の識別子をインデントする
* `<<~識別子`: 最もインデントが少ない行を基準にして全ての行の先頭から空白を除去する,
* `<<'識別子'`: 式展開が無効、特殊な記号等もそのままの文字列になる
* `<<"識別子"`: 式展開が有効、`<<識別子`と同じ

# ヒアドキュメントを使って長い文字列の可読性を改善する

長い文字列をヒアドキュメントに置き換えて可読性を改善する

```
# 出力したい文字列
aaaaaaa
bbbbbbb
ccccccc

# 基本
string = "aaaaaaa\nbbbbbb\nccccccc\n"
puts string

# ヒアドキュメント
string = <<TEXT
aaaaaaa
bbbbbbb
ccccccc
TEXT
puts string

# 変数を使わなでヒアドキュメントで文字列を定義
puts <<TEXT
aaaaaaa
bbbbbbb
ccccccc
TEXT
```

# ヒアドキュメントの識別子の文字列

* 最初と最後で同じ文字列を使う
* 記述する文字列の中に含まれない文字列を使う
* `_`等の記号も使えるみたい？
* 一般的な識別子の文字列
  * EOD: End of Document
  * EOF: End of File
  * EOL: End of Line
  * EOM: End of Message
  * EOS: End of String
  * EOT: End of Text

# 識別子の文字列には意味のある文字列が推奨される

[Class: RuboCop::Cop::Naming::HeredocDelimiterNaming — Documentation for rubocop \(1\.26\.1\)](https://www.rubydoc.info/gems/rubocop/RuboCop/Cop/Naming/HeredocDelimiterNaming)

識別子の文字列には意味のある文字列でないとRubocop で`Use meaningful heredoc delimiters.`と指摘される

* NG例
  * EOS
  * EOF
* OK例
  * TEXT
  * SQL
  * FILE

# 長い式を途中で改行する

[Ruby 1行が長くなりすぎるときの対応策 \- No Solution for Life](https://masuyama13.hatenablog.com/entry/2020/05/15/200125)

* `+`、`=` の後で改行する
* 改行したい箇所に`\`を付けて改行する(できれば避ける)

```
# + と= の後で改行
sum =
1 +
1
puts sum
#=> 2

# + と= の前に改行すると式が終了したとみなされる
sum =
1 # ここで式が終了している
+ 1
puts sum
#=> 1

# \ を使い改行
sum =
1\
+ 1
puts sum
#=> 2

# \ の前に空白が入っても改行とみなされる
sum =
1    \
+ 1
puts sum
#=> 2

# 文字列の場合は\の前の空白も文字列としてみなされる
string =
"a  \
b"
puts string
#=> "a  b"
```

# レビューから得た知見

* `.rubocop.yml` は上位ディレクトリが自動的に読み込まれる
* 小さいプログラムの場合はプロジェクト毎に`Gemfile` を作成する必要は無い
* ハッシュ名とキー名がセットでどんな値か想像できる命名にする(外側の箱と内側のモノは同じ命名はしない)
* `sum` は内部的にループが走るので処理効率を考えて使用する
* リファクタリングには行数を減らすことと処理の重さのバランスを考えることが重要
* Aメソッド内から別のBメソッドに引数を渡す為だけにAメソッドにBメソッドの引数を渡すのはOK
* どこでも変数を呼び出したいことを目的にしたインスタンス変数の使い方はNG

# インスタンス変数の間違った使い方

[改訂版・（あなたの周りでも見かけるかもしれない）インスタンス変数の間違った使い方 \- Qiita](https://qiita.com/jnchito/items/4d62693525f5023013cc)

## 戻り値と引数を使う理由

> 処理の過程で使う一時的なデータは、戻り値と引数を使おう

* ローカル変数を使うことでデータのスコープを過度に広げることを防止する
* メソッドの戻り値と引数を使うことでデータがどこで作られたか、どこに渡っていっているのか明確になる

## インスタンス変数について

> インスタンス変数はインスタンスが作成されてから（newされてから）最後までほとんど変わらない値

> インスタンス変数は「変数」という名前が付いていますが、処理の最中にコロコロ変わる値ではなく、異なるインスタンスが別個に保持するデータを格納するための変数だと考える

> 自分で独自のクラスを定義していないのであれば、基本的にインスタンス変数の出番は無い

# puts の長い文字列を改行する

[Ruby 1行が長くなりすぎるときの対応策 \- No Solution for Life](https://masuyama13.hatenablog.com/entry/2020/05/15/200125)

* `+`、`\`で文字列を連結する
* `+` の前で改行すると式が終わったと認識されるのでNG
* `\` を使った改行は可能であれば避けるべき

```
# + で文字列を連結
puts "text1 text2" +
      " text3"
=> text1 text2 text3

# これだとNG
puts "text1 text2"
     + " text3"
=> text1 text2

# \ で改行
puts "text1 text2"\
      " text3"
=> text1 text2 text3
```

## ハッシュの色々な初期化方法

* 空のハッシュを作成
* デフォルト値を設定1
* デフォルト値を設定2

```ruby
# 空のハッシュを作成
hash = Hash.new
hash
=> {}

# デフォルト値を設定1
hash = {key1: 1, key2: 2}
hash
=> {:key1=>1, :key2=>2}

# デフォルト値を設定2
hash = Hash.new { |hash, key| hash[key] = 0 } # 値が指定されていないkey には0をデフォルト値とする
hash[:key0] # デフォルト値が指定される
hash[:key1] = 1 
hash[:key2] = 2 
hash
=> {:key0=>0, :key1=>1, :key2=>2}
```