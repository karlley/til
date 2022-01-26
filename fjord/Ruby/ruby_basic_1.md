# ruby_basic_1

ゼロからわかるRuby超入門

## レシーバ

* メソッドを呼び出されるオブジェクト
* レシーバのオブジェクトを調べるには`self` を使用

## attr_reader

同名のインスタンス変数を戻り値とするメソッドを定義するメソッド

* インスタンス変数の取得
* `attr_reader` の後ろにインスタンス変数から`@`を取り除いた名前をシンボルで記述
* `attr_reader` を使わない場合のメソッド名は「インスタンス変数名から`@`を取り除いたもの」

```Ruby
attr_reader :name
```

`attr_reader` を使わない記述

```Ruby
class Drink
  def order(item)
    puts "#{item}をください"
    @name = item
  end

  # drink オブジェクトの@name をクラス外から呼び出す為のメソッド
  def name
    @name
  end
end

drink = Drink.new
drink.order("カフェラテ")
puts drink.name #=> カフェラテ
```

## attr_writer

同名のインスタンス変数へ代入するメソッド

* インスタンス変数へ代入
* `attr_writer` の後ろにインスタンス変数から`@`を取り除いた名前をシンボルで記述
* `attr_writer`を使わない場合のメソッド名は「インスタンス変数名から`@`を取り除き、末尾に`=` を加えたもの」

```Ruby
attr_writer :name
```

`attr_writer` を使わない記述

```Ruby
class Drink
  # @name に代入する値を引数で受け取る
  def name=(text)
    @name = text
  end

  def name
    @name
  end
end

drink = Drink.new
drink.name= "カフェオレ"
puts drink.name #=> カフェオレ
```

## attr_accessor

`attr_reader` と`attr_writer` を同時に定義する

```Ruby
attr_accessor :name
```

## instance_variables メソッド

* オブジェクトが持っている全てのインスタンス変数を返す
* new されたオブジェクトに対してのみ使用可

## インスタンスメソッド/クラスメソッド

* インスタンスメソッド: インスタンスをレシーバとするメソッド
  * 定義方法: `def メソッド名`
  * 呼出方法: `インスタンス.メソッド名`
  * クラスメソッドを呼べる
* クラスメソッド: クラスをレシーバとするメソッド
  * 定義方法: `def self.メソッド名`
  * 呼出方法: `クラス.メソッド名`
  * インスタンスメソッドは呼べない(レシーバとなるインスタンスを決めれないから)

## クラスメソッドの定義方法

クラスメソッドの定義方法は複数ある

```Ruby
# 1つのメソッドのみ
class ClassName
  def self.method_name
  end
end

# 複数のメソッドを同時に定義
class ClassName
  class << self
    def method_1
    end

    def method_2
    end
  end
end
```

## メソッドの表記

各メソッドを記号で表記する方法

* インスタンスメソッド: `クラス名#メソッド名`
* クラスメソッド: `クラス名.メソッド名`、`クラス名::メソッド名`

```Ruby
# インスタンスメソッド
Drink#name

# クラスメソッド
Cafe.welcome
Cafe::welcome
```

## 継承

* 他のクラス(スーパークラス)のメソッド等を使えるようにした新たなクラス(サブクラス)を作成すること
* 継承元: スーパークラス、親クラス
* 継承先: サブクラス、子クラス
* `ancestors` メソッドで継承関係を表示できる
* 親子内で同名のメソッドが呼ばれば場合、自分のクラスのメソッドが優先して呼ばれる
* 親クラスのメソッドを呼び出す場合は`super` で呼び出せる

## private

`private` 以下に書いたメソッドのレシーバを指定しての呼び出しを禁止する

* 逆にレシーバを指定して呼び出せるメソッドは`public` メソッド
* `private` メソッドは`public` メソッドの下に書く
* `private_class_method` を使う事でクラスメソッドを`private` 化できる

```Ruby
class Cafe
  def staff
    makanai
  end

  private

  def makanai
    "staff only menu"
  end
end

cafe = Cafe.new
puts cafe.staff #=> staff only menu
puts cafe.makanai #=> 呼び出せない
```

## module

処理をまとめてクラスに読み込むことで処理を共通化できる

* 使うクラスでモジュールを`include` してインスタンスメソッドとして使用する
* モジュール自身ではインスタンスを作ることはできない
* 1つのファイルで使用する場合は`include` するクラスの前にモジュールを定義する
* `extend` メソッドを使用することでモジュールのメソッドをクラスメソッドとして使用可能になる
* `include` するクラス毎にモジュール名を分けたい場合にも使用する(名前空間)

## require_relative/require

処理やファイルを読み込むメソッドの違い

* `require_relative`
  * `./` が不要
  * 呼び出し元からの相対パス
* `require`
  * `./` が必要
  * カレントディレクトリからの相対パス

`include` との違い

* `require_relative`: クラスやモジュールを読み込む
* `include`: メソッドを読み込む

## 例外処理

例外処理に関する記述

* `begin`: 例外が発生する可能性がある処理
* `rescue`: 例外が発生した場合の処理
* `reise`: 例外を発生させる
* `ensure`: 例外の有無に関わらず必ず処理を実行

## self

その場所でのレシーバを返すもの

* インスタンスメソッド: そのクラスのインスタンスになる
* クラスメソッド: そのクラスになる
* 予約後の為、代入できない
* インスタンスメソッドとクラスメソッドの`self` はそれぞれ別のオブジェクト

## 変数

変数の種類

[ローカル変数・インスタンス変数・クラス変数の違い\(Ruby\) \- Qiita](https://qiita.com/mary_new_programmer/items/4c4353c3d1df7242f515)

* ローカル変数: `name`、定義したメソッドやブロック内で共有できる
* インスタンス変数: `@name`、インスタンス(オブジェクト)が共有できる
* クラス変数: `@@name`、継承したクラスでも値を共有できる

## ブロック

* `block_given?`: ブロックを渡されたか判断するメソッド
* `yield`: 渡されたブロックを実行する
* ブロックを引数で受け取るには引数の先頭に`&`を追加する
* 変数に代入されたブロックは`call` で実行できる
* `Proc`: ブロックの処理をオブジェクト化したもの
