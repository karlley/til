# rails_present?_any?

`present?` と`any` の違い

## 参照

[present? と any? の使い分けを読み手の気持ちになって考える \- Qiita](https://qiita.com/kfukai23/items/dd88aaebbeae1dfcd755)

[Array, Hash, Range, Enumerator](https://docs.ruby-lang.org/ja/latest/class/Enumerable.html)

[Ruby Enumerableモジュールを理解して繰り返し処理を書く際の選択肢を増やす \- bagelee（ベーグリー）](https://bagelee.com/programming/ruby-on-rails/ruby-enumerable/)

## ポイント

* `present?`: すべてのどのクラスのインスタンスに対しても呼べる
* `any?`: Enumerble モジュールをインクルードしているクラスのインスタンスにしか呼べない
* Enumerble モジュールをインクルードしているクラス: Array, Hash, Range, Enumerator等

## 使い分け

* `present?`: レシーバの型を意識せずに、単に値が存在しているかを確認する際に使用
* `any?`: 繰り返しを意識した型の存在を確認する際に使用
