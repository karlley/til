# Rails method

メソッドに関して

## 参考URL

[オブジェクトのことをオブジェクト自身に問い合わせて確認する方法](https://qiita.com/mogulla3/items/7736fe54ebf97cacd898)

## method の分類

Rails で使用可能なメソッドの分類

* Ruby: インスタンス生成, 配列/ハッシュの生成/取得
  `new, each, sort`

* Rails: Ruby 単体では使用不可, Ruby のメソッドを拡張させたメソッドの集合体
  * ActiveSupport: Ruby を拡張した開発用のメソッドの集合体, gem として個別に公開されているのでRuby でも使用可
    `present?, blank?`
  * ActiveRecord: SQL が走るメソッド
    `all, save, order`

## 使用可能なメソッドの確認

* `respond_to? :method_name` でModel で使用できるメソッドを確認できる

```Ruby
class Sample
  def sample_method
  end
end
```

```Ruby
# sample_method が使用可能か確認

> sample = Sample.new
> sample.respond_to :sample_method
true
```
