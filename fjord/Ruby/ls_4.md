## lsコマンドのlオプションについて

表示されるファイルの属性一覧

[「ls \-l」コマンドの表示からファイルの属性を理解しよう：“応用力”をつけるためのLinux再入門（9）（1/4 ページ） \- ＠IT](https://atmarkit.itmedia.co.jp/ait/articles/1605/18/news015.html)

* ファイルの種類
  * `-`: ファイル
  * `d`: ディレクトリ
  * `l`: シンボリックリンク
  * `c`: キャラクタ型デバイスファイル
  * `b`: ブロック型デバイスファイル
* パーミッション
  * user、group、others 毎に権限を表示
  * `r`: 読み出し
  * `w`: 書き込み
  * `x`: 実行
  * `-`: 権限無し
* ハードリンク数
* オーナー、グループ
  * `オーナー`: ファイルを作成したuser？
  * `グループ`: user が属しているグループ？
* ファイルサイズ
* タイムスタンプ
  * `Access`: 最終アクセス日
  * `Modify`: 最終更新日
  * `Change`: 最終ステイタス変更日
  * lオプションでは`Modify` が表示される
* ファイル名

## ハードリンク数について

ファイルのハードリンク数は該当ファイルに下記の2つが必ず追加される

* `.`: ファイル自身
* `..`: 親ディレクトリ

結果的にハードリンク数は最低でも`サブディレクトリの数 + 2` になる

## ファイル、ディレクトリの詳細を出力する

* `File.stat`: ファイルの情報を表示、シンボリックリンク先の情報を取得
* `File.lstat`: ファイルの情報を表示、シンボリックリンク自体の情報を取得

[File::Stat\.new \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/File=3a=3aStat/s/new.html)

[File\.lstat \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/File/s/lstat.html)

[RubyでFile::Statを使って情報引き出す \- Qiita](https://qiita.com/uetennis/items/0591409523ee670c79e9)

* `File.stat(詳細を表示したいファイル、ディレクトリ)`
* File クラスのインスタンスが作成される
* File クラスのインスタンスメソッドを使ってファイルの詳細にアクセスできる

```
# ファイルの詳細を表示
> data = File::Stat.new($:[0])
> pp data
#<File::Stat
 dev=0x100000e,
 ino=1216074,
 mode=040755 (directory rwxr-xr-x),
 nlink=3,
 uid=501 (karlley),
 gid=80 (admin),
 rdev=0x0 (0, 0),
 size=96,
 blksize=4096,
 blocks=0,
 atime=2021-12-25 15:56:33 +0900 (1640415393),
 mtime=2021-09-30 03:47:10 +0900 (1632941230),
 ctime=2021-12-25 15:56:33.252585932 +0900 (1640415393)>
=> #<File::Stat dev=0x100000e, ino=1216074, mode=040755 (directory rwxr-xr-x), nlink=3, uid=501 (karlley), gid=80 (admin), rdev=0x0 (0, 0), size=96, blksize=4096, blocks=0, atime=2021-12-25 15:56:33 +0900 (1640415393), mtime=2021-09-30 03:47:10 +0900 (1632941230), ctime=2021-12-25 15:56:33.252585932 +0900 (1640415393)>

$ data.class
=> File::Stat

$ data.size
ファイルサイズが表示
```

## lオプションの表示に必要な属性

[ファイルの情報を取得する \- Ruby Tips\!](https://rubytips86.hatenablog.com/entry/2014/03/19/211341)

* ファイルの種類: `mode`
* パーミッション: `mode`
* ハードリンク数: `nlink`
* オーナー: `uid`
* グループ: `gid`
* ファイルサイズ: `size`
* タイムスタンプ: `mtime`
* ファイル名: `引数のファイル名`

## lオプションのtotalについて

[lsコマンド　ファイルサイズ \| 覚え書き](https://ameblo.jp/keizounyan/entry-10027511662.html)

* 表示ファイルのsize の合計
* 表示ファイルが1件の場合は非表示

## ディレクトリとファイル名を連結する

[File\.join \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/File/s/join.html)

* `File.join(親ディレクトリ、連結したいファイル or ディレクトリ)`
* 連結したいディレクトリは複数指定可能
* 返り値は文字列

## 真偽値を返すメソッド名について

> 真偽値を返すメソッドには?をつける。isやhasを頭につけたり、pを最後につけたりはしない。

> この場合の「?」はメソッド名の一部分です。 慣用的に、真偽値を返すタイプのメソッドを示すために使われます。

[Rubyの命名規約 \- Qiita](https://qiita.com/takahashim/items/ccfd489c9b26f15b7193)

[Rubyで使われる記号の意味（正規表現の複雑な記号は除く） \(Ruby 1\.8\.7\)](https://docs.ruby-lang.org/ja/1.8.7/doc/symref.html)

[なぜrubyでget\_xxxというメソッド名をあまり使わないか \- komagataのブログ](https://docs.komagata.org/5620)

* `?` をメソッド名の末尾に付けることで真偽値を返すメソッドだと明示する
* `user?` のような`名詞 + ?` の形のメソッド名は名詞だけどok なのかな？

## uid からユーザ名を表示する

[module Etc \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/class/Etc.html)

[Rubyでファイルの所有ユーザ・グループを調べる \- Qiita](https://qiita.com/ainame/items/82896932ac6a8a8e3051)

> /etc に存在するデータベースから情報を得るためのモジュールです。

* `Etc` モジュールを使う
* `File.stat` でuid を取得

```
require 'etc'

uid = File.stat('.').uid
user = Etc.getpwuid(uid)
user_name = Etc.getpwuid(uid).name

puts uid
#=> 501

puts user
#=> #<struct Etc::Passwd name="karlley", passwd="********", uid=501, gid=20, gecos="karlley", dir="/Users/karlley", shell="/bin/zsh", change=0, uclass="", expire=0>

puts user_name
#=> "karlley"
```

## Time オブジェクトから目的の値だけ抜き出す

[class Time \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/class/Time.html)

[\[Ruby入門\] 14\. 日付と時刻を扱う（全パターン網羅） \- Qiita](https://qiita.com/prgseek/items/c0fc2ffc8e1736348486)

```
now = Time.now

puts now
=> 2022-02-14 06:04:57.7617 +0900

puts "#{now.year}年 #{now.month}月 #{now.day}日 #{now.hour}時#{now.min}分"
#=> 2022年 2月 14日 6時4分
```

## Time オブジェクトを文字列に変換する

[Time\#strftime \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Time/i/strftime.html)

[\[Ruby入門\] 14\. 日付と時刻を扱う（全パターン網羅） \- Qiita](https://qiita.com/prgseek/items/c0fc2ffc8e1736348486)

```
now = Time.now

puts now
=> 2022-02-14 06:04:57.7617 +0900

puts now.strftime("%H:%M")
#=> "04:22"
```

## File::Stat#mode の表示内容ついて

[lsコマンドで表示されるファイルのモード\(drwxr\-xr\-x\) 〜RubyのFile::Stat\#modeとは〜](https://zenn.dev/universato/articles/20201202-z-mode)

ファイルタイプとパーミッションが数値で表示される

* 10進数で表示される
  * ファイルタイプ
  * スティッキービット、SUID、SGID
  * 所有者権限
  * グループ権限
  * その他のユーザー権限

## 2進数、8進数について

* 2進数(binary digit): `0b` で始まる
* 8進数(octal digit): `0o`か`0` で始まる(必ずではないみたい)

## mode のファイルタイプとパーミッションを変換する

* `file.mode` での出力は10進数
* `file.mode.to_s(8)` で8進数に変換
* `file.ftype` でファイルタイプを文字列で表示

```
# カレントディレクトリのパスを取得
> path = Dir.getwd
#=> "/Users/karlley/Workspace/ruby-practices/05.ls"

# カレントディレクトリからFile インスタンス作成
> File::Stat.new(path)
#=> ...
#=> mode=040755 (directory rwxr-xr-x),

# mode のみ表示(10進数)
> file.mode
#=> 16877

# mode のみ表示(8進数)
> file.mode.to_s(8)
> printf "%o\n", fs.mode
#=> 40755

# ファイルタイプを文字列で表示
> file.ftype
#=> "directory"
```

## FIFO とは

最初に入れたデータを最初に取り出すデータ構造

* First In First Out
* 先入れ先出し

## スティッキービット

[スティッキービット（sticky bit）とは \- IT用語辞典 e\-Words](https://e-words.jp/w/%E3%82%B9%E3%83%86%E3%82%A3%E3%83%83%E3%82%AD%E3%83%BC%E3%83%93%E3%83%83%E3%83%88.html)

[【Linux】スティッキービットはなぜ必要なのか？ \- \(O\+P\)ut](https://www.mtioutput.com/entry/2018/11/20/194738)

[Linux: SUID、SGID、スティッキービットまとめ \- Qiita](https://qiita.com/aosho235/items/16434a490f9a05ddb0dc)

* 特殊なアクセス権
* 8進数での数値表現: `1000`
* ディレクトリに指定する
* ファイル/ディレクトリの所有者とスーパーユーザのみ変更や削除が出来るようにする
* システムが自動生成するファイルを一般ユーザに削除できないようにする目的等で使用される
* 削除に対して制限する為の機能
* 一般ユーザの実行権限が`t`になる(実行権限が無い場合は`T`)
* 英語圏では`restricted deletion bit`(削除を制限するビット)と呼ばれる

## SUID、SGID

[Linux: SUID、SGID、スティッキービットまとめ \- Qiita](https://qiita.com/aosho235/items/16434a490f9a05ddb0dc)

特殊な実行権限

* SUID: `Set User ID`
  * 8進数での数値表現: `4000`
  * ファイルに指定する
  * 所有者の権限で実行される
  * 所有者の実行権限が`s`になる(実行権限が無い場合は`S`)
* SGID: `Set Group ID`
  * 8進数での数値表現: `2000`
  * ファイル、ディレクトリ共に指定可能
  * ファイル: 所有グループの権限で実行される
  * ディレクトリ: SGID を指定したディレクトリ配下のファイルとディレクトリにグループを継承する
  * 所有者の実行権限が`s`になる(実行権限が無い場合は`S`)

## ハードリンク、シンボリックリンクの違い

[シンボリックリンクとハードリンクの違い \- Qiita](https://qiita.com/katsuo5/items/fc57eaa9330d318ee342)

[ハードリンク \(hard link\)とは｜「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典](https://wa3.i-3-i.info/word1152.html)

* ハードリンク
  * 実在するファイルへのリンク
  * 参照元を削除してもアクセス可能
* シンボリックリンク
  * ショートカット的な
  * 参照元を削除するとアクセスできない
  * ファイル、ディレクトリ両方を参照可能

## シンボリックリンクの作成

`ln` コマンドに`-s` オプションを追加して作成する

```
$ ln -s ファイル名 リンク名

# file_name を参照するシンボリックリンクfile_name_link を作成
$ ln -s file_name file_name_link
```

## パーミッション変更

数値モードで全体のパーミッションを変更

```
$ chmod パーミッションを8進数を指定 ファイル/ディレクトリ名
```

シンボルモードで特定のパーミッションのみ変更

```
$ chmod [ugoa][+-=][rwx] 
$ chmod u+x file_name # file_name の所有者に実行権限を追加
```

## スティッキービット、SUID、SGID の追加

`chmod` コマンドで追加したいパーミッションを8進数で追加

* スティッキービット: `1000`
* SUID: `4000`
* SGID: `2000`

```
# スティッキービット
$ chmod 1777 dir_name

# SUID
$ chmod 4777 file_name

# SGID
$ chmod 2777 file_name
```

## 文字列から指定した文字列のみ抽出

[String\#\[\] \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/String/i/=5b=5d.html)

`slice`、[nth] を使った2つの方法がある

```
> s = "abcdefg"

# bを抽出
> s.slice(1)
> s[1]
#=> "b"

# 末尾からfを抽出
> s.slice(-2)
> s[-2]
#=> "f"

# 末尾から3文字efgを抽出
> s.slice(-3,3)
> s.slice(-3..-1)
> s[-3,3]
> s[-3..-1]
#=> "efg"
```

## case文をハッシュで置き換える

* case文はできるだけ使わない方が良いということ？
* rubocop で``Consider replacing `case-when` with a hash lookup.``と怒られた際の対策

[rubocopに追加された Style/HashLikeCase Consider replacing case\-when with a hash lookup \- Qiita](https://qiita.com/rorensu2236/items/86350b99c36336ef4850)

```
s = 'a'

# case文
case s
when 'a'
  'aだよ'
when 'b'
  'bだよ'
end

# ハッシュで置き換え
RESULT = {
  'a' => 'aだよ',
  'b' => 'bだよ',
}
RESULT[s]
```

## シンボルに数値は使えない

ハッシュのキーとして使用するシンボルに数字から始まる文字は使えない

```
str = "0"
int = 0

str.to_sym
#=> :"0"

int.to_sym
#=> undefined method `to_sym' for 0:Integer (NoMethodError)
```

## 文字列のn番目を置換する

* `string[]`を使ってn番目の文字列を置換する
* 返り値は引数
* 元の文字列が置換される

[String\#\[\]= \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/String/i/=5b=5d=3d.html)

```
string = "abc"

# 2番目のbをBに置換
string[1] = "B"
#=> "B"

puts string
#=> "aBc"
```

## lオプションのtotal について

`total` で表示される数値について

[コマンド ls \-l　で出力されるtotalについて](https://teratail.com/questions/239417)

[ls \-laコマンドのtotalについて](https://teratail.com/questions/292844)

[【コマンド】lsの\-lオプションとは何か？ファイルやディレクトリのアクセス権限と所有者をまとめて確認・変更する方法（見方, ブロック数, d lとは何か）](https://prograshi.com/general/command/ls-l-option/)

* ファイルサイズの合計ではない
* 全てのファイルのサイズをブロック化した合計値
* サブディレクトリのファイルサイズはカウントされない
* `512バイト`のブロック単位で切り上げで表示
* 1ブロックサイズは`512バイト`(デフォルト値)
* デフォルトのブロックサイズは`512バイト`や`1024バイト` などファイルシステムによって異なる
* `ls -ls`で表示されるブロック数の合計

```
$ ls -ls
total 24
# 一番左のブロック数(8,8,8,0,0)の合計値がtotal(24)に表示される
8 -rw-r--r--   1 karlley  staff   228  2  3 05:13 Gemfile
8 -rw-r--r--   1 karlley  staff   862  2  3 05:13 Gemfile.lock
8 -rwxr-xr-x   1 karlley  staff  2503  2 19 03:27 list.rb
0 drwxr-xr-x@ 18 karlley  staff   576  2 13 06:13 sample_dir_1
0 drwxr-xr-x  15 karlley  staff   480  2 17 07:44 sample_dir_2
```

```
# man ls でのlオプションについての説明
The listing of a directory's contents is preceded by a labeled total number of blocks used in the file system by the files which are listed as the directory's contents (which may or may not include . and .. and other files which start with a dot, depending on other options).

The default block size is 512 bytes.  The block size may be set with option -k or environment variable BLOCKSIZE.  Numbers of blocks in the output will have been rounded up so the numbers of bytes is at least as many as used by the corresponding file system blocks (which might have a different size).

# 和訳
ディレクトリの内容のリストには，そのディレクトリの内容としてリストアップされるファイル (他のオプションによっては，.や.などのドットで始まるファイルを含むことも含まないこともある) によってファイルシステムで使われるブロック数の合計が表示される．

デフォルトのブロックサイズは512バイトです。 ブロックサイズはオプション -k または環境変数 BLOCKSIZE で設定することができる。 出力されるブロックの数は、対応するファイルシステムのブロックが使用するバイト数と同じになるように切り上げられる。
(サイズが異なる場合があります)。
```

## 配列内の数値の合計を算出する

`inject` メソッドを使って配列内の数値の合計を算出する

[Enumerable\#inject \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/inject.html)

[【Ruby】\`inject\(:\+\)\`によるリファクタリングと\`:\+\`の意味 \- Qiita](https://qiita.com/terufumi1122/items/c060d86f966e566a0520)

```
array = [1, 2, 3]

# ブロックを使うパターン
array.inject{ |sum, i| sum + i}
#=> 6

# シンボルを使うパターン
array.inject(:+)
#=> 6
```

## 数値0の時にtrue を返す

* 数値`0` を`zero?` で判断する
* 似たメソッドに`empty?` があるが配列の要素数などを判断するものなので使用用途が異なる

[Numeric\#zero? \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Numeric/i/zero=3f.html)

[Array\#empty? \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Array/i/empty=3f.html)

```
0.zero? #=> true
[].empty? #=> true
```

## total の仕様について

* 分かっていること
* `man ls` で分かったこと
* 新たに分かったこと
* ファイルシステムのブロックサイズについて

### 分かっていること

* ブロックサイズの合計値
* サブディレクトリはカウントしない
* `ls -ls` でファイル毎のブロック数を表示
* ファイル毎のファイルサイズからブロック数を算出する？
* `Fileオブジェクト.blksize` でブロックサイズを表示できる

### man ls で分かったこと

* デフォルトのブロックサイズは512B(←何のデフォルト？ OS？ lsコマンド？)
* ファイルシステムで使われるブロック数の合計値
* `出力されるブロックの数は、対応するファイルシステムのブロックが使用するバイト数と同じになるように切り上げられる。`

### 新たに分かったこと

* OS のファイルシステムのブロックサイズの情報が必要？
* `ls -lsk` でキロバイト表示でブロック数を表示できる
* mac のファイルシステムは`APFS`(macOS 付属のディスクユーティリティで確認)
* `4096バイト` = 1ブロック

```
# mac のファイルシステム？を表示
$ diskutil info /

# mac のブロックサイズを確認
$ diskutil info / | grep "Block Size"
   Device Block Size:         4096 Bytes
   Allocation Block Size:     4096 Bytes
```

[hard drive \- What are the sector sizes on Mac OS X? \- Ask Different](https://apple.stackexchange.com/questions/78802/what-are-the-sector-sizes-on-mac-os-x)

[du と ls ファイルサイズの違い \- よくわからないエンジニア](https://www.unknownengineer.net/entry/2019/01/08/171500#:~:text=Block%20size%E3%81%AF4096%E3%81%A7,%E9%A0%98%E5%9F%9F%E3%82%92%E7%A2%BA%E4%BF%9D%E3%81%97%E3%81%BE%E3%81%99%E3%80%82)

[ls4 に終わりが見えた！ \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/reports/48504)

[\-lオプションの「total」は計算できたが、今度は並び替えがむずそう \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/reports/48239)

### ファイルシステムのブロックサイズについて

検証用ファイルを作成して色々確認

* 0バイトの時のブロックサイズ: `0`
* 1バイトの時のブロックサイズ: `8`
* 4096バイトの時のブロックサイズ: `8`
* 4097バイトの時のブロックサイズ: `16`

```
# 0バイト
$ ls -ls test.md
0 -rw-r--r--@ 1 karlley  staff  0  2 21 06:13 test.md

# 1バイト
$ ls -ls test.md
8 -rw-r--r--@ 1 karlley  staff  1  2 21 06:15 test.md

# 4096バイト
$ ls -ls test.md
8 -rw-r--r--@ 1 karlley  staff  4096  2 21 06:19 test.md

# 4097バイト
$ ls -ls test.md
16 -rw-r--r--@ 1 karlley  staff  4097  2 21 06:20 test.md
```

分かった事

* 0バイトは0ブロック
* 1バイトから4096バイトまでは1ブロック
* 4096を1ブロックとしてブロック数を計算
* `(ファイルサイズ / ブロックサイズ).ceil`で繰り上げを行い計算する？
* そもそも0バイトは計算しない？


## 細かい重複をメソッド化することについて

* 重複する記述の量によってメソッドに切り出すかどうか判断する
* 10行を超えるかどうかをメソッドに切り出す目安の1つにする
* 短い処理を無理にメソッドに切り出す必要は無い
* 過度なDRYによるメソッド数の増加は逆に可読性を下げる可能性もある

## 1つの文字列を配列化する

文字列を`[]`で囲むことで配列化できる

```
string = "abc"
array = [string]
array
#=> ["abc]
array.class
#=> String
```
