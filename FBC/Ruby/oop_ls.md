# OOP版lsコマンド

## オブジェクト指向のクラス設計のポイント

[Docs: オブジェクト指向「らしいクラス」と「らしくないクラス」の見分け方 \| FBC](https://bootcamp.fjord.jp/pages/class-like-class-and-non-class-like-class)

- オブジェクトを作成することでデータを表現するような設計が望ましい
  - 例) 投球: Shotオブジェクトを複数作成することで投球を表現する 

- オブジェクト指向らしいクラス設計
  - クラス名が単数形の名詞
  - インスタンス変数で保持する値を表現している 

- クラスの抽出方法
  1. プログラムを実行する上で必要な「データ」に着目する
  2. 一緒に使われることの多いデータをグルーピングする
  3. 最初はクラスではなく、ハッシュで定義してニュアンスを掴む

```
# データをハッシュとしてまとめる
alice = {
  name: "Alice",
  date_of_birth: Date.new(2000, 4, 1),
  address: "東京"
}
```

## 参照URL

[lsコマンドの使い方と覚えたい15のオプション【Linuxコマンド集】](https://eng-entrance.com/linux_command_ls)

[lsコマンドで表示されるファイルのモード\(drwxr\-xr\-x\) 〜RubyのFile::Stat\#modeとは〜](https://zenn.dev/universato/articles/20201202-z-mode)

[library optparse \(Ruby 3\.3 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/library/optparse.html)

[コマンドライン引数によるオプションに対応する \(optparse\) \- まくまくRubyノート](https://maku77.github.io/ruby/io/optparse.html)

[Docs: コマンドライン引数・オプションの処理 \| FBC](https://bootcamp.fjord.jp/pages/251)

[class File \(Ruby 3\.3 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/class/File.html)

[提出物: ls コマンドを作る5 \| FBC](https://bootcamp.fjord.jp/products/3763)

[提出物: ls コマンドを作る5 \| FBC](https://bootcamp.fjord.jp/products/8188)


## クラス設計

[コマンドラインメモアプリを作成 by karlley · Pull Request \#3 · karlley/js\-practices](https://github.com/karlley/js-practices/pull/3/files)

[OOP版ボウリングのスコア計算プログラムを作成 by karlley · Pull Request \#11 · karlley/ruby\-practices](https://github.com/karlley/ruby-practices/pull/11/files)

[Docs: オブジェクト指向「らしいクラス」と「らしくないクラス」の見分け方 \| FBC](https://bootcamp.fjord.jp/pages/class-like-class-and-non-class-like-class)

[Docs: 良いクラス・悪いクラス \| FBC](https://bootcamp.fjord.jp/pages/good-class-bad-class)

## 手順

[Docs: プログラムを書くときの考え方 \| FBC](https://bootcamp.fjord.jp/pages/147)

1. 入力と出力を考える
2. データ構造を考える
3. ライブラリ探す
4. ベタに書く
5. クラスに分ける
6. 清書する

## values_at

分割代入できる
https://docs.ruby-lang.org/ja/latest/method/Hash/i/values_at.html
