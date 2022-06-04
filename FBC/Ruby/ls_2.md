## parse とは

プログラムで扱えるようなデータ構造の集合体に変換すること

## コマンドにオプションを設定する

[library optparse \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/library/optparse.html)

[ruby で ARGV をうけるコマンドっぽいものを作ってみる \| DriftwoodJP](https://www.d-wood.com/blog/2014/05/15_6221.html)

`getopts ` で複数のオプションをハッシュで管理する

* `getopts('オプション:)` でオプションに引数を追加
* オプションを複数追加してもコマンド引数のインデックスは`ARGV[0]`からスタートする

3つのオプションとコマンド引数を与える

```
# command.rb

#!/usr/bin/ruby
require 'optparse'
options = ARGV.getopts('a', 'r', 'l')
p ARGV[0]
p options
```

実行結果

```
$ ./command.rb -a -r -l filename
#=> "augument"
#=> {"a"=>true, "r"=>true, "l"=>true}
```

## PR のテンプレートを作成する

### 目的

* レビュワーの読むコストを下げる
* 良いレビューをもらい易くなる
* PR はドキュメントでもある

### テンプレートの設定方法

[リポジトリ用のプルリクエストテンプレートの作成 \- GitHub Docs](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)

[\.github/pull\_request\_template\.md](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)

1. GitHub 上で`Add file` からリポジトリのルートに`.github/pull_request_template.md` を作成
2. 作成したテンプレートの内容を追記
3. ページ下部の`Commit new file` から`デフォルトブランチ` にコミットする

* テンプレートはデフォルトブランチ(`main` や`master`)に設置する必要がある
* テンプレートの設置ディレクトリは`.github/`、`docs`、`プロジェクトのルート` が選べる

## 配列を正規表現で抽出する

[Enumerable\#grep \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/grep.html)

[Enumerable\#grep\_v \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/grep_v.html)

### grep

* 配列の中から正規表現にマッチした配列を返す
* マッチしない場合は空の配列を返す

```
ary = ['a', 'b', 'c']
ary.grep('b')
#=> ["b"]
```

### grep_v

* 配列の中から正規表現にマッチした値以外の配列を返す
* マッチしない場合は空の配列を返す

```
ary = ['a', 'b', 'c']
ary.grep_v('b')
#=> ["a", "c"]
```

## 二重コロン(::)について


[Rubyにおけるドット記法，二重コロン記法 \- Qiita](https://qiita.com/ktarow/items/772014a4f0d48905f3ef)

[rubyにおける：：（ダブルコロン、コロンコロン）についてのメモ \- Qiita](https://qiita.com/hatorijobs/items/87a2bd93f8666d77d711)

[chef \- ruby でクラス名の先頭に :: がつくときの意味について \- スタック・オーバーフロー](https://ja.stackoverflow.com/questions/2446/ruby-%E3%81%A7%E3%82%AF%E3%83%A9%E3%82%B9%E5%90%8D%E3%81%AE%E5%85%88%E9%A0%AD%E3%81%AB-%E3%81%8C%E3%81%A4%E3%81%8F%E3%81%A8%E3%81%8D%E3%81%AE%E6%84%8F%E5%91%B3%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)

[Ruby初心者を脱した人が悩みがちな、ちょっと特殊な記法・演算子・イディオム \- Qiita](https://qiita.com/nashirox/items/0c885edf7d78fd5a83f1#hogefoo)


* `.`と同じようにメソッド呼び出しに使用する
* 定数の呼び出しは`::`のみ可能
* スコープ演算子: クラスまたはモジュールで定義された定数を外部から参照できる

```
class Foo
  CONSTANT = "Foo::CONSTANT"

  def method_a
    puts "method_a"
  end
end

foo = Foo.new

# どちらもメソッド呼び出しに使える
foo.method_a #=> "method_a"
foo::method_a #=> "method_a"

# 定数呼び出しは:: のみ可能
Foo.CONSTANT #=> undefined method
Foo::CONSTANT #=> "Foo::CONSTANT"
```


