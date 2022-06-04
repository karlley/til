# bowling

## each_with_index

[Enumerable\#each\_with\_index \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/each_with_index.html)

* `each` を使ったループ中のインデックス番号を利用できる
* インデックス番号は`0` から始まる
* インデックス番号を`0` 以外から始めたい場合は`each.with_index(n)` とする事で`n` からインデックス番号を始められる

## each_slice

[Enumerable\#each\_slice \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/each_slice.html)

* 配列をn個の要素で分割できる
* 要素数が割り切れない場合は最後の要素数が減少する

## 変数の比較と代入は、条件式の返り値を使う

rubocop で条件式の記述について指摘に合わせた修正

```
Use the return of the conditional for variable assignment and comparison.

# 和訳
変数の代入や比較には、条件の戻り値を使用します。
```

Ruby のif 文は戻り値を返すので変数へ代入する記述は1箇所にまとめるべきみたいなので下記のように修正

```
# 修正前
point = 0
frames.each do |f|
  if f[0] == 10 # strike
    point += 30
  elsif f.sum == 10 # spare
    point += f[0] + 10
  else
    point += f.sum
  end
end
```

```
# 修正後
point = 0
frames.each do |f|
  point += if f[0] == 10 # strike
             30
           elsif f.sum == 10 # spare
             f[0] + 10
           else
             f.sum
           end
end
```

rubocop はこれでパスしましたが、すごく右までインデントされて見慣れない記述ですが、これで正しいのでしょうか？

## frozen_string_literal について

rubocop に指摘された`Missing frozen string literal comment.` について

[Ruby \- Ruby frozen\_string\_literal について｜teratail](https://teratail.com/questions/74592)

[文字列をfreezeさせるいくつかの方法 \- Qiita](https://qiita.com/jnchito/items/e2687e2bf2f49411a1b4)

[\(1\) Rubyではimmutable string literalといった非互換な機能の導入はどのようなプロセスを経て決定されますか？ \- Quora](https://jp.quora.com/Ruby%E3%81%A7%E3%81%AFimmutable-string-literal%E3%81%A8%E3%81%84%E3%81%A3%E3%81%9F%E9%9D%9E%E4%BA%92%E6%8F%9B%E3%81%AA%E6%A9%9F%E8%83%BD%E3%81%AE%E5%B0%8E%E5%85%A5%E3%81%AF%E3%81%A9%E3%81%AE%E3%82%88%E3%81%86)

> Ruby3においても frozen string literals をデフォルトとすることはない

> 使い捨てのRubyプログラムや小さなサンプルコードであれば、frozen_string_literalをわざわざ付けるメリットは少ないと思います。

* Ruby の文字列はミュータブル(変数の値を変更可能)である
* String オブジェクトに`freez` メソッドを呼び出すことでイミュータブル(変数の値が変更不可)にできる
* 実行ファイルに`# frozen_string_literal: true` を追記する事でファイル全体のString オブジェクトをイミュータブルにできる
* `# frozen_string_literal: true` でイミュータブルにできるのは文字列リテラルのみ
* 単項演算子`-` で`freez` された文字列を返すことができる(元の文字列は`freez` されない)
* 将来的にRuby の文字列リテラルがデフォルトでイミュータブルになるかも(Ruby3 ではなっていない)
* 小規模なコードの場合は`frozen_string_literal` を付ける必要性は低い

rubocop の指摘をパスするには`rubocop -A` で自動修正を掛けると実行ファイルに下記が追記される

```
# frozen_string_literal: true
```

## flatten メソッド

[Array\#flatten \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Array/i/flatten.html)

入れ子になった配列を平坦化する

```
# 平坦化
a = [1, [2, 3, [4], 5]].flatten
#=> [1, 2, 3, 4, 5]

# 平坦化の深さを指定する
a = [ 1, 2, [3, [4, 5] ] ].flatten(1)
#=> [1, 2, 3, [4, 5]]
```
